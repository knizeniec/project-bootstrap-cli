---
title: Business Case Template
status: draft
record_class: canonical
audience: [internal, manager, client]
owner: project-sponsor
capability: governance
phase: initiation
cadence: one-shot
last_reviewed: 2026-05-07
---

# Business Case Template

> **Purpose**: justify the initiative by comparing options, costs, benefits, and risks before major commitment.
> **Audience**: sponsors, managers, and client decision-makers who approve funding and strategic direction.
> **When to update**: update before approval, and refresh only when the value case or recommended option changes materially.

## How to use this template

Use this template when a project needs a formal investment rationale beyond the initial brief. Keep the analysis decision-oriented, show the options considered, and link assumptions to supporting evidence where possible.

- Mandatory: problem, options, recommendation, costs, benefits, and risks.
- Optional: full financial model if the numbers are maintained elsewhere.
- Remove draft-only assumptions or placeholder figures before approval.

## What not to include

- **Delivery milestones or sprint plans** — execution scheduling belongs in the delivery plan ([../07_delivery/01_delivery_plan_TEMPLATE.md](../07_delivery/01_delivery_plan_TEMPLATE.md)). The business case justifies the investment; it does not plan the delivery.
- **Technical architecture detail** — design decisions belong in the solution design ([../03_architecture/01_solution_design_TEMPLATE.md](../03_architecture/01_solution_design_TEMPLATE.md)). Describe the option shape here without prescribing the implementation.
- **Benefits tracking actuals** — post-delivery benefit measurement belongs in the benefits realization plan ([08_benefits_realization_TEMPLATE.md](08_benefits_realization_TEMPLATE.md)). The business case projects benefits; the realization plan tracks them.
- **Full RAID register detail** — summarise risks here and track them in the RAID register ([06_raid_register_TEMPLATE.md](06_raid_register_TEMPLATE.md)) once the project is active.
- **Stakeholder engagement actions** — stakeholder planning belongs in the stakeholder register and communications plan, not embedded in the investment justification.

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

## Problem statement

Describe the current pain, missed opportunity, or mandatory change the initiative addresses. Make the problem concrete enough that readers can judge whether the proposed investment is proportionate.

- Example: manual onboarding causes a 10-day delay and repeated compliance defects.
- Example: an end-of-life platform creates unbudgeted support and security risk.

## Strategic alignment

Show how the proposal supports business goals, policy commitments, or roadmap themes. This section should connect the local problem to higher-level organizational priorities.

- Example: aligns to the digital self-service strategy and cost-to-serve reduction target.
- Example: supports a regulatory remediation objective due this financial year.

## Options considered

List the realistic options, including a do-nothing or defer option where relevant. Explain each option in a comparable way so decision-makers can see the trade-offs clearly.

- Example: do nothing, buy a vendor product, or build in-house.
- Example: limited remediation now versus full platform replacement.

## Recommended option

State the chosen option and why it is preferred over the alternatives. Focus on the decision criteria that mattered most, such as value, risk, time, or strategic fit.

- Example: recommend phased in-house delivery because it reduces vendor lock-in and fits current capability.
- Example: recommend a commercial tool because time-to-compliance outweighs customization needs.

## Costs (capex/opex)

Summarize the expected cost profile, including one-time and ongoing costs where applicable. Keep the line items simple here and reference a detailed estimate if one exists.

- Example: implementation services, licenses, migration effort, and training.
- Example: yearly support, hosting, and operational staffing costs.

## Benefits (financial/non-financial)

Describe both measurable and non-financial benefits so the decision is not based on cost alone. Benefits should map forward into the benefits realization plan and status reporting.

- Example: lower handling cost, reduced defect rate, faster cycle time.
- Example: improved customer experience, auditability, or staff resilience.

## NPV/payback

Capture the headline financial indicators that help compare options consistently. If exact figures are uncertain, note the modelling basis and the confidence level.

- Example: expected payback in 18 months under the base case.
- Example: positive NPV under moderate adoption assumptions.

## Risks

Summarize the major risks that could undermine value, delivery, or adoption. Keep the focus on decision-relevant risk rather than the full delivery RAID detail.

- Example: dependency on vendor contract approval or scarce specialist skills.
- Example: benefit assumptions depend on policy change adoption outside the project team.

## Recommendation

End with a clear ask that the approving body can accept, reject, or send back for revision. State any conditions that must be met for approval to remain valid.

- Example: approve funding for discovery and planning only, with a stage gate before build.
- Example: approve full delivery subject to security review and named sponsor sign-off.

## Related documents

- [00_project_brief_TEMPLATE.md](00_project_brief_TEMPLATE.md) — provides the short-form project framing that precedes the business case.
- [08_benefits_realization_TEMPLATE.md](08_benefits_realization_TEMPLATE.md) — tracks whether the promised benefits are later realized.
- [../01_strategy/02_roadmap_TEMPLATE.md](../01_strategy/02_roadmap_TEMPLATE.md) — shows where the approved investment fits in the broader delivery horizon.
