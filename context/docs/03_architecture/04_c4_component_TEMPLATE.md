---
title: C4 Component Template
status: draft
record_class: canonical
audience: [internal]
owner: architecture-maintainer
capability: architecture
phase: planning
cadence: per-stage
last_reviewed: 2026-05-07
---

# C4 Component Template

> **Purpose**: describe the internal components inside a key container and how they collaborate.
> **Audience**: engineers and reviewers working inside a specific container boundary.
> **When to update**: update when a container's internal structure or responsibilities materially change.

## How to use this template

- Create one component view per important container.
- Keep components responsibility-based, not file-list-based.
- Link durable design rules to ADRs instead of duplicating rationale here.

## What not to include

- **File or directory listings** — the component view shows responsibility boundaries, not file-system structure. Code-level detail belongs in the codebase.
- **Cross-container communication paths** — these belong in the container view ([03_c4_container_TEMPLATE.md](03_c4_container_TEMPLATE.md)) and interface control documents ([06_interface_control_document_TEMPLATE.md](06_interface_control_document_TEMPLATE.md)).
- **Full API or data schemas** — exact schemas belong in the interface control document or data design ([05_data_design_TEMPLATE.md](05_data_design_TEMPLATE.md)).
- **Architecture decision rationale** — binding decisions belong in ADRs ([../adr/ADR-000-template.md](../adr/ADR-000-template.md)); link them rather than duplicating the reasoning.
- **Runbook procedures or operational steps** — operational procedures belong in runbooks ([../06_security_operations/11_runbook_TEMPLATE.md](../06_security_operations/11_runbook_TEMPLATE.md)).

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

## Container in scope

Name the parent container and link back to the container view.

- Example: API service, web application, ingestion worker.

## Components

List the main components inside the container.

- Example: API layer, domain service, repository, adapter, scheduler.

## Responsibilities

State what each component owns and the boundary between components.

- Prefer clear verbs and nouns over implementation detail.

## Collaborators

Describe the main dependencies between components and any external collaborators.

- Note direction, protocol, and important invariants.

## Technology

Record the key frameworks, libraries, or runtime patterns that matter for the component view.

- Example: framework layer, ORM boundary, event bus adapter.

## Diagram placeholder

Add a PlantUML or Mermaid component diagram.

```text
[API Layer] -> [Domain Service] -> [Repository]
```

## Related documents

- [03_c4_container_TEMPLATE.md](03_c4_container_TEMPLATE.md) — parent container structure.
- [06_interface_control_document_TEMPLATE.md](06_interface_control_document_TEMPLATE.md) — external or cross-boundary contracts.
- [../adr/INDEX.md](../adr/INDEX.md) — decisions that constrain internal decomposition.
