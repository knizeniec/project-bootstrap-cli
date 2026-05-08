---
title: Superpowers Working History
status: active
record_class: supporting
audience: [internal]
owner: Planning maintainers
capability: knowledge
phase: n/a
cadence: ad-hoc
last_reviewed: 2026-05-07
---

# Superpowers Working History

> **Purpose**: define `docs/superpowers/` as tracked working history rather than canonical documentation.
> **Audience**: contributors reviewing or preserving specs, plans, and work records.
> **When to update**: update when the handling rules for planning records change.

This folder stores specs, plans, and other working records created during implementation. It exists for traceability and collaboration, not as the source of truth for current product, architecture, governance, delivery, operations, or user documentation.

## Folders

- [specs/README.md](specs/README.md) for date-stamped design notes and scoped specs.
- [plans/README.md](plans/README.md) for date-stamped implementation plans.
- [../99_archive/repo/superpowers/README.md](../99_archive/repo/superpowers/README.md) for archived repository-specific specs and plans.

## Rule

If a decision or requirement still matters after the work is done, move it into the active root docs tree or the relevant ADR. Do not cite `superpowers/` as canonical guidance.

## Related documents

- [../00-source-of-truth.md](../00-source-of-truth.md) — canonical ownership map.
- [../adr/README.md](../adr/README.md) — durable implementation decisions.
- [../README.md](../README.md) — root docs entry point.
- [../99_archive/README.md](../99_archive/README.md) — evidence-only storage for retired material.
- [../99_archive/repo/superpowers/README.md](../99_archive/repo/superpowers/README.md) — archived repository working history.
