---
title: Support Model Template
status: draft
record_class: canonical
audience: [internal, manager]
owner: support-lead
capability: operations
phase: execution
cadence: monthly
last_reviewed: 2026-05-07
source_of_truth: repo
---

# Support Model Template

> **Purpose**: define how support requests are received, triaged, resolved, and handed between support tiers.
> **Audience**: support leads, operations teams, delivery teams, and managers responsible for service-support effectiveness.
> **When to update**: update when support tiers, channels, SLA targets, or handover arrangements change.

## How to use this template

Use this template to describe the ongoing support path for live users and operators. Keep it centered on who handles what, how requests arrive, and how delivery hands work over to support.

- Mandatory: support tiers, intake channels, SLA expectations, knowledge base, and handover.
- Optional: tool-specific queue settings if those are maintained elsewhere.
- Remove one-off launch-support arrangements once steady-state support begins.

## What not to include

- **Individual support tickets or incident records** — live ticket content belongs in the support tool. This template defines the process model, not individual cases.
- **Incident response lifecycle steps** — incident response procedures belong in the incident response template ([06_incident_response_TEMPLATE.md](06_incident_response_TEMPLATE.md)). The support model handles intake and escalation, not major incident management.
- **Operational runbook content** — runbook procedures belong in runbooks ([11_runbook_TEMPLATE.md](11_runbook_TEMPLATE.md)). The support model points to runbooks as part of the knowledge base.
- **Operating model ownership and escalation** — broader live-service ownership belongs in the operating model ([03_operating_model_TEMPLATE.md](03_operating_model_TEMPLATE.md)). The support model covers the support function within that model.
- **Service acceptance criteria** — readiness conditions belong in the service acceptance template ([08_service_acceptance_TEMPLATE.md](08_service_acceptance_TEMPLATE.md)). The handover section here summarises what delivery must provide, not the acceptance check.

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

## Support tiers (L1/L2/L3)

Define what each support tier owns and where escalation between tiers should occur. Keep the distinctions practical so ticket routing is predictable.

- Example: L1 handles intake and known issues, L2 handles environment and data checks, L3 handles code or deep platform defects.
- Example: product or delivery teams provide temporary L3 cover during early life support.

## Intake channels

List the approved channels for incidents, requests, and service questions. Clarify what should and should not be raised through each path.

- Example: service desk portal for standard requests, paging tool for major incidents, support email for low-severity issues.
- Example: customer success route for business-process questions that are not service faults.

## SLAs

Describe the response and resolution targets by severity or ticket type. Keep the commitments aligned with support capacity and business expectations.

- Example: critical service outage acknowledged in 15 minutes, standard request within 1 business day.
- Example: target resolution clock pauses when waiting for customer input.

## Knowledge base

Explain where repeatable support knowledge is stored and how it stays current. This should connect directly to the runbook index and user-facing help where relevant.

- Example: internal runbooks for operators and separate how-to articles for end users.
- Example: every resolved recurring issue must produce or update a knowledge article.

## Handover from delivery

Describe what delivery must provide before support can own the service confidently. Focus on knowledge transfer, readiness evidence, and unresolved risk visibility.

- Example: runbooks, known issues list, service acceptance record, and on-call contacts provided before handover.
- Example: open medium-risk items transferred with owner, target date, and mitigation notes.

## Related documents

- [03_operating_model_TEMPLATE.md](03_operating_model_TEMPLATE.md) — defines the wider live-service ownership and escalation structure for support.
- [10_runbook_index_TEMPLATE.md](10_runbook_index_TEMPLATE.md) — points to the operational procedures support teams use.
- [08_service_acceptance_TEMPLATE.md](08_service_acceptance_TEMPLATE.md) — ensures support readiness is complete before acceptance.
- [09_post_launch_review_TEMPLATE.md](09_post_launch_review_TEMPLATE.md) — reviews whether the support model worked in early life.
