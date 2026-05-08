---
title: "Helio — Real-Time Invoicing API: Product Requirements Document"
status: active
record_class: canonical
audience: [internal, manager]
owner: product-management
capability: product
phase: planning
cadence: per-release
last_reviewed: 2026-05-07
---

> **EXAMPLE — not a real project.** This is a worked example based on the fictional "Helio" project. Use it as a reference for tone, depth, and cross-link patterns when filling [the corresponding template](../02_product/01_prd_TEMPLATE.md). Anonymise / replace all values when adapting.

# Helio — Real-Time Invoicing API: Product Requirements Document

## Problem

B2B customers receive invoices once per day via a scheduled batch export that runs at 02:00 CET. Any invoice created after the previous day's cut-off waits up to 24 hours before the customer can reconcile, dispute, or pay it. For customers with automated accounts-payable pipelines, a delayed invoice is a blocked process — they cannot release a purchase order, update their ledger, or meet their own internal SLAs until the invoice arrives.

Two enterprise accounts escalated in Q4 2025: one paused renewal discussions citing invoice latency as an operational risk; one raised a formal support ticket after a month-end close was delayed by a missing batch file. Finance Ops has logged 14 customer complaints in the last two quarters, all attributable to the batch window. The payments team has a Q3 2026 customer commitment to deliver real-time invoice availability. Failure to meet that commitment affects at least three renewal contracts worth €2.4 M ARR.

## Users

**Primary users — B2B Finance teams.** Finance and accounts-payable staff at enterprise customers who integrate Helio invoices into their own ERP or AP systems via API. These users need invoices to be available immediately after a billable event occurs, with machine-readable status and a stable document ID they can track.

**Secondary users — Internal billing administrators.** The payments team's billing ops staff who create, correct, and query invoices on behalf of customers. They use an internal admin console backed by the same API. They need reliable audit trails, category-correction tools, and clear error feedback.

**Affected stakeholders — Finance Ops (Lena Rosen) and Engineering (Tom Vance).** Lena's team owns customer SLA commitments and will sign off on the release gate. Tom's team owns the platform and must approve deployment topology. The AI category suggestion feature (see FR-005) affects invoice processing accuracy and falls under the oversight scope of the AI governance review that Priya Saxena manages.

## Goals

- Reduce invoice availability time from ≤24 h to ≤5 min at p95, measured from billable-event creation to invoice available via GET `/v1/invoices/{id}`.
- Reduce invoice rejection rate (customer-initiated disputes citing wrong category or missing line item) from the current 3.1 % to below 0.5 %.
- Enable B2B customers to subscribe to invoice-ready events via webhook or event bus subscription, eliminating polling.
- Achieve zero data-residency violations: all invoice data processed and stored within the EU.
- Reach 99.9 % monthly uptime for the invoice creation and retrieval endpoints (excluding planned maintenance).

## Non-goals

- **Payment processing.** Helio creates and tracks invoices; it does not capture card payments, initiate bank transfers, or integrate with payment gateways. That capability is owned by a separate payments-core team.
- **Refund and credit-note flows.** Refund issuance and credit-note generation are deferred to a follow-on increment (target: Q1 2027).
- **B2C consumers.** The API and data model are designed for B2B billing only. Consumer-facing invoice download (PDF email) remains unchanged in the existing portal.
- **Legacy batch replacement for internal reporting.** The daily batch job produces a parallel feed for an internal BI reporting tool. Replacing that feed is out of scope; the batch job continues for reporting purposes until a separate data-platform initiative migrates it.
- **Multi-currency settlement.** Helio invoices are denominated in EUR only for the initial release. Currency conversion is a later capability.

## User stories

| ID | User story | Priority | Notes |
| --- | --- | --- | --- |
| US-001 | As a B2B finance system, I want to receive an invoice-ready webhook within 5 minutes of a billable event so that I can update my AP ledger without polling. | Must | Drives NFR-001. Requires webhook delivery reliability target. |
| US-002 | As a billing administrator, I want to view the AI-suggested invoice category before saving so that I can accept or override it with one action. | Must | Drives FR-005, FR-006. Human override must not require extra steps. |
| US-003 | As a billing administrator, I want to retrieve the full audit trail for any invoice — including who changed what, when, and whether the AI suggestion was accepted or overridden — so that I can respond to a customer dispute within minutes. | Must | Drives FR-007. Audit trail must be immutable. |
| US-004 | As a B2B finance team, I want to query all invoices by status (draft, issued, paid, disputed) so that I can reconcile open items at any time without waiting for an export file. | Should | Pagination and filtering required. |
| US-005 | As a billing administrator, I want the system to retry a failed invoice delivery automatically so that I do not need to manually re-trigger the notification. | Should | Drives FR-008. Max 3 retries with exponential back-off. |

## Functional requirements

| ID | Requirement | Priority | Acceptance reference |
| --- | --- | --- | --- |
| FR-001 | The system shall create an invoice record via `POST /v1/invoices` and return a stable, unique invoice ID within 500 ms p95. | Must | AC-001 |
| FR-002 | The system shall make a created invoice available via `GET /v1/invoices/{id}` immediately after creation, with no eventual-consistency delay visible to the caller. | Must | AC-002 |
| FR-003 | The system shall expose invoice status (`draft`, `issued`, `paid`, `disputed`, `cancelled`) and reflect status transitions within 30 s of the triggering event. | Must | AC-003 |
| FR-004 | The system shall emit an `invoice.created` and `invoice.status_changed` event to the event bus for each state transition, with the event payload matching the agreed schema in [../03_architecture/06_interface_control_document_helio.md](../03_architecture/06_interface_control_document_helio.md). | Must | AC-004 |
| FR-005 | The system shall call the invoice-category classifier during invoice creation and return the top suggested category with a confidence score (0.0–1.0) in the API response. | Must | AC-005 |
| FR-006 | The system shall allow a billing administrator to accept or override the AI-suggested category before the invoice is issued; override action must require no more than two UI interactions. | Must | AC-006 |
| FR-007 | The system shall record an immutable audit log entry for every invoice create, update, status change, and category accept/override event, capturing timestamp, actor ID, and field-level delta. | Must | AC-007 |
| FR-008 | The system shall retry a failed invoice-ready webhook delivery up to 3 times using exponential back-off, and mark the delivery as `failed` if all retries are exhausted. | Should | AC-008 |
| FR-009 | The system shall support idempotent invoice creation using a caller-supplied `Idempotency-Key` header, returning the original response if the same key is replayed within 24 h. | Should | AC-009 |
| FR-010 | The system shall allow an authorised caller to cancel an invoice in `draft` or `issued` status; cancellation shall be irreversible once confirmed. | Should | AC-010 |

## Non-functional requirements

| ID | Requirement | Measure or constraint | Acceptance reference |
| --- | --- | --- | --- |
| NFR-001 | API response latency — invoice creation | ≤500 ms p95 under sustained load of 200 req/s | AC-NFR-001 |
| NFR-002 | Invoice availability end-to-end | ≤5 min p95 from billable-event to invoice retrievable via API | AC-NFR-002 |
| NFR-003 | Monthly uptime (creation + retrieval endpoints) | ≥99.9 % (excluding planned maintenance windows ≤4 h/month) | AC-NFR-003 |
| NFR-004 | Data residency | All invoice data processed and stored in EU regions only (eu-west-1, eu-central-1); zero egress to non-EU services | AC-NFR-004 |
| NFR-005 | GDPR — data subject requests | Invoice records containing personal data must support erasure or pseudonymisation within 30 days of a verified request | AC-NFR-005 |
| NFR-006 | AI category classifier latency | Category suggestion must add ≤150 ms to the invoice creation p95 response time | AC-NFR-006 |
| NFR-007 | AI classifier availability | When the classifier is unavailable, invoice creation must succeed with `category_suggestion: null`; the circuit breaker must open within 5 consecutive failures | AC-NFR-007 |
| NFR-008 | Audit log retention | Audit entries retained for 7 years in immutable storage; readable within 10 s of query | AC-NFR-008 |

## Out of scope

- Payment capture, settlement, and reconciliation with banking partners.
- Credit notes, refunds, and partial payment tracking.
- B2C invoice distribution (PDF email delivery) — remains in the existing portal.
- Internal BI batch feed replacement.
- Multi-currency invoicing (EUR only in this release).
- Self-service customer onboarding or API key management — handled by the existing developer portal.
- Invoice dispute resolution workflow — disputes are logged but the resolution workflow is a Q4 2026 increment.

## Acceptance criteria

The release is acceptable when all of the following conditions are met:

**Given** a billable event is recorded in the billing system, **when** the invoice creation endpoint processes the event, **then** a GET request to `/v1/invoices/{id}` returns the invoice with HTTP 200 and the correct status within 5 minutes at p95, measured across a 2-hour load test at 200 req/s.

**Given** the AI category classifier returns a confidence score below 0.6, **when** the billing administrator views the draft invoice, **then** the UI displays the suggestion with a visible low-confidence indicator and the administrator can override the category in at most two actions before the invoice is issued.

**Given** a webhook delivery fails on first attempt, **when** the retry mechanism is triggered, **then** the system retries up to 3 times with exponential back-off and logs each attempt in the audit trail; after 3 failures the delivery status is set to `failed` and an alert is raised.

In addition: no unresolved Severity-1 defects at release gate; NFR-001 through NFR-008 measured thresholds met; sign-off received from Lena Rosen (Sponsor), Priya Saxena (Product Lead), and Diego Reyes (QA Lead).

## Release plan

- **Target release window:** end of Q3 2026 (week of 28 September 2026), subject to stage-gate sign-off at the end of the build phase (target: 31 July 2026).
- **Rollout stages:** internal dogfooding with the billing-ops team for two weeks; limited external beta with three opted-in enterprise accounts for four weeks; full GA with feature flag lift.
- **Key dependencies:** AI governance approval for the category classifier use case (see [../04_ai_governance/01_ai_use_policy_helio_classifier.md](../04_ai_governance/01_ai_use_policy_helio_classifier.md)); deployment topology sign-off from Anna Mirov (SRE); EU data-residency review by legal (due 15 June 2026).
- **Rollback assumption:** the new API is deployed alongside the existing batch job for the beta period; the batch job can be re-enabled as the primary feed within 30 minutes if a critical defect is discovered post-GA.
- **Detailed delivery mechanics:** see [../07_delivery/01_delivery_plan_helio.md](../07_delivery/01_delivery_plan_helio.md).

## Related documents

- [../03_architecture/01_solution_design_helio.md](../03_architecture/01_solution_design_helio.md) — architecture baseline for the Helio invoice API; shows how the functional and non-functional requirements are met technically. See also the worked example: [filled_solution_design_example.md](filled_solution_design_example.md).
- [../04_ai_governance/01_ai_use_policy_helio_classifier.md](../04_ai_governance/01_ai_use_policy_helio_classifier.md) — AI use policy for the invoice-category classifier; see also the worked example: [filled_ai_use_policy_example.md](filled_ai_use_policy_example.md).
- [../00_governance/00_project_brief_helio.md](../00_governance/00_project_brief_helio.md) — project brief that approved the Helio initiative.
- [../02_product/01_prd_TEMPLATE.md](../02_product/01_prd_TEMPLATE.md) — the template this example was derived from.
- [../07_delivery/01_delivery_plan_helio.md](../07_delivery/01_delivery_plan_helio.md) — delivery plan with milestones, stage gates, and rollout schedule.
- [../00_governance/12_requirements_traceability_matrix_helio.md](../00_governance/12_requirements_traceability_matrix_helio.md) — maps each FR/NFR to design, test, and release evidence.
