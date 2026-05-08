---
title: Dependency Log Template
status: draft
record_class: canonical
audience: [internal, manager]
owner: delivery-manager
capability: execution
phase: monitoring
cadence: weekly
last_reviewed: 2026-05-07
source_of_truth: repo
---

# Dependency Log Template

> **Purpose**: maintain the live list of dependencies that affect delivery sequencing, confidence, or readiness.
> **Audience**: delivery leads and managers coordinating cross-team or external dependencies.
> **When to update**: update whenever dependency status changes and review at least weekly.

## How to use this template

Use this template when the project needs more detailed dependency tracking than a plan summary can provide. Keep one row per dependency and make blocking status obvious so teams can act early.

- Mandatory: ID, type, owner, target date, status, and blocking flag.
- Optional: impact note or recovery action if it helps decision-making.
- Remove closed dependencies only after they are no longer relevant to reporting or audit trail.

## What not to include

- **Risk and issue entries** — risks and issues belong in the RAID register ([../00_governance/06_raid_register_TEMPLATE.md](../00_governance/06_raid_register_TEMPLATE.md)). A dependency may become a risk, but track them separately.
- **Formal change requests** — change decisions belong in the change control log ([../00_governance/07_change_control_log_TEMPLATE.md](../00_governance/07_change_control_log_TEMPLATE.md)). Flag when a blocked dependency triggers a change request, then track the request there.
- **Implementation or milestone scheduling** — milestone and workstream planning belongs in the delivery plan ([01_delivery_plan_TEMPLATE.md](01_delivery_plan_TEMPLATE.md)). This log tracks dependencies, not the plan.
- **Technical architecture decisions** — design decisions belong in ADRs ([../adr/ADR-000-template.md](../adr/ADR-000-template.md)). An ADR may resolve a technical dependency; link it from the row.
- **Full status report narrative** — weekly delivery progress belongs in the status report ([04_status_report_TEMPLATE.md](04_status_report_TEMPLATE.md)), which references this log.

## Frontmatter quick reference

This template's typical frontmatter values. The full schema (with all enums and conditional rules) is at [docs/00_operating_model/04_frontmatter_schema.md](../00_operating_model/04_frontmatter_schema.md).

| Field | Typical value here | Notes |
|---|---|---|
| `status` | `draft` → `active` → `superseded` | Use `superseded` when replaced; required `superseded_by:` link |
| `record_class` | `canonical` | This template defines a canonical artifact |
| `audience` | `[internal, manager]` | Add `client` only when client-export-safe |
| `capability` | `execution` | Fixed for this folder |
| `phase` | `monitoring` | One of `initiation`, `planning`, `execution`, `monitoring`, `closure`, `n/a` |
| `cadence` | `weekly` | One of `ad-hoc`, `weekly`, `monthly`, `per-stage`, `per-release`, `one-shot` |

> When `capability: execution`, both `cadence` and `source_of_truth` are required by the validator. Add `source_of_truth: repo` (or the appropriate value) to the frontmatter.

## Per-dependency register

Capture dependencies in a simple table so the team can see ownership and timing at a glance. Separate incoming and outgoing dependencies clearly to avoid disputes about who must act next.

- Example types: in, out, external, internal.
- Example statuses: on track, at risk, blocked, complete.

| Dependency ID | Type | Owner | Target date | Status | Blocking? |
| --- | --- | --- | --- | --- | --- |
| DEP-001 | [In/Out] | [Role or team] | [YYYY-MM-DD] | [Status] | [Yes/No] |

## Related documents

- [01_delivery_plan_TEMPLATE.md](01_delivery_plan_TEMPLATE.md) — summarizes the critical-path dependencies at baseline level.
- [03_stage_plan_TEMPLATE.md](03_stage_plan_TEMPLATE.md) — uses dependency status to manage stage exceptions.
- [../00_governance/06_raid_register_TEMPLATE.md](../00_governance/06_raid_register_TEMPLATE.md) — provides the governance-facing view of dependency risk and escalation.
