---
title: Role and Audience Map
status: draft
record_class: canonical
audience: [internal, manager]
owner: Documentation maintainers
capability: operating_model
phase: n/a
cadence: monthly
last_reviewed: 2026-05-07
---

# Role and Audience Map

> **Purpose**: define the main reader and decision roles, the supported audience tags, and the reading paths they follow.
> **Audience**: internal teams and managers assigning document ownership, review routes, and export scope.
> **When to update**: update when roles, audience tags, review patterns, or operating-model reading paths change.

## How to use this template

Use this map when deciding who owns a document, who should read it, and which audience tags it should carry. Pair role decisions with the frontmatter schema so metadata and routing stay aligned. When a new role appears, add it here before using it in process guidance elsewhere.

- Use this page before creating approval flows.
- Use this page before defining export bundles.
- Validate audience tags against [04_frontmatter_schema.md](04_frontmatter_schema.md).

## Roles

Roles should describe decision posture and information needs, not job-title vanity. The goal is to give each reader group a stable route into the docs system even if teams change names. Keep the set small enough to be reusable across projects.

- Executive: sponsors outcomes, funding, and major escalations.
- PM: coordinates lifecycle, controls, schedule, and reporting.
- Product: owns problem framing, priorities, and acceptance intent.
- Engineer: owns technical implementation and delivery detail.
- QA: owns verification depth, evidence expectations, and acceptance confidence.
- Ops: owns readiness, supportability, and live-service handoff.
- Compliance: owns policy alignment, required controls, and audit posture.
- End User: consumes user-facing outputs and acceptance cues.
- Client/Sponsor: reviews externally shareable scope, progress, and commitments.

## Audience taxonomy

Audience tags describe who a document is written for, not who happens to have access to it. Use the narrowest set that still reflects real readers because export and review rules depend on these tags. The allowed values are fixed by the schema and should not be improvised locally.

- `internal`: delivery and support teams working inside the repo.
- `manager`: accountable reviewers, sponsors, or portfolio owners.
- `client`: external commercial or delivery stakeholders.
- `end_user`: users of the product or service.
- `auditor`: reviewers checking compliance, traceability, or control evidence.

## Per-role primary docs and decision posture

Each role should have a small set of primary documents and a clear default posture in decisions. This avoids making every role read everything and helps reviewers know where to intervene. The examples below are defaults, not hard limits.

- Executive: reads brief, roadmap, status summary; posture: approve funding, scope shifts, and escalations.
- PM: reads lifecycle, RAID, plans, change log; posture: coordinate, escalate, and recommend.
- Product: reads vision, PRD, roadmap, acceptance docs; posture: prioritize and accept fit-for-purpose scope.
- Engineer: reads design, ADRs, implementation plan, interface docs; posture: propose and execute technical choices.
- QA: reads requirements traceability, test strategy, readiness; posture: challenge evidence sufficiency.
- Ops: reads runbooks, release and cutover plans; posture: approve operability and support readiness.
- Compliance: reads policy, control evidence, audit bundle; posture: require control completion or exceptions.
- End User: reads user docs and release notes; posture: consume and provide feedback.
- Client/Sponsor: reads client-safe brief, roadmap, status; posture: confirm commitments and outcomes.

## Reading paths

Reading paths should give each audience the minimum coherent route through the docs tree. Start with orientation, then route to the artifacts that support that role's typical decisions. Keep paths short so they remain usable during onboarding and reviews.

- Manager path: [README.md](README.md) -> [01_lifecycle_map.md](01_lifecycle_map.md) -> [07_tailoring_matrix.md](07_tailoring_matrix.md) -> [08_decision_rights_matrix.md](08_decision_rights_matrix.md)
- Delivery lead path: [01_lifecycle_map.md](01_lifecycle_map.md) -> [03_source_of_truth_map.md](03_source_of_truth_map.md) -> [06_naming_and_structure.md](06_naming_and_structure.md)
- Compliance path: [04_frontmatter_schema.md](04_frontmatter_schema.md) -> [05_audience_and_export_profiles.md](05_audience_and_export_profiles.md) -> [08_decision_rights_matrix.md](08_decision_rights_matrix.md)

## Related documents

- [01_lifecycle_map.md](01_lifecycle_map.md)
- [04_frontmatter_schema.md](04_frontmatter_schema.md)
- [05_audience_and_export_profiles.md](05_audience_and_export_profiles.md)
- [08_decision_rights_matrix.md](08_decision_rights_matrix.md)
