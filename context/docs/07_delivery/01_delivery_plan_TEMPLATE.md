---
title: Delivery Plan Template
status: draft
record_class: canonical
audience: [internal, manager]
owner: delivery-manager
capability: execution
phase: planning
cadence: per-stage
last_reviewed: 2026-05-07
source_of_truth: repo
---

# Delivery Plan Template

> **Purpose**: define the execution baseline for workstreams, milestones, resources, dependencies, risks, and reporting.
> **Audience**: delivery leads and managers who need the canonical view of how approved scope will be delivered.
> **When to update**: update when milestone sequencing, major dependencies, staffing assumptions, or reporting expectations change.

## How to use this template

Copy this template when a project or major initiative needs a managed delivery baseline. Keep it aligned to the roadmap and governance artifacts, and summarize only the execution decisions that belong in canonical documentation.

- Mandatory: workstreams, milestones, resourcing, dependencies, risks, and reporting.
- Optional: links to detailed estimates or tooling views if they are authoritative elsewhere.
- Remove duplicate implementation detail once it is captured in the implementation plan.

## What not to include

- **Sprint-level tasks or backlog items** — day-to-day work belongs in the backlog and delivery tooling. This plan covers workstreams and milestones, not individual tickets.
- **Detailed implementation steps or cutover procedures** — implementation detail belongs in the implementation plan ([02_implementation_plan_TEMPLATE.md](02_implementation_plan_TEMPLATE.md)) and cutover runbook ([08_cutover_and_rollback_TEMPLATE.md](08_cutover_and_rollback_TEMPLATE.md)).
- **Product requirements or acceptance criteria** — product scope belongs in the PRD ([../02_product/01_prd_TEMPLATE.md](../02_product/01_prd_TEMPLATE.md)). This plan references the approved scope; it does not define it.
- **Full RAID register entries** — detailed RAID items belong in the RAID register ([../00_governance/06_raid_register_TEMPLATE.md](../00_governance/06_raid_register_TEMPLATE.md)). Summarise plan-level risks here; track individual items there.
- **Architecture decisions or technical design** — design belongs in ADRs and the solution design ([../03_architecture/01_solution_design_TEMPLATE.md](../03_architecture/01_solution_design_TEMPLATE.md)).

## Frontmatter quick reference

This template's typical frontmatter values. The full schema (with all enums and conditional rules) is at [docs/00_operating_model/04_frontmatter_schema.md](../00_operating_model/04_frontmatter_schema.md).

| Field | Typical value here | Notes |
|---|---|---|
| `status` | `draft` → `active` → `superseded` | Use `superseded` when replaced; required `superseded_by:` link |
| `record_class` | `canonical` | This template defines a canonical artifact |
| `audience` | `[internal, manager]` | Add `client` only when client-export-safe |
| `capability` | `execution` | Fixed for this folder |
| `phase` | `planning` | One of `initiation`, `planning`, `execution`, `monitoring`, `closure`, `n/a` |
| `cadence` | `per-stage` | One of `ad-hoc`, `weekly`, `monthly`, `per-stage`, `per-release`, `one-shot` |

> When `capability: execution`, both `cadence` and `source_of_truth` are required by the validator. Add `source_of_truth: repo` (or the appropriate value) to the frontmatter.

## Workstreams

List the major streams of work and the outcome each stream is expected to produce. Workstreams should be broad enough for leadership reporting but specific enough to anchor ownership and dependencies.

- Example: product configuration, integration build, migration, readiness, training.
- Example: each workstream row shows owner, outcome, and the main dependency edge.

## Milestones

Show the key dates or stage markers that define the delivery rhythm. Milestones should connect upward to roadmap horizons and downward to implementation sequencing.

- Example: design sign-off, build complete, pilot ready, go-live, closure review.
- Example: stage exit criteria or release decision points.

## Resourcing

Summarize the teams, roles, or capacity assumptions needed to deliver the plan. This section is for execution-critical resource shape, not for detailed staffing rosters.

- Example: dedicated delivery manager, shared architect, part-time operations lead.
- Example: external vendor support needed during migration and cutover.

## Dependencies

Capture the major internal and external dependencies that shape sequencing or confidence. Use this section to highlight the critical path rather than every small coordination item.

- Example: procurement approval, environment readiness, external API availability.
- Example: service acceptance evidence needed before release planning can lock.

## Risks

Summarize the delivery risks that materially affect the baseline and should be watched in reporting. Keep the detailed item management in the RAID register and use this section for the headline plan-level view.

- Example: scope compression risk, staffing bottleneck, late data remediation.
- Example: mitigation through phased rollout, contingency capacity, or earlier testing.

## Reporting

Define how progress against the plan will be reported and reviewed. The reporting model should line up with governance forums and the cadence of live control artifacts.

- Example: weekly status report to sponsor and monthly board review.
- Example: readiness review before each release and stage-end summary pack.

## Related documents

- [../01_strategy/02_roadmap_TEMPLATE.md](../01_strategy/02_roadmap_TEMPLATE.md) — provides the horizon and milestone context this plan executes.
- [../00_governance/01_business_case_TEMPLATE.md](../00_governance/01_business_case_TEMPLATE.md) — explains the value case and investment assumptions behind the plan.
- [02_implementation_plan_TEMPLATE.md](02_implementation_plan_TEMPLATE.md) — expands the baseline into sequencing, environments, and cutover detail.
