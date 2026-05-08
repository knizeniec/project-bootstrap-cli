---
title: Solution Design Template
status: draft
record_class: canonical
audience: [internal]
owner: architecture-maintainer
capability: architecture
phase: planning
cadence: per-stage
last_reviewed: 2026-05-07
---

# Solution Design Template

> **Purpose**: provide the primary arc42-style architecture baseline for a solution, service, or major change.
> **Audience**: architects, engineers, reviewers, and delivery leads who need one agreed technical baseline.
> **When to update**: update when goals, scope, design structure, key quality attributes, or linked decisions change.

## How to use this template

- Copy this file when a project needs a canonical architecture baseline.
- Keep each section short, decision-oriented, and linked to supporting C4 views and ADRs.
- Replace placeholder bullets with project-specific facts; remove sections that are truly out of scope only if the omission is explicit.

## What not to include

- **Product requirements or user stories** — these belong in the PRD ([../02_product/01_prd_TEMPLATE.md](../02_product/01_prd_TEMPLATE.md)) and requirements catalog. The solution design explains how; the PRD explains what and why.
- **Step-by-step operational runbooks** — procedures for operators belong in runbooks ([../06_security_operations/11_runbook_TEMPLATE.md](../06_security_operations/11_runbook_TEMPLATE.md)). The solution design describes how the system is built, not how it is operated day to day.
- **Low-level code or configuration dump** — implementation detail belongs in the code, ADRs, or the configuration baseline ([10_configuration_baseline_TEMPLATE.md](10_configuration_baseline_TEMPLATE.md)). Keep this document at the design level.
- **Architecture decision rationale** — binding decisions belong in ADRs ([../adr/ADR-000-template.md](../adr/ADR-000-template.md)). Reference ADR IDs here rather than duplicating the rationale.
- **Project delivery timelines or milestones** — scheduling belongs in the delivery plan ([../07_delivery/01_delivery_plan_TEMPLATE.md](../07_delivery/01_delivery_plan_TEMPLATE.md)).

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

## 1. Introduction and goals

State the problem, intended outcome, and why this architecture exists.

- Example goal: reduce manual processing by introducing a single service boundary.
- Example success measure: support the target throughput and recovery objective.

## 2. Constraints

List the non-negotiables that shape the design.

- Example: must run in an existing cloud account and CI platform.
- Example: must meet compliance, latency, or migration deadlines.

## 3. Context and scope

Describe the system in scope, what is out of scope, and the main neighboring actors and systems.

- Link the high-level diagram to [02_c4_context_TEMPLATE.md](02_c4_context_TEMPLATE.md).
- Note trust boundaries, ownership boundaries, and external dependencies.

## 4. Solution strategy

Summarize the design approach and why it fits the goals and constraints.

- Example: modular monolith first, with explicit seams for later extraction.
- Example: event-driven integration only where asynchronous decoupling is required.

## 5. Building block view

Describe the main structural elements and their responsibilities.

- Link container-level structure to [03_c4_container_TEMPLATE.md](03_c4_container_TEMPLATE.md).
- Link component-level structure for critical containers to [04_c4_component_TEMPLATE.md](04_c4_component_TEMPLATE.md).

## 6. Runtime view

Explain the most important request, event, or batch flows.

- Show happy path, failure path, and recovery or retry behavior.
- Keep sequence details aligned with interface and observability documents.

## 7. Deployment view

Summarize environments, topology, and release shape.

- Link to [07_deployment_TEMPLATE.md](07_deployment_TEMPLATE.md) for environment and infrastructure detail.
- Note major runtime tiers, data stores, and externally managed services.

## 8. Cross-cutting concepts

Capture concepts that apply across the whole design.

- Example: authn/authz, tenancy, audit logging, configuration, secrets, resilience patterns.
- Link to detailed docs where the concept has its own artifact.

## 9. Architectural decisions

List the decisions that govern this design and link to the source of truth.

- Add accepted ADRs from [../adr/INDEX.md](../adr/INDEX.md).
- Use [11_adr_index_TEMPLATE.md](11_adr_index_TEMPLATE.md) if the project keeps a local architecture decision snapshot.

## 10. Quality requirements

State the main quality attributes and how the design addresses them.

- Reliability: availability target, recovery target, degradation policy.
- Performance: key latency or throughput expectations.
- Security: trust boundaries, controls, and review points.

## 11. Risks and technical debt

List known architecture risks, deferred work, and monitoring triggers.

- Example: scaling assumption not yet load-tested.
- Example: dependency lifecycle risk or migration coupling risk.

## 12. Glossary

Define project-specific terms, abbreviations, and overloaded words.

- Prefer short definitions with links to the canonical glossary when one exists.

## Related documents

- [02_c4_context_TEMPLATE.md](02_c4_context_TEMPLATE.md) — system context for this design.
- [03_c4_container_TEMPLATE.md](03_c4_container_TEMPLATE.md) — container responsibilities and communication paths.
- [04_c4_component_TEMPLATE.md](04_c4_component_TEMPLATE.md) — component detail for key containers.
- [11_adr_index_TEMPLATE.md](11_adr_index_TEMPLATE.md) — local architecture decision listing.
- [../adr/INDEX.md](../adr/INDEX.md) — canonical ADR source of truth.
