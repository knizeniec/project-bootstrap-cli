---
title: Readiness Tracker Template
status: draft
record_class: canonical
audience: [internal, manager]
owner: release-manager
capability: execution
phase: monitoring
cadence: weekly
last_reviewed: 2026-05-07
source_of_truth: repo
---

# Readiness Tracker Template

> **Purpose**: track whether delivery, testing, release, and operational domains are ready to pass the next gate or release decision.
> **Audience**: delivery leads, release managers, and managers reviewing go/no-go readiness.
> **When to update**: update whenever readiness evidence changes and review at least weekly near release milestones.

## How to use this template

Use this template as the canonical view of release and stage readiness across key domains. Keep statuses evidence-based, and link to the proof needed for each domain rather than replacing it with narrative.

- Mandatory: domain, status, owner, and gating evidence link.
- Optional: note required recovery actions for amber or red items.
- Remove green-by-assumption entries that do not yet have evidence.

## What not to include

- **Evidence content itself** — link to evidence artefacts rather than embedding them. This tracker is the status view; the evidence lives in the source documents.
- **Status report narrative** — delivery progress belongs in the status report ([04_status_report_TEMPLATE.md](04_status_report_TEMPLATE.md)). The readiness tracker provides the domain-level evidence links that the status report references.
- **Defect records or test run results** — defect tracking belongs in the defect management process ([../05_testing_acceptance/05_defect_management_TEMPLATE.md](../05_testing_acceptance/05_defect_management_TEMPLATE.md)). Surface the overall defect status as a readiness indicator; link to the defect tool.
- **Architecture decisions** — design belongs in ADRs. A readiness domain may check that a required ADR exists and is accepted; the ADR content stays there.
- **RAID entries** — individual risks belong in the RAID register ([../00_governance/06_raid_register_TEMPLATE.md](../00_governance/06_raid_register_TEMPLATE.md)). The readiness tracker surfaces whether RAID-related risks have acceptable mitigations.

## Frontmatter quick reference

This template's typical frontmatter values. The full schema (with all enums and conditional rules) is at [docs/00_operating_model/04_frontmatter_schema.md](../00_operating_model/04_frontmatter_schema.md).

| Field | Typical value here | Notes |
|---|---|---|
| `status` | `draft` → `active` → `superseded` | Use `superseded` when replaced; required `superseded_by:` link |
| `record_class` | `canonical` | This template defines a canonical artifact |
| `audience` | `[internal, manager]` | Add `client` only when client-export-safe |
| `capability` | `execution` | Fixed for this folder |
| `phase` | `monitoring` | One of `initiation`, `planning`, `execution`, `monitoring`, `closure`, `n/a` |
| `cadence` | `weekly` | One of `ad-hoc`, `weekly`, `monthly`, `per-stage`, `per-release`, `one-shot` |

> When `capability: execution`, both `cadence` and `source_of_truth` are required by the validator. Add `source_of_truth: repo` (or the appropriate value) to the frontmatter.

## Per-readiness-domain register

Track readiness by domain so the team can spot weak areas early and focus action where it matters most. The listed domains should cover the whole delivery path from scope stability through operations preparedness.

- Example domains: scope, design, build, test, release, operations.
- Example statuses: green, amber, red, not started.

| Domain | Status | Owner | Gating evidence link |
| --- | --- | --- | --- |
| [Scope/Design/Build/Test/Release/Ops] | [Green/Amber/Red] | [Role or team] | [Link or reference] |

## Related documents

- [04_status_report_TEMPLATE.md](04_status_report_TEMPLATE.md) — summarizes the readiness picture in the weekly management report.
- [07_release_plan_TEMPLATE.md](07_release_plan_TEMPLATE.md) — uses readiness state to confirm release scope and sequence.
- [../00_governance/09_stage_gate_checklist_TEMPLATE.md](../00_governance/09_stage_gate_checklist_TEMPLATE.md) — checks readiness evidence before stage or release approval.
