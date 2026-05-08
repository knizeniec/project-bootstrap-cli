---
title: Lessons Learned Index Template
status: draft
record_class: canonical
audience: [internal, manager]
owner: continuous-improvement-lead
capability: references
phase: closure
cadence: per-release
last_reviewed: 2026-05-07
---

# Lessons Learned Index Template

> **Purpose**: keep a reusable index of delivery, quality, security, and operational lessons that should influence future work.
> **Audience**: internal teams and managers who need to carry learning forward across releases and projects.
> **When to update**: update after post-launch reviews, major incidents, closure reviews, or when action status materially changes.

## How to use this template

Use this template as the reusable memory layer above individual retrospective or post-launch review records. Capture only lessons that should affect future planning, design, testing, operations, or governance behavior.

- Keep one row per lesson, not one row per meeting note.
- Link back to the source review or incident record for detail.
- Track whether the action is still open, adopted, or intentionally retired.

## What not to include

- **Retrospective meeting notes or raw session outputs** — raw retrospective content belongs in dedicated review records. This index captures reusable lessons distilled from those sessions, not the meeting minutes.
- **Post-launch review narrative** — the full review analysis belongs in the post-launch review template ([../06_security_operations/09_post_launch_review_TEMPLATE.md](../06_security_operations/09_post_launch_review_TEMPLATE.md)). Link from here; don't reproduce the narrative.
- **Open defects or incident tickets** — live defects belong in the defect management log ([../05_testing_acceptance/05_defect_management_TEMPLATE.md](../05_testing_acceptance/05_defect_management_TEMPLATE.md)) and incidents in the incident response record ([../06_security_operations/06_incident_response_TEMPLATE.md](../06_security_operations/06_incident_response_TEMPLATE.md)). Lessons that emerged from an incident are fair game here; the incident record itself is not.
- **Action items that belong in a current plan** — if a lesson produces an active delivery action, capture that action in the relevant delivery plan or stage plan ([../07_delivery/03_stage_plan_TEMPLATE.md](../07_delivery/03_stage_plan_TEMPLATE.md)), not only here.
- **Project-specific complaints without generalizable root causes** — keep only lessons that influence future templates, processes, or controls. One-off team friction that cannot be acted on more broadly does not belong here.

## Frontmatter quick reference

This template's typical frontmatter values. The full schema (with all enums and conditional rules) is at [docs/00_operating_model/04_frontmatter_schema.md](../00_operating_model/04_frontmatter_schema.md).

| Field | Typical value here | Notes |
|---|---|---|
| `status` | `draft` → `active` → `superseded` | Use `superseded` when replaced; required `superseded_by:` link |
| `record_class` | `canonical` | This template defines a canonical artifact |
| `audience` | `[internal, manager]` | Add `client` only when client-export-safe |
| `capability` | `references` | Fixed for this folder |
| `phase` | `closure` | One of `initiation`, `planning`, `execution`, `monitoring`, `closure`, `n/a` |
| `cadence` | `per-release` | One of `ad-hoc`, `weekly`, `monthly`, `per-stage`, `per-release`, `one-shot` |

## Index rules

This index should stay short enough to scan but rich enough to reuse.

- Prefer durable lessons over one-off complaints.
- Record the root cause in plain language, not only the symptom.
- If a lesson produced a standards, policy, or template update, link that target too.

## Per-lesson register

| Project or release | What happened | Root cause | Action taken | Status | Detail link |
| --- | --- | --- | --- | --- | --- |
| Release X | Readiness review found rollback evidence too late for approval. | Service acceptance inputs were gathered near the release gate instead of during stage execution. | Add rollback-evidence checkpoints to the readiness tracker and release plan. | Open | [../06_security_operations/09_post_launch_review_TEMPLATE.md](../06_security_operations/09_post_launch_review_TEMPLATE.md) |
| Project Y | Escaped integration defect reached early live use. | Contract assumptions were not covered by the planned verification depth. | Add contract-focused scenarios to the acceptance catalog and verification evidence index. | Planned | [../05_testing_acceptance/03_verification_evidence_index_TEMPLATE.md](../05_testing_acceptance/03_verification_evidence_index_TEMPLATE.md) |

## Lesson themes to watch

Group recurring lessons so maintainers can tell when an isolated issue has become a systemic pattern.

- Governance and decision latency.
- Requirements and acceptance ambiguity.
- Architecture, integration, or environment mismatch.
- Operational readiness, support handoff, and incident response gaps.

## Action follow-through

Every reusable lesson should have a visible path to closure.

- Link governance or process changes to the relevant template update.
- Link delivery changes to active plans, readiness trackers, or release controls.
- Close the loop in a later review once the action has been tested in practice.

## Related documents

- [../06_security_operations/09_post_launch_review_TEMPLATE.md](../06_security_operations/09_post_launch_review_TEMPLATE.md) — primary review source for early-live lessons and follow-up actions.
- [../07_delivery/07_release_plan_TEMPLATE.md](../07_delivery/07_release_plan_TEMPLATE.md) — common destination when lessons change release sequencing or evidence expectations.
- [../05_testing_acceptance/05_defect_management_TEMPLATE.md](../05_testing_acceptance/05_defect_management_TEMPLATE.md) — useful when lessons come from escaped defects or triage failures.
- [04_policy_mappings_TEMPLATE.md](04_policy_mappings_TEMPLATE.md) — useful when lessons trigger policy or control remapping.
