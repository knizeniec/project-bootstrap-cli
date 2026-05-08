---
title: Roadmap Template
status: draft
record_class: canonical
audience: [internal, manager, client]
owner: product-manager
capability: strategy
phase: planning
cadence: monthly
last_reviewed: 2026-05-07
---

# Roadmap Template

> **Purpose**: show how strategy will be sequenced into themes, milestones, and dependencies over time.
> **Audience**: product leaders, managers, delivery leads, and client stakeholders planning investment and sequencing.
> **When to update**: update when themes, horizons, milestones, or major dependencies change.

## How to use this template

Use this template to communicate sequencing and intent rather than to create a task backlog. Keep the roadmap at the level of themes and milestones so it remains stable enough for governance and portfolio decisions.

- Mandatory: themes, Now/Next/Later horizon view, milestones, dependencies, and owner.
- Optional: confidence notes if the later horizon is especially uncertain.
- Remove sprint-level detail that belongs in delivery tooling or plans.

## What not to include

- **Sprint-level tasks or backlog items** — day-to-day work items belong in the backlog and delivery plan ([../07_delivery/01_delivery_plan_TEMPLATE.md](../07_delivery/01_delivery_plan_TEMPLATE.md)). The roadmap shows the horizon view, not the task list.
- **Detailed requirements or acceptance criteria** — product requirements belong in the PRD ([../02_product/01_prd_TEMPLATE.md](../02_product/01_prd_TEMPLATE.md)) and requirements catalog.
- **Investment justification or cost analysis** — financial rationale belongs in the business case ([../00_governance/01_business_case_TEMPLATE.md](../00_governance/01_business_case_TEMPLATE.md)).
- **Architecture or technical design** — design decisions belong in ADRs and the solution design ([../03_architecture/01_solution_design_TEMPLATE.md](../03_architecture/01_solution_design_TEMPLATE.md)).
- **RAID register detail** — operational risks and issues belong in the RAID register ([../00_governance/06_raid_register_TEMPLATE.md](../00_governance/06_raid_register_TEMPLATE.md)). The roadmap may flag horizon-level dependencies, not live RAID entries.

## Frontmatter quick reference

This template's typical frontmatter values. The full schema (with all enums and conditional rules) is at [docs/00_operating_model/04_frontmatter_schema.md](../00_operating_model/04_frontmatter_schema.md).

| Field | Typical value here | Notes |
|---|---|---|
| `status` | `draft` → `active` → `superseded` | Use `superseded` when replaced; required `superseded_by:` link |
| `record_class` | `canonical` | This template defines a canonical artifact |
| `audience` | `[internal, manager, client]` | Add `client` only when client-export-safe |
| `capability` | `strategy` | Fixed for this folder |
| `phase` | `planning` | One of `initiation`, `planning`, `execution`, `monitoring`, `closure`, `n/a` |
| `cadence` | `monthly` | One of `ad-hoc`, `weekly`, `monthly`, `per-stage`, `per-release`, `one-shot` |

> When `capability: execution`, both `cadence` and `source_of_truth` are required by the validator.

## Themes

Group work into a small number of strategic themes that explain why roadmap items belong together. Themes help readers understand trade-offs without getting lost in feature lists.

- Example: compliance foundation, self-service adoption, operations efficiency.
- Example: platform modernization, reporting transparency, service resilience.

## Horizon view (Now/Next/Later)

Show the intended sequence across near, medium, and later horizons. Use wording that signals confidence levels honestly, especially in the later horizon.

- Now example: approved releases already moving into delivery.
- Next example: prioritized themes needing final estimates or dependency confirmation.

## Milestones

List the major decision or delivery points that matter at roadmap level. Milestones should be understandable to leaders and clients without exposing internal team plans.

- Example: business case approval, pilot go-live, full rollout, benefits review.
- Example: data migration ready, service acceptance complete, closure review.

## Dependencies

Capture the cross-team, commercial, or regulatory factors that shape roadmap timing. This section should explain why some items cannot simply be pulled forward.

- Example: procurement lead times, platform upgrades, compliance approvals.
- Example: integration readiness or client policy decisions.

## Owner

State who owns the roadmap and who approves significant changes to it. Ownership keeps sequencing decisions anchored when priorities shift.

- Example: product manager owns maintenance; sponsor approves major re-sequencing.
- Example: portfolio board reviews roadmap changes monthly.

## Related documents

- [01_product_vision_TEMPLATE.md](01_product_vision_TEMPLATE.md) — provides the strategic direction the roadmap sequences.
- [../00_governance/01_business_case_TEMPLATE.md](../00_governance/01_business_case_TEMPLATE.md) — supports investment choices behind roadmap moves.
- [../07_delivery/01_delivery_plan_TEMPLATE.md](../07_delivery/01_delivery_plan_TEMPLATE.md) — converts roadmap milestones into executable delivery control.
