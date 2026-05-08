---
title: ADR Directory
status: active
record_class: supporting
audience: [internal]
owner: architecture-maintainer
capability: architecture
phase: planning
cadence: monthly
last_reviewed: 2026-05-07
---

# ADR Directory

> **Purpose**: define `docs/adr/` as the canonical home for durable implementation and architecture decisions.
> **Audience**: architects, engineers, reviewers, and delivery leads who need binding decision records.
> **When to update**: update when ADR policy, lifecycle, or relationship to RFCs changes.

## How to use this template

- Create an ADR when a decision will materially shape implementation, architecture, operations, security, or delivery constraints.
- Keep one ADR focused on one durable decision.
- Treat ADRs here as the primary source of truth when other documents summarize the same decision.

## When to write an ADR

Create an ADR when a decision materially affects one or more of these areas:

- system structure or major component boundaries
- interfaces, contracts, or integration patterns
- data model or storage direction
- security, privacy, compliance, or operational controls
- delivery constraints, rollout strategy, or major implementation tradeoffs
- non-functional requirements such as reliability, performance, scalability, or maintainability

## Status lifecycle

- Proposed — under review and not yet binding.
- Accepted — approved and binding for active work until superseded.
- Superseded — replaced by a later ADR.
- Archived — retained for history without active authority.

## Relationship to RFCs

- Use RFCs in `../03_architecture/rfcs/` for open proposal review.
- Create or update an ADR when the outcome becomes a durable implementation decision.
- RFCs may inform ADRs, but ADRs are the binding record once accepted.

## Authority rules

- `docs/adr/` is the primary source of truth for durable implementation decisions.
- Update ADRs before implementation when a decision changes.
- Other docs may reference ADRs but must not override them.
- Archived repository ADR history is stored under [../99_archive/repo/adr/README.md](../99_archive/repo/adr/README.md); keep this directory focused on active ADR templates and current records.

## Which template should I use?

Pick the smallest set of templates that matches your project's risk and scope. Add more only when justified by complexity, regulation, or stakeholder need.

| Decision type | Recommended templates | Skip |
|---|---|---|
| **ADR** — decision is durable (intended to hold across multiple releases or teams); implementation has already been agreed; future work should reference and uphold this choice | [ADR-000-template.md](ADR-000-template.md) — record context, decision, consequences, and status; register in [INDEX.md](INDEX.md) | [../03_architecture/rfcs/README.md](../03_architecture/rfcs/README.md) — an RFC is not needed once a decision is made; go straight to an ADR |
| **RFC** — decision is not yet made; structured team review is needed; change crosses component, team, or organisational boundaries; alternatives must be evaluated before committing | [../03_architecture/rfcs/RFC-000-template.md](../03_architecture/rfcs/RFC-000-template.md) — open for review, not yet binding; graduate to an ADR when the outcome becomes durable | [ADR-000-template.md](ADR-000-template.md) — do not write an ADR until the decision is settled; premature ADRs mislead readers about what is actually binding |
| **Nothing** — change is confined to a single component, does not alter shared interfaces, is a routine refactor or bug fix, or merely implements an already-accepted ADR | Neither — proceed with implementation and reference any existing ADR in commit messages or PR descriptions | Both — writing an ADR or RFC for every change creates noise that obscures genuinely durable decisions |

**Rules of thumb:**
- When in doubt whether a change warrants an ADR, ask whether a new team member joining in six months would need to understand why this choice was made to work safely in the codebase — if yes, write the ADR.
- Open an RFC rather than an ADR when the team disagrees on approach; the RFC is the forum for that disagreement, and the ADR records the resolution.
- Supersede, do not delete, ADRs when a decision changes — the history of why an earlier approach was chosen and then replaced is part of the architectural record.

## Related documents

- [ADR-000-template.md](ADR-000-template.md) — default starting point for new ADRs.
- [INDEX.md](INDEX.md) — canonical ADR listing with status tracking.
- [../03_architecture/README.md](../03_architecture/README.md) — active architecture document set.
- [../03_architecture/rfcs/README.md](../03_architecture/rfcs/README.md) — proposal-stage review before ADR graduation.
