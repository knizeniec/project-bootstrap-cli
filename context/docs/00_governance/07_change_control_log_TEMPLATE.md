---
title: Change Control Log Template
status: draft
record_class: canonical
audience: [internal, manager, client]
owner: change-authority
capability: governance
phase: monitoring
cadence: weekly
last_reviewed: 2026-05-07
---

# Change Control Log Template

> **Purpose**: record formal project changes, their impact, and the decisions made about them.
> **Audience**: project managers, change authorities, managers, and client stakeholders approving or tracking controlled changes.
> **When to update**: update whenever a controlled change is raised, decided, or implemented.

## How to use this template

Use this template when scope, cost, schedule, or quality changes need a recorded decision trail. Keep one row per request and make the decision status obvious enough for governance reviews and audits.

- Mandatory: ID, requestor, type, impact, decision, decision-maker, and date.
- Optional: link to supporting analysis, estimates, or approval minutes.
- Remove duplicate requests by linking them to the surviving change ID.

## What not to include

- **RAID items that have not yet become approved changes** — the RAID register ([06_raid_register_TEMPLATE.md](06_raid_register_TEMPLATE.md)) tracks risks and issues; only raise a change request when a formal decision is needed.
- **Architecture decisions** — technical design choices belong in ADRs ([../adr/ADR-000-template.md](../adr/ADR-000-template.md)). A change request may trigger an ADR, but the design rationale stays in the ADR.
- **Status updates or progress notes** — delivery progress belongs in the status report ([../07_delivery/04_status_report_TEMPLATE.md](../07_delivery/04_status_report_TEMPLATE.md)).
- **Detailed impact analysis or estimates** — supporting analysis can be linked from each row, not embedded in the log itself.
- **Board meeting minutes** — governance discussion records should stay in separate meeting notes; the log records only the formal change decision.

## Frontmatter quick reference

This template's typical frontmatter values. The full schema (with all enums and conditional rules) is at [docs/00_operating_model/04_frontmatter_schema.md](../00_operating_model/04_frontmatter_schema.md).

| Field | Typical value here | Notes |
|---|---|---|
| `status` | `draft` → `active` → `superseded` | Use `superseded` when replaced; required `superseded_by:` link |
| `record_class` | `canonical` | This template defines a canonical artifact |
| `audience` | `[internal, manager, client]` | Add `client` only when client-export-safe |
| `capability` | `governance` | Fixed for this folder |
| `phase` | `monitoring` | One of `initiation`, `planning`, `execution`, `monitoring`, `closure`, `n/a` |
| `cadence` | `weekly` | One of `ad-hoc`, `weekly`, `monthly`, `per-stage`, `per-release`, `one-shot` |

> When `capability: execution`, both `cadence` and `source_of_truth` are required by the validator.

## Per-change register

Capture each change request in a single table so approvers can compare impact and history consistently. Make the impact statement practical enough that readers understand what changes if the request is accepted.

- Example types: scope, cost, schedule, quality.
- Example decisions: approved, rejected, deferred, approved with conditions.

| Change ID | Requestor | Type | Impact summary | Decision | Decision-maker | Date |
| --- | --- | --- | --- | --- | --- | --- |
| CHG-001 | [Role or name] | [Scope/Cost/Schedule/Quality] | [What changes and why it matters] | [Approved/Rejected/Deferred] | [Board or role] | [YYYY-MM-DD] |

## Related documents

- [03_board_terms_of_reference_TEMPLATE.md](03_board_terms_of_reference_TEMPLATE.md) — defines who has authority to approve major changes.
- [06_raid_register_TEMPLATE.md](06_raid_register_TEMPLATE.md) — provides the risks, issues, and dependencies that often trigger changes.
- [../07_delivery/04_status_report_TEMPLATE.md](../07_delivery/04_status_report_TEMPLATE.md) — surfaces change decisions that affect the current reporting period.
