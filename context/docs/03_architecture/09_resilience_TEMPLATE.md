---
title: Resilience Template
status: draft
record_class: canonical
audience: [internal]
owner: architecture-maintainer
capability: architecture
phase: execution
cadence: per-stage
last_reviewed: 2026-05-07
---

# Resilience Template

> **Purpose**: capture the expected failure modes, redundancy model, degradation policy, and validation approach for resilience.
> **Audience**: architects, engineers, operators, and reviewers focused on continuity and recovery.
> **When to update**: update when failure assumptions, redundancy patterns, or recovery expectations change.

## How to use this template

- Focus on the failures that materially affect users or operations.
- Describe what the system should do under stress, dependency loss, or partial outage.
- Link detailed procedures to runbooks instead of duplicating them here.

## What not to include

- **Step-by-step recovery procedures** — detailed recovery steps belong in runbooks ([../06_security_operations/11_runbook_TEMPLATE.md](../06_security_operations/11_runbook_TEMPLATE.md)). This document describes the resilience model; runbooks execute it.
- **Backup schedules or restore targets** — backup and recovery policy belongs in the backup and recovery template ([../06_security_operations/07_backup_and_recovery_TEMPLATE.md](../06_security_operations/07_backup_and_recovery_TEMPLATE.md)).
- **Observability configuration** — telemetry and alerting setup belongs in the observability template ([08_observability_TEMPLATE.md](08_observability_TEMPLATE.md)). Reference the alerts that signal failure here, but define them there.
- **Architecture decision rationale** — binding decisions on redundancy or failover patterns belong in ADRs ([../adr/ADR-000-template.md](../adr/ADR-000-template.md)).
- **Incident postmortem narratives** — incident learning records belong in the postmortem template ([../06_security_operations/12_incident_postmortem_TEMPLATE.md](../06_security_operations/12_incident_postmortem_TEMPLATE.md)).

## Frontmatter quick reference

This template's typical frontmatter values. The full schema (with all enums and conditional rules) is at [docs/00_operating_model/04_frontmatter_schema.md](../00_operating_model/04_frontmatter_schema.md).

| Field | Typical value here | Notes |
|---|---|---|
| `status` | `draft` → `active` → `superseded` | Use `superseded` when replaced; required `superseded_by:` link |
| `record_class` | `canonical` | This template defines a canonical artifact |
| `audience` | `[internal]` | Add `manager` only when needed for reviews |
| `capability` | `architecture` | Fixed for this folder |
| `phase` | `execution` | One of `initiation`, `planning`, `execution`, `monitoring`, `closure`, `n/a` |
| `cadence` | `per-stage` | One of `ad-hoc`, `weekly`, `monthly`, `per-stage`, `per-release`, `one-shot` |

> When `capability: execution`, both `cadence` and `source_of_truth` are required by the validator.

## Failure modes

List the most important component, dependency, data, and human-process failures.

- Example: database unavailable, queue backlog growth, third-party timeout, bad deployment.

## Redundancy

Describe the redundancy model for critical compute, data, and connectivity paths.

- Note single points of failure that are accepted or still unresolved.

## Failover

Explain automated and manual failover expectations.

- Include triggers, owner, and recovery target where relevant.

## Degradation policy

State what features may degrade, what must remain available, and what must fail closed.

- Example: read-only mode during write-path recovery.

## Chaos testing approach

Describe how resilience assumptions are tested.

- Example: game days, fault injection, dependency throttling, restore drills.

## Related documents

- [07_deployment_TEMPLATE.md](07_deployment_TEMPLATE.md) — topology and environment dependencies.
- [08_observability_TEMPLATE.md](08_observability_TEMPLATE.md) — telemetry used to detect and confirm resilience behavior.
- [../06_security_operations/10_runbook_index_TEMPLATE.md](../06_security_operations/10_runbook_index_TEMPLATE.md) — operational response procedures.
