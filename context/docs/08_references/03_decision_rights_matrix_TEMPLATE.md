---
title: Decision Rights Matrix Template
status: draft
record_class: canonical
audience: [internal, manager]
owner: governance-coordinator
capability: references
phase: initiation
cadence: per-stage
last_reviewed: 2026-05-07
---

# Decision Rights Matrix Template

> **Purpose**: define who recommends, decides, contributes to, and is informed about each major class of project decision.
> **Audience**: internal teams and managers who need clear authority boundaries for governance, product, architecture, delivery, release, and operational changes.
> **When to update**: update when project roles, board structure, tolerances, or change authority rules change.

## How to use this template

Use this template to instantiate a project-specific RACI or DACI view from the operating-model baseline. Keep the rows focused on real decision classes, not generic activities, so approvals and escalations stay easy to interpret.

- Pick one notation per matrix: RACI for responsibility clarity or DACI when a single driver and decider model is needed.
- Reuse the same role names already used in the PID, board terms, and change-control log.
- Update this matrix before major governance or release reviews, not after confusion has already happened.

## What not to include

- **Individual change requests or approval records** — formal change decisions belong in the change control log ([../00_governance/07_change_control_log_TEMPLATE.md](../00_governance/07_change_control_log_TEMPLATE.md)). This matrix defines who has authority; that log records how it was exercised.
- **Board governance terms and quorum rules** — these belong in the board terms of reference ([../00_governance/03_board_terms_of_reference_TEMPLATE.md](../00_governance/03_board_terms_of_reference_TEMPLATE.md)). Reference the board ToR from a row here rather than re-stating its rules.
- **RACI for task-level activities** — keep rows at decision class level, not at the level of individual work tasks. Task assignments belong in delivery tooling or stage plans ([../07_delivery/03_stage_plan_TEMPLATE.md](../07_delivery/03_stage_plan_TEMPLATE.md)).
- **Glossary definitions for role names** — if a role name needs explanation, define it in the glossary ([02_glossary_TEMPLATE.md](02_glossary_TEMPLATE.md)) and reference it here.
- **Risk escalation procedures** — escalation paths for issues and risks belong in the RAID register ([../00_governance/06_raid_register_TEMPLATE.md](../00_governance/06_raid_register_TEMPLATE.md)). The matrix says who decides; the RAID register tracks what needs a decision.

## Frontmatter quick reference

This template's typical frontmatter values. The full schema (with all enums and conditional rules) is at [docs/00_operating_model/04_frontmatter_schema.md](../00_operating_model/04_frontmatter_schema.md).

| Field | Typical value here | Notes |
|---|---|---|
| `status` | `draft` → `active` → `superseded` | Use `superseded` when replaced; required `superseded_by:` link |
| `record_class` | `canonical` | This template defines a canonical artifact |
| `audience` | `[internal, manager]` | Add `client` only when client-export-safe |
| `capability` | `references` | Fixed for this folder |
| `phase` | `initiation` | One of `initiation`, `planning`, `execution`, `monitoring`, `closure`, `n/a` |
| `cadence` | `per-stage` | One of `ad-hoc`, `weekly`, `monthly`, `per-stage`, `per-release`, `one-shot` |

## Matrix setup

State the notation, the role set, and any decision thresholds before filling the table. This keeps readers from guessing whether a row is advisory, binding, or outside the project's tolerance model.

- Example notation: RACI for stage approval, release readiness, and change escalation.
- Example role set: sponsor, PM, product lead, engineering lead, QA lead, operations lead, compliance lead.

## Decision-class matrix

Use one row per decision class and one column per role.

| Decision class | Sponsor | PM | Product | Engineering | QA | Ops | Compliance | Notes |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Funding and business case changes | A | R | C | I | I | I | C | Use board approval path for changes above tolerance. |
| Scope and acceptance changes | C | R | A | C | C | I | I | Link to change request when scope shifts after baseline. |
| Architecture and technical design | I | C | C | A | C | C | I | Link to ADR or RFC when material. |
| Release readiness and cutover | I | C | C | R | A | A | C | Check service acceptance and rollback evidence. |

## Decision classes to consider

Tailor the rows to the project, but keep the core control surfaces visible.

- Governance: funding, stage progression, tolerance breaches, major change approval.
- Product: scope, acceptance intent, release content, prioritization exceptions.
- Engineering and quality: design decisions, technical debt acceptance, verification recommendation.
- Operations: service acceptance, incident escalation, rollback, support ownership.

## Change-control integration

Every materially approved change should be traceable from this matrix to the board or authority that can decide it. If a change falls outside the matrix, treat that as a governance gap and fix the matrix first.

- Link scope, schedule, cost, and quality changes to [../00_governance/07_change_control_log_TEMPLATE.md](../00_governance/07_change_control_log_TEMPLATE.md).
- Align escalation and quorum expectations to [../00_governance/03_board_terms_of_reference_TEMPLATE.md](../00_governance/03_board_terms_of_reference_TEMPLATE.md).
- Keep the operating-model baseline in sync with [../00_operating_model/08_decision_rights_matrix.md](../00_operating_model/08_decision_rights_matrix.md).

## Related documents

- [../00_operating_model/08_decision_rights_matrix.md](../00_operating_model/08_decision_rights_matrix.md) — operating-model baseline this project-specific matrix should instantiate.
- [../00_governance/02_project_initiation_document_TEMPLATE.md](../00_governance/02_project_initiation_document_TEMPLATE.md) — defines roles, tolerances, and project controls this matrix must match.
- [../00_governance/07_change_control_log_TEMPLATE.md](../00_governance/07_change_control_log_TEMPLATE.md) — records the controlled changes that rely on these decision rights.
- [../00_governance/03_board_terms_of_reference_TEMPLATE.md](../00_governance/03_board_terms_of_reference_TEMPLATE.md) — defines the forum that usually owns the highest-impact decision rows.
