---
title: Status Report Template
status: draft
record_class: canonical
audience: [internal, manager]
owner: project-manager
capability: execution
phase: monitoring
cadence: weekly
last_reviewed: 2026-05-07
source_of_truth: repo
---

# Status Report Template

> **Purpose**: provide the recurring management snapshot of progress, health, risks, and immediate decisions needed.
> **Audience**: delivery leads and managers who review weekly execution status and decide on actions.
> **When to update**: update on the agreed weekly cadence and before any governance forum needing current status.

## How to use this template

Use this template for the canonical weekly status view, even if detailed task tracking lives elsewhere. Keep it brief, decision-oriented, and consistent from week to week so trends are easy to read.

- Mandatory: period, RAG summary, progress, risks/issues, decisions needed, and next-period focus.
- Optional: supporting links to detailed metrics or delivery tooling.
- Remove narrative history that no longer affects the current reporting decision.

## What not to include

- **Decisions or change approvals** — formal decisions belong in the change control log ([../00_governance/07_change_control_log_TEMPLATE.md](../00_governance/07_change_control_log_TEMPLATE.md)) or ADRs. The status report flags that a decision is needed; the log records the outcome.
- **Detailed implementation progress or technical narrative** — keep delivery status at a management summary level. Detailed task status belongs in delivery tooling.
- **Full RAID entries** — individual risk and issue detail belongs in the RAID register ([../00_governance/06_raid_register_TEMPLATE.md](../00_governance/06_raid_register_TEMPLATE.md)). The status report summarises the most important changes; the register tracks all items.
- **Test results or defect counts** — raw testing data belongs in the verification evidence index ([../05_testing_acceptance/03_verification_evidence_index_TEMPLATE.md](../05_testing_acceptance/03_verification_evidence_index_TEMPLATE.md)) and defect management process. Surface the trend signal here.
- **Architecture or design rationale** — design decisions belong in ADRs ([../adr/ADR-000-template.md](../adr/ADR-000-template.md)). Reference a decision by ID if it affects the status picture.

## Frontmatter quick reference

This template's typical frontmatter values. The full schema (with all enums and conditional rules) is at [docs/00_operating_model/04_frontmatter_schema.md](../00_operating_model/04_frontmatter_schema.md).

| Field | Typical value here | Notes |
|---|---|---|
| `status` | `draft` → `active` → `superseded` | Use `superseded` when replaced; required `superseded_by:` link |
| `record_class` | `canonical` | This template defines a canonical artifact |
| `audience` | `[internal, manager]` | Add `client` only when client-export-safe |
| `capability` | `execution` | Fixed for this folder |
| `phase` | `monitoring` | One of `initiation`, `planning`, `execution`, `monitoring`, `closure`, `n/a` |
| `cadence` | `weekly` | One of `ad-hoc`, `weekly`, `monthly`, `per-stage`, `per-release`, `one-shot` |

> When `capability: execution`, both `cadence` and `source_of_truth` are required by the validator. Add `source_of_truth: repo` (or the appropriate value) to the frontmatter.

## Period

Record the reporting window, report owner, and any meeting or decision forum the update supports. A clear period marker keeps weekly reports auditable and comparable.

- Example: week ending date, board pack reference, and author role.
- Example: current stage or release wave covered by the report.

## RAG status (scope/schedule/cost/quality)

Provide the headline RAG view and a short explanation for any amber or red area. Keep the logic stable so readers can compare one report to the next without re-learning the scale.

- Example: schedule amber due to an external dependency slip.
- Example: quality red because critical defects exceed release threshold.

## Progress against plan

Summarize what moved forward during the period relative to the delivery or stage plan. Focus on completed outcomes and meaningful variance instead of listing every team activity.

- Example: build completed for workstream A and rehearsal started on plan.
- Example: milestone achieved early or delayed, with the reason noted.

## Risks and issues since last report

Highlight what changed in the control picture and what management attention is needed now. This section should pull from the live RAID register rather than maintain a competing list.

- Example: new high risk raised, issue closed, dependency escalated.
- Example: assumption invalidated and converted into a tracked issue.

## Decisions needed

List the decisions or approvals the project needs before the next reporting cycle. Make each ask explicit so the report drives action rather than passive awareness.

- Example: approve scope trade-off to protect release date.
- Example: confirm additional resourcing or accept stage tolerance breach.

## Next-period focus

State the main objectives for the next reporting period so readers know what good progress looks like next week. This also helps reviewers judge whether the plan remains realistic.

- Example: close top three readiness gaps and complete cutover rehearsal.
- Example: secure change approval and finalize client communications.

## Related documents

- [../00_governance/06_raid_register_TEMPLATE.md](../00_governance/06_raid_register_TEMPLATE.md) — provides the underlying risks, assumptions, issues, and dependencies summarized here.
- [../00_governance/07_change_control_log_TEMPLATE.md](../00_governance/07_change_control_log_TEMPLATE.md) — records formal change decisions referenced by the report.
- [06_readiness_tracker_TEMPLATE.md](06_readiness_tracker_TEMPLATE.md) — shows whether execution and release-readiness domains are trending green.
