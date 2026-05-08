---
title: Strategy Templates
status: draft
record_class: canonical
audience: [internal, manager, client]
owner: product-strategy-lead
capability: strategy
phase: n/a
cadence: monthly
last_reviewed: 2026-05-07
---

# Strategy Templates

> **Purpose**: orient readers to the strategy templates that define product direction and roadmap intent.
> **Audience**: internal teams, managers, and client stakeholders aligning work to strategic outcomes.
> **When to update**: update when strategy templates, sequencing, or ownership guidance changes.

## How to use this template

Use this landing page to select the strategy artifact that best explains why the work matters and where it fits over time. Read the vision first to align on destination, then use the roadmap to explain sequencing and investment timing.

- Start with the vision when the problem space or proposition is still being framed.
- Use the roadmap when approved themes must be sequenced across horizons.
- Link out to governance and delivery templates for decision and execution detail.

## Folder purpose

This folder holds the strategic framing artifacts that sit above detailed product and delivery planning. Use it to explain intent, target audience, differentiation, and the sequence of strategic bets.

- Typical use: align leaders on why a product direction is worth pursuing.
- Typical use: show how near-term work fits into a longer horizon.

## Index of templates

The strategy layer is intentionally small because it should stay high signal and easy to maintain. Each template exists to answer a distinct question: what future are we aiming for, and what is the path to get there.

- [01_product_vision_TEMPLATE.md](01_product_vision_TEMPLATE.md) — defines destination, target customer, and strategic bets.
- [02_roadmap_TEMPLATE.md](02_roadmap_TEMPLATE.md) — sequences themes, milestones, and dependencies across horizons.

## Reading order

Read the vision first so roadmap decisions have context. Move to governance and delivery once the strategic narrative is stable enough to justify a specific investment or release sequence.

1. [01_product_vision_TEMPLATE.md](01_product_vision_TEMPLATE.md)
2. [02_roadmap_TEMPLATE.md](02_roadmap_TEMPLATE.md)
3. [../00_governance/01_business_case_TEMPLATE.md](../00_governance/01_business_case_TEMPLATE.md) and [../07_delivery/01_delivery_plan_TEMPLATE.md](../07_delivery/01_delivery_plan_TEMPLATE.md)

## Which template should I use?

Pick the smallest set of templates that matches your project's risk and scope. Add more only when justified by complexity, regulation, or stakeholder need.

| When | Recommended templates | Skip |
|---|---|---|
| **Vision only** — problem space still being framed, no approved horizon or funding yet, single team exploring direction | [01_product_vision_TEMPLATE.md](01_product_vision_TEMPLATE.md) — defines destination, target customer, differentiation, and strategic bets | [02_roadmap_TEMPLATE.md](02_roadmap_TEMPLATE.md) — premature until themes and sequencing are approved |
| **Roadmap only** — vision is already stable and shared, team needs to communicate sequencing and investment timing | [02_roadmap_TEMPLATE.md](02_roadmap_TEMPLATE.md) — sequences themes, milestones, and dependencies across horizons | [01_product_vision_TEMPLATE.md](01_product_vision_TEMPLATE.md) — skip if an approved vision already exists in another document |
| **Both** — new product or major pivot where leaders need alignment on destination before sequencing; external stakeholders require both artefacts | [01_product_vision_TEMPLATE.md](01_product_vision_TEMPLATE.md) then [02_roadmap_TEMPLATE.md](02_roadmap_TEMPLATE.md) — vision first so roadmap decisions have narrative context | Nothing at this layer — both artefacts are warranted |

**Rules of thumb:**
- Write the vision before the roadmap; a roadmap without a shared vision produces sequencing debates rather than strategic alignment.
- Promote the roadmap to its own document only when the planning horizon exceeds one quarter or when dependencies span more than one team.
- Keep both documents short — a vision longer than two pages and a roadmap with more than three horizons are signs that product or delivery detail has leaked into the wrong layer.

## Related documents

- [../00_governance/README.md](../00_governance/README.md) — governance records turn strategy into approved scope and funding.
- [../02_product/README.md](../02_product/README.md) — product artifacts refine strategy into requirements and acceptance detail.
- [../07_delivery/README.md](../07_delivery/README.md) — delivery artifacts convert roadmap intent into execution control.
