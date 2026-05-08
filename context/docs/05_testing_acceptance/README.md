---
title: Testing And Acceptance Templates
status: draft
record_class: canonical
audience: [internal, manager]
owner: quality-lead
capability: quality
phase: n/a
cadence: monthly
last_reviewed: 2026-05-07
source_of_truth: repo
---

# Testing And Acceptance Templates

> **Purpose**: orient readers to the canonical templates for test strategy, quality control, evidence, UAT, and defect management.
> **Audience**: internal teams and managers responsible for verification scope, acceptance readiness, and quality evidence.
> **When to update**: update when testing templates, quality gates, evidence expectations, or acceptance flows change.

## How to use this template

Use this landing page to pick the smallest set of quality artifacts needed for the work. Start with strategy and quality planning, then add evidence, UAT, and defect controls as delivery moves toward acceptance.

- Start with the test strategy for scope and level decisions.
- Keep detailed execution logs in tools; summarize canonical quality controls here.
- Link to product acceptance and delivery readiness records instead of duplicating them.

## Folder purpose

This folder holds the canonical templates for planning, evidencing, and approving testing and acceptance work. Use it to show what will be tested, how quality will be measured, and what evidence supports release and sign-off.

- Typical use: define test scope, levels, environments, and quality gates before execution starts.
- Typical use: maintain evidence and UAT sign-off paths as release readiness approaches.

## Index

Use the templates below according to the maturity of the verification effort. Keep the cross-links between product acceptance, evidence, and delivery readiness intact.

- [01_test_strategy_TEMPLATE.md](01_test_strategy_TEMPLATE.md) — baseline testing scope, levels, environments, and entry or exit controls.
- [02_quality_plan_TEMPLATE.md](02_quality_plan_TEMPLATE.md) — quality objectives, measures, reviews, and phase gates.
- [03_verification_evidence_index_TEMPLATE.md](03_verification_evidence_index_TEMPLATE.md) — traceable evidence matrix linking requirements, tests, runs, and results.
- [04_uat_plan_TEMPLATE.md](04_uat_plan_TEMPLATE.md) — UAT participants, scenarios, pass criteria, and sign-off.
- [05_defect_management_TEMPLATE.md](05_defect_management_TEMPLATE.md) — triage, severity, SLA, and escaped-defect handling.

## Reading order

Read the strategy first, then the quality plan, then the evidence index, and finally the acceptance and defect-control documents. This sequence keeps policy, measurement, and release-readiness evidence aligned.

1. [01_test_strategy_TEMPLATE.md](01_test_strategy_TEMPLATE.md)
2. [02_quality_plan_TEMPLATE.md](02_quality_plan_TEMPLATE.md)
3. [03_verification_evidence_index_TEMPLATE.md](03_verification_evidence_index_TEMPLATE.md)
4. [04_uat_plan_TEMPLATE.md](04_uat_plan_TEMPLATE.md)
5. [05_defect_management_TEMPLATE.md](05_defect_management_TEMPLATE.md)

## Which template should I use?

Pick the smallest set of templates that matches your project's risk and scope. Add more only when justified by complexity, regulation, or stakeholder need.

| Release risk | Recommended templates | Skip |
|---|---|---|
| **Low** — internal tooling or prototype, single team, no customer-facing SLA, no compliance obligation, defects caught by developers before release | [01_test_strategy_TEMPLATE.md](01_test_strategy_TEMPLATE.md) — scope, levels, environments, and entry or exit controls | [02_quality_plan_TEMPLATE.md](02_quality_plan_TEMPLATE.md), [03_verification_evidence_index_TEMPLATE.md](03_verification_evidence_index_TEMPLATE.md), [04_uat_plan_TEMPLATE.md](04_uat_plan_TEMPLATE.md), [05_defect_management_TEMPLATE.md](05_defect_management_TEMPLATE.md) |
| **Medium** — customer-facing release, multiple contributors, stakeholder sign-off required, or service SLA that defects could breach | [01_test_strategy_TEMPLATE.md](01_test_strategy_TEMPLATE.md); [02_quality_plan_TEMPLATE.md](02_quality_plan_TEMPLATE.md) — quality objectives, measures, and phase gates; [04_uat_plan_TEMPLATE.md](04_uat_plan_TEMPLATE.md) — when business or end-user acceptance sign-off is needed before go-live | [03_verification_evidence_index_TEMPLATE.md](03_verification_evidence_index_TEMPLATE.md) — add only when an auditor or client must trace requirements to test results; [05_defect_management_TEMPLATE.md](05_defect_management_TEMPLATE.md) — add only when defect triage SLA is formally owned |
| **High** — regulated environment or safety-critical domain; external audit evidence required; contractual or legal acceptance obligations; defects have financial, safety, or compliance consequences | Full set: all medium templates plus [03_verification_evidence_index_TEMPLATE.md](03_verification_evidence_index_TEMPLATE.md) — traceable evidence matrix linking requirements, tests, runs, and results; [05_defect_management_TEMPLATE.md](05_defect_management_TEMPLATE.md) — triage, severity, SLA, and escaped-defect handling | Nothing — all documents apply |

**Rules of thumb:**
- Add the verification evidence index ([03_verification_evidence_index_TEMPLATE.md](03_verification_evidence_index_TEMPLATE.md)) only when a third party must independently verify that every requirement has a passing test — not as a default for internal releases.
- Add the UAT plan ([04_uat_plan_TEMPLATE.md](04_uat_plan_TEMPLATE.md)) as soon as a business owner or client will formally sign off on acceptance — even for medium-risk releases.
- Keep the test strategy in every profile; test execution without a scope-and-level decision is the primary cause of quality surprises near release.

## Related documents

- [../02_product/05_acceptance_catalog_TEMPLATE.md](../02_product/05_acceptance_catalog_TEMPLATE.md) — defines the acceptance scenarios that testing and UAT must prove.
- [../07_delivery/07_release_plan_TEMPLATE.md](../07_delivery/07_release_plan_TEMPLATE.md) — consumes quality and acceptance evidence during release approval.
- [../07_delivery/06_readiness_tracker_TEMPLATE.md](../07_delivery/06_readiness_tracker_TEMPLATE.md) — tracks whether required verification gates are complete.
