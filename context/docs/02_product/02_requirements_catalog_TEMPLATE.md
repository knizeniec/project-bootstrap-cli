---
title: Requirements Catalog Template
status: draft
record_class: canonical
audience: [internal, manager]
owner: product-management
capability: product
phase: planning
cadence: per-release
last_reviewed: 2026-05-07
---

# Requirements Catalog Template

> **Purpose:** provide a structured catalog for tracking each requirement statement, its source, owner, acceptance reference, and status.
> **Audience:** product, engineering, QA, and governance roles who need a sortable requirement inventory instead of narrative-only PRD text.
> **When to update:** update when requirements are added, reworded, reprioritized, accepted, deferred, or linked to new acceptance evidence.

## How to use this template

Use this catalog when the PRD needs a more detailed requirement inventory than a short narrative can carry. Keep one row per requirement and maintain stable IDs so traceability remains intact across testing, architecture, and release records.

- Split functional and non-functional items with the `type` column instead of separate files unless scale demands more.
- Keep requirement wording testable and link each row to an acceptance reference.
- Remove sample rows before publishing a real catalog.

## What not to include

- **User story narratives or problem context** — problem framing and user stories belong in the PRD ([01_prd_TEMPLATE.md](01_prd_TEMPLATE.md)). This catalog tracks the requirement inventory, not the reasoning behind it.
- **Detailed test scenarios or acceptance scripts** — test detail belongs in the acceptance catalog ([05_acceptance_catalog_TEMPLATE.md](05_acceptance_catalog_TEMPLATE.md)). Link acceptance references here; do not reproduce scenario steps.
- **Architectural design or implementation detail** — design belongs in the solution design ([../03_architecture/01_solution_design_TEMPLATE.md](../03_architecture/01_solution_design_TEMPLATE.md)). Requirements state behavior; the design states how to achieve it.
- **Defect records or test run results** — defect tracking belongs in the defect management process ([../05_testing_acceptance/05_defect_management_TEMPLATE.md](../05_testing_acceptance/05_defect_management_TEMPLATE.md)).
- **Delivery scheduling or milestone targets** — scheduling belongs in the delivery plan ([../07_delivery/01_delivery_plan_TEMPLATE.md](../07_delivery/01_delivery_plan_TEMPLATE.md)).

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

## Requirements catalog

Use the table below as the canonical inventory for requirement-level tracking. Each row should be specific enough for a reviewer to understand what is required, where it came from, who owns it, and how it will be accepted.

| ID | Statement | Type | Priority | Source | Owner | Acceptance reference | Status |
| --- | --- | --- | --- | --- | --- | --- | --- |
| FR-001 | [Requirement statement] | functional | Must | PRD / stakeholder / regulation | [Role] | AC-001 | proposed |
| NFR-001 | [Requirement statement] | NFR | Should | Risk / architecture / policy | [Role] | AC-NFR-001 | proposed |

- Example `source` values: PRD goal, stakeholder interview, compliance rule, incident learning.
- Example `status` values: proposed, accepted, deferred, superseded.

## Related documents

- [01_prd_TEMPLATE.md](01_prd_TEMPLATE.md) — the PRD provides the narrative problem, goals, and scope that explain why these requirements exist.
- [05_acceptance_catalog_TEMPLATE.md](05_acceptance_catalog_TEMPLATE.md) — each requirement should map to acceptance scenarios or pass criteria here.
- [../00_governance/12_requirements_traceability_matrix_TEMPLATE.md](../00_governance/12_requirements_traceability_matrix_TEMPLATE.md) — use the RTM when formal cross-artifact traceability is required.
- [../05_testing_acceptance/03_verification_evidence_index_TEMPLATE.md](../05_testing_acceptance/03_verification_evidence_index_TEMPLATE.md) — link accepted requirements to concrete verification evidence.
