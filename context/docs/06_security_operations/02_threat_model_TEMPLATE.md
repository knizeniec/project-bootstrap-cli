---
title: Threat Model Template
status: draft
record_class: canonical
audience: [internal, manager]
owner: security-architect
capability: operations
phase: planning
cadence: per-stage
last_reviewed: 2026-05-07
source_of_truth: repo
---

# Threat Model Template

> **Purpose**: identify system threats, trust boundaries, mitigations, and accepted residual risks.
> **Audience**: security architects, engineers, reviewers, and managers assessing design-time security risk.
> **When to update**: update when architecture, data flows, integrations, or risk assumptions materially change.

## How to use this template

Use this template during design and major change review to reason about threat exposure before release. Keep the model concise, evidence-backed, and tied to practical mitigations.

- Mandatory: system decomposition, trust boundaries, STRIDE analysis, mitigations, and residual risks.
- Optional: diagrams if they help explain complex trust boundaries.
- Remove duplicate architectural detail that is already canonical elsewhere and link to it instead.

## What not to include

- **Security control implementation detail** — how controls are implemented belongs in the security baseline ([01_security_baseline_TEMPLATE.md](01_security_baseline_TEMPLATE.md)) and access control template ([05_access_control_TEMPLATE.md](05_access_control_TEMPLATE.md)). The threat model identifies threats and mitigations at a conceptual level.
- **Incident narratives or past breach records** — historical incidents belong in the postmortem template ([12_incident_postmortem_TEMPLATE.md](12_incident_postmortem_TEMPLATE.md)). The threat model addresses future risk, not past events.
- **Architecture design rationale** — design decisions belong in ADRs ([../adr/ADR-000-template.md](../adr/ADR-000-template.md)) and the solution design ([../03_architecture/01_solution_design_TEMPLATE.md](../03_architecture/01_solution_design_TEMPLATE.md)). Reference the design; do not duplicate it here.
- **Compliance standard text** — restate standards by linking to them in the standards register ([../08_references/01_standards_register_TEMPLATE.md](../08_references/01_standards_register_TEMPLATE.md)).
- **Runbook procedures** — step-by-step response actions belong in runbooks ([11_runbook_TEMPLATE.md](11_runbook_TEMPLATE.md)). The threat model names the mitigations; runbooks implement them.

## Frontmatter quick reference

This template's typical frontmatter values. The full schema (with all enums and conditional rules) is at [docs/00_operating_model/04_frontmatter_schema.md](../00_operating_model/04_frontmatter_schema.md).

| Field | Typical value here | Notes |
|---|---|---|
| `status` | `draft` → `active` → `superseded` | Use `superseded` when replaced; required `superseded_by:` link |
| `record_class` | `canonical` | This template defines a canonical artifact |
| `audience` | `[internal, manager]` | Add `client` only when client-export-safe |
| `capability` | `operations` | Fixed for this folder |
| `phase` | `planning` | One of `initiation`, `planning`, `execution`, `monitoring`, `closure`, `n/a` |
| `cadence` | `per-stage` | One of `ad-hoc`, `weekly`, `monthly`, `per-stage`, `per-release`, `one-shot` |

> When `capability: execution`, both `cadence` and `source_of_truth` are required by the validator.

## System decomposition

Break the service into the components, actors, and integrations needed for threat analysis. The goal is to show meaningful security-relevant boundaries, not every implementation detail.

- Example: user client, public API, admin portal, worker queue, database, and identity provider.
- Example: third-party email service and analytics sink included because they handle sensitive events.

## Trust boundaries

Identify where data or control crosses into a different trust level, environment, or operational domain. These are the places where authentication, validation, and monitoring expectations usually intensify.

- Example: public internet to API gateway, workforce network to admin plane, application to managed database.
- Example: boundary between internal services and external SaaS webhook callbacks.

## STRIDE per component

Assess spoofing, tampering, repudiation, information disclosure, denial of service, and elevation of privilege for each relevant component or boundary. Keep entries short and traceable to mitigations.

- Example: admin portal spoofing risk mitigated by SSO and MFA; API DoS risk mitigated by rate limiting.
- Example: webhook tampering risk mitigated by signature validation and replay protection.

## Mitigations

List the controls, design decisions, or operational practices that reduce the identified threats. Prefer actions that can be verified later in implementation or acceptance reviews.

- Example: role-based access control, signed webhooks, encrypted storage, alerting on privileged actions.
- Example: queue isolation, retry limits, dependency pinning, and restore-tested backups.

## Residual risks

Document the risks that remain after mitigation and explain why they are acceptable, temporary, or escalated. Include ownership and follow-up so residual risk does not become invisible risk.

- Example: accepted low-probability vendor outage risk with manual fallback and named owner.
- Example: temporary risk accepted until network segmentation change lands in the next stage.

## Related documents

- [01_security_baseline_TEMPLATE.md](01_security_baseline_TEMPLATE.md) — sets the control expectations that should respond to the threats identified here.
- [03_operating_model_TEMPLATE.md](03_operating_model_TEMPLATE.md) — defines the operational ownership and escalation paths that carry some mitigations.
- [08_service_acceptance_TEMPLATE.md](08_service_acceptance_TEMPLATE.md) — verifies mitigations and residual-risk decisions before go-live.
- [../03_architecture/01_solution_design_TEMPLATE.md](../03_architecture/01_solution_design_TEMPLATE.md) — provides the broader design context for the threat decomposition.
