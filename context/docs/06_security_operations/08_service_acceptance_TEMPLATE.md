---
title: Service Acceptance Template
status: draft
record_class: canonical
audience: [internal, manager]
owner: service-owner
capability: operations
phase: execution
cadence: per-release
last_reviewed: 2026-05-07
source_of_truth: repo
---

# Service Acceptance Template

> **Purpose**: define the operational, security, support, and readiness checks required before a service or release is accepted.
> **Audience**: service owners, operations teams, release managers, and managers making go or no-go decisions.
> **When to update**: update when readiness criteria, sign-off roles, conditional acceptance rules, or release dependencies change.

## How to use this template

Use this template at the point where delivery hands work over to live-service ownership or where a release needs formal operational acceptance. Keep it check-based, evidence-linked, and decision-ready.

- Mandatory: criteria, checks, sign-off roles, and conditional acceptance rules.
- Optional: links to detailed execution evidence if the summary here is sufficient for decision forums.
- Remove checks that are no longer relevant to the service boundary or support model.

## What not to include

- **Product acceptance or UAT scenarios** — user acceptance scenarios belong in the acceptance catalog ([../02_product/05_acceptance_catalog_TEMPLATE.md](../02_product/05_acceptance_catalog_TEMPLATE.md)) and UAT plan ([../05_testing_acceptance/04_uat_plan_TEMPLATE.md](../05_testing_acceptance/04_uat_plan_TEMPLATE.md)). This template covers operational readiness, not product acceptance.
- **Security control design or threat analysis** — control design belongs in the security baseline ([01_security_baseline_TEMPLATE.md](01_security_baseline_TEMPLATE.md)) and threat model ([02_threat_model_TEMPLATE.md](02_threat_model_TEMPLATE.md)). Reference the baseline as evidence here.
- **Runbook content** — operational procedures belong in runbooks ([11_runbook_TEMPLATE.md](11_runbook_TEMPLATE.md)). Check that runbooks exist; do not embed their content.
- **Defect records or open bug lists** — defect tracking belongs in the defect management process ([../05_testing_acceptance/05_defect_management_TEMPLATE.md](../05_testing_acceptance/05_defect_management_TEMPLATE.md)).
- **Delivery status or milestone reports** — delivery progress belongs in the status report ([../07_delivery/04_status_report_TEMPLATE.md](../07_delivery/04_status_report_TEMPLATE.md)) and release plan ([../07_delivery/07_release_plan_TEMPLATE.md](../07_delivery/07_release_plan_TEMPLATE.md)).

## Frontmatter quick reference

This template's typical frontmatter values. The full schema (with all enums and conditional rules) is at [docs/00_operating_model/04_frontmatter_schema.md](../00_operating_model/04_frontmatter_schema.md).

| Field | Typical value here | Notes |
|---|---|---|
| `status` | `draft` → `active` → `superseded` | Use `superseded` when replaced; required `superseded_by:` link |
| `record_class` | `canonical` | This template defines a canonical artifact |
| `audience` | `[internal, manager]` | Add `client` only when client-export-safe |
| `capability` | `operations` | Fixed for this folder |
| `phase` | `execution` | One of `initiation`, `planning`, `execution`, `monitoring`, `closure`, `n/a` |
| `cadence` | `per-release` | One of `ad-hoc`, `weekly`, `monthly`, `per-stage`, `per-release`, `one-shot` |

> When `capability: execution`, both `cadence` and `source_of_truth` are required by the validator.

## Acceptance criteria

State the high-level conditions that must be true before the service can be accepted. These criteria should summarize operational readiness, not reproduce every lower-level task.

- Example: service has named owners, tested support path, approved security posture, and monitored release plan.
- Example: open risks above agreed threshold must be resolved or explicitly accepted.

## Checks (security/operability/observability/runbooks/training)

List the required checks and where the supporting evidence lives. Keep the checks grouped so reviewers can quickly see if a readiness area is incomplete.

- Example: security baseline met, alerts tested, dashboards reviewed, runbooks published, support training completed.
- Example: backup drill evidence, access review completed, and incident contacts confirmed.

## Sign-off roles

Define who recommends acceptance, who approves it, and who records the final state. Separate delivery, operations, security, and business responsibilities when needed.

- Example: release manager recommends, service owner and operations lead approve, PM records the decision.
- Example: security lead signs off only the security control subset.

## Conditional acceptance

Explain how temporary exceptions are handled when the service can launch with bounded residual risk. Conditional acceptance should always include owner, expiry, and follow-up action.

- Example: low-risk documentation gap accepted for 7 days with named owner and completion date.
- Example: performance tuning accepted only for limited pilot traffic, not full-scale rollout.

## Related documents

- [10_runbook_index_TEMPLATE.md](10_runbook_index_TEMPLATE.md) — provides the runbook set that must exist before acceptance.
- [../07_delivery/07_release_plan_TEMPLATE.md](../07_delivery/07_release_plan_TEMPLATE.md) — uses this acceptance outcome during final release approval.
- [03_operating_model_TEMPLATE.md](03_operating_model_TEMPLATE.md) — defines who will own and operate the accepted service.
- [01_security_baseline_TEMPLATE.md](01_security_baseline_TEMPLATE.md) — provides the baseline control expectations checked here.
