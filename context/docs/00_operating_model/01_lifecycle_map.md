---
title: Lifecycle Map
status: draft
record_class: canonical
audience: [internal, manager]
owner: Documentation maintainers
capability: operating_model
phase: n/a
cadence: monthly
last_reviewed: 2026-05-07
---

# Lifecycle Map

> **Purpose**: define the standard documentation lifecycle and the evidence expected at each phase.
> **Audience**: internal teams and managers planning, governing, or reviewing work.
> **When to update**: update when lifecycle phases, evidence expectations, or tailoring profiles change.

## How to use this template

Use this map to decide which documents are expected as work moves from idea to closure. Pair it with the tailoring matrix to avoid producing more process than the risk profile needs. Link project-specific artifacts back to the relevant phase so reviews stay traceable.

- Start here when shaping a new project baseline.
- Re-check this page at phase transitions and stage gates.
- Cross-check metadata rules in [04_frontmatter_schema.md](04_frontmatter_schema.md).

## Overview

This lifecycle gives one common operating pattern for canonical project documents. It is intentionally simple: each phase has a purpose, expected evidence, and a clear transition signal. Teams should tailor depth, not invent a new lifecycle unless a durable decision says otherwise.

- Example outcome: a project brief anchors initiation.
- Example outcome: a delivery or implementation plan anchors execution.
- Example outcome: closure records capture release, handoff, and residual risks.

## Phases

The standard phases are initiation, planning, execution, monitoring, and closure. These phases describe how documentation matures, not just how work is scheduled. A single document can support more than one phase, but each phase still needs explicit evidence.

- `initiation`: define the problem, sponsor, scope frame, and approval to proceed.
- `planning`: define requirements, architecture, delivery approach, risks, and controls.
- `execution`: record active plans, change control, readiness, and implementation details.
- `monitoring`: track status, issues, benefits, dependencies, and assurance signals.
- `closure`: capture handoff, retrospective decisions, and archive-ready evidence.

## Per-phase entry, exit, and evidence

Each phase should have a practical entry condition, a reviewable exit condition, and tangible evidence that the condition was met. Keep evidence linked to canonical artifacts rather than restating it in status updates. If a phase is intentionally skipped, record the rationale in the project's operating baseline.

- `initiation` entry: a sponsor asks for work to be framed; exit: scope and sponsorship are accepted; evidence: brief, assumptions, initial risks.
- `planning` entry: work is approved for shaping; exit: delivery approach and controls are agreed; evidence: requirements, design, decision log, plan.
- `execution` entry: implementation is authorized; exit: scoped work is delivered or staged; evidence: implementation plan, change log, readiness tracker.
- `monitoring` entry: work is underway or live; exit: exceptions are resolved or accepted; evidence: status reports, RAID, metrics, approvals.
- `closure` entry: objectives are met or formally stopped; exit: ownership is handed off and residuals are recorded; evidence: release record, lessons, archive references.

## Visual diagram placeholder

Use this section to add a simple lifecycle diagram when the repo adopts one. Keep the visual lightweight and aligned to the phase names on this page so text and diagram never drift. Until then, a text flow is enough for scanning.

- Placeholder flow: `initiation -> planning -> execution -> monitoring -> closure`
- Placeholder note: show review gates between planning and execution, and between monitoring and closure.
- Placeholder note: annotate key evidence sets under each phase, not every possible document.

## Tailoring profiles

The lifecycle supports three tailoring profiles: minimum, standard, and high-assurance. All three use the same phase names, but they differ in how much evidence is required and how formal approvals are. Use [07_tailoring_matrix.md](07_tailoring_matrix.md) for the artifact-level breakdown.

- `minimum`: lean evidence, fast decisions, low external assurance needs.
- `standard`: balanced control set for most internal delivery work.
- `high-assurance`: stronger traceability, approvals, and audit-ready evidence.

## Related documents

- [01_lifecycle_map.md](01_lifecycle_map.md)
- [README.md](README.md)
- [04_frontmatter_schema.md](04_frontmatter_schema.md)
- [07_tailoring_matrix.md](07_tailoring_matrix.md)
- [08_decision_rights_matrix.md](08_decision_rights_matrix.md)
