---
title: Data Design Template
status: draft
record_class: canonical
audience: [internal]
owner: architecture-maintainer
capability: architecture
phase: planning
cadence: per-stage
last_reviewed: 2026-05-07
---

# Data Design Template

> **Purpose**: capture the domain model, storage choices, lifecycle rules, and migration approach for solution data.
> **Audience**: architects, engineers, DBAs, and reviewers responsible for data integrity and evolution.
> **When to update**: update when core entities, storage direction, retention policy, or migration strategy changes.

## How to use this template

- Use this document for solution-level data structure, not low-level schema dumps.
- Keep domain concepts and lifecycle rules readable before listing storage specifics.
- Link storage-affecting decisions to ADRs.

## What not to include

- **Full DDL scripts or raw schema exports** — database migration scripts and DDL belong in version-controlled migration files in the repository, not in this canonical document.
- **Operational backup or restore procedures** — these belong in the backup and recovery template ([../06_security_operations/07_backup_and_recovery_TEMPLATE.md](../06_security_operations/07_backup_and_recovery_TEMPLATE.md)).
- **Access control rules for data** — identity and access management belongs in the access control template ([../06_security_operations/05_access_control_TEMPLATE.md](../06_security_operations/05_access_control_TEMPLATE.md)) and security baseline ([../06_security_operations/01_security_baseline_TEMPLATE.md](../06_security_operations/01_security_baseline_TEMPLATE.md)).
- **Architecture decision rationale** — binding decisions belong in ADRs ([../adr/ADR-000-template.md](../adr/ADR-000-template.md)); link the relevant ADR for each significant storage choice.
- **API or interface contracts** — exact interface schemas belong in the interface control document ([06_interface_control_document_TEMPLATE.md](06_interface_control_document_TEMPLATE.md)).

## Frontmatter quick reference

This template's typical frontmatter values. The full schema (with all enums and conditional rules) is at [docs/00_operating_model/04_frontmatter_schema.md](../00_operating_model/04_frontmatter_schema.md).

| Field | Typical value here | Notes |
|---|---|---|
| `status` | `draft` → `active` → `superseded` | Use `superseded` when replaced; required `superseded_by:` link |
| `record_class` | `canonical` | This template defines a canonical artifact |
| `audience` | `[internal]` | Add `manager` only when needed for reviews |
| `capability` | `architecture` | Fixed for this folder |
| `phase` | `planning` | One of `initiation`, `planning`, `execution`, `monitoring`, `closure`, `n/a` |
| `cadence` | `per-stage` | One of `ad-hoc`, `weekly`, `monthly`, `per-stage`, `per-release`, `one-shot` |

> When `capability: execution`, both `cadence` and `source_of_truth` are required by the validator.

## Domain model

Describe the main business concepts and how they relate.

- Example: account, order, policy, event, audit record.

## Entities and relationships

List the most important entities, keys, and relationship patterns.

- Call out ownership, cardinality, and referential rules.

## Storage choices

Explain which stores are used and why.

- Example: relational store for transactional integrity, object storage for large artifacts.

## Data lifecycle

Define creation, retention, archival, deletion, and recovery expectations.

- Include privacy, legal hold, and restoration considerations where relevant.

## Migration strategy

Summarize how data structure changes are introduced safely.

- Example: additive change first, dual-read period, backfill, then cleanup.

## Related documents

- [01_solution_design_TEMPLATE.md](01_solution_design_TEMPLATE.md) — parent architecture baseline.
- [07_deployment_TEMPLATE.md](07_deployment_TEMPLATE.md) — environment and infrastructure implications.
- [../adr/INDEX.md](../adr/INDEX.md) — durable decisions affecting data structure and storage.
