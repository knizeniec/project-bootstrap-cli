---
title: Frontmatter Schema
status: draft
record_class: canonical
audience: [internal, manager]
owner: Documentation maintainers
capability: operating_model
phase: n/a
cadence: monthly
last_reviewed: 2026-05-07
---

# Frontmatter Schema

> **Purpose**: define the normative frontmatter contract for canonical docs in this repository and how it is validated.
> **Audience**: internal teams and managers creating, reviewing, or validating canonical records.
> **When to update**: update when schema fields, allowed values, or validator commands change.

## How to use this template

Use this page as the single reference for YAML frontmatter on canonical Markdown artifacts. Copy the examples, then validate locally before asking others to rely on the file. Keep this page aligned to the validator implementation rather than to informal preference.

- Start here before creating new canonical docs.
- Re-check this page when validation errors mention metadata.
- Pair it with [01_lifecycle_map.md](01_lifecycle_map.md) when choosing `phase` and review cadence.

## Normative schema

The schema is strict: unknown keys are not allowed, required keys must be present, and some fields become required only in specific cases. Canonical operating-model docs in this folder use the full baseline set requested by the plan. Supporting and historical records may use a different subset when the validator permits it.

- Required keys: `title`, `status`, `record_class`, `audience`, `owner`, `capability`
- Conditional keys: `superseded_by` when `status: superseded`; `cadence` and `source_of_truth` when `capability: execution`
- Optional keys: `phase`, `last_reviewed`, `supersedes`, `source_of_truth`, `tags`

## Allowed values

Allowed values are defined by the validator enums and should be treated as the contract. Reuse these exact strings so automation can classify records consistently. If a new value is needed, change the schema and validator first.

- `status`: `draft`, `proposed`, `accepted`, `active`, `superseded`, `archived`
- `record_class`: `canonical`, `supporting`, `historical`
- `audience`: `internal`, `manager`, `client`, `end_user`, `auditor`
- `capability`: `governance`, `product`, `architecture`, `execution`, `quality`, `operations`, `knowledge`, `references`, `user_docs`, `operating_model`, `strategy`, `ai_governance`
- `phase`: `initiation`, `planning`, `execution`, `monitoring`, `closure`, `n/a`
- `cadence`: `ad-hoc`, `weekly`, `monthly`, `per-stage`, `per-release`, `one-shot`

## Validation rules

Validation checks syntax, schema conformance, supersession integrity, and stale-review warnings. Keep YAML simple, avoid empty required strings, and do not invent extra metadata keys. Treat warnings as prompts to review, even when they are not hard errors.

- Rule: `audience` must contain at least one supported value.
- Rule: `status: superseded` must include `superseded_by`.
- Rule: `capability: execution` must include both `cadence` and `source_of_truth`.
- Rule: stale review warnings depend on `cadence` and `last_reviewed`.

## How to run the validator locally and in CI

Run the validator from the repository root so relative paths resolve predictably. Local validation should happen before review, and CI should repeat the same check so the contract stays machine-enforced. Prefer validating the smallest folder you changed first, then broaden if needed.

- Local folder check: `PYTHONPATH=tools/docs_validator/src python3 -m docs_validator.cli docs/00_operating_model`
- Local file check: `PYTHONPATH=tools/docs_validator/src python3 -m docs_validator.cli docs/00_operating_model/04_frontmatter_schema.md`
- CI expectation: use the same CLI module and fail the job on non-zero exit.

## Examples linking to `docs/_examples/`

Use the seeded examples in `docs/_examples/` as known-good frontmatter references for common audience and lifecycle patterns. They are intentionally small, but they show the minimum metadata contract plus the supersession pattern used by the validator.

- [../_examples/canonical-manager.md](../_examples/canonical-manager.md) — manager-facing canonical artifact.
- [../_examples/canonical-internal.md](../_examples/canonical-internal.md) — internal-only canonical artifact.
- [../_examples/end-user-doc.md](../_examples/end-user-doc.md) — end-user-facing Diataxis-style example.
- [../_examples/replacement.md](../_examples/replacement.md) — replacement target for supersession.
- [../_examples/superseded.md](../_examples/superseded.md) — superseded record pointing to its replacement.

## Related documents

- [01_lifecycle_map.md](01_lifecycle_map.md)
- [04_frontmatter_schema.md](04_frontmatter_schema.md)
- [03_source_of_truth_map.md](03_source_of_truth_map.md)
- [05_audience_and_export_profiles.md](05_audience_and_export_profiles.md)
- [../00-documentation-standards.md](../00-documentation-standards.md)
