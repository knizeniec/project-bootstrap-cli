---
title: Project Initiation Document Template
status: draft
record_class: canonical
audience: [internal, manager, client]
owner: project-manager
capability: governance
phase: initiation
cadence: one-shot
last_reviewed: 2026-05-07
---

# Project Initiation Document Template

> **Purpose**: define the formal management baseline for delivering an approved project.
> **Audience**: project managers, sponsors, managers, and client stakeholders who need the agreed control model.
> **When to update**: update at project start and when approved tolerances or control arrangements change.

## How to use this template

Use this template after the brief and business case are accepted and the project needs a formal initiation baseline. Keep the content managerial rather than technical, and link to specialist plans instead of duplicating them.

- Mandatory: definition, approach, tolerances, roles, and controls.
- Optional: deeper references to quality or configuration procedures if separate files exist.
- Remove duplicate narrative once linked control documents are in place.

## What not to include

- **Detailed technical design** — architecture and design decisions belong in the solution design ([../03_architecture/01_solution_design_TEMPLATE.md](../03_architecture/01_solution_design_TEMPLATE.md)). The PID defines the management baseline, not the technical approach.
- **Test case detail or acceptance scenarios** — these belong in the test strategy and acceptance catalog ([../05_testing_acceptance/01_test_strategy_TEMPLATE.md](../05_testing_acceptance/01_test_strategy_TEMPLATE.md)). The PID references quality management, not test content.
- **Full communications content** — message templates and audience-specific updates belong in the communications plan ([05_communications_plan_TEMPLATE.md](05_communications_plan_TEMPLATE.md)).
- **Live RAID register entries** — risks and issues should be tracked in the RAID register ([06_raid_register_TEMPLATE.md](06_raid_register_TEMPLATE.md)), not duplicated in the PID body.
- **Benefits tracking or financial model** — realized-value tracking belongs in the benefits realization plan ([08_benefits_realization_TEMPLATE.md](08_benefits_realization_TEMPLATE.md)).

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

## Project definition

Describe what the project is delivering, for whom, and within what boundaries. This section should align directly to the approved brief and business case so governance records tell one story.

- Example: objective, scope summary, sponsor, and planned delivery horizon.
- Example: key products, excluded work, and success definition.

## Approach

Explain how the project will be managed and delivered at a high level. This should give readers the chosen delivery style without replacing the detailed delivery or implementation plans.

- Example: phased delivery with stage gates and pilot release.
- Example: vendor-supported implementation with internal assurance reviews.

## Tolerances

Set the boundaries within which the project manager can operate without escalation. Tolerances make decision rights explicit and reduce ambiguity during execution.

- Example: schedule tolerance of plus/minus two weeks per stage.
- Example: cost tolerance of plus/minus five percent before sponsor approval is required.

## Roles and responsibilities

List the main governance and delivery roles and the accountability expected of each. Keep it concise here and link to fuller board or decision-rights documents if they exist.

- Example: sponsor owns funding and benefits; project manager owns day-to-day delivery.
- Example: product lead approves scope; technical lead assures solution fitness.

## Quality management

Summarize how the project will assure quality and who signs off key outputs. Focus on checkpoints, evidence expectations, and escalation rather than test-case detail.

- Example: stage-end review, independent quality check, and exit criteria.
- Example: acceptance evidence required before release approval.

## Configuration management

Explain how baselines, versions, and controlled changes will be handled. Readers should understand where formal records live and how approved changes are tracked.

- Example: canonical docs in repo, change decisions in the change control log.
- Example: named baseline approved at each stage gate.

## Communication management

State how stakeholders will be kept informed and how governance decisions will be communicated. This section should summarize the plan and point to the fuller communications artifact.

- Example: weekly status reporting to sponsor and monthly board review.
- Example: client updates after gate decisions or major risks.

## Project plan summary

Provide the headline delivery view without recreating the whole delivery plan. The aim is to show the expected stages, milestones, and decision points.

- Example: discovery, build, pilot, release, and closure stages.
- Example: dependency on procurement or environment readiness before build.

## Controls

Bring together the live registers and checkpoints the project will use. This helps readers see how the management baseline is enforced once execution begins.

- Example: RAID register, change log, status report, readiness tracker.
- Example: stage-gate checklist and requirements traceability matrix.

## Related documents

- [00_project_brief_TEMPLATE.md](00_project_brief_TEMPLATE.md) — provides the concise mandate that the PID elaborates.
- [05_communications_plan_TEMPLATE.md](05_communications_plan_TEMPLATE.md) — details the communication model referenced by the PID.
- [../07_delivery/01_delivery_plan_TEMPLATE.md](../07_delivery/01_delivery_plan_TEMPLATE.md) — carries the execution baseline summarized here.
