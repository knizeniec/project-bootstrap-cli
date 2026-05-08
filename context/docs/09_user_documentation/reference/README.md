---
title: Reference Overview
status: draft
record_class: canonical
audience: [end_user]
owner: Documentation maintainers
capability: user_docs
phase: n/a
cadence: per-release
last_reviewed: 2026-05-07
---

# Reference Overview

> **Purpose**: explain when to write reference documentation and route readers to exact, lookup-oriented product information.
> **Audience**: end users who need precise facts, fields, commands, options, or error information.
> **When to update**: update when reference-writing conventions, navigation, or supported product surfaces change.

## How to use this template

Use this page when deciding whether content should be optimized for lookup instead of teaching or task completion. Reference pages should be factual, structured, and easy to scan non-linearly.

- Write reference when accuracy and completeness matter more than narrative flow.
- Organize entries consistently so users can compare items quickly.
- Keep procedures and rationale in linked how-to or explanation pages.

## When to write reference

Write reference documentation when the reader asks, “What are the supported options?” or “What does this field mean?” In Diátaxis terms, reference is information-oriented and should minimize interpretation.

- Good fit: command flags, settings fields, API parameters, error codes, limits.
- Bad fit: onboarding lessons, troubleshooting journeys, or design rationale.

## Reference characteristics

Good reference docs use stable headings, tables, signatures, examples, and predictable error descriptions. They should help users locate one fact fast without reading the whole page.

- Include signatures, defaults, allowed values, and error cases where relevant.
- Keep examples short and directly tied to the documented item.

## Diátaxis reference

Use the Diátaxis framework as the standard for keeping reference separate from tutorials, how-to guides, and explanation. Keep this page aligned to the canonical model at [diataxis.fr](https://diataxis.fr/).

- Canonical overview: [Diátaxis](https://diataxis.fr/)
- Local example reference: [01-feature-reference-TEMPLATE.md](01-feature-reference-TEMPLATE.md)

## Related documents

- [01-feature-reference-TEMPLATE.md](01-feature-reference-TEMPLATE.md) — starter scaffold for a lookup-oriented reference page.
- [../tutorials/README.md](../tutorials/README.md) — use this for guided learning.
- [../how-to/README.md](../how-to/README.md) — use this for task completion.
- [../explanation/README.md](../explanation/README.md) — use this for concepts and tradeoffs.
- [../README.md](../README.md) — returns readers to the top-level user-doc navigation.
