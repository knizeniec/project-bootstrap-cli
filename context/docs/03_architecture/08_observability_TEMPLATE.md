---
title: Observability Template
status: draft
record_class: canonical
audience: [internal]
owner: architecture-maintainer
capability: architecture
phase: execution
cadence: monthly
last_reviewed: 2026-05-07
---

# Observability Template

> **Purpose**: define the logging, metrics, traces, alerts, and dashboards needed to operate the solution.
> **Audience**: engineers, SRE or operations teams, and reviewers validating operational readiness.
> **When to update**: update when telemetry coverage, SLOs, retention, or alerting strategy changes.

## How to use this template

- Capture only the telemetry design that needs shared agreement.
- Link implementation-specific dashboards or queries rather than pasting large exports.
- Keep signals mapped to important user journeys and failure modes.

## What not to include

- **Dashboard query exports or raw metric definitions** — link to dashboards rather than pasting long query strings. Vendor-specific configuration belongs in version-controlled infrastructure code.
- **Incident response procedures** — what to do when an alert fires belongs in runbooks ([../06_security_operations/11_runbook_TEMPLATE.md](../06_security_operations/11_runbook_TEMPLATE.md)) and the incident response template ([../06_security_operations/06_incident_response_TEMPLATE.md](../06_security_operations/06_incident_response_TEMPLATE.md)).
- **Resilience design or failure modes** — failure modeling belongs in the resilience template ([09_resilience_TEMPLATE.md](09_resilience_TEMPLATE.md)). Observability describes what to measure; resilience describes what to expect when things fail.
- **Architecture decision rationale for telemetry choices** — binding decisions belong in ADRs ([../adr/ADR-000-template.md](../adr/ADR-000-template.md)).
- **Test plans for monitoring coverage** — verification of alert correctness belongs in the test strategy and acceptance catalog.

## Frontmatter quick reference

This template's typical frontmatter values. The full schema (with all enums and conditional rules) is at [docs/00_operating_model/04_frontmatter_schema.md](../00_operating_model/04_frontmatter_schema.md).

| Field | Typical value here | Notes |
|---|---|---|
| `status` | `draft` → `active` → `superseded` | Use `superseded` when replaced; required `superseded_by:` link |
| `record_class` | `canonical` | This template defines a canonical artifact |
| `audience` | `[internal]` | Add `manager` only when needed for reviews |
| `capability` | `architecture` | Fixed for this folder |
| `phase` | `execution` | One of `initiation`, `planning`, `execution`, `monitoring`, `closure`, `n/a` |
| `cadence` | `monthly` | One of `ad-hoc`, `weekly`, `monthly`, `per-stage`, `per-release`, `one-shot` |

> When `capability: execution`, both `cadence` and `source_of_truth` are required by the validator.

## Logs

Define log structure, severity levels, redaction rules, and retention expectations.

- Example: JSON logs with request ID, actor ID, and outcome code.

## Metrics

List the golden signals, business metrics, and service-level indicators that matter.

- Include target SLOs where they exist.

## Traces

Describe trace boundaries, span naming expectations, and cross-service correlation rules.

- Note where end-to-end tracing is mandatory.

## Alerts

Explain the conditions that should page, warn, or create tickets.

- Keep alert ownership and escalation paths explicit.

## Dashboards

List the minimum dashboard views needed for release, incident response, and routine operation.

- Example: service health, dependency health, business throughput.

## Related documents

- [07_deployment_TEMPLATE.md](07_deployment_TEMPLATE.md) — runtime topology and environment placement.
- [09_resilience_TEMPLATE.md](09_resilience_TEMPLATE.md) — failure modes that drive monitoring priorities.
- [../06_security_operations/10_runbook_index_TEMPLATE.md](../06_security_operations/10_runbook_index_TEMPLATE.md) — operational procedures used when alerts fire.
