---
title: Backlog Policy Template
status: draft
record_class: canonical
audience: [internal, manager]
owner: product-management
capability: product
phase: planning
cadence: monthly
last_reviewed: 2026-05-07
---

# Backlog Policy Template

> **Purpose:** define what belongs in the backlog, how work enters it, how it is prioritized, and where the operational backlog system of record lives.
> **Audience:** product and delivery leads, plus contributors who need consistent intake and prioritization rules.
> **When to update:** update when the team changes backlog scope, intake workflow, prioritization method, refinement rhythm, or tooling boundaries.

## How to use this template

Use this template when a project needs explicit rules for backlog hygiene and decision-making. Keep it policy-level: describe the operating rules here, but point to the actual tool for day-to-day backlog state.

- State clearly which work items belong in backlog tooling versus canonical docs.
- Use definitions that are strict enough to support triage and planning discipline.
- Remove sample options that do not apply to the project’s chosen workflow.

## What not to include

- **Live backlog items or ticket content** — the actual backlog lives in the delivery tool (for example, Jira or GitHub Issues). This document defines the policy for what belongs there, not the content itself.
- **Acceptance criteria or test scenarios** — acceptance criteria belong in the acceptance catalog ([05_acceptance_catalog_TEMPLATE.md](05_acceptance_catalog_TEMPLATE.md)). Backlog items should link to acceptance criteria, not reproduce them.
- **Requirements narrative or PRD content** — the PRD ([01_prd_TEMPLATE.md](01_prd_TEMPLATE.md)) explains the problem and scope. This policy explains how items enter and move through the backlog once the problem is understood.
- **Delivery plan milestones** — scheduling and milestone ownership belongs in the delivery plan ([../07_delivery/01_delivery_plan_TEMPLATE.md](../07_delivery/01_delivery_plan_TEMPLATE.md)).
- **Defect management rules** — defect triage and severity guidance belongs in the defect management template ([../05_testing_acceptance/05_defect_management_TEMPLATE.md](../05_testing_acceptance/05_defect_management_TEMPLATE.md)).

## Frontmatter quick reference

This template’s typical frontmatter values. The full schema (with all enums and conditional rules) is at [docs/00_operating_model/04_frontmatter_schema.md](../00_operating_model/04_frontmatter_schema.md).

| Field | Typical value here | Notes |
|---|---|---|
| `status` | `draft` → `active` → `superseded` | Use `superseded` when replaced; required `superseded_by:` link |
| `record_class` | `canonical` | This template defines a canonical artifact |
| `audience` | `[internal, manager]` | Add `client` only when client-export-safe |
| `capability` | `product` | Fixed for this folder |
| `phase` | `planning` | One of `initiation`, `planning`, `execution`, `monitoring`, `closure`, `n/a` |
| `cadence` | `monthly` | One of `ad-hoc`, `weekly`, `monthly`, `per-stage`, `per-release`, `one-shot` |

> When `capability: execution`, both `cadence` and `source_of_truth` are required by the validator.

## What goes in the backlog

Define which classes of work belong in the product backlog and which belong elsewhere. This keeps the backlog useful instead of turning it into a general note dump.

- Example inclusions: user-facing features, defects, technical enablers, compliance work, discovery spikes.
- Example exclusions: meeting notes, settled decisions already captured in ADRs, long-form requirements already stored in canonical docs.

## Intake

Describe how new items enter the backlog, who can submit them, and what minimum information is required. A lightweight intake gate prevents low-signal items from consuming refinement time.

- Example prompts: required fields, triage owner, intake SLA, duplicate-check step.
- Example evidence: link to PRD, incident, customer feedback, or compliance driver.

## Prioritization model

Explain the scoring or decision model used to order work, along with any non-negotiable overrides. Keep the model simple enough that contributors can predict outcomes without long debates.

- Example models: MoSCoW, RICE, WSJF, fixed regulatory override plus product-value scoring.
- Example factors: user impact, urgency, risk reduction, dependency criticality, effort range.

## Refinement cadence

State how often backlog refinement happens, who attends, and what “good enough” looks like for items moving toward delivery. The aim is to keep near-term work ready without over-specifying distant work.

- Example cadence: weekly triage, fortnightly refinement, pre-release scope review.
- Example outputs: clarified story, linked requirement, dependency flag, acceptance draft.

## Ready/done definitions

Define the minimum standard for a backlog item to be considered ready for implementation and done for planning purposes. Keep these definitions consistent with product, testing, and delivery handoffs.

- Example ready criteria: owner assigned, value clear, acceptance known, dependencies visible.
- Example done criteria: acceptance met, evidence linked, docs updated, rollout notes prepared.

## Tool-boundary statement

State where the operational backlog actually lives and what this document does not replace. Canonical docs should explain the policy and the links, not mirror volatile ticket-by-ticket state.

- Example statement: “The live backlog is maintained in Jira; this document defines the policy for what belongs there and how items are governed.”
- Example statement: “Canonical requirements remain in `docs/02_product/`; implementation status remains in the delivery tool.”

## Related documents

- [01_prd_TEMPLATE.md](01_prd_TEMPLATE.md) — the PRD defines the product problem and scope that should drive backlog intake.
- [02_requirements_catalog_TEMPLATE.md](02_requirements_catalog_TEMPLATE.md) — use the catalog for stable requirement IDs that backlog items can reference.
- [05_acceptance_catalog_TEMPLATE.md](05_acceptance_catalog_TEMPLATE.md) — backlog items should point to the acceptance conditions they must satisfy.
- [../07_delivery/01_delivery_plan_TEMPLATE.md](../07_delivery/01_delivery_plan_TEMPLATE.md) — delivery planning consumes the prioritized backlog and release assumptions.
