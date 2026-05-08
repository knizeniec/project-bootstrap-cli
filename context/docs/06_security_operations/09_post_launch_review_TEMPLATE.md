---
title: Post-Launch Review Template
status: draft
record_class: canonical
audience: [internal, manager]
owner: service-owner
capability: operations
phase: closure
cadence: per-release
last_reviewed: 2026-05-07
source_of_truth: repo
---

# Post-Launch Review Template

> **Purpose**: review early-live outcomes, issues, benefits, and follow-up actions after launch.
> **Audience**: service owners, delivery leads, support teams, and managers assessing whether launch goals were achieved.
> **When to update**: update after launch milestones, early-life support periods, or material incidents that change the review picture.

## How to use this template

Use this template after go-live to capture what actually happened, what it means, and what should change next. Keep it evidence-based and action-oriented rather than retrospective-only.

- Mandatory: outcomes, benefits status, incidents and learnings, defect leakage, team feedback, and actions.
- Optional: trend charts if they materially support the review.
- Remove speculation that is not grounded in observed launch data.

## What not to include

- **Full incident postmortem content** — detailed incident analysis belongs in the postmortem template ([12_incident_postmortem_TEMPLATE.md](12_incident_postmortem_TEMPLATE.md)). Summarise and link to postmortems here; do not reproduce their content.
- **Benefits realization tracking** — detailed post-delivery benefit measurement belongs in the benefits realization plan ([../00_governance/08_benefits_realization_TEMPLATE.md](../00_governance/08_benefits_realization_TEMPLATE.md)). The post-launch review captures the initial benefits picture; the realization plan tracks it over time.
- **Ongoing defect management** — live defect status belongs in the defect management process ([../05_testing_acceptance/05_defect_management_TEMPLATE.md](../05_testing_acceptance/05_defect_management_TEMPLATE.md)). This review captures what escaped and what pattern it reveals.
- **Status report content or delivery metrics** — delivery-phase tracking belongs in the status report ([../07_delivery/04_status_report_TEMPLATE.md](../07_delivery/04_status_report_TEMPLATE.md)). This review is a post-go-live retrospective, not a delivery status update.
- **Future feature requirements** — product ideas arising from feedback belong in the PRD ([../02_product/01_prd_TEMPLATE.md](../02_product/01_prd_TEMPLATE.md)) and backlog, not embedded in the launch review.

## Frontmatter quick reference

This template's typical frontmatter values. The full schema (with all enums and conditional rules) is at [docs/00_operating_model/04_frontmatter_schema.md](../00_operating_model/04_frontmatter_schema.md).

| Field | Typical value here | Notes |
|---|---|---|
| `status` | `draft` → `active` → `superseded` | Use `superseded` when replaced; required `superseded_by:` link |
| `record_class` | `canonical` | This template defines a canonical artifact |
| `audience` | `[internal, manager]` | Add `client` only when client-export-safe |
| `capability` | `operations` | Fixed for this folder |
| `phase` | `closure` | One of `initiation`, `planning`, `execution`, `monitoring`, `closure`, `n/a` |
| `cadence` | `per-release` | One of `ad-hoc`, `weekly`, `monthly`, `per-stage`, `per-release`, `one-shot` |

> When `capability: execution`, both `cadence` and `source_of_truth` are required by the validator.

## Outcomes vs goals

Compare actual launch outcomes against the goals, success measures, or release criteria that justified the launch. Focus on what changed in reality.

- Example: adoption target met in pilot cohort but response-time target missed during peak window.
- Example: release completed on schedule with one deferred non-critical capability.

## Benefits status

State whether the expected business or operational benefits are emerging, delayed, or disproven. This section should connect launch outcomes to the original value case.

- Example: support volume reduced as expected after self-service flow shipped.
- Example: cost-saving benefit delayed because manual fallback remained active.

## Incidents and learnings

Summarize notable incidents, near misses, or support escalations and the main lessons from them. Link formal incident review records rather than repeating all details.

- Example: onboarding latency incident exposed missing queue-capacity alert.
- Example: security near miss led to tighter access-review checks before the next release.

## Defect leakage

Describe which defects escaped into early live use and what patterns they reveal. Keep the focus on prevention and risk reduction, not blame.

- Example: two medium-severity usability defects escaped because UAT scenario coverage was too narrow.
- Example: recurring integration defects indicate missing contract-test depth.

## Team feedback

Capture structured feedback from delivery, support, operations, security, and product participants. The goal is to improve the system of work, not just the service itself.

- Example: support requested clearer rollback guidance and richer dashboard naming.
- Example: delivery team reported that acceptance evidence collection started too late.

## Actions

List the concrete follow-up actions, owners, and due dates created from the review. Actions should be small enough to track and specific enough to verify.

- Example: add restore drill evidence to service acceptance checklist by next release.
- Example: expand acceptance catalog scenario coverage for exception-handling paths.

## Related documents

- [06_incident_response_TEMPLATE.md](06_incident_response_TEMPLATE.md) — provides the incident lifecycle and formal review trigger referenced here.
- [08_service_acceptance_TEMPLATE.md](08_service_acceptance_TEMPLATE.md) — shows the readiness assumptions that should be checked against actual launch outcomes.
- [../07_delivery/07_release_plan_TEMPLATE.md](../07_delivery/07_release_plan_TEMPLATE.md) — defines the release goals and success criteria reviewed here.
- [../05_testing_acceptance/05_defect_management_TEMPLATE.md](../05_testing_acceptance/05_defect_management_TEMPLATE.md) — supports escaped-defect analysis and follow-up.
