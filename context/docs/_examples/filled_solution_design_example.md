---
title: "Helio — Real-Time Invoicing API: Solution Design"
status: active
record_class: canonical
audience: [internal]
owner: architecture-maintainer
capability: architecture
phase: planning
cadence: per-stage
last_reviewed: 2026-05-07
---

> **EXAMPLE — not a real project.** This is a worked example based on the fictional "Helio" project. Use it as a reference for tone, depth, and cross-link patterns when filling [the corresponding template](../03_architecture/01_solution_design_TEMPLATE.md). Anonymise / replace all values when adapting.

# Helio — Real-Time Invoicing API: Solution Design

## 1. Introduction and goals

Helio replaces a nightly batch invoice-export job with a real-time invoicing API. The goal is to reduce invoice availability time from up to 24 hours to ≤5 minutes p95, measured from billable-event creation to invoice retrievable via the API. Secondary goals are auditability (immutable event trail per invoice), AI-assisted categorisation (confidence-gated suggestion with human override), and EU data residency.

This document is the primary architecture baseline for the Helio initiative. It covers the `invoice-api` service, the `category-suggester` component, the invoice data store, and the event-publishing integration. The product requirements that drive these goals are in [../02_product/01_prd_helio.md](../02_product/01_prd_helio.md) — see also the worked example: [filled_prd_example.md](filled_prd_example.md).

Key architecture success measures:
- Invoice creation endpoint: ≤500 ms p95 at 200 req/s sustained load.
- Category classifier adds ≤150 ms to the p95 creation path.
- Circuit breaker for classifier: open after 5 consecutive failures; invoice creation still succeeds with `category_suggestion: null`.
- 99.9 % monthly uptime for creation and retrieval endpoints.

## 2. Constraints

| Constraint | Detail |
| --- | --- |
| Identity provider | The organisation runs a single OAuth2 IdP (Keycloak, company-managed). All Helio API calls must use JWT bearer tokens issued by this IdP. No new auth layer. |
| Event bus | An internal Kafka cluster (eu-west-1 primary) is the approved async integration surface. Helio must publish to it; it cannot introduce a second broker. |
| EU data residency | Invoice data — including line-item text sent to the AI classifier — must not leave the EU. Approved deployment regions: eu-west-1 (primary) and eu-central-1 (DR). Non-EU LLM API endpoints are prohibited. |
| API latency budget | ≤500 ms p95 for invoice creation (NFR-001). The category classifier is called on the critical path and is allocated a 150 ms sub-budget. |
| IaC platform | Terraform via the company's shared platform team module registry. No manual cloud-console provisioning. |
| No greenfield data warehouse | The existing batch job's BI feed is not in scope. Helio adds a new Postgres instance for invoice storage; it does not replace the data warehouse. |

## 3. Context and scope

**In scope:** the `invoice-api` service (REST API, event publisher), the `category-suggester` component (LLM client + rule-based fallback), the `invoice-store` Postgres database, and the Kafka topic `invoices.events`. The admin UI is a thin React layer maintained by the payments front-end team and is out of scope except for the API contract it consumes.

**Out of scope:** payment processing, refund flows, B2C invoice delivery, BI batch feed, and multi-currency support (deferred per PRD).

**Neighboring actors:**
- Billing system (upstream): emits billable events consumed by `invoice-api`.
- B2B customer systems (downstream): poll the REST API and subscribe to webhook deliveries.
- LLM provider (Claude Haiku Helio-1, EU-hosted endpoint): provides category classification.
- OAuth2 IdP (Keycloak): issues tokens for all API callers.
- Kafka cluster: receives `invoices.events` topic publications from `invoice-api`.

A C4 context diagram is maintained at [02_c4_context_helio.md](../03_architecture/02_c4_context_helio.md).

## 4. Solution strategy

Helio is built as a single-purpose microservice (`invoice-api`) rather than an extension of the existing monolith billing system. This boundary keeps the new API independently deployable, lets the team iterate on invoice logic without touching billing, and respects the existing service-ownership structure.

The category classifier is deployed as a co-located sidecar process (`category-suggester`) rather than a separate networked service. Co-location keeps the 150 ms latency budget achievable without a network hop, while still allowing the component to be swapped independently. The sidecar calls the LLM provider over HTTPS; the circuit breaker is local to the sidecar.

Async integration is Kafka-first: invoice state changes are published as domain events. Webhook delivery to B2B customers is handled by a separate outbound-webhook worker that consumes from Kafka and manages per-customer retry state. This decouples reliability of the API path from reliability of customer notification.

Decisions that shaped this strategy are captured in ADR-0001 (invoice-api service isolation) and ADR-0002 (LLM provider selection), linked in section 9.

## 5. Building block view

| Component | Responsibility | Technology |
| --- | --- | --- |
| `invoice-api` | REST API: create, read, update, cancel invoices; publish domain events; enforce idempotency; write audit log | Python / FastAPI, deployed on ECS Fargate |
| `category-suggester` | Call LLM provider to classify invoice line-item text; return top category + confidence; fall back to rule-based classifier when LLM unavailable | Python sidecar, co-located with `invoice-api` |
| `invoice-store` | Persistent store for invoice records, audit log, and idempotency keys | Postgres 15, AWS RDS, eu-west-1 primary with eu-central-1 read replica |
| `event-publisher` | Publish `invoice.created` and `invoice.status_changed` events to Kafka | In-process library within `invoice-api`; uses transactional outbox pattern |
| `webhook-worker` | Consume Kafka events and deliver HTTP webhooks to B2B subscribers with retry | Python / Celery, separate ECS task, shares `invoice-store` for delivery-state tracking |

Container-level responsibilities are detailed in [03_c4_container_helio.md](../03_architecture/03_c4_container_helio.md).

## 6. Runtime view

### Happy path: create invoice

1. B2B caller sends `POST /v1/invoices` with a JWT bearer token and an `Idempotency-Key` header.
2. `invoice-api` validates the token against the Keycloak JWKS endpoint (cached, 5-minute TTL).
3. `invoice-api` checks the `invoice-store` for the idempotency key; if found, returns the cached response.
4. `invoice-api` calls `category-suggester` synchronously with the invoice line-item text (≤150 ms budget). The suggester calls Claude Haiku Helio-1; if the circuit breaker is open, the rule-based fallback runs instead.
5. `invoice-api` writes the invoice record and an `INVOICE_CREATED` audit log entry to `invoice-store` within a single Postgres transaction. The outbox table is updated in the same transaction.
6. The transactional outbox relay publishes the `invoice.created` event to Kafka (asynchronous, ≤2 s).
7. `invoice-api` returns HTTP 201 with the invoice ID, status, and category suggestion (or `null` if the classifier was unavailable).

### Failure path: classifier unavailable

At step 4, if the circuit breaker is open or if the LLM returns an error, `category-suggester` returns `{category: null, confidence: null, source: "unavailable"}`. Invoice creation continues normally (steps 5–7). The UI shows the billing administrator that no suggestion is available and prompts manual category entry. The circuit breaker half-opens after 30 s and attempts a single probe call.

### Failure path: Postgres write failure

If the Postgres write at step 5 fails, `invoice-api` returns HTTP 503 with a `Retry-After: 5` header. No event is published and no audit entry is written (atomic rollback). The caller may retry with the same `Idempotency-Key`.

## 7. Deployment view

**Regions:** eu-west-1 (primary, active), eu-central-1 (DR, warm standby). RDS Multi-AZ in eu-west-1; cross-region read replica in eu-central-1 promoted in DR scenarios. Kafka is the company-shared cluster in eu-west-1 only (no cross-region replication required for this use case).

**Release shape:** blue-green deployment on ECS Fargate managed by the platform team's Terraform module (`platform//ecs-service`). Each deployment creates a new task set; traffic shifts 10 % → 50 % → 100 % over 20 minutes with automatic rollback if p99 error rate exceeds 1 % during the shift.

**IaC reference:** Terraform root module at `infrastructure/helio/` in the main platform repository. Managed by Anna Mirov (SRE). All secrets (DB credentials, LLM API key) are stored in AWS Secrets Manager; no secrets in environment variables or IaC state.

**Scaling:** ECS service auto-scales on CPU (target 60 %) and ALB request count. Minimum 2 tasks per region; maximum 20 tasks. `webhook-worker` scales on Kafka consumer-group lag.

Detailed environment topology is in [07_deployment_helio.md](../03_architecture/07_deployment_helio.md).

## 8. Cross-cutting concepts

**Authentication and authorisation.** All API calls require a valid JWT from the Keycloak IdP. Scopes: `invoices:read`, `invoices:write`, `invoices:admin`. The admin console uses the `invoices:admin` scope; B2B API callers use `invoices:read` and `invoices:write`. The `invoice-api` validates tokens locally using the cached JWKS.

**Idempotency.** Invoice creation accepts an `Idempotency-Key` header (UUID v4, caller-generated). The key is stored in the `invoice-store` with the response body for 24 hours. Replays within the window return the cached response without side effects.

**Observability.** All services emit OpenTelemetry traces (OTLP to company Grafana Tempo), structured logs (JSON, shipped to Loki), and Prometheus metrics scraped by the platform observability stack. Mandatory trace attributes: `invoice.id`, `idempotency.key`, `classifier.source` (`llm` | `fallback` | `unavailable`), `classifier.confidence`.

**Audit logging.** Every invoice state change writes an immutable row to the `invoice_audit` table in `invoice-store`. Rows include: `event_type`, `actor_id`, `timestamp` (UTC), `field_delta` (JSON), `classifier_suggestion`, `classifier_accepted` (boolean). Audit rows are append-only; no UPDATE or DELETE is permitted on this table (enforced by Postgres row-level security).

**Retry and resilience.** The `webhook-worker` retries failed deliveries 3 times with exponential back-off (5 s, 25 s, 125 s). After 3 failures the delivery is marked `failed` and an alert fires to the billing-ops PagerDuty channel. The LLM circuit breaker uses a 5-failure threshold, 30 s recovery window.

## 9. Architectural decisions

| ADR | Decision | Status |
| --- | --- | --- |
| [ADR-0001](../adr/ADR-0001-invoice-api-isolation.md) | Deploy `invoice-api` as an independent microservice rather than extending the billing monolith | Accepted |
| [ADR-0002](../adr/ADR-0002-llm-provider-choice.md) | Use Claude Haiku Helio-1 (EU-hosted endpoint) as the primary category classifier with a rule-based fallback | Accepted |

See [../adr/INDEX.md](../adr/INDEX.md) for the full ADR index.

## 10. Quality requirements

**Latency.** Invoice creation ≤500 ms p95 at 200 req/s sustained; classifier sub-budget ≤150 ms p95. Retrieval (`GET /v1/invoices/{id}`) ≤100 ms p95 (served from Postgres read path with a 60 s cache for immutable invoices).

**Durability.** Invoice records must survive a single-AZ failure without data loss. RDS Multi-AZ provides synchronous replication. The transactional outbox ensures events are published exactly once even if the relay restarts mid-flight.

**Auditability.** Every invoice create, update, and status change must be traceable to an actor and timestamp. The audit table is the source of truth; logs are supplementary. Audit rows are retained for 7 years in S3-backed cold storage after 90 days in Postgres.

**AI output traceability.** Every invoice must record whether the category was set by the classifier or by a human, and — if by the classifier — the confidence score and model version (`classifier.model_version` attribute on the audit row). This supports the monthly drift review required by the AI use policy (see [filled_ai_use_policy_example.md](filled_ai_use_policy_example.md)).

**Security.** Trust boundary between `invoice-api` and the LLM provider is HTTPS-only. Line-item text is the only data sent; no customer PII (name, address, contact) is included in classifier requests. This constraint is enforced by the `category-suggester` request builder, which strips all fields except `line_items[].description` and `line_items[].amount`.

## 11. Risks and technical debt

| Risk | Likelihood | Impact | Mitigation |
| --- | --- | --- | --- |
| LLM inference cost variance | Medium | Medium | Monthly cost ceiling alert at €500; circuit breaker to fallback cuts cost immediately. Model usage reviewed monthly by Priya Saxena. |
| EU data egress if LLM provider changes endpoint routing | Low | High | Contractual guarantee from provider; automated egress monitoring via VPC flow logs; re-evaluate at each provider contract renewal. |
| Cross-region latency for DR failover | Low | Medium | eu-central-1 warm standby has ≤60 s RTO for read traffic; write failover is manual (Anna Mirov runbook). Acceptable given 99.9 % target. |
| Transactional outbox relay lag | Medium | Low | Relay runs on a 1 s poll; acceptable for the ≤5 min end-to-end target. If Kafka is unavailable, events queue in the outbox table; alert fires at 5-minute lag. |
| Classifier drift reducing category accuracy | Medium | Medium | Monthly precision/recall review on labelled samples (Diego Reyes + Priya Saxena). Rollback path: disable classifier, default to manual entry. See AI use policy. |

**Technical debt:** The idempotency key store is currently in the same Postgres instance as invoice records. At scale this may become a contention point. A separate Redis-backed idempotency cache is logged as a follow-on architectural improvement (target: Q4 2026, after load profile is known).

## 12. Glossary

| Term | Definition |
| --- | --- |
| Billable event | A record emitted by the upstream billing system that triggers invoice generation in Helio. |
| Category suggestion | The top invoice-line category label returned by the `category-suggester`, accompanied by a confidence score (0.0–1.0). |
| Circuit breaker | A resilience pattern that stops calling a failing downstream service after a threshold of consecutive failures and periodically probes for recovery. |
| Idempotency key | A caller-supplied UUID that allows the API to return the same response for a repeated request without creating duplicate records. |
| Outbox pattern | A technique for reliably publishing events by writing them to a database table (the outbox) in the same transaction as the business record, then relaying them to the event bus asynchronously. |
| Transactional outbox relay | The in-process background loop that reads unpublished rows from the outbox table and publishes them to Kafka. |

## Related documents

- [filled_prd_example.md](filled_prd_example.md) — worked example PRD for Helio; defines the functional and non-functional requirements this design addresses.
- [filled_ai_use_policy_example.md](filled_ai_use_policy_example.md) — worked example AI use policy for the invoice-category classifier.
- [../03_architecture/01_solution_design_TEMPLATE.md](../03_architecture/01_solution_design_TEMPLATE.md) — the template this example was derived from.
- [../02_product/01_prd_helio.md](../02_product/01_prd_helio.md) — the project's actual PRD (not an example).
- [../adr/ADR-0001-invoice-api-isolation.md](../adr/ADR-0001-invoice-api-isolation.md) — decision: service isolation approach.
- [../adr/ADR-0002-llm-provider-choice.md](../adr/ADR-0002-llm-provider-choice.md) — decision: LLM provider and fallback strategy.
- [02_c4_context_helio.md](../03_architecture/02_c4_context_helio.md) — system context diagram.
- [03_c4_container_helio.md](../03_architecture/03_c4_container_helio.md) — container-level diagram.
