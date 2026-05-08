---
title: Product Requirements Document Template
status: draft
record_class: canonical
audience: [internal, manager]
owner: product-management
capability: product
phase: planning
cadence: per-release
last_reviewed: 2026-05-07
---

# Product Requirements Document Template

> **Purpose:** provide the canonical template for describing the product problem, target users, requirement set, and release intent for a feature or initiative.
> **Audience:** product, delivery, design, engineering, and QA stakeholders who need a shared planning baseline before implementation and acceptance.
> **When to update:** update when the product problem, goals, scope, requirements, acceptance expectations, or release assumptions materially change.

## How to use this template

Copy this file when a project needs a single narrative source for why a product increment exists and what success looks like. Keep this document concise, then push detailed inventories into the requirements and acceptance catalogs.

- Keep every requirement and story traceable to an outcome or stakeholder need.
- Move large tables into [02_requirements_catalog_TEMPLATE.md](02_requirements_catalog_TEMPLATE.md) and [05_acceptance_catalog_TEMPLATE.md](05_acceptance_catalog_TEMPLATE.md) once the list grows.
- Remove placeholder bullets before publishing an active PRD.

## What not to include

- **Implementation timelines or delivery milestones** — scheduling belongs in the delivery plan ([../07_delivery/01_delivery_plan_TEMPLATE.md](../07_delivery/01_delivery_plan_TEMPLATE.md)). The PRD defines what to build and why; the delivery plan defines when.
- **Architectural design or technical decisions** — design belongs in the solution design ([../03_architecture/01_solution_design_TEMPLATE.md](../03_architecture/01_solution_design_TEMPLATE.md)) and ADRs. The PRD specifies desired behavior, not implementation.
- **Detailed test cases or verification steps** — test scenarios belong in the acceptance catalog ([05_acceptance_catalog_TEMPLATE.md](05_acceptance_catalog_TEMPLATE.md)) and test strategy ([../05_testing_acceptance/01_test_strategy_TEMPLATE.md](../05_testing_acceptance/01_test_strategy_TEMPLATE.md)).
- **Roadmap items or strategy horizon** — product sequencing belongs in the roadmap ([../01_strategy/02_roadmap_TEMPLATE.md](../01_strategy/02_roadmap_TEMPLATE.md)). The PRD covers one release or increment, not the whole product journey.
- **Stakeholder analysis or engagement plans** — stakeholder management belongs in the stakeholder register ([../00_governance/04_stakeholder_register_TEMPLATE.md](../00_governance/04_stakeholder_register_TEMPLATE.md)).

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

## Problem

State the user or business problem in one or two short paragraphs, then explain why it matters now. Focus on the gap between the current state and the desired outcome so downstream teams can test the right thing.

- Example prompt: what is failing today, for whom, and with what impact?
- Example prompt: what business or user signal says this problem deserves investment now?

## Users

Describe the primary and secondary users, plus any internal stakeholders who shape requirements or approvals. Keep the list short and action-oriented so readers understand whose outcomes drive prioritization.

- Example bullets: primary user groups, key stakeholder teams, accessibility or compliance-sensitive audiences.
- Example prompt: who receives value directly, and who is affected operationally?

## Goals

List the outcomes this release should achieve in observable terms. Goals should be measurable enough to inform acceptance and release decisions, not just aspirations.

- Example bullets: reduce task time, increase completion rate, improve adoption, remove a manual step.
- Example prompt: what changes in behavior or business performance should be visible after release?

## Non-goals

Record important boundaries so the project does not grow by assumption. Non-goals are especially useful when adjacent improvements are tempting but not funded or not ready.

- Example bullets: no redesign of adjacent workflows, no migration of legacy data, no new admin console in this release.
- Example prompt: what reasonable requests are explicitly out of scope for this iteration?

## User stories

Capture the most important user stories in a lightweight format that explains actor, need, and expected outcome. Keep stories outcome-focused and avoid turning implementation details into story text.

| ID | User story | Priority | Notes |
| --- | --- | --- | --- |
| US-001 | As a [persona], I want to [goal], so that [outcome]. | Must | Link to journey or requirement IDs. |
| US-002 | As a [persona], I want to [goal], so that [outcome]. | Should | Note dependencies or assumptions. |

## Functional requirements

List the required capabilities the product must provide. Keep each statement testable and atomic enough that it can map to acceptance scenarios and evidence later.

| ID | Requirement | Priority | Acceptance reference |
| --- | --- | --- | --- |
| FR-001 | The system shall [behavior]. | Must | AC-001 |
| FR-002 | The system shall [behavior]. | Should | AC-002 |

## Non-functional requirements

Capture quality, compliance, reliability, security, or usability constraints that shape the solution. Express each requirement as a measurable target, threshold, or rule wherever possible.

| ID | Requirement | Measure or constraint | Acceptance reference |
| --- | --- | --- | --- |
| NFR-001 | [Performance, reliability, security, or usability expectation] | [Threshold or rule] | AC-NFR-001 |
| NFR-002 | [Constraint] | [Threshold or rule] | AC-NFR-002 |

## Out of scope

Repeat the most important exclusions in a dedicated delivery-friendly form. Use this section to prevent downstream teams from assuming related work is implicitly included.

- Example bullets: excluded channels, unsupported user segments, deferred integrations, postponed reporting needs.
- Example prompt: what will stakeholders be tempted to infer that this release does not promise?

## Acceptance criteria

Summarize the conditions that must be true for the release to be considered acceptable. Keep the detailed scenario list in the acceptance catalog, but include the main success conditions here so executives and delivery leads can scan them quickly.

- Example bullets: critical journeys complete successfully, core NFR thresholds met, no unresolved severity-1 defects, named approvers sign off.
- Example prompt: what minimum bar separates “implemented” from “acceptable for release”?

## Release plan

Describe the intended release shape, dependencies, sequencing assumptions, and rollout guardrails at a summary level. Link out to delivery and testing artifacts for the detailed mechanics.

- Example bullets: target release window, rollout stages, dependency checkpoints, rollback assumptions.
- Example prompt: what has to be true for this PRD scope to ship safely?

## Related documents

- [02_requirements_catalog_TEMPLATE.md](02_requirements_catalog_TEMPLATE.md) — expands the requirement inventory with source, owner, and status tracking.
- [05_acceptance_catalog_TEMPLATE.md](05_acceptance_catalog_TEMPLATE.md) — holds the detailed Given/When/Then scenarios and UAT pass criteria linked from this PRD.
- [../05_testing_acceptance/01_test_strategy_TEMPLATE.md](../05_testing_acceptance/01_test_strategy_TEMPLATE.md) — explains how the PRD and acceptance criteria will be verified.
- [../00_governance/12_requirements_traceability_matrix_TEMPLATE.md](../00_governance/12_requirements_traceability_matrix_TEMPLATE.md) — connects requirements to design, test, and release evidence.
