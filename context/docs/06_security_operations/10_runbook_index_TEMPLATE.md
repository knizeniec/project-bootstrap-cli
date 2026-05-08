---
title: Runbook Index Template
status: draft
record_class: canonical
audience: [internal, manager]
owner: operations-lead
capability: operations
phase: execution
cadence: monthly
last_reviewed: 2026-05-07
source_of_truth: repo
---

# Runbook Index Template

> **Purpose**: provide the central index of operational runbooks and their ownership, purpose, and location.
> **Audience**: operations teams, support teams, service owners, and managers who need to confirm runbook coverage.
> **When to update**: update when runbooks are added, moved, renamed, retired, or when ownership changes.

## How to use this template

Use this template as the canonical pointer list for runbooks, even when the runbooks themselves live next to code or in another docs area. Keep entries short, current, and ownership-based.

- Mandatory: name, purpose, owner, and location for each runbook entry.
- Optional: environment or service tags if they help readers find the right runbook quickly.
- Remove stale links as soon as a runbook is superseded or retired.

## What not to include

- **Runbook procedure content** — step-by-step procedures belong in individual runbooks ([11_runbook_TEMPLATE.md](11_runbook_TEMPLATE.md)). This index is a navigation layer, not a procedure store.
- **Incident records or postmortem summaries** — incident records belong in the postmortem template ([12_incident_postmortem_TEMPLATE.md](12_incident_postmortem_TEMPLATE.md)). The runbook index points to operational procedures, not incident history.
- **Service acceptance criteria** — readiness checks belong in the service acceptance template ([08_service_acceptance_TEMPLATE.md](08_service_acceptance_TEMPLATE.md)). The index confirms that runbooks exist; the acceptance check confirms they are sufficient.
- **Operating model or on-call ownership** — broader service ownership belongs in the operating model ([03_operating_model_TEMPLATE.md](03_operating_model_TEMPLATE.md)).
- **Security policy or threat model content** — control design belongs in the security baseline ([01_security_baseline_TEMPLATE.md](01_security_baseline_TEMPLATE.md)).

## Frontmatter quick reference

This template's typical frontmatter values. The full schema (with all enums and conditional rules) is at [docs/00_operating_model/04_frontmatter_schema.md](../00_operating_model/04_frontmatter_schema.md).

| Field | Typical value here | Notes |
|---|---|---|
| `status` | `draft` → `active` → `superseded` | Use `superseded` when replaced; required `superseded_by:` link |
| `record_class` | `canonical` | This template defines a canonical artifact |
| `audience` | `[internal, manager]` | Add `client` only when client-export-safe |
| `capability` | `operations` | Fixed for this folder |
| `phase` | `execution` | One of `initiation`, `planning`, `execution`, `monitoring`, `closure`, `n/a` |
| `cadence` | `monthly` | One of `ad-hoc`, `weekly`, `monthly`, `per-stage`, `per-release`, `one-shot` |

> When `capability: execution`, both `cadence` and `source_of_truth` are required by the validator.

## Central index linking to per-component runbooks (some may live next to code)

Explain how the runbook set is organized and how readers should find the right operating procedure under pressure. Make clear that this index is the discovery layer, not the full procedure content.

- Example: separate sections for application, data, infrastructure, security, and support runbooks.
- Example: note when a runbook lives in a code-adjacent docs folder because it changes with the component.

## Per-runbook entry: name, purpose, owner, location

Provide the minimum fields needed to identify and trust each runbook entry. Keep naming and ownership stable so responders can find the right procedure quickly.

- Example: "API rollback runbook" | restore prior release safely | release manager | docs or service path.
- Example: "Database restore runbook" | restore point-in-time backup | operations lead | linked recovery location.

## Related documents

- [08_service_acceptance_TEMPLATE.md](08_service_acceptance_TEMPLATE.md) — checks that the required runbooks exist and are usable before acceptance.
- [../07_delivery/07_release_plan_TEMPLATE.md](../07_delivery/07_release_plan_TEMPLATE.md) — depends on runbook coverage for release execution confidence.
- [03_operating_model_TEMPLATE.md](03_operating_model_TEMPLATE.md) — defines the operating ownership for the runbook set.
- [06_incident_response_TEMPLATE.md](06_incident_response_TEMPLATE.md) — relies on the indexed runbooks during active incident response.
