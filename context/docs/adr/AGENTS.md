---
name: Project ADR Records
description: Use for work under docs/adr/. Covers ADR authoring and durable implementation decision records.
applyTo: "docs/adr/**"
---

# Project ADR Records

This directory is reserved for Architecture Decision Records.

## Most Important Rules

- Treat `docs/adr/` as the primary source of truth for implementation decisions.
- Update ADRs before implementation when a decision changes.
- Do not place active root documentation here outside `docs/adr/`.

## Required Structure

- Keep ADR canonical files in `docs/adr/`.
- Keep canonical area docs in root `docs/`.

## Relationship to RFCs

- RFCs live in `../03_architecture/rfcs/` and represent proposals that are not yet decided. Use an RFC when structured team review of a change is needed before committing.
- ADRs record a decision that has already been made and should be upheld. Create an ADR when an RFC is accepted and the outcome becomes durable implementation direction.
- Do not write an ADR until the decision is settled; premature ADRs mislead readers about what is actually binding.
- Supersede, do not delete, ADRs when a decision changes — the history of why an earlier approach was chosen and then replaced is part of the architectural record.

## Authoring Rules

- Keep ADRs concise, dated, and explicit about the decision, context, and consequences.
- Link ADRs back to canonical root docs when they refine an active area.
- Use the ADR template as the default starting point for new records.

## Quality Rules

- Keep guidance concise and executable.
- Preserve traceability across requirements, decisions, tests, and controls.
- Do not restate broad repo rules already covered by root or parent `AGENTS.md` files.
