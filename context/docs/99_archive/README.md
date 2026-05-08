---
title: Archive Documentation
status: active
record_class: supporting
audience: [internal, auditor]
owner: Documentation maintainers
capability: knowledge
phase: closure
cadence: ad-hoc
last_reviewed: 2026-05-07
---

# Archive Documentation

> **Purpose**: define `docs/99_archive/` as evidence-only storage for retired or historical material.
> **Audience**: maintainers and auditors checking traceability without treating archived files as current guidance.
> **When to update**: update when archive intake or retrieval rules change.

This area stores historical records, superseded material, and evidence kept for traceability. It does not define the active baseline.

Repository-specific archived documentation lives under [repo/README.md](repo/README.md).

## Use this area for

- retired documents kept for auditability
- evidence snapshots that support past decisions
- records that must remain available but are no longer current
- repository-specific archived ADRs, specs, and plans under `repo/`

## Rule

If historical content becomes active again, move it back into the root docs tree or the relevant ADR instead of treating the archive as canonical.

## Related documents

- [../00-source-of-truth.md](../00-source-of-truth.md) — active ownership map.
- [../superpowers/README.md](../superpowers/README.md) — working-history records.
- [repo/README.md](repo/README.md) — archived repository-specific ADRs and working records.
- [../INDEX.md](../INDEX.md) — full navigation across the active tree.
- [../README.md](../README.md) — root docs entry point.
