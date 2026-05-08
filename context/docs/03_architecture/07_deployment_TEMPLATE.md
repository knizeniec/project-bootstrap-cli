---
title: Deployment Template
status: draft
record_class: canonical
audience: [internal]
owner: architecture-maintainer
capability: architecture
phase: execution
cadence: per-stage
last_reviewed: 2026-05-07
---

# Deployment Template

> **Purpose**: describe environments, topology, release path, and operational dependencies for deploying the solution safely.
> **Audience**: architects, engineers, operators, and release managers coordinating runtime rollout.
> **When to update**: update when environments, topology, pipelines, secrets handling, or release controls change.

## How to use this template

- Use this document for deployment shape and environment rules, not for step-by-step runbooks.
- Link the operational procedures to the runbook index.
- Keep infrastructure, release, and secrets guidance at the level needed for design review and delivery planning.

## What not to include

- **Step-by-step operational procedures** — runbook-style steps belong in runbooks ([../06_security_operations/11_runbook_TEMPLATE.md](../06_security_operations/11_runbook_TEMPLATE.md)). This document describes deployment shape, not execution procedure.
- **Secret values or credentials** — never store secrets in documentation. Describe where secrets live and how they are managed, but do not put values here.
- **Application-level architecture or component design** — internal structure belongs in the C4 views and solution design ([01_solution_design_TEMPLATE.md](01_solution_design_TEMPLATE.md)).
- **Observability or alerting configuration** — telemetry setup belongs in the observability template ([08_observability_TEMPLATE.md](08_observability_TEMPLATE.md)).
- **Architecture decision rationale** — binding infrastructure or platform decisions belong in ADRs ([../adr/ADR-000-template.md](../adr/ADR-000-template.md)).

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

## Environments

List the environments, their purpose, and the promotion path.

- Example: local, integration, staging, production.

## Topology

Describe the deployed units, network boundaries, and runtime dependencies.

- Note ingress, compute, data, messaging, and managed services.

## Infrastructure-as-code reference

Point to the source of truth for infrastructure definitions and ownership.

- Example: Terraform root module, platform-managed templates, environment manifests.

## Secrets management

Explain where secrets live, how they are rotated, and who can change them.

- Keep secrets out of the document itself.

## CI/CD pipeline outline

Summarize build, verification, approval, deployment, and rollback stages.

- Link detailed operational procedures to the runbook index.

## Related documents

- [01_solution_design_TEMPLATE.md](01_solution_design_TEMPLATE.md) — architecture baseline for deployment scope.
- [08_observability_TEMPLATE.md](08_observability_TEMPLATE.md) — telemetry and alerting needed after deployment.
- [09_resilience_TEMPLATE.md](09_resilience_TEMPLATE.md) — failure handling and recovery expectations.
- [../06_security_operations/10_runbook_index_TEMPLATE.md](../06_security_operations/10_runbook_index_TEMPLATE.md) — operational procedures and owner-indexed runbooks.
