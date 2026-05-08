---
title: Board Terms of Reference Template
status: draft
record_class: canonical
audience: [internal, manager, client]
owner: steering-committee-chair
capability: governance
phase: initiation
cadence: per-stage
last_reviewed: 2026-05-07
---

# Board Terms of Reference Template

> **Purpose**: define how the project board or steering group is formed, what it decides, and how it operates.
> **Audience**: board members, sponsors, managers, and client representatives participating in governance decisions.
> **When to update**: update when membership, decision rights, or board operating rules change.

## How to use this template

Use this template when a project needs a formal decision forum with repeatable governance rules. Keep it role-based and practical so board members can quickly see how meetings and decisions should work.

- Mandatory: purpose, composition, decision rights, cadence, quorum, reporting, and escalation.
- Optional: appendices for named members if the project needs them.
- Remove draft commentary once the forum is formally approved.

## What not to include

- **Meeting minutes or action logs** — individual meeting records should be kept separately and linked to this document, not embedded in the Terms of Reference.
- **Status report content** — delivery progress belongs in the status report ([../07_delivery/04_status_report_TEMPLATE.md](../07_delivery/04_status_report_TEMPLATE.md)), which the board consumes but does not own.
- **RAID item detail** — risks and issues belong in the RAID register ([06_raid_register_TEMPLATE.md](06_raid_register_TEMPLATE.md)); the ToR defines how the board handles escalated RAID items, not their content.
- **Change request specifics** — formal change decisions belong in the change control log ([07_change_control_log_TEMPLATE.md](07_change_control_log_TEMPLATE.md)).
- **Technical design decisions** — architecture choices belong in ADRs or the solution design, not in governance forum rules.

## Frontmatter quick reference

This template's typical frontmatter values. The full schema (with all enums and conditional rules) is at [docs/00_operating_model/04_frontmatter_schema.md](../00_operating_model/04_frontmatter_schema.md).

| Field | Typical value here | Notes |
|---|---|---|
| `status` | `draft` → `active` → `superseded` | Use `superseded` when replaced; required `superseded_by:` link |
| `record_class` | `canonical` | This template defines a canonical artifact |
| `audience` | `[internal, manager, client]` | Add `client` only when client-export-safe |
| `capability` | `governance` | Fixed for this folder |
| `phase` | `initiation` | One of `initiation`, `planning`, `execution`, `monitoring`, `closure`, `n/a` |
| `cadence` | `per-stage` | One of `ad-hoc`, `weekly`, `monthly`, `per-stage`, `per-release`, `one-shot` |

> When `capability: execution`, both `cadence` and `source_of_truth` are required by the validator.

## Purpose

State why the board exists and which classes of project decision it is expected to govern. This keeps the forum focused and avoids overlap with product, architecture, or delivery working groups.

- Example: approve scope changes above tolerance and authorize stage progression.
- Example: resolve escalated issues that cross business, delivery, and client boundaries.

## Composition

List the roles that must be represented and any optional attendees. Use role descriptions so the document stays current even as people rotate.

- Example: sponsor, project manager, product lead, technical lead, client representative.
- Example: risk or compliance advisor attends when required.

## Decision rights

Define the decisions the board owns and the decisions it only reviews or advises on. This should align with tolerances in the PID and change-control rules.

- Example: approve business case changes, stage entry, and major risk responses.
- Example: receive status updates but delegate day-to-day prioritization to delivery leads.

## Cadence

Set the expected meeting rhythm and how extraordinary sessions are called. Regular cadence helps keep governance light-touch while still predictable.

- Example: monthly scheduled meeting plus ad-hoc sessions for exceptions.
- Example: mandatory review before each stage gate or release milestone.

## Quorum

State the minimum roles required for a valid decision and what happens if quorum is not met. Keep the rule simple enough that meeting organizers can apply it without interpretation.

- Example: sponsor or delegate plus project manager and one business representative.
- Example: deferred decision if a required approving role is absent.

## Reporting

Explain what information the board receives and in what format. This section should point to the live delivery and control artifacts used as the board’s evidence base.

- Example: weekly status report, RAID summary, and change requests.
- Example: stage-gate checklist and benefits status at key decision points.

## Escalation

Describe how issues reach the board and what happens if the board cannot resolve them. This ensures the governance path remains clear under pressure.

- Example: project manager escalates breaches of tolerance within one working day.
- Example: unresolved client-commercial matters move to the sponsor forum.

## Related documents

- [02_project_initiation_document_TEMPLATE.md](02_project_initiation_document_TEMPLATE.md) — defines the wider project control model that this board supports.
- [07_change_control_log_TEMPLATE.md](07_change_control_log_TEMPLATE.md) — records major change decisions taken by the board.
- [../07_delivery/04_status_report_TEMPLATE.md](../07_delivery/04_status_report_TEMPLATE.md) — provides the regular reporting pack consumed by the board.
