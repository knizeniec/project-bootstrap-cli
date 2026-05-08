---
title: Operating Model Template
status: draft
record_class: canonical
audience: [internal, manager]
owner: service-owner
capability: operations
phase: execution
cadence: monthly
last_reviewed: 2026-05-07
source_of_truth: repo
---

# Operating Model Template

> **Purpose**: define how the service is owned, operated, escalated, changed, and funded in live use.
> **Audience**: service owners, operations teams, delivery leads, and managers responsible for live-service accountability.
> **When to update**: update when ownership, support coverage, change windows, capacity assumptions, or cost accountability change.

## How to use this template

Use this template to document the live-service operating model once a product moves beyond build-only delivery. Keep it centered on ongoing ownership, operational cadence, and decision rights.

- Mandatory: service catalog, on-call, escalation, change windows, capacity planning, and cost ownership.
- Optional: links to detailed schedules or runbooks maintained elsewhere.
- Remove delivery-only responsibilities once the service has a stable live owner.

## What not to include

- **Step-by-step operational runbook procedures** — specific procedures belong in runbooks ([11_runbook_TEMPLATE.md](11_runbook_TEMPLATE.md)). The operating model defines who does what; runbooks define how to do it.
- **Incident narrative or postmortem records** — incident records belong in the postmortem template ([12_incident_postmortem_TEMPLATE.md](12_incident_postmortem_TEMPLATE.md)). The operating model defines the response framework, not individual incident histories.
- **Support intake procedures or ticket content** — support tier rules and intake flows belong in the support model ([04_support_model_TEMPLATE.md](04_support_model_TEMPLATE.md)).
- **Security control posture** — control design belongs in the security baseline ([01_security_baseline_TEMPLATE.md](01_security_baseline_TEMPLATE.md)). The operating model covers live ownership and escalation, not the security baseline.
- **Delivery project management or delivery tracking** — delivery milestones belong in the delivery plan ([../07_delivery/01_delivery_plan_TEMPLATE.md](../07_delivery/01_delivery_plan_TEMPLATE.md)). The operating model covers live-service operations, not project delivery.

## Frontmatter quick reference

This template's typical frontmatter values. The full schema (with all enums and conditional rules) is at [docs/00_operating_model/04_frontmatter_schema.md](../00_operating_model/04_frontmatter_schema.md).

| Field | Typical value here | Notes |
|---|---|---|
| `status` | `draft` → `active` → `superseded` | Use `superseded` when replaced; required `superseded_by:` link |
| `record_class` | `canonical` | This template defines a canonical artifact |
| `audience` | `[internal, manager]` | Add `client` only when client-export-safe |
| `capability` | `operations` | Fixed for this folder |
| `phase` | `execution` | One of `initiation`, `planning`, `execution`, `monitoring`, `closure`, `n/a` |
| `cadence` | `monthly` | One of `ad-hoc`, `weekly`, `monthly`, `per-stage`, `per-release`, `one-shot` |

> When `capability: execution`, both `cadence` and `source_of_truth` are required by the validator.

## Service catalog

List the services, service components, or operated capabilities covered by this operating model. Make ownership and customer-facing purpose visible.

- Example: customer messaging API, moderation dashboard, notification worker, and reporting export job.
- Example: each catalog entry names the service owner and primary support route.

## On-call rota

Describe who provides operational coverage, when, and with what handoff expectations. Clarify whether the rota is formal, best-effort, or follows regional coverage.

- Example: primary and secondary weekly rota for platform and application support.
- Example: business-hours support with named after-hours escalation for high-severity incidents.

## Escalation

Explain how incidents, major risks, and unresolved operational issues move through support and management layers. Keep both technical and managerial escalation paths visible.

- Example: L1 to L2 to service owner to incident commander for service-impacting events.
- Example: unresolved dependency on external vendor escalates through commercial owner after 2 hours.

## Change windows

State when changes are normally allowed, who approves exceptions, and what blackout periods exist. This should help release planning avoid avoidable operational risk.

- Example: standard weekday evening release window with month-end blackout.
- Example: emergency changes allowed outside window with incident manager approval.

## Capacity planning

Describe how demand, scaling limits, and growth assumptions are tracked. Keep this section tied to operational decision-making, not low-level infrastructure tuning.

- Example: monthly review of traffic, queue depth, storage growth, and alert saturation.
- Example: scale threshold defined for adding worker capacity before peak season.

## Cost ownership

State who owns live-service cost decisions and what cost categories matter most. Link cost ownership to usage and capacity choices where possible.

- Example: service owner approves infrastructure growth and third-party SaaS spend.
- Example: engineering lead reviews cost anomalies after unusual traffic events.

## Related documents

- [04_support_model_TEMPLATE.md](04_support_model_TEMPLATE.md) — defines support tiers and intake flows that sit within this operating model.
- [10_runbook_index_TEMPLATE.md](10_runbook_index_TEMPLATE.md) — points to the operational procedures that implement this model day to day.
- [08_service_acceptance_TEMPLATE.md](08_service_acceptance_TEMPLATE.md) — checks that live ownership and operational readiness are complete before go-live.
- [../07_delivery/07_release_plan_TEMPLATE.md](../07_delivery/07_release_plan_TEMPLATE.md) — should align release sequencing to the operating constraints recorded here.
