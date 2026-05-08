---
title: UAT Plan Template
status: draft
record_class: canonical
audience: [internal, manager]
owner: product-owner
capability: quality
phase: execution
cadence: per-release
last_reviewed: 2026-05-07
source_of_truth: repo
---

# UAT Plan Template

> **Purpose**: define how user acceptance testing will be organized, executed, and signed off.
> **Audience**: product owners, business testers, delivery leads, and managers responsible for acceptance decisions.
> **When to update**: update when participants, environments, scenarios, pass criteria, or sign-off expectations change.

## How to use this template

Use this template when a release or milestone needs structured business validation. Keep it focused on user-facing scenarios, participants, and acceptance decisions, with technical evidence linked rather than repeated.

- Mandatory: scope, participants, environment, scenarios, pass criteria, and sign-off roles.
- Optional: detailed scheduling if it is managed in a release calendar.
- Remove scenarios that no longer represent approved acceptance outcomes.

## What not to include

- **Technical test scripts or automated test detail** — automation scripts and tool configuration belong in the test framework. Reference automated test coverage here; do not reproduce script content.
- **Defect records or individual defect status** — defect tracking belongs in the defect management process ([05_defect_management_TEMPLATE.md](05_defect_management_TEMPLATE.md)). The UAT plan references pass criteria; actual defects are tracked separately.
- **Acceptance scenario steps in full** — detailed scenario steps belong in the acceptance catalog ([../02_product/05_acceptance_catalog_TEMPLATE.md](../02_product/05_acceptance_catalog_TEMPLATE.md)). Link to scenario IDs here; do not duplicate step content.
- **Requirements or product narrative** — requirements belong in the PRD ([../02_product/01_prd_TEMPLATE.md](../02_product/01_prd_TEMPLATE.md)) and requirements catalog. The UAT plan exercises the requirements; it does not restate them.
- **Release readiness decisions or go/no-go approvals** — release decisions belong in the release plan ([../07_delivery/07_release_plan_TEMPLATE.md](../07_delivery/07_release_plan_TEMPLATE.md)) and readiness tracker ([../07_delivery/06_readiness_tracker_TEMPLATE.md](../07_delivery/06_readiness_tracker_TEMPLATE.md)).

## Frontmatter quick reference

This template's typical frontmatter values. The full schema (with all enums and conditional rules) is at [docs/00_operating_model/04_frontmatter_schema.md](../00_operating_model/04_frontmatter_schema.md).

| Field | Typical value here | Notes |
|---|---|---|
| `status` | `draft` → `active` → `superseded` | Use `superseded` when replaced; required `superseded_by:` link |
| `record_class` | `canonical` | This template defines a canonical artifact |
| `audience` | `[internal, manager]` | Add `client` only when client-export-safe |
| `capability` | `quality` | Fixed for this folder |
| `phase` | `execution` | One of `initiation`, `planning`, `execution`, `monitoring`, `closure`, `n/a` |
| `cadence` | `per-release` | One of `ad-hoc`, `weekly`, `monthly`, `per-stage`, `per-release`, `one-shot` |

> When `capability: execution`, both `cadence` and `source_of_truth` are required by the validator.

## UAT scope

State the business workflows, user groups, or release increment covered by UAT. Make the scope narrow enough to be testable and broad enough to support a real acceptance decision.

- Example: new customer onboarding, approval workflow, and audit export journey for release 2.1.
- Example: excluded back-office maintenance tasks handled through engineering regression only.

## Participants

List the people or roles who will execute, observe, support, and approve UAT. Clarify whether they are testers, decision-makers, or subject-matter experts.

- Example: product owner, operations representative, support lead, and two pilot users.
- Example: QA analyst facilitates runs while business owner owns acceptance sign-off.

## Environment

Describe the UAT environment, data setup, access method, and any limitations participants need to know. Call out differences from production if they may affect acceptance confidence.

- Example: UAT tenant with masked production-like data and integrated notification sandbox.
- Example: test payment gateway and reduced batch volume compared with live operations.

## Scenarios (link to acceptance catalog)

List the business scenarios to be exercised and explicitly link them back to the acceptance catalog. Use stable IDs so UAT outcomes can map to formal product acceptance criteria.

- Example: UAT-01 maps to AC-014 for successful onboarding with approval and confirmation.
- Example: UAT-05 maps to AC-021 for exception handling and operator recovery.

## Pass criteria

Define what must happen for UAT to be considered successful and what defects or exceptions are tolerable. Keep the criteria specific enough to support go or no-go decisions.

- Example: all critical scenarios pass and no Sev-1 or Sev-2 defect remains open.
- Example: minor usability issues accepted with named follow-up actions and due dates.

## Sign-off roles

State who recommends acceptance, who approves it, and who records the final decision. Separate operational readiness advice from product or sponsor acceptance authority if they differ.

- Example: product owner recommends acceptance, sponsor approves, QA records the evidence set.
- Example: operations lead confirms environment and support readiness before final sign-off.

## Related documents

- [../02_product/05_acceptance_catalog_TEMPLATE.md](../02_product/05_acceptance_catalog_TEMPLATE.md) — defines the acceptance scenarios that UAT must validate.
- [03_verification_evidence_index_TEMPLATE.md](03_verification_evidence_index_TEMPLATE.md) — stores the evidence and results produced during UAT.
- [01_test_strategy_TEMPLATE.md](01_test_strategy_TEMPLATE.md) — shows how UAT fits within the wider test approach.
- [../07_delivery/07_release_plan_TEMPLATE.md](../07_delivery/07_release_plan_TEMPLATE.md) — uses UAT outcomes as part of release approval.
