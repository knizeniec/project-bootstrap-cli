---
title: User Journeys Template
status: draft
record_class: canonical
audience: [internal, manager]
owner: product-management
capability: product
phase: planning
cadence: per-release
last_reviewed: 2026-05-07
---

# User Journeys Template

> **Purpose:** capture the key user journeys that explain how personas move through the product, where friction occurs, and what success looks like.
> **Audience:** product, design, engineering, and QA roles who need behavior-centered context for requirements and acceptance.
> **When to update:** update when personas, workflow steps, pain points, or success measures change.

## How to use this template

Use this template when a requirement list alone does not explain the real user flow. Keep journeys focused on high-value outcomes, and use them to expose edge cases, handoffs, and experience risks before design or implementation deepens.

- Create one subsection or table row per meaningful journey.
- Link journey IDs back to PRD stories, requirements, and acceptance scenarios.
- Remove low-value narrative detail that does not change product or test decisions.

## What not to include

- **Detailed UI wireframes or visual designs** — design artefacts live in the design system or linked design tools, not in this canonical document.
- **Requirement statements or acceptance criteria** — these belong in the requirements catalog ([02_requirements_catalog_TEMPLATE.md](02_requirements_catalog_TEMPLATE.md)) and acceptance catalog ([05_acceptance_catalog_TEMPLATE.md](05_acceptance_catalog_TEMPLATE.md)). User journeys reveal which requirements are needed; they do not replace them.
- **Test case scripts or step-by-step verification** — test scenarios belong in the acceptance catalog and UAT plan ([../05_testing_acceptance/04_uat_plan_TEMPLATE.md](../05_testing_acceptance/04_uat_plan_TEMPLATE.md)). Journey maps explain what happens; UAT plans describe how to verify it.
- **Technical implementation or API detail** — implementation belongs in the solution design ([../03_architecture/01_solution_design_TEMPLATE.md](../03_architecture/01_solution_design_TEMPLATE.md)).
- **Analytics dashboards or quantitative data dumps** — link to dashboards as evidence sources; do not embed raw data tables in journey maps.

## Frontmatter quick reference

This template's typical frontmatter values. The full schema (with all enums and conditional rules) is at [docs/00_operating_model/04_frontmatter_schema.md](../00_operating_model/04_frontmatter_schema.md).

| Field | Typical value here | Notes |
|---|---|---|
| `status` | `draft` → `active` → `superseded` | Use `superseded` when replaced; required `superseded_by:` link |
| `record_class` | `canonical` | This template defines a canonical artifact |
| `audience` | `[internal, manager]` | Add `client` only when client-export-safe |
| `capability` | `product` | Fixed for this folder |
| `phase` | `planning` | One of `initiation`, `planning`, `execution`, `monitoring`, `closure`, `n/a` |
| `cadence` | `per-release` | One of `ad-hoc`, `weekly`, `monthly`, `per-stage`, `per-release`, `one-shot` |

> When `capability: execution`, both `cadence` and `source_of_truth` are required by the validator.

## User journeys

Describe each journey in enough detail that a reader can understand the user goal, the main steps, and the points most likely to fail. A simple table is often enough if the flow is linear; use bullets under the table for exceptions or edge cases.

| Journey ID | Persona | Goal | Steps | Pain points | Opportunities | Success measure |
| --- | --- | --- | --- | --- | --- | --- |
| J-001 | [Persona] | [Outcome the user wants] | [Key steps] | [Current friction] | [Improvement idea] | [Observable measure] |
| J-002 | [Persona] | [Outcome] | [Key steps] | [Current friction] | [Improvement idea] | [Observable measure] |

- Example pain points: duplicate entry, unclear status, delayed response, inaccessible control.
- Example success measures: completion rate, task time, error rate, support contact reduction.

## Related documents

- [01_prd_TEMPLATE.md](01_prd_TEMPLATE.md) — the PRD explains which users and outcomes matter most.
- [02_requirements_catalog_TEMPLATE.md](02_requirements_catalog_TEMPLATE.md) — journeys often reveal missing requirements or clarify ambiguous ones.
- [05_acceptance_catalog_TEMPLATE.md](05_acceptance_catalog_TEMPLATE.md) — critical journey paths should become explicit acceptance scenarios.
- [../05_testing_acceptance/04_uat_plan_TEMPLATE.md](../05_testing_acceptance/04_uat_plan_TEMPLATE.md) — UAT scenarios should exercise the most important journeys end to end.
