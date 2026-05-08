---
title: RC1 Evidence - Incident Drill
status: active
record_class: supporting
audience: [internal, manager]
owner: incident-manager
capability: execution
phase: monitoring
cadence: per-release
last_reviewed: 2026-05-08
source_of_truth: repo
---

## Drill ID

RC1-DRILL-001

## Date

2026-05-08

## Participants

Incident manager, operations lead, release manager, QA lead

## Scenario

Sev-1 outage in generation workflow during release window.

## Outcome

- Commander assignment and escalation flow completed within target times.
- Rollback decision path validated against release plan.
- Follow-up: add explicit runbook index gating link in readiness tracker.

## Timing measurements

| Metric | Target | Actual |
| --- | --- | --- |
| Commander assigned | <= 15 minutes | 8 minutes |
| First stakeholder update | <= 30 minutes | 22 minutes |
| Rollback decision checkpoint | <= 20 minutes from Sev-1 confirmation | 14 minutes |

Timeline reference:

- 14:00 UTC incident declared (simulation)
- 14:08 UTC commander assigned
- 14:22 UTC first stakeholder update sent
- 14:14 UTC rollback decision checkpoint completed

## Evidence link

- [incident-drill-timeline.md](incident-drill-timeline.md)
- [artifacts/RC1-REL-REHEARSAL-2026-05-08.md](artifacts/RC1-REL-REHEARSAL-2026-05-08.md)

## Result

Pass
