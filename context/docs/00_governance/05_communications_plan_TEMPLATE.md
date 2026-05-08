---
title: Communications Plan Template
status: draft
record_class: canonical
audience: [internal, manager, client]
owner: project-manager
capability: governance
phase: planning
cadence: monthly
last_reviewed: 2026-05-07
---

# Communications Plan Template

> **Purpose**: define what will be communicated, to whom, through which channel, and on what cadence.
> **Audience**: project managers, sponsors, managers, and client stakeholders coordinating project communications.
> **When to update**: update when audiences, channels, cadence, or escalation needs change.

## How to use this template

Use this template to turn stakeholder analysis into a simple communication operating model. Keep it practical, schedule-based, and linked to the live governance and delivery cadence.

- Mandatory: audiences, messages, channels, cadence, owners, feedback loop, escalation.
- Optional: separate internal and client communication tables if the project is large.
- Remove one-off launch notes once they are replaced by normal reporting cadence.

## What not to include

- **Message drafts or full communications copy** — draft communications content should be kept in separate documents or templates, not embedded in the plan. This plan defines the communication framework, not the messages.
- **Stakeholder influence analysis or sentiment** — that belongs in the stakeholder register ([04_stakeholder_register_TEMPLATE.md](04_stakeholder_register_TEMPLATE.md)). Reference it here but do not duplicate it.
- **Status report content or delivery updates** — live delivery status belongs in the status report ([../07_delivery/04_status_report_TEMPLATE.md](../07_delivery/04_status_report_TEMPLATE.md)). This plan defines when and how to report, not what is being reported.
- **Board governance rules or meeting agendas** — governance forum rules belong in the board terms of reference ([03_board_terms_of_reference_TEMPLATE.md](03_board_terms_of_reference_TEMPLATE.md)).
- **Incident response communications** — crisis comms procedures belong in the incident response template ([../06_security_operations/06_incident_response_TEMPLATE.md](../06_security_operations/06_incident_response_TEMPLATE.md)).

## Frontmatter quick reference

This template's typical frontmatter values. The full schema (with all enums and conditional rules) is at [docs/00_operating_model/04_frontmatter_schema.md](../00_operating_model/04_frontmatter_schema.md).

| Field | Typical value here | Notes |
|---|---|---|
| `status` | `draft` → `active` → `superseded` | Use `superseded` when replaced; required `superseded_by:` link |
| `record_class` | `canonical` | This template defines a canonical artifact |
| `audience` | `[internal, manager, client]` | Add `client` only when client-export-safe |
| `capability` | `governance` | Fixed for this folder |
| `phase` | `planning` | One of `initiation`, `planning`, `execution`, `monitoring`, `closure`, `n/a` |
| `cadence` | `monthly` | One of `ad-hoc`, `weekly`, `monthly`, `per-stage`, `per-release`, `one-shot` |

> When `capability: execution`, both `cadence` and `source_of_truth` are required by the validator.

## Audiences

Group readers by the information they need rather than by organization chart alone. This helps keep communication targeted and avoids producing the same update in too many forms.

- Example: sponsor and board, delivery team, client approvers, end-user change champions.
- Example: operations/support stakeholders needing readiness and incident information.

## Messages

Define the recurring messages each audience should receive and why they matter. Keep the content concise so the plan drives useful updates instead of communication overhead.

- Example: decision required, status and risks, release timing, benefit progress.
- Example: change impact, training expectation, operational readiness, issue escalation.

## Channels

List the approved communication channels and meeting formats used for each audience. Choose channels that match the urgency and durability needed for the information.

- Example: steering meeting, weekly report, workshop, team chat summary, email brief.
- Example: demo session, release note, service-desk bulletin, client checkpoint.

## Cadence

State how often each communication happens and which events trigger ad-hoc updates. This ensures reporting is predictable without blocking urgent escalation.

- Example: weekly delivery report, monthly board pack, per-release go-live brief.
- Example: immediate escalation when a tolerance breach or Sev-1 issue occurs.

## Owners

Assign a role to prepare, approve, and send each communication type. Ownership should be explicit so communications keep moving during absence or role changes.

- Example: project manager authors weekly updates and sponsor approves board summaries.
- Example: release manager owns go-live communications and service desk bulletins.

## Feedback loops

Explain how responses, questions, and stakeholder sentiment will be captured and acted on. Good communication plans are two-way, not just broadcast schedules.

- Example: capture action items in governance minutes and review stakeholder sentiment monthly.
- Example: route release feedback into the dependency log or RAID register.

## Escalation

Describe how urgent communication differs from routine reporting. This helps the team act quickly when risk, issue, or change impact crosses tolerance.

- Example: project manager escalates critical blockers to sponsor within the same day.
- Example: release risk escalates to the board if readiness criteria are red.

## Related documents

- [04_stakeholder_register_TEMPLATE.md](04_stakeholder_register_TEMPLATE.md) — identifies the audiences and their engagement needs.
- [03_board_terms_of_reference_TEMPLATE.md](03_board_terms_of_reference_TEMPLATE.md) — defines the formal governance forums and meeting rhythm.
- [../07_delivery/04_status_report_TEMPLATE.md](../07_delivery/04_status_report_TEMPLATE.md) — provides the recurring reporting artifact referenced by this plan.
