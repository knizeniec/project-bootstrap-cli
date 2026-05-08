---
title: Access Control Template
status: draft
record_class: canonical
audience: [internal, manager]
owner: security-lead
capability: operations
phase: monitoring
cadence: monthly
last_reviewed: 2026-05-07
source_of_truth: repo
---

# Access Control Template

> **Purpose**: define how identities, roles, reviews, and offboarding are controlled for the service.
> **Audience**: security teams, operations teams, service owners, and managers accountable for least-privilege access.
> **When to update**: update when identity sources, role definitions, review cadence, or offboarding workflows change.

## How to use this template

Use this template to capture the canonical access model for workforce, service, and privileged identities. Keep it policy-level and point to detailed implementation settings elsewhere.

- Mandatory: identity sources, role definitions, least-privilege policy, periodic review, and offboarding.
- Optional: system-specific role maps if they live in a dedicated reference.
- Remove temporary elevated access once it is no longer justified.

## What not to include

- **Individual user accounts or named credentials** — access control lists for specific accounts belong in identity management tooling, not in a canonical document.
- **Incident response procedures for access breaches** — breach response belongs in the incident response template ([06_incident_response_TEMPLATE.md](06_incident_response_TEMPLATE.md)).
- **Security control posture or threat model** — control design belongs in the security baseline ([01_security_baseline_TEMPLATE.md](01_security_baseline_TEMPLATE.md)) and threat model ([02_threat_model_TEMPLATE.md](02_threat_model_TEMPLATE.md)).
- **Runbook steps for role provisioning** — operational provisioning procedures belong in runbooks ([11_runbook_TEMPLATE.md](11_runbook_TEMPLATE.md)). This template defines the policy; runbooks implement it.
- **Compliance standard text** — do not reproduce standards verbatim; link to the standards register ([../08_references/01_standards_register_TEMPLATE.md](../08_references/01_standards_register_TEMPLATE.md)).

## Frontmatter quick reference

This template's typical frontmatter values. The full schema (with all enums and conditional rules) is at [docs/00_operating_model/04_frontmatter_schema.md](../00_operating_model/04_frontmatter_schema.md).

| Field | Typical value here | Notes |
|---|---|---|
| `status` | `draft` → `active` → `superseded` | Use `superseded` when replaced; required `superseded_by:` link |
| `record_class` | `canonical` | This template defines a canonical artifact |
| `audience` | `[internal, manager]` | Add `client` only when client-export-safe |
| `capability` | `operations` | Fixed for this folder |
| `phase` | `monitoring` | One of `initiation`, `planning`, `execution`, `monitoring`, `closure`, `n/a` |
| `cadence` | `monthly` | One of `ad-hoc`, `weekly`, `monthly`, `per-stage`, `per-release`, `one-shot` |

> When `capability: execution`, both `cadence` and `source_of_truth` are required by the validator.

## Identity sources

State where user and service identities originate and how they are trusted. Different identity sources often imply different review and provisioning controls.

- Example: workforce SSO, customer identity platform, and managed service accounts for automation.
- Example: local break-glass accounts tightly limited and separately reviewed.

## Role definitions

Describe the approved role set and the kind of access each role is meant to carry. Keep role names stable and tied to job purpose, not individuals.

- Example: viewer, operator, administrator, auditor, and deployment approver.
- Example: service account roles separated by runtime function and environment.

## Least-privilege policy

Explain the default rule for granting access and when exceptions are allowed. Make privileged or production access controls especially clear.

- Example: grant the minimum role needed for the task and time-bound elevated access.
- Example: production admin access requires stronger approval and audit logging.

## Periodic access review

State how often access is reviewed, by whom, and what evidence is retained. Reviews should be regular enough to catch drift before it becomes a material risk.

- Example: monthly privileged-access review and quarterly standard-role review.
- Example: review output records removed, retained, and exception-approved accounts.

## Offboarding

Describe how access is revoked when people leave, change role, or when service credentials are retired. Include the expected timing and any verification step.

- Example: workforce access revoked on HR termination event and validated the same day.
- Example: service credentials rotated immediately when a privileged maintainer departs.

## Related documents

- [01_security_baseline_TEMPLATE.md](01_security_baseline_TEMPLATE.md) — sets the baseline control posture that access rules must satisfy.
- [06_incident_response_TEMPLATE.md](06_incident_response_TEMPLATE.md) — uses access records during containment and investigation.
- [08_service_acceptance_TEMPLATE.md](08_service_acceptance_TEMPLATE.md) — verifies access controls and reviews before go-live.
- [../08_references/01_standards_register_TEMPLATE.md](../08_references/01_standards_register_TEMPLATE.md) — records the standards that may shape access policy requirements.
