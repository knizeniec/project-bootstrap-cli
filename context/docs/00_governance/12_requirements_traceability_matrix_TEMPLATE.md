---
title: Requirements Traceability Matrix Template
status: draft
record_class: canonical
audience: [internal, manager, client]
owner: quality-manager
capability: governance
phase: monitoring
cadence: per-stage
last_reviewed: 2026-05-07
---

# Requirements Traceability Matrix Template

> **Purpose**: trace requirements from source through design, delivery, release, and verification.
> **Audience**: delivery leads, quality leads, managers, and client reviewers who need confidence that approved scope is implemented and tested.
> **When to update**: update when requirements, design references, test references, or release references change.

## How to use this template

Use this template when requirements need explicit traceability across the lifecycle. Keep one row per requirement or acceptance item, use stable IDs, and link to the most specific source available.

- Mandatory: requirement ID, source, design ref, test ref, release ref, and status.
- Optional: summary or owner columns if they help the project manage scale.
- Remove placeholder rows before the matrix is treated as live evidence.

## What not to include

- **Requirement text or detailed acceptance criteria** — full requirement statements belong in the requirements catalog ([../02_product/02_requirements_catalog_TEMPLATE.md](../02_product/02_requirements_catalog_TEMPLATE.md)) and acceptance catalog ([../02_product/05_acceptance_catalog_TEMPLATE.md](../02_product/05_acceptance_catalog_TEMPLATE.md)). Use IDs to link, not duplicate text.
- **Defect logs or bug lists** — defect tracking belongs in the defect management process ([../05_testing_acceptance/05_defect_management_TEMPLATE.md](../05_testing_acceptance/05_defect_management_TEMPLATE.md)).
- **Architecture design rationale** — design rationale belongs in ADRs and the solution design ([../03_architecture/01_solution_design_TEMPLATE.md](../03_architecture/01_solution_design_TEMPLATE.md)); reference the relevant section, do not copy it here.
- **Release approval records or sign-off decisions** — formal approvals belong in the stage gate checklist ([09_stage_gate_checklist_TEMPLATE.md](09_stage_gate_checklist_TEMPLATE.md)) and verification evidence index.
- **User story narrative** — user story detail belongs in the PRD ([../02_product/01_prd_TEMPLATE.md](../02_product/01_prd_TEMPLATE.md)); only the ID and status belong in the RTM.

## Frontmatter quick reference

This template's typical frontmatter values. The full schema (with all enums and conditional rules) is at [docs/00_operating_model/04_frontmatter_schema.md](../00_operating_model/04_frontmatter_schema.md).

| Field | Typical value here | Notes |
|---|---|---|
| `status` | `draft` → `active` → `superseded` | Use `superseded` when replaced; required `superseded_by:` link |
| `record_class` | `canonical` | This template defines a canonical artifact |
| `audience` | `[internal, manager, client]` | Add `client` only when client-export-safe |
| `capability` | `governance` | Fixed for this folder |
| `phase` | `monitoring` | One of `initiation`, `planning`, `execution`, `monitoring`, `closure`, `n/a` |
| `cadence` | `per-stage` | One of `ad-hoc`, `weekly`, `monthly`, `per-stage`, `per-release`, `one-shot` |

> When `capability: execution`, both `cadence` and `source_of_truth` are required by the validator.

## Traceability matrix

Use the matrix to prove that each approved requirement has a design home, a verification path, and a release destination. Keep status values simple so reviewers can spot gaps quickly during stage gates or audits.

- Example statuses: proposed, active, verified, deferred, retired.
- Example references: PRD section, architecture section, test case ID, release milestone.

| Requirement ID | Source | Design ref | Test ref | Release ref | Status |
| --- | --- | --- | --- | --- | --- |
| R-001 | [PRD section, brief, or contract reference] | [Design section or ADR] | [Test case, evidence ID, or UAT scenario] | [Release or milestone] | [Status] |
| R-002 | [Source reference] | [Design reference] | [Test reference] | [Release reference] | [Status] |

## Related documents

- [00_project_brief_TEMPLATE.md](00_project_brief_TEMPLATE.md) — provides the initial scope and success frame that requirements should support.
- [09_stage_gate_checklist_TEMPLATE.md](09_stage_gate_checklist_TEMPLATE.md) — uses traceability evidence during stage reviews.
- [../07_delivery/02_implementation_plan_TEMPLATE.md](../07_delivery/02_implementation_plan_TEMPLATE.md) — maps traced requirements into executable work and validation gates.
