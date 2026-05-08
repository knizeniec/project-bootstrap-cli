---
title: Project Brief Template
status: draft
record_class: canonical
audience: [internal, manager, client]
owner: project-manager
capability: governance
phase: initiation
cadence: one-shot
last_reviewed: 2026-05-07
---

# Project Brief Template

> **Purpose**: capture the minimum project framing needed to explain the initiative, its scope, and its approval path.
> **Audience**: sponsors, delivery leads, and client stakeholders who need a quick shared understanding of the work.
> **When to update**: update when the project is initiated or when approved scope, goals, or governance materially change.

## How to use this template

Copy this template when a new initiative needs a concise mandate before deeper planning documents exist. Keep it short, replace placeholders with approved facts, and link out to detailed artifacts rather than repeating them.

- Mandatory: summary, goals, scope, stakeholders, and governance fields.
- Optional: deeper risk detail if a separate RAID register already exists.
- Remove any example bullets and unused placeholders before publishing.

## What not to include

- **Detailed business case analysis or cost modelling** — this belongs in the business case ([01_business_case_TEMPLATE.md](01_business_case_TEMPLATE.md)). The brief states the problem and approval ask; the business case justifies the investment.
- **Full delivery milestones or stage plans** — milestone detail belongs in the delivery plan ([../07_delivery/01_delivery_plan_TEMPLATE.md](../07_delivery/01_delivery_plan_TEMPLATE.md)). The brief summarises the intent, not the execution schedule.
- **Technical architecture or solution design** — design decisions belong in the solution design ([../03_architecture/01_solution_design_TEMPLATE.md](../03_architecture/01_solution_design_TEMPLATE.md)). The brief explains what is being built, not how.
- **Detailed RAID entries** — risks and dependencies should be summarised here and tracked in the RAID register ([../00_governance/06_raid_register_TEMPLATE.md](06_raid_register_TEMPLATE.md)) once the project is active.
- **Stakeholder sentiment or engagement actions** — stakeholder analysis belongs in the stakeholder register ([04_stakeholder_register_TEMPLATE.md](04_stakeholder_register_TEMPLATE.md)).

## Frontmatter quick reference

This template's typical frontmatter values. The full schema (with all enums and conditional rules) is at [docs/00_operating_model/04_frontmatter_schema.md](../00_operating_model/04_frontmatter_schema.md).

| Field | Typical value here | Notes |
|---|---|---|
| `status` | `draft` → `active` → `superseded` | Use `superseded` when replaced; required `superseded_by:` link |
| `record_class` | `canonical` | This template defines a canonical artifact |
| `audience` | `[internal, manager, client]` | Add `client` only when client-export-safe |
| `capability` | `governance` | Fixed for this folder |
| `phase` | `initiation` | One of `initiation`, `planning`, `execution`, `monitoring`, `closure`, `n/a` |
| `cadence` | `one-shot` | One of `ad-hoc`, `weekly`, `monthly`, `per-stage`, `per-release`, `one-shot` |

> When `capability: execution`, both `cadence` and `source_of_truth` are required by the validator.

## Summary

Use this section to state what the project is, why it exists, and who is accountable for it. A reader should understand the initiative in under a minute without opening any other file.

- Example: project name, sponsor, delivery lead, and target outcome.
- Example: problem statement, target users, and planned start or decision date.

## Goals and success measures

Record the outcomes the project is expected to achieve and how success will be judged. Keep measures observable so the brief can be used later during stage gates and benefits reviews.

- Example: reduce onboarding time by 30% within two releases.
- Example: launch a compliant self-service workflow adopted by 80% of the target group.

## Scope (in/out)

Define what the project will deliver and what it explicitly will not address. Clear in-scope and out-of-scope lists help prevent the brief from becoming a vague aspiration document.

- In scope example: deliver a new customer intake portal and supporting admin workflow.
- Out of scope example: replace the legacy billing engine in this phase.

## Stakeholders

List the primary roles that fund, decide, build, operate, and are affected by the initiative. Keep the list role-based so it remains useful even if named individuals change.

- Example: sponsor, product owner, technical owner, delivery lead, operations lead.
- Example: client approver, compliance reviewer, service desk representative.

## Constraints and assumptions

Capture the conditions the project must work within and the assumptions that planning currently depends on. This helps later readers understand which boundaries were fixed versus estimated.

- Constraint example: fixed budget ceiling, mandatory release window, or regulatory deadline.
- Assumption example: dependent API will be available by the start of build.

## Risks and dependencies

Summarize the highest-impact risks and the external or upstream items that could affect delivery. Keep this section brief and move ongoing detail into the RAID register once the project is active.

- Risk example: unresolved data-quality issues could delay migration.
- Dependency example: security review, vendor contract, or upstream platform upgrade.

## Governance and approval

State who approves the work, where key decisions are made, and how escalation works. Readers should be able to see the approval path without opening the full board or communication plans.

- Example: steering group approves stage progression and major scope changes.
- Example: escalation path from project manager to sponsor to board chair.

## Related documents

- [01_business_case_TEMPLATE.md](01_business_case_TEMPLATE.md) — expands the value case and recommendation behind the initiative.
- [02_project_initiation_document_TEMPLATE.md](02_project_initiation_document_TEMPLATE.md) — turns the brief into a fuller management baseline.
- [../07_delivery/01_delivery_plan_TEMPLATE.md](../07_delivery/01_delivery_plan_TEMPLATE.md) — translates the approved brief into execution milestones and controls.
