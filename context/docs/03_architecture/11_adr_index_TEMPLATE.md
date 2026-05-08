---
title: ADR Index Template
status: draft
record_class: canonical
audience: [internal]
owner: architecture-maintainer
capability: architecture
phase: planning
cadence: per-stage
last_reviewed: 2026-05-07
---

# ADR Index Template

> **Purpose**: provide the project-facing index of architecture decisions relevant to the current solution design.
> **Audience**: architects, engineers, and reviewers who need a quick decision map from design to ADR.
> **When to update**: update when ADRs are created, accepted, superseded, or newly linked to the solution design.

## How to use this template

- Use this file as the local architecture-side listing for ADRs that matter to a specific solution or program.
- Keep the canonical ADR records in `../adr/`.
- Include only enough metadata to help readers find the binding decision quickly.

## What not to include

- **Full ADR content** — the complete ADR lives in `../adr/`. This index provides a navigation layer; it does not duplicate the content.
- **Open proposals or RFCs under debate** — proposals in progress belong in the RFC index ([12_rfc_index_TEMPLATE.md](12_rfc_index_TEMPLATE.md)) and `rfcs/` directory. Only accepted or formally rejected ADRs belong here.
- **Architecture design rationale** — detailed reasoning belongs in the ADR itself and the solution design ([01_solution_design_TEMPLATE.md](01_solution_design_TEMPLATE.md)).
- **Delivery status or implementation tracking** — progress on implementing a decision belongs in the delivery plan and status reports.
- **Test or quality evidence** — evidence of decision compliance belongs in the verification evidence index ([../05_testing_acceptance/03_verification_evidence_index_TEMPLATE.md](../05_testing_acceptance/03_verification_evidence_index_TEMPLATE.md)).

## Frontmatter quick reference

This template's typical frontmatter values. The full schema (with all enums and conditional rules) is at [docs/00_operating_model/04_frontmatter_schema.md](../00_operating_model/04_frontmatter_schema.md).

| Field | Typical value here | Notes |
|---|---|---|
| `status` | `draft` → `active` → `superseded` | Use `superseded` when replaced; required `superseded_by:` link |
| `record_class` | `canonical` | This template defines a canonical artifact |
| `audience` | `[internal]` | Add `manager` only when needed for reviews |
| `capability` | `architecture` | Fixed for this folder |
| `phase` | `planning` | One of `initiation`, `planning`, `execution`, `monitoring`, `closure`, `n/a` |
| `cadence` | `per-stage` | One of `ad-hoc`, `weekly`, `monthly`, `per-stage`, `per-release`, `one-shot` |

> When `capability: execution`, both `cadence` and `source_of_truth` are required by the validator.

## ADR list

Use a simple table with one row per ADR.

| ADR | Title | Status | Date | Notes |
| --- | --- | --- | --- | --- |
| ADR-001 | Example decision | accepted | YYYY-MM-DD | Link from the solution design section it affects |

## Linkage rules

- Every entry should link to the canonical file in `../adr/`.
- Note whether the ADR shapes context, container, component, data, deployment, or operations design.

## Related documents

- [01_solution_design_TEMPLATE.md](01_solution_design_TEMPLATE.md) — references the ADR set from the architecture baseline.
- [02_c4_context_TEMPLATE.md](02_c4_context_TEMPLATE.md) — context-shaping decisions often appear here first.
- [03_c4_container_TEMPLATE.md](03_c4_container_TEMPLATE.md) — major technology and boundary decisions.
- [../adr/INDEX.md](../adr/INDEX.md) — canonical ADR source of truth.
