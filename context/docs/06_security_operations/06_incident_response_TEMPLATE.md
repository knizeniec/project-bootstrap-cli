---
title: Incident Response Template
status: draft
record_class: canonical
audience: [internal, manager]
owner: incident-manager
capability: operations
phase: execution
cadence: ad-hoc
last_reviewed: 2026-05-07
source_of_truth: repo
---

# Incident Response Template

> **Purpose**: define how incidents are classified, managed, communicated, and reviewed from declaration through learning.
> **Audience**: incident managers, operations teams, support teams, security teams, and managers involved in service recovery.
> **When to update**: update when incident classes, response workflow, communication templates, or review expectations change.

## How to use this template

Use this template as the canonical response model for operational and security incidents. Keep it short enough to use under pressure and link to component runbooks for detailed steps.

- Mandatory: severity classes, lifecycle stages, communication templates, and post-incident review expectations.
- Optional: incident tooling screenshots if they materially help responders.
- Remove team-local habits that conflict with the canonical response flow.

## What not to include

- **Individual incident timelines or root cause analysis** — incident-specific records belong in the postmortem template ([12_incident_postmortem_TEMPLATE.md](12_incident_postmortem_TEMPLATE.md)). This template defines the response framework; the postmortem records what happened in a specific event.
- **Detailed restore or failover procedures** — step-by-step recovery belongs in runbooks ([11_runbook_TEMPLATE.md](11_runbook_TEMPLATE.md)) and the backup and recovery template ([07_backup_and_recovery_TEMPLATE.md](07_backup_and_recovery_TEMPLATE.md)).
- **Customer-specific SLA commitments** — SLA obligations belong in the support model ([04_support_model_TEMPLATE.md](04_support_model_TEMPLATE.md)) and customer contracts.
- **Security control or threat model detail** — control design belongs in the security baseline ([01_security_baseline_TEMPLATE.md](01_security_baseline_TEMPLATE.md)) and threat model ([02_threat_model_TEMPLATE.md](02_threat_model_TEMPLATE.md)).
- **Status report content or operational updates** — live incident communications belong in the incident channel; post-incident summaries belong in the status report ([../07_delivery/04_status_report_TEMPLATE.md](../07_delivery/04_status_report_TEMPLATE.md)).

## Frontmatter quick reference

This template's typical frontmatter values. The full schema (with all enums and conditional rules) is at [docs/00_operating_model/04_frontmatter_schema.md](../00_operating_model/04_frontmatter_schema.md).

| Field | Typical value here | Notes |
|---|---|---|
| `status` | `draft` → `active` → `superseded` | Use `superseded` when replaced; required `superseded_by:` link |
| `record_class` | `canonical` | This template defines a canonical artifact |
| `audience` | `[internal, manager]` | Add `client` only when client-export-safe |
| `capability` | `operations` | Fixed for this folder |
| `phase` | `execution` | One of `initiation`, `planning`, `execution`, `monitoring`, `closure`, `n/a` |
| `cadence` | `ad-hoc` | One of `ad-hoc`, `weekly`, `monthly`, `per-stage`, `per-release`, `one-shot` |

> When `capability: execution`, both `cadence` and `source_of_truth` are required by the validator.

## Severity classes

Define the severity levels used to declare and prioritize incidents. The scale should support consistent response mobilization and stakeholder communication.

- Example: Sev-1 complete outage or critical data risk, Sev-2 major degradation, Sev-3 limited-impact issue.
- Example: security incident severity may escalate based on confidentiality or regulatory impact.

## Declare/contain/eradicate/recover/learn lifecycle

Describe the response lifecycle from initial declaration through restoration and learning. Make handoffs between technical response, communications, and review explicit.

- Example: declare and mobilize, contain spread, eradicate root cause, recover service, then document lessons.
- Example: move to post-incident review only after service stability is confirmed.

## Comms templates

Summarize the required communications and where the reusable message templates live. Focus on who needs updates, when, and at what level of detail.

- Example: internal incident channel update every 30 minutes for active major incidents.
- Example: manager or client-facing holding statement approved by incident manager and communications owner.

## Post-incident review

Explain when a formal review is required, what it must cover, and where the outcome is recorded. Tie the review directly to follow-up actions and service improvement.

- Example: all Sev-1 and security incidents require a post-launch review entry within 5 working days.
- Example: repeated Sev-2 pattern may also trigger formal review at service owner discretion.

## Related documents

- [09_post_launch_review_TEMPLATE.md](09_post_launch_review_TEMPLATE.md) — captures the formal learning and follow-up actions after incidents.
- [10_runbook_index_TEMPLATE.md](10_runbook_index_TEMPLATE.md) — points responders to the component runbooks used during containment and recovery.
- [04_support_model_TEMPLATE.md](04_support_model_TEMPLATE.md) — defines intake and escalation routes that often trigger incident declaration.
- [07_backup_and_recovery_TEMPLATE.md](07_backup_and_recovery_TEMPLATE.md) — supports recovery decisions when restore actions are needed.
