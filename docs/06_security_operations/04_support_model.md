---
title: Documentation Development Tool - Support Model
status: active
record_class: canonical
audience: [internal, manager]
owner: operations-lead
capability: operations
phase: execution
cadence: monthly
last_reviewed: 2026-05-08
source_of_truth: repo
---

## Support tiers

- L1: intake, known-issue handling, user guidance.
- L2: workflow and environment diagnostics, evidence collection.
- L3: engineering escalation for code-level defects and hotfix decisions.

## Support coverage model

- Business-hours primary coverage for normal support traffic.
- On-call coverage for Sev-1 incidents outside business hours.
- Single-operator fallback allowed for small team periods, with engineering backup on request.

## Intake channels

- Primary: issue tracker queue for bugs and support requests.
- Urgent: incident channel for service-impacting failures.
- Documentation requests: backlog label for quality and template issues.

Ticket intake policy:

- Every ticket must include severity, affected workflow, and reproduction notes.
- Missing details are routed back to requester before triage completion.
- Duplicate tickets are linked to a canonical issue.

## SLA targets

- Sev-1: acknowledge in 15 minutes, active response immediately.
- Sev-2: acknowledge in 1 business hour, mitigation plan in same day.
- Normal requests: acknowledge within 1 business day.

Resolution targets (guidance):

- Sev-1: mitigation in under 2 hours.
- Sev-2: mitigation or clear workaround in 1 business day.
- Normal: resolution in 5 business days.

## Knowledge base

- Canonical operating docs are maintained in [../README.md](../README.md).
- Troubleshooting notes are linked from issue tracker tickets.
- Repeated issues require a reusable resolution note in the docs tree.

Knowledge base maintenance rule:

- If an issue repeats twice in 30 days, add or update a troubleshooting note.

## Handover from delivery

Delivery to support handover requires:

- Approved release plan and readiness tracker updates.
- Test evidence links for core workflows.
- Known issues register with owner and mitigation status.

Handover sign-off checklist:

- Contact list for release, operations, and engineering roles is current ([../evidence/rc1/support-contact-roster.md](../evidence/rc1/support-contact-roster.md)).
- Support escalation path has been rehearsed once ([../evidence/rc1/incident-drill-2026-05-08.md](../evidence/rc1/incident-drill-2026-05-08.md)).
- Incident response template is linked from support runbook entry.

## Escalation path

1. L1 triages and routes.
2. L2 investigates and mitigates.
3. L3 escalates to engineering lead and release manager if service risk remains.

Escalation timing:

- L1 to L2 within 30 minutes for Sev-1, 2 hours for Sev-2.
- L2 to L3 immediately when customer-impacting workaround is unavailable.

## Ownership matrix

| Area | Primary owner | Backup owner |
| --- | --- | --- |
| Ticket triage | Operations lead | QA lead |
| Incident communication | Incident manager | Release manager |
| Hotfix decision support | Engineering lead | Product owner |
| Knowledge base updates | Operations lead | Documentation owner |

## Related documents

- [06_incident_response.md](06_incident_response.md)
- [10_runbook_index.md](10_runbook_index.md)
- [../07_delivery/07_release_plan.md](../07_delivery/07_release_plan.md)
- [../07_delivery/06_readiness_tracker.md](../07_delivery/06_readiness_tracker.md)
