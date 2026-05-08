---
title: Interface Control Document Template
status: draft
record_class: canonical
audience: [internal]
owner: architecture-maintainer
capability: architecture
phase: planning
cadence: per-stage
last_reviewed: 2026-05-07
---

# Interface Control Document Template

> **Purpose**: provide a durable contract document for one interface, integration boundary, or external exchange.
> **Audience**: engineers, integrators, reviewers, and operators who need an exact boundary contract.
> **When to update**: update when an interface contract, compatibility rule, operational behavior, or ownership changes.

## How to use this template

- Keep this document specific to one interface or integration.
- Link it to the parent solution design, relevant C4 views, and governing ADRs.
- Include examples only for the parts of the contract that are easy to get wrong.

## What not to include

- **Multiple interface contracts in one document** — create one ICD per interface or integration. Bundling unrelated contracts makes each one harder to maintain and version independently.
- **Internal component structure** — internals belong in the component view ([04_c4_component_TEMPLATE.md](04_c4_component_TEMPLATE.md)). The ICD defines the external boundary, not what is inside.
- **Architecture decision rationale** — binding decisions belong in ADRs ([../adr/ADR-000-template.md](../adr/ADR-000-template.md)); link the ADR that governs versioning or compatibility policy.
- **Runbook procedures or incident response steps** — operational procedures belong in runbooks ([../06_security_operations/11_runbook_TEMPLATE.md](../06_security_operations/11_runbook_TEMPLATE.md)).
- **Test scenarios or acceptance criteria** — testing the contract belongs in the acceptance catalog ([../02_product/05_acceptance_catalog_TEMPLATE.md](../02_product/05_acceptance_catalog_TEMPLATE.md)) and verification evidence index.

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

## Interface summary

- Interface name: [Name]
- Owner: [Role or team]
- Consumers: [Systems, teams, or user groups]
- Purpose: [Why this interface exists]

## Contract definition

- Inputs: [Requests, messages, files, or events received]
- Outputs: [Responses, messages, files, or state changes produced]
- Protocol or transport: [HTTP, queue, file exchange, internal API, and so on]
- Data contract: [Schema, payload, or field expectations]

## Operational behavior

- Authentication and authorization: [Rules]
- Error handling and retries: [Behavior]
- Timeout, latency, or throughput expectations: [Constraints]
- Observability: [Logs, metrics, traces, alerts]

## Dependencies and failure modes

- Upstream dependencies: [List]
- Downstream dependencies: [List]
- Failure mode 1: [What can fail and what happens]
- Failure mode 2: [What can fail and what happens]

## Change control

- Versioning approach: [How contract changes are managed]
- Compatibility expectations: [Backward compatibility or migration rules]
- Governing ADRs: [ADR references]

## Related documents

- [01_solution_design_TEMPLATE.md](01_solution_design_TEMPLATE.md) — parent architecture baseline.
- [03_c4_container_TEMPLATE.md](03_c4_container_TEMPLATE.md) — container-level communication paths.
- [04_c4_component_TEMPLATE.md](04_c4_component_TEMPLATE.md) — internal component collaborators behind this contract.
- [../adr/INDEX.md](../adr/INDEX.md) — durable contract and integration decisions.
