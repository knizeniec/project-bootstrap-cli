---
title: Stakeholder Register Template
status: draft
record_class: canonical
audience: [internal, manager, client]
owner: project-manager
capability: governance
phase: planning
cadence: monthly
last_reviewed: 2026-05-07
---

# Stakeholder Register Template

> **Purpose**: track who is affected by the project, their influence, and how the team should engage with them.
> **Audience**: project managers, sponsors, managers, and client stakeholders coordinating engagement and approvals.
> **When to update**: update when stakeholders change, sentiment shifts, or the engagement plan needs adjustment.

## How to use this template

Use this template as the single register for stakeholder analysis and engagement planning. Keep one row per stakeholder or stakeholder group, then summarize the key engagement actions beneath the table.

- Mandatory row fields: name or group, role, interest, influence, preference, sentiment.
- Optional: substitute named individuals with role groups if privacy or churn is a concern.
- Remove stale names and duplicate groups during each review cycle.

## What not to include

- **Personal contact data beyond role and preferred channel** — do not record personal email addresses, phone numbers, or home details; use role-based or official contact methods only to protect privacy.
- **Status updates or delivery progress** — delivery progress belongs in the status report ([../07_delivery/04_status_report_TEMPLATE.md](../07_delivery/04_status_report_TEMPLATE.md)). This register tracks who stakeholders are and how to engage them, not what is happening.
- **Meeting minutes or individual stakeholder conversations** — capture those in separate meeting notes or communications logs; this register summarises the overall picture.
- **Detailed communications content or messaging** — message content and scheduling belong in the communications plan ([05_communications_plan_TEMPLATE.md](05_communications_plan_TEMPLATE.md)).
- **Board governance or decision rights** — formal authority is defined in the board terms of reference ([03_board_terms_of_reference_TEMPLATE.md](03_board_terms_of_reference_TEMPLATE.md)).

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

## Per-stakeholder register

Capture stakeholders in a structured table so the team can prioritize communication and approval effort. Keep the entries current enough that a new delivery lead can understand the political landscape quickly.

- Example columns: name or group, role, interest, influence, communication preference, current sentiment.
- Example rows: executive sponsor, client approver, service desk lead, data protection reviewer.

| Stakeholder | Role | Interest | Influence | Communication preference | Current sentiment |
| --- | --- | --- | --- | --- | --- |
| [Stakeholder or group] | [Role] | [High/Medium/Low] | [High/Medium/Low] | [Forum, email, workshop, demo] | [Supportive, neutral, concerned] |

## Engagement plan

Summarize the actions the team will take for the most important stakeholders and groups. Focus on what the team must do differently for advocates, blockers, and decision-makers.

- Example: hold fortnightly demos for impacted operations leads.
- Example: brief the sponsor before each stage gate and major change request.

## Related documents

- [05_communications_plan_TEMPLATE.md](05_communications_plan_TEMPLATE.md) — turns stakeholder analysis into scheduled communication actions.
- [03_board_terms_of_reference_TEMPLATE.md](03_board_terms_of_reference_TEMPLATE.md) — identifies which stakeholder roles sit in formal governance forums.
- [../01_strategy/01_product_vision_TEMPLATE.md](../01_strategy/01_product_vision_TEMPLATE.md) — helps explain the proposition different stakeholder groups need to support.
