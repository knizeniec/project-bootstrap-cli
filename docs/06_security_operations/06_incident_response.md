---
title: Documentation Development Tool - Incident Response
status: active
record_class: canonical
audience: [internal, manager]
owner: incident-manager
capability: operations
phase: execution
cadence: ad-hoc
last_reviewed: 2026-05-08
source_of_truth: repo
---

<!-- markdownlint-disable MD013 -->

## Severity classes

- Sev-1: complete outage, major data risk, or blocking failure of core workflow.
- Sev-2: major degradation with workaround.
- Sev-3: localized issue with limited user impact.

## Response time targets

| Severity | Acknowledge | Commander assigned | Stakeholder update cadence |
| --- | --- | --- | --- |
| Sev-1 | 15 minutes | 15 minutes | Every 30 minutes |
| Sev-2 | 1 hour | 1 hour | Every 2 hours |
| Sev-3 | 1 business day | as needed | Daily while active |

## Lifecycle

1. Declare: assign incident commander and severity.
2. Contain: stabilize service and prevent spread.
3. Eradicate: identify and remove root technical cause.
4. Recover: restore normal operation and verify health checks.
5. Learn: record follow-up actions and prevention tasks.

Operational checklist for active incidents:

1. Open incident channel and assign commander.
2. Confirm severity and impacted workflows.
3. Start incident timeline log.
4. Apply containment action and confirm effect.
5. Decide rollback vs forward-fix path.
6. Verify recovery with smoke checks.
7. Close incident and create follow-up actions.

## Roles

- Incident commander: single decision authority during an active incident.
- Incident manager: preparedness owner outside active incidents (runbooks, drills, staffing checks).
- Operations lead: service state and communications cadence.
- Engineering lead: root-cause remediation and patch guidance.
- Release manager: go/no-go decision for recovery releases.

Authority rule:

- During active incidents, the incident commander is the only decision authority for containment and recovery steps.
- The incident manager supports coordination but does not override active-incident decisions.

## Communications

- Internal updates every 30 minutes for active Sev-1 incidents.
- Stakeholder summary at declaration, major state changes, and closure.
- Post-incident summary with timeline, impact, and action items.

Minimum update format:

- Current severity and impact.
- What changed since last update.
- Next action and owner.
- Next update time.

## Post-incident review

Formal review required for all Sev-1 incidents and repeated Sev-2 patterns.

Review outputs:

- Root cause summary.
- Corrective actions with owners and due dates.
- Preventive backlog items linked to product and delivery docs.

Review SLA:

- Sev-1: review completed within 5 working days.
- Sev-2 repeat pattern: review completed within 10 working days.

## Drill approach

- Run one tabletop drill per release cycle using a realistic Sev-1 scenario.
- Capture gaps and add corrective actions to the delivery backlog.
- Update this response doc when drill outcomes reveal process weaknesses.

## Drill schedule tracking

- Last drill date: TBD
- Next drill due: TBD (before next go/no-go checkpoint)

## Related documents

- [04_support_model.md](04_support_model.md)
- [../07_delivery/07_release_plan.md](../07_delivery/07_release_plan.md)
- [../07_delivery/06_readiness_tracker.md](../07_delivery/06_readiness_tracker.md)

<!-- markdownlint-enable MD013 -->
