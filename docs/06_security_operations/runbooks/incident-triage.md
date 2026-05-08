---
title: Runbook - Incident Triage
status: active
record_class: supporting
audience: [internal]
owner: incident-manager
capability: operations
phase: execution
cadence: monthly
last_reviewed: 2026-05-08
source_of_truth: repo
---

## Trigger

Use when Sev-1 or Sev-2 service-impacting issue is reported.

## Steps

1. Assign incident commander and open incident channel.
2. Confirm severity using [../06_incident_response.md](../06_incident_response.md).
3. Capture impacted workflow and current user impact.
4. Start timeline log with UTC timestamps.
5. Decide contain-now action and announce next update time.

## Exit criteria

- Incident is handed to containment/recovery workflow with clear owner.
- Stakeholder update cadence is active.
