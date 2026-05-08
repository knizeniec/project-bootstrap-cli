---
title: RFC Directory
status: active
record_class: supporting
audience: [internal]
owner: architecture-maintainer
capability: architecture
phase: planning
cadence: monthly
last_reviewed: 2026-05-07
---

# RFC Directory

> **Purpose:** define when to write an RFC, how the review lifecycle works, and how accepted proposals graduate into ADRs.
> **Audience:** architects, engineers, and reviewers collaborating on non-final technical proposals that affect more than one component or team.
> **When to update:** update when RFC lifecycle rules, review expectations, or the ADR handoff process change.

## How to use this template

This README governs the RFC process for this repository. Read it before authoring your first RFC and refer back to it when reviewing proposals.

- To open a new RFC, copy [RFC-000-template.md](RFC-000-template.md), assign the next available number, and place it in this directory.
- Keep one RFC focused on one proposal or a tightly related set of options. Split broad topics into separate RFCs.
- Once an RFC is accepted and the decision becomes durable implementation direction, create an ADR and link it from the RFC's Outcome section.
- Archive rejected and superseded RFCs in place — do not delete them. Their reasoning is part of the decision record.

## When to write an RFC

Open an RFC when a proposed change meets one or more of the following criteria. If you are unsure whether your change qualifies, err on the side of opening one — the review process is lightweight and the traceability is valuable.

**Open an RFC when:**
- The decision affects more than one component, service, or team. Cross-boundary decisions benefit from structured input before the approach is locked in.
- A new external dependency (vendor, protocol, or platform) is being introduced. These are hard to reverse and warrant explicit review.
- A cross-team interface is being changed — an API contract, event schema, or shared data model that other teams depend on.
- An architectural pattern is being adopted or deprecated for the first time in this repository.
- The team disagrees on the best approach and needs a structured forum to resolve the question.

**You do not need an RFC when:**
- The change is confined to a single component or team and does not affect shared interfaces.
- The decision is already documented in an ADR and you are implementing it rather than re-evaluating it.
- The change is a routine refactor, dependency upgrade, or bug fix that does not alter architectural behaviour.

## Lifecycle states

An RFC moves through the following states in its frontmatter `status` field and body Status section. Update both when the state changes.

| State | Meaning |
|---|---|
| **Draft** | The author is shaping the proposal. Not ready for formal review. |
| **Proposed** | The RFC is complete and formally open for review. The review window, reviewers, and decision owner are named. |
| **Accepted** | The review closed with approval. The Outcome section records the rationale and links the resulting ADR (if applicable). |
| **Rejected** | The review closed without approval. The Outcome section records why. The file is retained for traceability. |
| **Superseded** | Replaced by a later RFC or ADR. The Outcome section links the successor. |

## Review cadence and process

Keep RFC review lightweight and time-bounded. A long-running RFC that never reaches a decision is worse than no RFC at all.

1. **Author** sets a review window (recommended: 5–10 working days) and names reviewers and a decision owner when moving the RFC to `proposed`.
2. **Reviewers** comment on the unresolved questions, drawbacks, and alternatives. Use pull-request comments or a linked discussion thread rather than email.
3. **Decision owner** synthesises feedback and records the decision in the RFC's Outcome section, updating status to `accepted` or `rejected`.
4. For complex proposals, a synchronous review meeting (30–60 minutes) may be called, but the written record must still be updated after the meeting.

## Graduation to ADR

An accepted RFC becomes an ADR when the decision is durable — meaning the team intends to hold to it across multiple releases or teams, and future decisions should reference it.

**Graduation steps:**
1. Create a new ADR file using [../../adr/ADR-000-template.md](../../adr/ADR-000-template.md) and assign the next ADR number.
2. In the ADR, reference the RFC in the "Related documents" metadata field.
3. In the RFC Outcome section, link the resulting ADR and set RFC status to `accepted`.
4. Update the ADR README index if one is maintained.

Not every accepted RFC needs to graduate to an ADR. If the decision is short-lived, experimental, or specific to a single release, the RFC record alone is sufficient.

## Relationship to ADRs

RFCs and ADRs serve complementary but distinct purposes:

| RFC | ADR |
|---|---|
| Invites structured review of a proposal that is not yet decided | Records a decision that has already been made and should be upheld |
| May be rejected; the rejection is itself valuable | Superseded rather than deleted; the reason for supersession is documented |
| Lives in `docs/03_architecture/rfcs/` | Lives in `docs/adr/` |
| Authored by the proposer; reviewed by the team | Authored by the decision-maker; consulted parties are named |
| Status: draft → proposed → accepted/rejected/superseded | Status: proposed → accepted → superseded/archived |

## Related documents

- [RFC-000-template.md](RFC-000-template.md) — the default RFC authoring template with full section guidance.
- [../../adr/README.md](../../adr/README.md) — ADR process, authority rules, and the index of durable decisions.
- [../../adr/ADR-000-template.md](../../adr/ADR-000-template.md) — the target format for durable accepted decisions.
- [../01_solution_design_TEMPLATE.md](../01_solution_design_TEMPLATE.md) — the architecture baseline that accepted RFCs typically update or extend.
