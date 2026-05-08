---
title: Acceptance Catalog Template
status: draft
record_class: canonical
audience: [internal, manager]
owner: product-management
capability: product
phase: planning
cadence: per-release
last_reviewed: 2026-05-07
---

# Acceptance Catalog Template

> **Purpose:** provide the canonical catalog for feature-level acceptance scenarios, UAT pass criteria, and named sign-off roles.
> **Audience:** product, QA, delivery, and approving stakeholders who need a shared definition of what “acceptable” means before release.
> **When to update:** update when scope changes, acceptance scenarios change, UAT expectations change, or sign-off roles change.

## How to use this template

Use this file when the PRD needs detailed acceptance coverage that should stay stable and reviewable. Keep each scenario linked to a feature, story, or requirement so testing and evidence can prove the right thing.

- Write scenarios in Given/When/Then form for clarity and testability.
- Keep UAT pass criteria release-focused rather than tool-specific.
- Remove placeholder examples before publishing a project-specific catalog.

## What not to include

- **Test execution records or run results** — evidence of what happened during testing belongs in the verification evidence index ([../05_testing_acceptance/03_verification_evidence_index_TEMPLATE.md](../05_testing_acceptance/03_verification_evidence_index_TEMPLATE.md)). This catalog defines scenarios; the evidence index records outcomes.
- **Defect records or bug reports** — defects belong in the defect management process ([../05_testing_acceptance/05_defect_management_TEMPLATE.md](../05_testing_acceptance/05_defect_management_TEMPLATE.md)).
- **Technical test tooling configuration** — test automation scripts and tool settings belong in the test framework, not in canonical acceptance documentation.
- **Non-functional requirements or load test results** — performance evidence belongs in the verification evidence index, linked from the test strategy ([../05_testing_acceptance/01_test_strategy_TEMPLATE.md](../05_testing_acceptance/01_test_strategy_TEMPLATE.md)).
- **Requirements narrative** — detailed requirement context belongs in the PRD ([01_prd_TEMPLATE.md](01_prd_TEMPLATE.md)) and requirements catalog ([02_requirements_catalog_TEMPLATE.md](02_requirements_catalog_TEMPLATE.md)). Reference IDs here; do not duplicate the requirement text.

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

## Given/When/Then acceptance scenarios

List the core acceptance scenarios for each feature or journey in a repeatable format. Prefer a small number of meaningful scenarios over a long list of trivial ones, and link them back to requirement IDs.

| Scenario ID | Feature or requirement | Given | When | Then |
| --- | --- | --- | --- | --- |
| AC-001 | [Feature / FR-001] | [Starting context] | [Action] | [Observable result] |
| AC-002 | [Feature / NFR-001] | [Starting context] | [Action] | [Observable result] |

- Example scenarios: happy path, permission failure, validation edge case, degraded dependency behavior.
- Example linkage: reference PRD user stories, requirement IDs, and UAT scenario IDs.

## UAT pass criteria

Define what must be true for user acceptance testing to be considered successful. Keep this section focused on release readiness rather than implementation detail.

- Example bullets: all critical scenarios pass, no unresolved blocking defects, agreed workarounds documented, evidence linked.
- Example prompts: what outcome would make business stakeholders comfortable signing off this release?

## Sign-off roles

Name the roles, not necessarily individuals, that must approve acceptance completion. This makes accountability explicit and prevents late-stage ambiguity.

- Example roles: product owner, QA lead, business sponsor, compliance reviewer.
- Example prompts: who owns business acceptance, who owns technical confidence, and who can block release?

## Related documents

- [01_prd_TEMPLATE.md](01_prd_TEMPLATE.md) — the PRD summarizes the acceptance conditions and product goals this catalog expands.
- [02_requirements_catalog_TEMPLATE.md](02_requirements_catalog_TEMPLATE.md) — acceptance scenarios should map back to stable requirement IDs.
- [../05_testing_acceptance/04_uat_plan_TEMPLATE.md](../05_testing_acceptance/04_uat_plan_TEMPLATE.md) — the UAT plan should reuse the scenarios and pass criteria from this catalog.
- [../05_testing_acceptance/03_verification_evidence_index_TEMPLATE.md](../05_testing_acceptance/03_verification_evidence_index_TEMPLATE.md) — use the evidence index to prove which scenarios passed and where the evidence lives.
