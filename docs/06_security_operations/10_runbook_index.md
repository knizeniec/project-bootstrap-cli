---
title: Documentation Development Tool - Runbook Index
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

## Purpose

This index provides the minimum live runbooks required for RC1 operations readiness.

## Runbooks

| Runbook | Scope | Owner | Last reviewed |
| --- | --- | --- | --- |
| [runbooks/incident-triage.md](runbooks/incident-triage.md) | First 60 minutes of incident response | Incident manager | 2026-05-08 |
| [runbooks/rollback-procedure.md](runbooks/rollback-procedure.md) | Controlled rollback during release incidents | Release manager | 2026-05-08 |
| [runbooks/restore-procedure.md](runbooks/restore-procedure.md) | Restore docs baseline and validate service state | Operations lead | 2026-05-08 |

## Usage rule

Ops readiness cannot be Green unless all runbooks above exist, are linked from readiness gating, and have an owner.

## Related documents

- [04_support_model.md](04_support_model.md)
- [06_incident_response.md](06_incident_response.md)
- [../07_delivery/06_readiness_tracker.md](../07_delivery/06_readiness_tracker.md)
