---
title: Product Vision Template
status: draft
record_class: canonical
audience: [internal, manager, client]
owner: product-manager
capability: strategy
phase: initiation
cadence: one-shot
last_reviewed: 2026-05-07
---

# Product Vision Template

> **Purpose**: define the future state the product is trying to create and why it matters.
> **Audience**: product leaders, managers, delivery leads, and client stakeholders aligning on long-term direction.
> **When to update**: update when the target customer, product proposition, or strategic bets materially change.

## How to use this template

Use this template to align teams on destination before detailed requirements or roadmaps are written. Keep it strategic, customer-centered, and concrete enough that readers can use it to accept or reject downstream proposals.

- Mandatory: vision, target customer, problem, differentiator, horizon, and bets.
- Optional: supporting market evidence if it helps explain the case.
- Remove workshop notes and exploratory options before publishing.

## What not to include

- **Specific features or requirements** — product requirements belong in the PRD ([../02_product/01_prd_TEMPLATE.md](../02_product/01_prd_TEMPLATE.md)). The vision describes the destination, not the feature list.
- **Delivery milestones or release sequencing** — sequencing belongs in the roadmap ([02_roadmap_TEMPLATE.md](02_roadmap_TEMPLATE.md)). The vision states where to go; the roadmap states how to get there.
- **Investment approval or business case analysis** — financial justification belongs in the business case ([../00_governance/01_business_case_TEMPLATE.md](../00_governance/01_business_case_TEMPLATE.md)).
- **Technical architecture choices** — design decisions belong in ADRs and the solution design ([../03_architecture/01_solution_design_TEMPLATE.md](../03_architecture/01_solution_design_TEMPLATE.md)).
- **Stakeholder names or individual commitments** — stakeholder ownership belongs in the stakeholder register ([../00_governance/04_stakeholder_register_TEMPLATE.md](../00_governance/04_stakeholder_register_TEMPLATE.md)).

## Frontmatter quick reference

This template's typical frontmatter values. The full schema (with all enums and conditional rules) is at [docs/00_operating_model/04_frontmatter_schema.md](../00_operating_model/04_frontmatter_schema.md).

| Field | Typical value here | Notes |
|---|---|---|
| `status` | `draft` → `active` → `superseded` | Use `superseded` when replaced; required `superseded_by:` link |
| `record_class` | `canonical` | This template defines a canonical artifact |
| `audience` | `[internal, manager, client]` | Add `client` only when client-export-safe |
| `capability` | `strategy` | Fixed for this folder |
| `phase` | `initiation` | One of `initiation`, `planning`, `execution`, `monitoring`, `closure`, `n/a` |
| `cadence` | `one-shot` | One of `ad-hoc`, `weekly`, `monthly`, `per-stage`, `per-release`, `one-shot` |

> When `capability: execution`, both `cadence` and `source_of_truth` are required by the validator.

## Vision statement

Write a concise statement of the future the product is meant to create. The statement should be memorable enough to guide choices after the workshop that produced it has ended.

- Example: become the default self-service path for compliant customer onboarding.
- Example: provide a single trusted workflow for internal case management across regions.

## Target customer

Describe the primary user or buyer the product is serving and the context they operate in. If there are multiple audiences, identify the priority audience and why they come first.

- Example: operations teams processing high-volume regulated requests.
- Example: end users who need a faster, lower-friction digital channel.

## Problem

Explain the pain, inefficiency, or unmet need that makes the product necessary. Keep the problem grounded in evidence or observable behavior rather than only opinion.

- Example: fragmented tools create duplicate entry and opaque approval trails.
- Example: current workflows require specialist support for routine tasks.

## Differentiator

State what will make the product meaningfully better than the current state or competing approaches. A differentiator should help prioritization later by clarifying what cannot be compromised.

- Example: integrated policy checks embedded directly in the workflow.
- Example: one shared platform replacing multiple bespoke team processes.

## Horizon (1y/3y)

Describe the expected product shape over the near and medium term. This helps teams distinguish current release decisions from the longer arc they should support.

- Example: one-year target of pilot adoption in two business units.
- Example: three-year target of platform standardization across all regions.

## Strategic bets

List the big assumptions or choices the organization is intentionally making. Strategic bets should be bold enough to shape investment and roadmap trade-offs.

- Example: prioritize platform consolidation over local customization.
- Example: invest early in self-service adoption before advanced analytics features.

## Related documents

- [02_roadmap_TEMPLATE.md](02_roadmap_TEMPLATE.md) — turns the vision into a sequenced horizon plan.
- [../00_governance/01_business_case_TEMPLATE.md](../00_governance/01_business_case_TEMPLATE.md) — tests whether the vision supports an investable proposition.
- [../02_product/01_prd_TEMPLATE.md](../02_product/01_prd_TEMPLATE.md) — refines the vision into product scope and requirements.
