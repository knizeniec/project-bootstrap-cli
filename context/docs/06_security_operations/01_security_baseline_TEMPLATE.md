---
title: Security Baseline Template
status: draft
record_class: canonical
audience: [internal, manager]
owner: security-lead
capability: operations
phase: planning
cadence: monthly
last_reviewed: 2026-05-07
source_of_truth: repo
---

# Security Baseline Template

> **Purpose**: define the minimum security controls and operational expectations required for the service.
> **Audience**: security leads, engineers, operations teams, and managers approving baseline control posture.
> **When to update**: update when the threat surface, standards, control obligations, or platform assumptions change.

## How to use this template

Use this template to document the default control stance before implementation and go-live reviews. Keep it technology-aware but policy-level, with detailed procedures linked from runbooks or standards where needed.

- Mandatory: threat surface, core controls, compliance frame, secrets handling, and vulnerability management.
- Optional: implementation references if control details live in architecture or runbook documents.
- Remove controls that do not apply to the service boundary and explain justified exceptions.

## What not to include

- **Incident-specific data or breach narratives** — incident records belong in the postmortem template ([12_incident_postmortem_TEMPLATE.md](12_incident_postmortem_TEMPLATE.md)). This baseline defines what controls are required; it does not record what went wrong.
- **Secret values or credentials** — never put actual credentials in a canonical document. Describe where secrets live and how they are rotated; do not put values here.
- **Full compliance standard text** — restate standards verbatim by linking to them in the standards register ([../08_references/01_standards_register_TEMPLATE.md](../08_references/01_standards_register_TEMPLATE.md)). This document states how controls are applied, not the full regulatory text.
- **Step-by-step operational procedures** — operational steps belong in runbooks ([11_runbook_TEMPLATE.md](11_runbook_TEMPLATE.md)) and the access control template ([05_access_control_TEMPLATE.md](05_access_control_TEMPLATE.md)).
- **Threat model detail or STRIDE analysis** — that belongs in the threat model ([02_threat_model_TEMPLATE.md](02_threat_model_TEMPLATE.md)). The baseline states the controls; the threat model explains the threats they address.

## Frontmatter quick reference

This template's typical frontmatter values. The full schema (with all enums and conditional rules) is at [docs/00_operating_model/04_frontmatter_schema.md](../00_operating_model/04_frontmatter_schema.md).

| Field | Typical value here | Notes |
|---|---|---|
| `status` | `draft` → `active` → `superseded` | Use `superseded` when replaced; required `superseded_by:` link |
| `record_class` | `canonical` | This template defines a canonical artifact |
| `audience` | `[internal, manager]` | Add `client` only when client-export-safe |
| `capability` | `operations` | Fixed for this folder |
| `phase` | `planning` | One of `initiation`, `planning`, `execution`, `monitoring`, `closure`, `n/a` |
| `cadence` | `monthly` | One of `ad-hoc`, `weekly`, `monthly`, `per-stage`, `per-release`, `one-shot` |

> When `capability: execution`, both `cadence` and `source_of_truth` are required by the validator.

## Threat surface

Summarize the main assets, entry points, privileged actions, and external dependencies that shape the control baseline. Focus on what must be protected and why.

- Example: public web UI, API endpoints, admin console, background workers, and third-party notification services.
- Example: sensitive assets include customer data, secrets, audit logs, and privileged admin actions.

## Controls (auth/authz/encryption/logging/network)

List the baseline control expectations across identity, authorization, cryptography, logging, and network posture. Use short statements that can later be checked during readiness reviews.

- Example: SSO for workforce users, least-privilege roles, encryption in transit and at rest, tamper-evident audit logs.
- Example: default-deny network rules, segmented admin access, and monitored ingress paths.

## Compliance frame (link to standards register)

State which external standards, policies, or internal control frames apply and where the canonical register lives. Keep this section focused on applicability, not full policy text.

- Example: align baseline controls to ISO 27001 themes and NIST CSF functions as adopted by the standards register.
- Example: identify any client or regulatory overlays that add stricter control expectations.

## Secrets handling

Describe how secrets are created, stored, rotated, and retired. Make the approved storage locations and access boundaries explicit.

- Example: secrets stored in managed secret storage, never in source control or unapproved CI variables.
- Example: rotation required on personnel change, suspected exposure, or scheduled quarterly review.

## Vulnerability management

Explain how vulnerabilities are detected, triaged, remediated, and verified. Include the minimum cadence for scans and how exceptions are recorded.

- Example: dependency and image scans on every release candidate with Sev-1 and Sev-2 fixes required before go-live.
- Example: accepted residual risks documented with owner, expiry date, and compensating controls.

## Related documents

- [02_threat_model_TEMPLATE.md](02_threat_model_TEMPLATE.md) — explains the threat scenarios this baseline is designed to address.
- [05_access_control_TEMPLATE.md](05_access_control_TEMPLATE.md) — details identity, role, review, and offboarding rules referenced by the baseline.
- [../08_references/01_standards_register_TEMPLATE.md](../08_references/01_standards_register_TEMPLATE.md) — records the external standards and frameworks linked here.
- [08_service_acceptance_TEMPLATE.md](08_service_acceptance_TEMPLATE.md) — checks that the baseline controls are satisfied before service acceptance.
