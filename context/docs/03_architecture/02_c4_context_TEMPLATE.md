---
title: C4 Context Template
status: draft
record_class: canonical
audience: [internal]
owner: architecture-maintainer
capability: architecture
phase: planning
cadence: per-stage
last_reviewed: 2026-05-07
---

# C4 Context Template

> **Purpose**: capture the system context view: people, external systems, system boundary, and key interactions.
> **Audience**: architects, engineers, product stakeholders, and reviewers aligning on scope.
> **When to update**: update when system boundaries, actors, or major integrations change.

## How to use this template

- Use one context view per system or major solution boundary.
- Keep names aligned with the solution design and ADR terminology.
- Include a diagram placeholder plus short text that explains the critical interactions.

## What not to include

- **Container or component-level detail** — internal structure belongs in the container view ([03_c4_container_TEMPLATE.md](03_c4_container_TEMPLATE.md)) and component view ([04_c4_component_TEMPLATE.md](04_c4_component_TEMPLATE.md)). The context view shows the boundary, not what is inside.
- **Interface contracts or API specifications** — exact interface detail belongs in the interface control document ([06_interface_control_document_TEMPLATE.md](06_interface_control_document_TEMPLATE.md)).
- **Architecture decision rationale** — binding decisions belong in ADRs ([../adr/ADR-000-template.md](../adr/ADR-000-template.md)); link them here rather than embedding the rationale.
- **Deployment or infrastructure topology** — environment and hosting details belong in the deployment template ([07_deployment_TEMPLATE.md](07_deployment_TEMPLATE.md)).
- **Data model or entity relationship diagrams** — data structure belongs in the data design ([05_data_design_TEMPLATE.md](05_data_design_TEMPLATE.md)).

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

## System context overview

Describe what the system is, why it exists, and what sits outside the boundary.

- Link the parent design in [01_solution_design_TEMPLATE.md](01_solution_design_TEMPLATE.md).

## People

List the human actors, roles, or operational personas that interact with the system.

- Example: end user, support analyst, partner operator.

## External systems

List the systems outside the boundary that exchange data, events, or control signals.

- Note owner, trust level, and interaction purpose.

## System in scope

Name the system being designed and summarize its primary responsibilities.

- Keep the wording consistent with the solution design and container view.

## Key interactions

Summarize the most important inbound and outbound interactions.

- Example: user submits request through the web app.
- Example: system publishes domain events to an external platform.

## Diagram placeholder

Add a PlantUML or Mermaid context diagram and keep the text version readable without it.

```text
[Person] -> [System in Scope] -> [External System]
```

## Related documents

- [01_solution_design_TEMPLATE.md](01_solution_design_TEMPLATE.md) — explains goals, scope, and architectural strategy.
- [03_c4_container_TEMPLATE.md](03_c4_container_TEMPLATE.md) — expands the system into deployable containers.
- [../adr/INDEX.md](../adr/INDEX.md) — links context-shaping decisions to accepted ADRs.
