---
title: Verification Evidence Index Template
status: draft
record_class: canonical
audience: [internal, manager]
owner: qa-lead
capability: quality
phase: monitoring
cadence: per-release
last_reviewed: 2026-05-07
source_of_truth: repo
---

# Verification Evidence Index Template

> **Purpose**: provide a traceable index of verification evidence from requirements through test execution and recorded results.
> **Audience**: QA leads, auditors, product owners, and managers who need proof that acceptance conditions were verified.
> **When to update**: update whenever new evidence is produced, reruns change outcomes, or release-readiness evidence is refreshed.

## How to use this template

Use this template as the canonical evidence register, not as a place to store raw artifacts. Link outward to run reports, screenshots, dashboards, or signed records and keep the matrix current.

- Mandatory: requirement reference, test reference, run ID, result, and evidence link.
- Optional: reviewer or approval columns if needed for regulated sign-off.
- Remove stale evidence links when superseded by newer approved results.

## What not to include

- **Raw test artefacts or log files** — link to stored artefacts (CI reports, screenshots, exports) rather than pasting them here. This index is the map, not the storage location.
- **Requirement text or acceptance scenario steps** — these belong in the requirements catalog ([../02_product/02_requirements_catalog_TEMPLATE.md](../02_product/02_requirements_catalog_TEMPLATE.md)) and acceptance catalog ([../02_product/05_acceptance_catalog_TEMPLATE.md](../02_product/05_acceptance_catalog_TEMPLATE.md)). Use IDs to link.
- **Defect records or bug narrative** — defects belong in the defect management process ([05_defect_management_TEMPLATE.md](05_defect_management_TEMPLATE.md)). Reference failed evidence rows; do not reproduce defect content.
- **Test strategy or approach narrative** — approach belongs in the test strategy ([01_test_strategy_TEMPLATE.md](01_test_strategy_TEMPLATE.md)). This index records outcomes, not approach.
- **Sign-off approvals or governance decisions** — formal sign-off belongs in the stage gate checklist ([../00_governance/09_stage_gate_checklist_TEMPLATE.md](../00_governance/09_stage_gate_checklist_TEMPLATE.md)) and UAT plan ([04_uat_plan_TEMPLATE.md](04_uat_plan_TEMPLATE.md)).

## Frontmatter quick reference

This template's typical frontmatter values. The full schema (with all enums and conditional rules) is at [docs/00_operating_model/04_frontmatter_schema.md](../00_operating_model/04_frontmatter_schema.md).

| Field | Typical value here | Notes |
|---|---|---|
| `status` | `draft` → `active` → `superseded` | Use `superseded` when replaced; required `superseded_by:` link |
| `record_class` | `canonical` | This template defines a canonical artifact |
| `audience` | `[internal, manager]` | Add `client` only when client-export-safe |
| `capability` | `quality` | Fixed for this folder |
| `phase` | `monitoring` | One of `initiation`, `planning`, `execution`, `monitoring`, `closure`, `n/a` |
| `cadence` | `per-release` | One of `ad-hoc`, `weekly`, `monthly`, `per-stage`, `per-release`, `one-shot` |

> When `capability: execution`, both `cadence` and `source_of_truth` are required by the validator.

## Per-requirement: design ref, test ref, run ID, result, evidence link

Record each requirement or acceptance item with the minimum metadata needed to trace from design intent to executed proof. Keep identifiers stable so release and audit reviews can follow them quickly.

- Example: AC-014 links to design section SD-3.2, test case API-22, run CI-1842, result pass, evidence artifact URL.
- Example: NFR-005 links to load-test report PERF-9 and dashboard snapshot for the approved run.

## Matrix view

Present the evidence in a compact matrix so reviewers can spot gaps, failures, duplicates, or outdated runs. If the full matrix lives in a tool export, summarize the canonical columns here and link to the source.

- Example: columns for requirement ID, scenario, latest run, status, evidence, and reviewer.
- Example: highlight blocked or failed rows that still affect release or UAT sign-off.

## Related documents

- [01_test_strategy_TEMPLATE.md](01_test_strategy_TEMPLATE.md) — defines the verification approach this evidence index must reflect.
- [../02_product/05_acceptance_catalog_TEMPLATE.md](../02_product/05_acceptance_catalog_TEMPLATE.md) — supplies the acceptance items that should appear in the evidence matrix.
- [04_uat_plan_TEMPLATE.md](04_uat_plan_TEMPLATE.md) — uses the indexed evidence to support business sign-off.
- [../07_delivery/06_readiness_tracker_TEMPLATE.md](../07_delivery/06_readiness_tracker_TEMPLATE.md) — consumes completion signals from this index for go or no-go decisions.
