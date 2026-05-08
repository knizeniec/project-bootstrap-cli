---
title: Stage Gate Checklist Template
status: draft
record_class: canonical
audience: [internal, manager, client]
owner: project-manager
capability: governance
phase: monitoring
cadence: per-stage
last_reviewed: 2026-05-07
---

# Stage Gate Checklist Template

> **Purpose**: define the evidence, approvals, and exit conditions required to move from one stage to the next.
> **Audience**: project managers, sponsors, managers, and client approvers responsible for gate decisions.
> **When to update**: update before each stage review or when stage evidence expectations change.

## How to use this template

Use this template at each planned gate to confirm readiness, evidence completeness, and required approvals. Keep the checklist objective so the gate decision depends on evidence rather than meeting sentiment.

- Mandatory: entry criteria, mandatory artifacts, sign-off roles, exit criteria, and evidence list.
- Optional: separate checklists per stage if the program is very large.
- Remove items that are not genuinely decision-relevant for the stage in question.

## What not to include

- **Detailed test cases or acceptance scenarios** — these belong in the acceptance catalog ([../02_product/05_acceptance_catalog_TEMPLATE.md](../02_product/05_acceptance_catalog_TEMPLATE.md)) and verification evidence index ([../05_testing_acceptance/03_verification_evidence_index_TEMPLATE.md](../05_testing_acceptance/03_verification_evidence_index_TEMPLATE.md)). The checklist references evidence; it does not reproduce it.
- **RAID register entries** — live risk and issue detail belongs in the RAID register ([06_raid_register_TEMPLATE.md](06_raid_register_TEMPLATE.md)). The checklist summarises RAID status for the gate review.
- **Architecture decisions or technical rationale** — design decisions belong in ADRs; the gate checklist confirms they exist and are accepted.
- **Status report narrative or progress history** — delivery progress belongs in the status report ([../07_delivery/04_status_report_TEMPLATE.md](../07_delivery/04_status_report_TEMPLATE.md)).
- **Future-stage planning** — this document covers one gate; next-stage planning belongs in the stage plan ([../07_delivery/03_stage_plan_TEMPLATE.md](../07_delivery/03_stage_plan_TEMPLATE.md)).

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

## Entry criteria

Define what must already be true before the gate review can start. This prevents time being spent on reviews that are not actually ready for a decision.

- Example: prior-stage actions closed and current baseline approved.
- Example: required reports, estimates, and risks circulated in advance.

## Mandatory artifacts

List the specific documents or records the reviewing body expects to see. Link to named artifacts so there is no ambiguity about the evidence set.

- Example: project brief, PID, delivery plan, RAID summary, status report.
- Example: architecture decision, test evidence, service acceptance input.

## Sign-off roles

Name the roles that must approve or acknowledge the stage decision. Keep sign-off role names aligned with the board terms and project tolerances.

- Example: sponsor, project manager, product lead, operations lead.
- Example: client representative or compliance approver for regulated scope.

## Exit criteria

State the conditions that must be met to pass the stage. Exit criteria should describe the outcome, not just the existence of documents.

- Example: prioritized scope approved and ready for build.
- Example: release readiness accepted with no unresolved red items.

## Evidence list

Provide a place to list links, file names, or references used in the gate pack. The checklist is more useful when reviewers can navigate directly to the supporting evidence.

- Example: links to RTM, readiness tracker, cutover plan, and benefits plan.
- Example: meeting minutes, approval comments, or exception records.

## Related documents

- [02_project_initiation_document_TEMPLATE.md](02_project_initiation_document_TEMPLATE.md) — defines the stage and tolerance model the checklist enforces.
- [06_raid_register_TEMPLATE.md](06_raid_register_TEMPLATE.md) — provides live control evidence for gate decisions.
- [../07_delivery/06_readiness_tracker_TEMPLATE.md](../07_delivery/06_readiness_tracker_TEMPLATE.md) — provides execution readiness evidence for late-stage gates.
