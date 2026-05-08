---
title: Configuration Baseline Template
status: draft
record_class: canonical
audience: [internal]
owner: architecture-maintainer
capability: architecture
phase: execution
cadence: monthly
last_reviewed: 2026-05-07
---

# Configuration Baseline Template

> **Purpose**: define the major configuration domains, environment policy, secrets boundaries, and change procedure for the solution.
> **Audience**: engineers, operators, and reviewers responsible for safe configuration management.
> **When to update**: update when configuration domains, environment rules, or change controls change.

## How to use this template

- Document configuration categories and control rules, not raw secret values or long variable dumps.
- Keep environment-specific differences explicit.
- Link durable configuration decisions to ADRs where the choice affects design.

## What not to include

- **Secret values or credentials of any kind** — never put actual secret values in a canonical document. Describe the secret store and rotation policy, not the values themselves.
- **Full variable listings or environment file dumps** — environment-specific variables belong in configuration management tooling or infrastructure-as-code. Reference the location here; do not reproduce them.
- **Architecture decision rationale** — binding decisions on platform or configuration approach belong in ADRs ([../adr/ADR-000-template.md](../adr/ADR-000-template.md)).
- **Deployment topology or environment promotion rules** — environment structure belongs in the deployment template ([07_deployment_TEMPLATE.md](07_deployment_TEMPLATE.md)).
- **Operational runbook steps** — procedures for applying or rolling back configuration changes belong in runbooks ([../06_security_operations/11_runbook_TEMPLATE.md](../06_security_operations/11_runbook_TEMPLATE.md)).

## Frontmatter quick reference

This template's typical frontmatter values. The full schema (with all enums and conditional rules) is at [docs/00_operating_model/04_frontmatter_schema.md](../00_operating_model/04_frontmatter_schema.md).

| Field | Typical value here | Notes |
|---|---|---|
| `status` | `draft` → `active` → `superseded` | Use `superseded` when replaced; required `superseded_by:` link |
| `record_class` | `canonical` | This template defines a canonical artifact |
| `audience` | `[internal]` | Add `manager` only when needed for reviews |
| `capability` | `architecture` | Fixed for this folder |
| `phase` | `execution` | One of `initiation`, `planning`, `execution`, `monitoring`, `closure`, `n/a` |
| `cadence` | `monthly` | One of `ad-hoc`, `weekly`, `monthly`, `per-stage`, `per-release`, `one-shot` |

> When `capability: execution`, both `cadence` and `source_of_truth` are required by the validator.

## Configuration domains

Group configuration by responsibility.

- Example: application settings, infrastructure settings, integration endpoints, feature flags.

## Per-environment values policy

Describe what may vary by environment and what must remain consistent.

- Example: credentials differ; domain behavior flags do not.

## Secrets

State how secrets are stored, injected, rotated, and reviewed.

- Do not place secret values in the document.

## Change procedure

Explain how configuration changes are proposed, reviewed, tested, and rolled back.

- Include emergency change handling if relevant.

## Related documents

- [07_deployment_TEMPLATE.md](07_deployment_TEMPLATE.md) — environment and release context.
- [09_resilience_TEMPLATE.md](09_resilience_TEMPLATE.md) — configuration changes that affect continuity or failover.
- [../adr/INDEX.md](../adr/INDEX.md) — durable configuration and platform decisions.
