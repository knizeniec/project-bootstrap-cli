---
title: Incident Postmortem Template
status: draft
record_class: canonical
audience: [internal, manager]
owner: incident-manager
capability: operations
phase: closure
cadence: ad-hoc
last_reviewed: 2026-05-07
---

# Incident Postmortem Template

> **Purpose:** provide the standard structure for a blameless postmortem — a written record of what happened during a specific incident, why it happened, and what actions the team will take to prevent or detect similar incidents in the future.
> **Audience:** the incident response team, operations leadership, and managers who need to review the incident, approve action items, and track service improvement.
> **When to update:** complete the postmortem within five working days of the incident being resolved. Review it with all involved parties before publishing. Update the action items section as items are completed.

## How to use this template

Copy this file and rename it: `12_postmortem_<YYYY-MM-DD>_<short-incident-title>.md`. Place it in the `docs/06_security_operations/` directory or a project-specific equivalent, and register it in the lessons-learned index.

- **This postmortem is blameless.** The goal is to understand system and process failure — not to assign personal fault. Refer to roles, not names, everywhere in the document except the sign-off section. If you find yourself writing "Alice forgot to..." stop and rewrite as "the on-call engineer did not have access to..." or "the process did not include a step to...".
- Mandatory: Incident metadata, Timeline, Detection, Root cause, Action items, and Sign-off.
- Optional: Customer communication (if no customer impact occurred, state that explicitly rather than leaving the section blank).
- Remove all example bullets and placeholder text before publishing.

## What not to include

- **Names of people in a blame context** — use roles ("on-call engineer", "deployment owner", "database team lead"). Names appear only in the sign-off section.
- **Speculation without evidence** — every causal claim in the Root cause section must be backed by a log entry, metric, or observable event in the Timeline. Write "We do not yet know why X occurred — this is a target for the investigation action item" rather than guessing.
- **Status updates about the incident while it is ongoing** — these belong in the incident channel and status report template (`docs/07_delivery/04_status_report_TEMPLATE.md`). The postmortem is a post-event artifact, not a live update.
- **Implementation of the action items** — track action item work in the backlog or issue tracker, not in this document. This document records the decision to act; the tracker records the work.

## Frontmatter quick reference

This template's typical frontmatter values. The full schema (with all enums and conditional rules) is at [docs/00_operating_model/04_frontmatter_schema.md](../00_operating_model/04_frontmatter_schema.md).

| Field | Typical value here | Notes |
|---|---|---|
| `status` | `draft` → `active` → `superseded` | Use `superseded` when replaced; required `superseded_by:` link |
| `record_class` | `canonical` | This template defines a canonical artifact |
| `audience` | `[internal, manager]` | Add `client` only when client-export-safe |
| `capability` | `operations` | Fixed for this folder |
| `phase` | `closure` | One of `initiation`, `planning`, `execution`, `monitoring`, `closure`, `n/a` |
| `cadence` | `ad-hoc` | One of `ad-hoc`, `weekly`, `monthly`, `per-stage`, `per-release`, `one-shot` |

> When `capability: execution`, both `cadence` and `source_of_truth` are required by the validator.

---

## Incident metadata

Record the facts needed to classify, find, and cross-reference this postmortem. Fill every field — incomplete metadata makes postmortems hard to find and trend over time.

| Field | Value |
|---|---|
| Incident ID | INC-NNNN or equivalent |
| Date | YYYY-MM-DD |
| Start time (UTC) | HH:MM |
| End time (UTC) | HH:MM |
| Total duration | Xh Ym |
| Severity | Sev-1 / Sev-2 / Sev-3 (as defined in incident response policy) |
| Services affected | [list services or components] |
| Customer impact | [Brief description, or "No direct customer impact"] |
| On-call at time of incident | [Role, not name] |
| Incident commander | [Role] |
| Postmortem author | [Role] |

## Timeline

Present a chronological record of the incident from the first anomaly to full resolution. Write in UTC. Include every significant event: when the anomaly began, when alerts fired, when the team was paged, what actions were taken and when, and when the incident was resolved.

Each entry should state the time, what happened or was observed, and who or what system was involved (by role or system name). Do not editorialize in the timeline — save analysis for later sections.

| Time (UTC) | Event |
|---|---|
| HH:MM | [What happened — observable fact, not interpretation] |
| HH:MM | [Alert fired / page sent to on-call role] |
| HH:MM | [Incident declared at Sev-N by incident commander role] |
| HH:MM | [Action taken by role — e.g., "Database team lead restarted replica"] |
| HH:MM | [Service restored to healthy state — confirmed by metric / log] |
| HH:MM | [Incident closed by incident commander] |

## Detection

Describe how the incident was discovered and quantify the detection lag. A well-run detection section identifies whether the alerting system worked as intended or whether the incident was found by a human, a customer, or luck.

Answer: who or what detected the incident, how long after the anomaly started it was detected, and what the detection method was. If detection was later than it should have been, note that here — it drives action items.

- Example: "The incident was detected by the Datadog monitor `api-error-rate-5xx` firing at 14:37 UTC. The underlying anomaly began at 14:22 UTC. Detection lag: 15 minutes. This is within the target SLO threshold."
- Example: "The incident was reported by a customer at 09:15 UTC. No alert fired. The monitoring gap that allowed this is recorded in Action item AI-003."

## Response

Summarize the actions taken by the response team from initial page to resolution. Describe what worked well and what slowed the response. This section is analytical — it connects the timeline to what the team learned about the response process.

Keep the description role-based. Avoid reconstructing blame. Focus on process, tooling, and communication effectiveness.

- Example: "The on-call engineer confirmed the database replica failure within three minutes of the page using the standard runbook. The runbook's failover steps were accurate and completed without deviation."
- Example: "The rollback decision took 40 minutes to reach because the approving manager was not reachable via the primary escalation path. The backup escalation contact was not up to date. See Action item AI-002."

## Root cause

State the technical root cause and contributing factors. The root cause is the specific, confirmable failure that directly caused the incident. Contributing factors are conditions that made the root cause possible, worse, or harder to detect.

Use the "five whys" technique or a similar causal chain to get below surface symptoms. Do not stop at the first proximate cause. Every causal claim must be backed by evidence from the Timeline or an observable artifact.

**Technical root cause:** [Specific, confirmable statement of what failed and why.]

**Contributing factors:**
- [Factor 1 — e.g., "Monitoring threshold was set too high to catch gradual degradation."]
- [Factor 2 — e.g., "The deployment process did not include a smoke test for this service path."]
- [Factor 3 — e.g., "The runbook step for replica promotion had not been tested in production for 6 months."]

## Resolution

Describe what was done to restore normal service and what the final state of the system was at incident closure. Keep this factual and traceable to the Timeline.

- Example: "The primary database was restored from the 03:00 UTC snapshot. Data between 03:00 and 14:22 UTC was replayed from the audit log. Service returned to `healthy` state at 16:45 UTC. No data loss confirmed by reconciliation check."
- Example: "The degraded pod was evicted and rescheduled to a healthy node. No data was affected. The root cause (memory leak) was not fixed in this response — see Action item AI-001 for the permanent fix."

## Customer communication

Record what customers were told, when, and through which channel. If no external communication was sent, state that explicitly and confirm it was a deliberate decision.

- Example: "A status page incident was posted at 14:45 UTC noting degraded API performance. An update was posted at 15:30 UTC confirming partial restoration. A resolution notice was posted at 16:50 UTC."
- Example: "No customer-facing communication was sent. The incident did not cause visible service degradation and the decision was made by the incident commander not to post a status update."

## What went well

Record the specific things that worked during the incident response. This section reinforces good practices so the team continues them intentionally. Be specific — "good communication" is too vague; "the incident commander posted an update every 20 minutes, which kept the team aligned" is useful.

- Example: "The on-call runbook was accurate and the failover completed in under 10 minutes."
- Example: "The incident commander opened a dedicated incident channel immediately, which prevented noise in the main ops channel."
- Example: "The database team lead was reachable within two minutes and immediately took ownership of the restoration steps."

## What did not go well

Record the specific things that slowed, complicated, or worsened the incident response. Keep this section blameless — focus on process gaps, tooling failures, and unclear procedures, not on individual mistakes.

- Example: "The alert fired 15 minutes after the anomaly started because the threshold was calibrated for peak load, not off-peak patterns."
- Example: "The runbook for replica promotion had a step referencing a deprecated CLI tool. The operator lost five minutes finding the correct command."
- Example: "There was no escalation path documented for the approving manager being unavailable."

## Where we got lucky

Record the factors that reduced the severity of the incident but that the team cannot rely on in the future. This section surfaces hidden fragility.

- Example: "The incident occurred at 14:30 UTC on a Tuesday, when traffic is 40% below peak. The same failure at 18:00 UTC on a Friday would have affected ten times as many users."
- Example: "The on-call engineer happened to have run this specific recovery procedure in the staging environment the previous week. Without that, the recovery would have taken significantly longer."

## Action items

List every concrete improvement the team commits to as a result of this postmortem. Each action item must have an owner, a due date, a ticket link, and a priority label. Unowned or undated action items are not action items — they are wishes.

| ID | Action | Owner (role) | Due date | Ticket | Priority |
|---|---|---|---|---|---|
| AI-001 | [Specific, implementable action] | [Role] | YYYY-MM-DD | [Link] | P1 / P2 / P3 |
| AI-002 | [Specific action] | [Role] | YYYY-MM-DD | [Link] | P1 / P2 / P3 |
| AI-003 | [Specific action] | [Role] | YYYY-MM-DD | [Link] | P1 / P2 / P3 |

**Priority definitions:**
- P1: Fix within one week. Prevents recurrence of a Sev-1 or removes a critical detection gap.
- P2: Fix within one sprint. Reduces risk or improves detection.
- P3: Fix within one quarter. Improves reliability or operational efficiency.

## Sign-off

Record who reviewed and approved this postmortem. Sign-off confirms that the facts are accurate, the root cause is sound, and the action items are committed and assigned.

| Role | Name | Date |
|---|---|---|
| Incident commander | | YYYY-MM-DD |
| Operations lead | | YYYY-MM-DD |
| Engineering lead | | YYYY-MM-DD |
| Manager / approver | | YYYY-MM-DD |

## Related documents

- [06_incident_response_TEMPLATE.md](06_incident_response_TEMPLATE.md) — the incident response lifecycle and severity model that this postmortem feeds back into.
- [../08_references/05_lessons_learned_index_TEMPLATE.md](../08_references/05_lessons_learned_index_TEMPLATE.md) — the cross-project lessons-learned index where this postmortem should be registered.
- [10_runbook_index_TEMPLATE.md](10_runbook_index_TEMPLATE.md) — the runbook index; action items from this postmortem may update or add runbooks.
