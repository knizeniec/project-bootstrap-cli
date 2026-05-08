---
title: Governance Templates
status: draft
record_class: canonical
audience: [internal, manager, client]
owner: project-management-office
capability: governance
phase: n/a
cadence: monthly
last_reviewed: 2026-05-07
---

# Governance Templates

> **Purpose**: orient readers to the governance templates used to frame, control, and assure a project.
> **Audience**: internal teams, managers, and client stakeholders who need the governance baseline and approval trail.
> **When to update**: update when governance templates, sequencing, or ownership guidance changes.

## How to use this template

Use this landing page to choose the right governance artifact before creating a project-specific document. Start with the brief and business case, then move into controls, registers, and traceability as the initiative matures.

- Copy the relevant `*_TEMPLATE.md` file into an active project document when needed.
- Use the reading order below if you are setting up a project from scratch.
- Link to this page from project onboarding notes instead of duplicating the template list.

## Folder purpose

This folder holds the canonical templates for project framing, approvals, oversight, and control records. Use it when you need a formal trail from why the work exists through how changes, risks, and benefits are governed.

- Typical use: start a new initiative with a brief, business case, and PID.
- Typical use: maintain live control records such as RAID, change log, and stage gates.

## Index of templates

The templates are arranged from early framing to ongoing control and assurance. Pick the smallest artifact that gives the project a clear decision or control point, then add the rest as complexity increases.

- Framing: [00_project_brief_TEMPLATE.md](00_project_brief_TEMPLATE.md), [01_business_case_TEMPLATE.md](01_business_case_TEMPLATE.md), [02_project_initiation_document_TEMPLATE.md](02_project_initiation_document_TEMPLATE.md)
- Oversight: [03_board_terms_of_reference_TEMPLATE.md](03_board_terms_of_reference_TEMPLATE.md), [04_stakeholder_register_TEMPLATE.md](04_stakeholder_register_TEMPLATE.md), [05_communications_plan_TEMPLATE.md](05_communications_plan_TEMPLATE.md)
- Control and assurance: [06_raid_register_TEMPLATE.md](06_raid_register_TEMPLATE.md), [07_change_control_log_TEMPLATE.md](07_change_control_log_TEMPLATE.md), [08_benefits_realization_TEMPLATE.md](08_benefits_realization_TEMPLATE.md), [09_stage_gate_checklist_TEMPLATE.md](09_stage_gate_checklist_TEMPLATE.md), [12_requirements_traceability_matrix_TEMPLATE.md](12_requirements_traceability_matrix_TEMPLATE.md)

## Reading order

The recommended reading order follows a normal governance lifecycle: define the work, confirm value, establish control, then maintain live registers. Readers can skip ahead to a specific register if they already have an approved brief and initiation baseline.

1. [00_project_brief_TEMPLATE.md](00_project_brief_TEMPLATE.md)
2. [01_business_case_TEMPLATE.md](01_business_case_TEMPLATE.md)
3. [02_project_initiation_document_TEMPLATE.md](02_project_initiation_document_TEMPLATE.md)
4. [03_board_terms_of_reference_TEMPLATE.md](03_board_terms_of_reference_TEMPLATE.md) and [04_stakeholder_register_TEMPLATE.md](04_stakeholder_register_TEMPLATE.md)
5. Operational control set: RAID, change log, benefits realization, stage gate checklist, and RTM

## Which template should I use?

Pick the smallest set of templates that matches your project's risk and scope. Add more only when justified by complexity, regulation, or stakeholder need.

| Project size | Recommended templates | Skip |
|---|---|---|
| **Small** — ≤10 requirements, single team, no external compliance, internal or low-visibility deliverable | [00_project_brief_TEMPLATE.md](00_project_brief_TEMPLATE.md) — frame why the work exists and what success looks like; [06_raid_register_TEMPLATE.md](06_raid_register_TEMPLATE.md) — track risks and issues | [01_business_case_TEMPLATE.md](01_business_case_TEMPLATE.md), [02_project_initiation_document_TEMPLATE.md](02_project_initiation_document_TEMPLATE.md), [03_board_terms_of_reference_TEMPLATE.md](03_board_terms_of_reference_TEMPLATE.md), [04_stakeholder_register_TEMPLATE.md](04_stakeholder_register_TEMPLATE.md), [05_communications_plan_TEMPLATE.md](05_communications_plan_TEMPLATE.md), [07_change_control_log_TEMPLATE.md](07_change_control_log_TEMPLATE.md), [08_benefits_realization_TEMPLATE.md](08_benefits_realization_TEMPLATE.md), [09_stage_gate_checklist_TEMPLATE.md](09_stage_gate_checklist_TEMPLATE.md), [12_requirements_traceability_matrix_TEMPLATE.md](12_requirements_traceability_matrix_TEMPLATE.md) |
| **Standard** — multi-team, customer-facing, or cross-organisation; 11–100 requirements; moderate stakeholder reporting needed | [00_project_brief_TEMPLATE.md](00_project_brief_TEMPLATE.md); [01_business_case_TEMPLATE.md](01_business_case_TEMPLATE.md) — justify investment; [02_project_initiation_document_TEMPLATE.md](02_project_initiation_document_TEMPLATE.md) — baseline scope and controls; [04_stakeholder_register_TEMPLATE.md](04_stakeholder_register_TEMPLATE.md); [06_raid_register_TEMPLATE.md](06_raid_register_TEMPLATE.md); [07_change_control_log_TEMPLATE.md](07_change_control_log_TEMPLATE.md) | [03_board_terms_of_reference_TEMPLATE.md](03_board_terms_of_reference_TEMPLATE.md), [05_communications_plan_TEMPLATE.md](05_communications_plan_TEMPLATE.md), [08_benefits_realization_TEMPLATE.md](08_benefits_realization_TEMPLATE.md), [09_stage_gate_checklist_TEMPLATE.md](09_stage_gate_checklist_TEMPLATE.md), [12_requirements_traceability_matrix_TEMPLATE.md](12_requirements_traceability_matrix_TEMPLATE.md) |
| **Regulated** — external audit or compliance obligation, board-level approval required, >100 requirements or formal stage-gate delivery | Full set: all standard templates plus [03_board_terms_of_reference_TEMPLATE.md](03_board_terms_of_reference_TEMPLATE.md) — formalise board authority; [05_communications_plan_TEMPLATE.md](05_communications_plan_TEMPLATE.md); [08_benefits_realization_TEMPLATE.md](08_benefits_realization_TEMPLATE.md); [09_stage_gate_checklist_TEMPLATE.md](09_stage_gate_checklist_TEMPLATE.md) — exit criteria per stage; [12_requirements_traceability_matrix_TEMPLATE.md](12_requirements_traceability_matrix_TEMPLATE.md) — traceability from requirement to acceptance | Nothing — all documents apply |

**Rules of thumb:**
- Promote from small to standard as soon as a second team or external stakeholder needs visibility into scope or approvals.
- Add the RTM ([12_requirements_traceability_matrix_TEMPLATE.md](12_requirements_traceability_matrix_TEMPLATE.md)) only when an auditor or client must verify that every requirement has a test and a sign-off.
- Keep the RAID register ([06_raid_register_TEMPLATE.md](06_raid_register_TEMPLATE.md)) in every profile — risk visibility is the minimum governance control regardless of project size.

## Related documents

- [../01_strategy/README.md](../01_strategy/README.md) — strategy artifacts explain why the initiative matters and how it fits the roadmap.
- [../07_delivery/README.md](../07_delivery/README.md) — delivery artifacts turn governance decisions into execution control.
- [../02_product/README.md](../02_product/README.md) — product artifacts provide the scoped requirements that governance records trace and approve.
