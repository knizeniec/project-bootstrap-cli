---
title: Stage Plan Template
status: draft
record_class: canonical
audience: [internal, manager]
owner: delivery-manager
capability: execution
phase: execution
cadence: per-stage
last_reviewed: 2026-05-07
source_of_truth: repo
---

# Stage Plan Template

> **Purpose**: define the objective, tolerances, activities, and gate criteria for a single delivery stage.
> **Audience**: delivery leads and managers planning or reviewing a defined stage of work.
> **When to update**: update before each stage starts and when approved stage assumptions or tolerances change.

## How to use this template

Use this template to turn the overall delivery plan into a stage-specific control artifact. Keep it focused on one stage only so the objective, tolerances, and exception triggers remain easy to review.

- Mandatory: stage objective, tolerances, activities, gate criteria, and exception triggers.
- Optional: stage-specific dependency or resource notes if they materially affect control.
- Remove completed draft assumptions once the stage baseline is approved.

## What not to include

- **Content from other stages** — keep one stage plan per stage. Cross-stage dependencies should be referenced in the delivery plan ([01_delivery_plan_TEMPLATE.md](01_delivery_plan_TEMPLATE.md)), not duplicated here.
- **Full RAID register entries** — individual risks and issues belong in the RAID register ([../00_governance/06_raid_register_TEMPLATE.md](../00_governance/06_raid_register_TEMPLATE.md)). Summarise the stage-level exceptions here.
- **Architecture decisions or technical rationale** — design belongs in ADRs ([../adr/ADR-000-template.md](../adr/ADR-000-template.md)). Reference relevant decisions; do not explain them here.
- **Test case or acceptance scenario detail** — these belong in the acceptance catalog ([../02_product/05_acceptance_catalog_TEMPLATE.md](../02_product/05_acceptance_catalog_TEMPLATE.md)). Gate criteria reference the evidence; they do not reproduce the scenarios.
- **Status report content** — weekly delivery status belongs in the status report ([04_status_report_TEMPLATE.md](04_status_report_TEMPLATE.md)), which reports against this plan.

## Frontmatter quick reference

This template's typical frontmatter values. The full schema (with all enums and conditional rules) is at [docs/00_operating_model/04_frontmatter_schema.md](../00_operating_model/04_frontmatter_schema.md).

| Field | Typical value here | Notes |
|---|---|---|
| `status` | `draft` → `active` → `superseded` | Use `superseded` when replaced; required `superseded_by:` link |
| `record_class` | `canonical` | This template defines a canonical artifact |
| `audience` | `[internal, manager]` | Add `client` only when client-export-safe |
| `capability` | `execution` | Fixed for this folder |
| `phase` | `execution` | One of `initiation`, `planning`, `execution`, `monitoring`, `closure`, `n/a` |
| `cadence` | `per-stage` | One of `ad-hoc`, `weekly`, `monthly`, `per-stage`, `per-release`, `one-shot` |

> When `capability: execution`, both `cadence` and `source_of_truth` are required by the validator. Add `source_of_truth: repo` (or the appropriate value) to the frontmatter.

## Stage objectives

Describe what the stage is meant to achieve and how it advances the overall plan. Objectives should be outcome-based so the gate review can assess completion clearly.

- Example: complete integration build and secure design approval for pilot.
- Example: finish release rehearsal and confirm operational readiness evidence.

## Tolerances

State the boundaries for schedule, scope, cost, or quality within the stage. Tolerances should align with the project initiation baseline and tell the team when escalation is required.

- Example: no more than one-week slippage without sponsor escalation.
- Example: no unresolved critical defects at stage exit.

## Activities

List the major stage activities and the expected outputs from each. This should be enough to guide oversight without turning the document into a sprint backlog.

- Example: environment preparation, build completion, rehearsal, stakeholder review.
- Example: updated RTM, readiness evidence, and gate pack creation.

## Gate criteria

Define the conditions that must be met for the stage to be accepted as complete. Keep criteria evidence-based so they support a crisp gate decision.

- Example: approved design decisions, passing test evidence, green readiness domains.
- Example: all mandatory artifacts linked and reviewed before the gate meeting.

## Exception triggers

List the conditions that would force escalation, re-planning, or a stage exception process. This prevents teams from drifting beyond agreed tolerance without governance visibility.

- Example: dependency delay affecting the critical path.
- Example: budget burn exceeding threshold or quality red status.

## Related documents

- [01_delivery_plan_TEMPLATE.md](01_delivery_plan_TEMPLATE.md) — defines the overall baseline that the stage plan decomposes.
- [04_status_report_TEMPLATE.md](04_status_report_TEMPLATE.md) — reports progress and exceptions against this stage.
- [../00_governance/09_stage_gate_checklist_TEMPLATE.md](../00_governance/09_stage_gate_checklist_TEMPLATE.md) — provides the gate evidence model used to approve stage exit.
