---
title: Delivery Templates
status: draft
record_class: canonical
audience: [internal, manager]
owner: delivery-manager
capability: execution
phase: n/a
cadence: monthly
last_reviewed: 2026-05-07
source_of_truth: repo
---

# Delivery Templates

> **Purpose**: orient readers to the execution-control templates used to plan, track, and release work.
> **Audience**: internal teams and managers responsible for sequencing, reporting, and readiness.
> **When to update**: update when delivery templates, sequencing guidance, or reporting expectations change.

## How to use this template

Use this landing page to choose the delivery artifact that matches the level of execution control you need. Start with the delivery plan and implementation plan, then add live reporting, readiness, and release artifacts as the work approaches execution.

- Use the reading order if you are establishing a delivery baseline from scratch.
- Keep sprint or ticket detail in execution tools and summarize only the canonical control view here.
- Link to governance, product, and operations docs instead of duplicating them.

## Folder purpose

This folder holds the canonical execution templates for planning, tracking, readiness, release, and rollback. Use it when approved scope needs to become a managed delivery baseline with evidence and reporting.

- Typical use: define workstreams, milestones, and dependencies before build begins.
- Typical use: track readiness, status, and release controls as delivery progresses.

## Index of templates

The templates move from baseline planning through live control to release execution. Use only the artifacts that match the delivery complexity, but keep the cross-links so governance and operations can trace decisions.

- Baselines: [01_delivery_plan_TEMPLATE.md](01_delivery_plan_TEMPLATE.md), [02_implementation_plan_TEMPLATE.md](02_implementation_plan_TEMPLATE.md), [03_stage_plan_TEMPLATE.md](03_stage_plan_TEMPLATE.md)
- Live control: [04_status_report_TEMPLATE.md](04_status_report_TEMPLATE.md), [05_dependency_log_TEMPLATE.md](05_dependency_log_TEMPLATE.md), [06_readiness_tracker_TEMPLATE.md](06_readiness_tracker_TEMPLATE.md)
- Release: [07_release_plan_TEMPLATE.md](07_release_plan_TEMPLATE.md), [08_cutover_and_rollback_TEMPLATE.md](08_cutover_and_rollback_TEMPLATE.md)
- Migration: [09_migration_plan_TEMPLATE.md](09_migration_plan_TEMPLATE.md) — data and service migration scope, steps, and validation; use only when existing data or services are being moved or transformed.

## Reading order

Read the delivery plan first to understand the overall execution baseline, then the implementation plan for approach and sequencing detail. As execution progresses, move to status, dependencies, readiness, and finally the release and cutover controls.

1. [01_delivery_plan_TEMPLATE.md](01_delivery_plan_TEMPLATE.md)
2. [02_implementation_plan_TEMPLATE.md](02_implementation_plan_TEMPLATE.md)
3. [03_stage_plan_TEMPLATE.md](03_stage_plan_TEMPLATE.md)
4. [04_status_report_TEMPLATE.md](04_status_report_TEMPLATE.md), [05_dependency_log_TEMPLATE.md](05_dependency_log_TEMPLATE.md), and [06_readiness_tracker_TEMPLATE.md](06_readiness_tracker_TEMPLATE.md)
5. [07_release_plan_TEMPLATE.md](07_release_plan_TEMPLATE.md) and [08_cutover_and_rollback_TEMPLATE.md](08_cutover_and_rollback_TEMPLATE.md)
6. [09_migration_plan_TEMPLATE.md](09_migration_plan_TEMPLATE.md) — when data or live services are in scope for migration

## Which template should I use?

Pick the smallest set of templates that matches your project's risk and scope. Add more only when justified by complexity, regulation, or stakeholder need.

| Delivery style | Recommended templates | Skip |
|---|---|---|
| **Single-stage** — one delivery team, one release, no formal stage gates, minimal external reporting; ≤3 months elapsed time | [01_delivery_plan_TEMPLATE.md](01_delivery_plan_TEMPLATE.md) — workstreams, milestones, and dependencies; [02_implementation_plan_TEMPLATE.md](02_implementation_plan_TEMPLATE.md) — approach and sequencing detail | [03_stage_plan_TEMPLATE.md](03_stage_plan_TEMPLATE.md), [04_status_report_TEMPLATE.md](04_status_report_TEMPLATE.md), [05_dependency_log_TEMPLATE.md](05_dependency_log_TEMPLATE.md), [06_readiness_tracker_TEMPLATE.md](06_readiness_tracker_TEMPLATE.md), [07_release_plan_TEMPLATE.md](07_release_plan_TEMPLATE.md), [08_cutover_and_rollback_TEMPLATE.md](08_cutover_and_rollback_TEMPLATE.md), [09_migration_plan_TEMPLATE.md](09_migration_plan_TEMPLATE.md) |
| **Multi-stage** — multiple teams or workstreams, phased delivery, stakeholder status reporting required, dependencies span team boundaries | All single-stage templates plus [03_stage_plan_TEMPLATE.md](03_stage_plan_TEMPLATE.md) — per-stage scope, entry, and exit criteria; [04_status_report_TEMPLATE.md](04_status_report_TEMPLATE.md) — regular reporting cadence; [05_dependency_log_TEMPLATE.md](05_dependency_log_TEMPLATE.md); [06_readiness_tracker_TEMPLATE.md](06_readiness_tracker_TEMPLATE.md); [07_release_plan_TEMPLATE.md](07_release_plan_TEMPLATE.md) — when release is a distinct, managed event | [08_cutover_and_rollback_TEMPLATE.md](08_cutover_and_rollback_TEMPLATE.md) — add only when cutover risk requires a tested rollback plan; [09_migration_plan_TEMPLATE.md](09_migration_plan_TEMPLATE.md) — add only when data or service migration is in scope |
| **Regulated** — formal stage-gate approval required; external audit or contractual evidence obligations; migration, cutover, or rollback risk requires tested procedures | Full set: all multi-stage templates plus [08_cutover_and_rollback_TEMPLATE.md](08_cutover_and_rollback_TEMPLATE.md) — explicit cutover steps and rollback trigger; [09_migration_plan_TEMPLATE.md](09_migration_plan_TEMPLATE.md) — data and service migration scope, steps, and validation | Nothing — all documents apply |

**Rules of thumb:**
- Add the stage plan ([03_stage_plan_TEMPLATE.md](03_stage_plan_TEMPLATE.md)) when a delivery phase has a distinct set of entry criteria, exit criteria, or approval owners — not when the phases are a calendar convenience.
- Add the readiness tracker ([06_readiness_tracker_TEMPLATE.md](06_readiness_tracker_TEMPLATE.md)) as soon as more than one team must confirm readiness before a release; without it, gaps surface too late.
- Add the migration plan ([09_migration_plan_TEMPLATE.md](09_migration_plan_TEMPLATE.md)) only when existing data or live services are being moved or transformed — keep it out of greenfield deliveries.

## Related documents

- [../00_governance/README.md](../00_governance/README.md) — governance records define the approvals and control rules delivery must satisfy.
- [../01_strategy/README.md](../01_strategy/README.md) — strategy records explain why the work is sequenced as it is.
- [../06_security_operations/README.md](../06_security_operations/README.md) — operations records own service acceptance and post-release readiness obligations.
