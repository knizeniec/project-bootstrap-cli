---
title: C4 Container Template
status: draft
record_class: canonical
audience: [internal]
owner: architecture-maintainer
capability: architecture
phase: planning
cadence: per-stage
last_reviewed: 2026-05-07
---

# C4 Container Template

> **Purpose**: describe the deployable applications, services, data stores, and their communication paths.
> **Audience**: architects, engineers, operators, and reviewers validating runtime structure.
> **When to update**: update when containers, technologies, or container communication paths change.

## How to use this template

- Use this view after the context view is stable.
- Keep one row or subsection per container with responsibility, technology, and dependencies.
- Link deep internal detail to the component view rather than overloading this document.

## What not to include

- **Internal component structure** — component-level decomposition belongs in the component view ([04_c4_component_TEMPLATE.md](04_c4_component_TEMPLATE.md)). The container view shows what is deployed, not what is inside each container.
- **Exact interface contracts or schema detail** — interface contracts belong in the interface control document ([06_interface_control_document_TEMPLATE.md](06_interface_control_document_TEMPLATE.md)).
- **Environment or infrastructure topology** — deployment environments belong in the deployment template ([07_deployment_TEMPLATE.md](07_deployment_TEMPLATE.md)).
- **Architecture decision rationale** — binding decisions belong in ADRs ([../adr/ADR-000-template.md](../adr/ADR-000-template.md)); link the relevant ADR to each technology choice.
- **Code-level or library implementation** — implementation stays in the codebase. Mention the framework or language only when the choice materially affects the architecture.

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

## Containers

List each application, service, worker, gateway, or data store in scope.

- Example: web application, background worker, relational database, cache.

## Technology choices

Record the primary technologies or platforms for each container.

- Explain why the choice matters only when it affects architecture or operation.

## Responsibilities

State what each container owns and what it must not own.

- Keep boundaries clear enough that later ADRs and components can refine them without contradiction.

## Communication paths

Describe how the containers communicate.

- Note protocol, direction, sync or async behavior, and notable failure handling.
- Link contract-heavy paths to [06_interface_control_document_TEMPLATE.md](06_interface_control_document_TEMPLATE.md).

## Diagram placeholder

Add a PlantUML or Mermaid container diagram.

```text
[User] -> [Web App] -> [API Service] -> [Database]
```

## Related documents

- [01_solution_design_TEMPLATE.md](01_solution_design_TEMPLATE.md) — parent architecture baseline.
- [04_c4_component_TEMPLATE.md](04_c4_component_TEMPLATE.md) — internal structure of critical containers.
- [06_interface_control_document_TEMPLATE.md](06_interface_control_document_TEMPLATE.md) — detailed interface contracts.
- [../adr/INDEX.md](../adr/INDEX.md) — decisions that explain major technology or boundary choices.
