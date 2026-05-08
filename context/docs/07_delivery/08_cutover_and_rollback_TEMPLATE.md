---
title: Cutover and Rollback Template
status: draft
record_class: canonical
audience: [internal, manager]
owner: release-manager
capability: execution
phase: execution
cadence: per-release
last_reviewed: 2026-05-07
source_of_truth: repo
---

# Cutover and Rollback Template

> **Purpose**: provide the ordered cutover and rollback runbooks, decision points, and communications for a controlled release.
> **Audience**: release managers, implementation leads, and managers overseeing production transition.
> **When to update**: update before each release rehearsal and final release approval.

## How to use this template

Use this template when the release plan is stable enough to define the operational sequence step by step. Keep the steps ordered, owned, and time-bounded so a live cutover team can run from the document without ambiguity.

- Mandatory: cutover runbook, rollback runbook, decision points, and communications.
- Optional: rehearsal findings appendix if it directly changes the runbook.
- Remove superseded steps immediately after rehearsal updates so operators see only the current path.

## What not to include

- **Release scope or product acceptance criteria** — what is being released belongs in the release plan ([07_release_plan_TEMPLATE.md](07_release_plan_TEMPLATE.md)) and acceptance catalog. This document describes how to execute the release, not what is included.
- **Architecture decisions or rationale** — design decisions belong in ADRs ([../adr/ADR-000-template.md](../adr/ADR-000-template.md)). Reference the relevant ADR if a cutover step depends on a design choice.
- **Post-release monitoring or incident response** — ongoing operational procedures belong in runbooks ([../06_security_operations/11_runbook_TEMPLATE.md](../06_security_operations/11_runbook_TEMPLATE.md)) and the incident response template ([../06_security_operations/06_incident_response_TEMPLATE.md](../06_security_operations/06_incident_response_TEMPLATE.md)).
- **Communications message drafts** — communications copy belongs in the communications plan ([../00_governance/05_communications_plan_TEMPLATE.md](../00_governance/05_communications_plan_TEMPLATE.md)). List who must be notified and when; keep the message in the comms plan.
- **Test evidence or UAT results** — test evidence belongs in the verification evidence index ([../05_testing_acceptance/03_verification_evidence_index_TEMPLATE.md](../05_testing_acceptance/03_verification_evidence_index_TEMPLATE.md)). A decision point may depend on test evidence; link to it, do not reproduce it.

## Frontmatter quick reference

This template's typical frontmatter values. The full schema (with all enums and conditional rules) is at [docs/00_operating_model/04_frontmatter_schema.md](../00_operating_model/04_frontmatter_schema.md).

| Field | Typical value here | Notes |
|---|---|---|
| `status` | `draft` → `active` → `superseded` | Use `superseded` when replaced; required `superseded_by:` link |
| `record_class` | `canonical` | This template defines a canonical artifact |
| `audience` | `[internal, manager]` | Add `client` only when client-export-safe |
| `capability` | `execution` | Fixed for this folder |
| `phase` | `execution` | One of `initiation`, `planning`, `execution`, `monitoring`, `closure`, `n/a` |
| `cadence` | `per-release` | One of `ad-hoc`, `weekly`, `monthly`, `per-stage`, `per-release`, `one-shot` |

> When `capability: execution`, both `cadence` and `source_of_truth` are required by the validator. Add `source_of_truth: repo` (or the appropriate value) to the frontmatter.

## Cutover runbook (ordered steps, owners, durations)

List the production transition steps in strict order with an owner and expected duration for each. The runbook should be detailed enough to execute but still readable under time pressure.

- Example columns: step, owner, duration, prerequisite, evidence of completion.
- Example steps: freeze changes, deploy artifacts, validate smoke tests, open traffic, monitor.

## Rollback runbook

Define the sequence used if the release must be reversed or paused. Rollback should be treated as a first-class plan, not an afterthought attached to the last line of the cutover.

- Example steps: halt rollout, restore prior version, validate data integrity, notify stakeholders.
- Example conditions: threshold breach, failed critical check, or operational approval withdrawn.

## Decision points

Mark the moments where a human go/no-go decision is required and who makes it. Decision points reduce confusion about when the team may proceed automatically versus when it must pause.

- Example: final go after readiness review and before production deployment.
- Example: continue, pause, or rollback after smoke-test validation.

## Comms

List the time-sensitive communications that must accompany cutover and rollback actions. Keep the comms sequence aligned with the communications and release plans so no critical audience is missed.

- Example: announce start of maintenance window, go-live confirmation, rollback notification.
- Example: update sponsor, service desk, client contact, and operations channel.

## Related documents

- [07_release_plan_TEMPLATE.md](07_release_plan_TEMPLATE.md) — provides the release-level scope, success criteria, and communication intent.
- [02_implementation_plan_TEMPLATE.md](02_implementation_plan_TEMPLATE.md) — explains the broader implementation and rollback strategy this runbook executes.
- [../06_security_operations/README.md](../06_security_operations/README.md) — points to the operations area that owns service acceptance and runbook governance.
