---
title: Release Plan Template
status: draft
record_class: canonical
audience: [internal, manager]
owner: release-manager
capability: execution
phase: execution
cadence: per-release
last_reviewed: 2026-05-07
source_of_truth: repo
---

# Release Plan Template

> **Purpose**: define what is being released, where it is going, how it will be sequenced, and how success will be judged.
> **Audience**: release managers, delivery leads, and managers making release readiness and go/no-go decisions.
> **When to update**: update when release scope, environment sequence, rollback assumptions, or communications change.

## How to use this template

Use this template when a delivery increment is approaching release and needs a formal execution plan. Keep it release-focused, with enough operational detail to support approval without duplicating the cutover runbook.

- Mandatory: release scope, environments, sequence, rollback, communications plan, and success criteria.
- Optional: deployment window details if they are owned in the cutover document.
- Remove backlog items that are no longer in the approved release scope.

## What not to include

- **Ordered cutover or rollback steps** — step-by-step operational procedures belong in the cutover and rollback template ([08_cutover_and_rollback_TEMPLATE.md](08_cutover_and_rollback_TEMPLATE.md)). The release plan summarises the approach; the runbook provides the ordered steps.
- **Product requirements or acceptance scenarios** — requirements belong in the PRD ([../02_product/01_prd_TEMPLATE.md](../02_product/01_prd_TEMPLATE.md)) and acceptance catalog. The release plan states what is in scope; requirements documents define what each item means.
- **UAT or test results** — test evidence belongs in the verification evidence index ([../05_testing_acceptance/03_verification_evidence_index_TEMPLATE.md](../05_testing_acceptance/03_verification_evidence_index_TEMPLATE.md)). Link to evidence; do not reproduce it.
- **Communications message drafts** — full communications copy belongs in the communications plan ([../00_governance/05_communications_plan_TEMPLATE.md](../00_governance/05_communications_plan_TEMPLATE.md)). Reference the plan here.
- **Architecture decisions or technical rationale** — design decisions belong in ADRs ([../adr/ADR-000-template.md](../adr/ADR-000-template.md)). Note relevant decisions without reproducing their rationale.

## Frontmatter quick reference

This template's typical frontmatter values. The full schema (with all enums and conditional rules) is at [docs/00_operating_model/04_frontmatter_schema.md](../00_operating_model/04_frontmatter_schema.md).

| Field | Typical value here | Notes |
|---|---|---|
| `status` | `draft` → `active` → `superseded` | Use `superseded` when replaced; required `superseded_by:` link |
| `record_class` | `canonical` | This template defines a canonical artifact |
| `audience` | `[internal, manager]` | Add `client` only when client-export-safe |
| `capability` | `execution` | Fixed for this folder |
| `phase` | `execution` | One of `initiation`, `planning`, `execution`, `monitoring`, `closure`, `n/a` |
| `cadence` | `per-release` | One of `ad-hoc`, `weekly`, `monthly`, `per-stage`, `per-release`, `one-shot` |

> When `capability: execution`, both `cadence` and `source_of_truth` are required by the validator. Add `source_of_truth: repo` (or the appropriate value) to the frontmatter.

## Release scope

State exactly what is included in the release and what is deferred. Clear scope helps stakeholders distinguish a release decision from a general status update.

- Example: features, fixes, migrations, and configuration changes in scope.
- Example: deferred items or post-release follow-ups explicitly out of scope.

## Environments

Identify the environments involved in the release path and any environment-specific considerations. Keep the information limited to what matters for readiness and decision-making.

- Example: staging sign-off environment, rehearsal environment, production target.
- Example: environment freeze window or access dependency.

## Sequence

Summarize the order in which release actions or waves will occur. This should align with the cutover runbook while staying readable for decision forums.

- Example: approve go/no-go, deploy, validate smoke tests, open user access, monitor.
- Example: phased wave release by region or customer cohort.

## Rollback

Describe the rollback approach at decision level and the conditions that would trigger it. The full operational detail belongs in the cutover and rollback runbook.

- Example: revert deployment and restore prior configuration if critical transaction errors exceed threshold.
- Example: pause additional waves while retaining completed pilot users.

## Comms plan

State who needs to be informed before, during, and after the release. Keep it synchronized with the wider communications plan so release messaging is consistent.

- Example: internal support bulletin, client release notice, sponsor checkpoint.
- Example: incident escalation channel if release health degrades.

## Success criteria

Define how the team will know the release succeeded beyond simply completing the deployment. Success criteria should be measurable within the early-live period.

- Example: all critical post-release checks pass and no Sev-1 incident occurs in 24 hours.
- Example: target user cohort can complete the primary workflow without support intervention.

## Related documents

- [08_cutover_and_rollback_TEMPLATE.md](08_cutover_and_rollback_TEMPLATE.md) — contains the ordered runbook and fallback steps that operationalize this plan.
- [../06_security_operations/README.md](../06_security_operations/README.md) — points to the service acceptance area that must be satisfied before release.
- [06_readiness_tracker_TEMPLATE.md](06_readiness_tracker_TEMPLATE.md) — provides the gating evidence used to confirm release readiness.
