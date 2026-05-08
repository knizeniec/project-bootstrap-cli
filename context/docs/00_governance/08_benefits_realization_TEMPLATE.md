---
title: Benefits Realization Template
status: draft
record_class: canonical
audience: [internal, manager, client]
owner: business-owner
capability: governance
phase: closure
cadence: monthly
last_reviewed: 2026-05-07
---

# Benefits Realization Template

> **Purpose**: define expected benefits, baselines, measures, and ownership for tracking realized value over time.
> **Audience**: sponsors, managers, business owners, and client stakeholders accountable for project outcomes after delivery.
> **When to update**: update when benefit measures change and during scheduled realization reviews after release.

## How to use this template

Use this template to turn the business case into a measurable benefits tracking plan. Keep each benefit specific, owned, and tied to a realistic measurement method rather than broad aspiration statements.

- Mandatory: benefit map, baseline, target, owner, method, and review cadence.
- Optional: separate benefit groups by financial and non-financial categories.
- Remove speculative benefits that cannot be measured or owned.

## What not to include

- **Investment justification or option analysis** — the case for investment belongs in the business case ([01_business_case_TEMPLATE.md](01_business_case_TEMPLATE.md)). This template tracks whether promised benefits are realized, not whether the project should proceed.
- **Delivery milestones or release plans** — delivery tracking belongs in the delivery plan and status report ([../07_delivery/01_delivery_plan_TEMPLATE.md](../07_delivery/01_delivery_plan_TEMPLATE.md)). Benefit reviews are post-delivery, not delivery tracking.
- **Feature requirements or acceptance criteria** — product requirements belong in the PRD ([../02_product/01_prd_TEMPLATE.md](../02_product/01_prd_TEMPLATE.md)). Benefits should connect to outcomes, not feature lists.
- **Operational incident reports or defect logs** — post-launch issues belong in the post-launch review ([../06_security_operations/09_post_launch_review_TEMPLATE.md](../06_security_operations/09_post_launch_review_TEMPLATE.md)).
- **Raw source data or survey instruments** — link to data sources here rather than embedding raw measurement artefacts in the plan.

## Frontmatter quick reference

This template's typical frontmatter values. The full schema (with all enums and conditional rules) is at [docs/00_operating_model/04_frontmatter_schema.md](../00_operating_model/04_frontmatter_schema.md).

| Field | Typical value here | Notes |
|---|---|---|
| `status` | `draft` → `active` → `superseded` | Use `superseded` when replaced; required `superseded_by:` link |
| `record_class` | `canonical` | This template defines a canonical artifact |
| `audience` | `[internal, manager, client]` | Add `client` only when client-export-safe |
| `capability` | `governance` | Fixed for this folder |
| `phase` | `closure` | One of `initiation`, `planning`, `execution`, `monitoring`, `closure`, `n/a` |
| `cadence` | `monthly` | One of `ad-hoc`, `weekly`, `monthly`, `per-stage`, `per-release`, `one-shot` |

> When `capability: execution`, both `cadence` and `source_of_truth` are required by the validator.

## Benefit map

List the major benefits the initiative is expected to produce and how they relate to project outputs. This helps later reviewers see the chain from delivered capability to business outcome.

- Example: self-service onboarding reduces manual handling time.
- Example: improved audit trail lowers compliance remediation effort.

## Baseline

Record the current-state measure before change is implemented. Baselines make later claims about improvement credible and comparable.

- Example: current processing time, error rate, support call volume.
- Example: current customer satisfaction score or policy exception count.

## Target

Define the expected future level for each benefit and the time horizon for achieving it. Targets should be ambitious enough to justify the work but realistic enough to review objectively.

- Example: reduce average handling time from 20 minutes to 12 minutes within six months.
- Example: reach 75% digital adoption by the second release.

## Measurement method

Explain how the team will collect and calculate the evidence for each benefit. Readers should know which system, report, or survey is treated as the source for each measure.

- Example: weekly service metrics dashboard or monthly finance report.
- Example: user survey, support ticket trend, or audit sample.

## Owner

Assign a business owner for each benefit who remains accountable after project delivery. Benefits without named owners often disappear once the implementation team closes.

- Example: operations manager owns handling-time reduction benefit.
- Example: product director owns adoption and satisfaction benefits.

## Review cadence

Set when benefits will be reviewed and who attends those reviews. Align the cadence to when evidence becomes meaningful rather than to project reporting habit alone.

- Example: monthly for adoption during rollout, then quarterly once stable.
- Example: formal post-launch review at 30, 90, and 180 days.

## Realized values over time

Reserve space to record actual values and commentary as benefits mature. This turns the template into a reusable tracking artifact rather than a one-time plan.

- Example: baseline, target, month 1, month 3, month 6 actuals.
- Example: notes on seasonal effects, delayed adoption, or changed measurement rules.

## Related documents

- [01_business_case_TEMPLATE.md](01_business_case_TEMPLATE.md) — provides the promised value case that benefits realization should test.
- [09_stage_gate_checklist_TEMPLATE.md](09_stage_gate_checklist_TEMPLATE.md) — can require benefit tracking readiness before closure.
- [../07_delivery/07_release_plan_TEMPLATE.md](../07_delivery/07_release_plan_TEMPLATE.md) — links realized value expectations to release sequencing and success criteria.
