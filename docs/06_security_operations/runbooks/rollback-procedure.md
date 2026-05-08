---
title: Runbook - Rollback Procedure
status: active
record_class: supporting
audience: [internal]
owner: release-manager
capability: operations
phase: execution
cadence: monthly
last_reviewed: 2026-05-08
source_of_truth: repo
---

## Trigger

Use when release health meets rollback trigger criteria from [../06_incident_response.md](../06_incident_response.md) and [../../07_delivery/07_release_plan.md](../../07_delivery/07_release_plan.md).

## Steps

1. Incident commander confirms rollback decision in channel.
2. Release manager identifies last known stable revision.
3. Revert release artifacts to prior stable set.
4. Run smoke checks for core flows (Q&A, assessment, generation, review).
5. Publish rollback completion update and next action.

## Exit criteria

- Stable revision restored.
- Smoke checks pass.
- Recovery path owner assigned.
