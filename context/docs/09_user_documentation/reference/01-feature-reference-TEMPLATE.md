---
title: Feature Reference Template
status: draft
record_class: canonical
audience: [end_user]
owner: Documentation maintainers
capability: user_docs
phase: n/a
cadence: per-release
last_reviewed: 2026-05-07
---

# Feature Reference Template

> **Purpose**: provide a fillable scaffold for documenting a user-facing feature, command, screen, or interface as structured reference material.
> **Audience**: end users who need exact supported behavior, option details, examples, and error information.
> **When to update**: update when signatures, parameters, fields, defaults, limits, or returned errors change.

## How to use this template

Copy this file when documenting a product surface that users will consult for exact facts. Keep the layout stable so users can scan quickly and compare entries across similar pages.

- Lead with an index if the page covers multiple items.
- Use one consistent entry format for every command, field, or object.
- Remove narrative guidance that belongs in how-to or explanation pages.

## What not to include

- **Step-by-step task instructions** — procedures belong in how-to guides ([../how-to/01-perform-a-common-task-TEMPLATE.md](../how-to/01-perform-a-common-task-TEMPLATE.md)). Reference pages are for looking up exact facts, not for completing a task sequence.
- **Conceptual explanations or design tradeoffs** — rationale and mental model content belong in explanation pages ([../explanation/01-concept-explanation-TEMPLATE.md](../explanation/01-concept-explanation-TEMPLATE.md)). A brief description of what an item does is appropriate; background on why it was designed that way is not.
- **First-run tutorials or learning exercises** — guided onboarding belongs in tutorials ([../tutorials/01-getting-started-TEMPLATE.md](../tutorials/01-getting-started-TEMPLATE.md)). Reference pages assume the reader already knows the product.
- **Internal system specifications or API contracts for developers** — internal interface contracts belong in the interface control document ([../../03_architecture/06_interface_control_document_TEMPLATE.md](../../03_architecture/06_interface_control_document_TEMPLATE.md)). This template documents user-facing behavior, not implementation-level interfaces.
- **Release notes or version history** — changes over time belong in release notes or the changelog. Reference pages document current behavior, not its history.

## Frontmatter quick reference

This template's typical frontmatter values. The full schema (with all enums and conditional rules) is at [docs/00_operating_model/04_frontmatter_schema.md](../../00_operating_model/04_frontmatter_schema.md).

| Field | Typical value here | Notes |
|---|---|---|
| `status` | `draft` → `active` → `superseded` | Use `superseded` when replaced; required `superseded_by:` link |
| `record_class` | `canonical` | This template defines a canonical artifact |
| `audience` | `[end_user]` | This folder targets end users; add `internal` only for dual-purpose content |
| `capability` | `user_docs` | Fixed for this folder |
| `phase` | `n/a` | One of `initiation`, `planning`, `execution`, `monitoring`, `closure`, `n/a` |
| `cadence` | `per-release` | One of `ad-hoc`, `weekly`, `monthly`, `per-stage`, `per-release`, `one-shot` |

## Surface listing

List the documented surface at a glance before diving into per-item details. This helps users jump directly to the command, field, option, or section they need.

- Example: commands and subcommands, settings sections, API endpoints, or UI panels.
- Example: limits, supported roles, or version availability notes.

## Per-item reference entries

Use one subsection per item and keep the same shape for each entry. A predictable structure turns this page into a reliable lookup tool instead of a loose narrative.

### `[item-name]`

- **Signature:** `command --flag <value>` or `FieldName: string`
- **Description:** explain what the item does in one or two short sentences.
- **Examples:** show one or two realistic uses.
- **Errors:** list the common error states or invalid-value responses.

```bash
app-cli feature run --mode example
```

## Examples

Provide working examples that map directly to the documented surface. Keep examples short, valid, and easy to adapt.

- Example command:

  ```bash
  app-cli feature describe --id example-123
  ```

- Example structured value:

  ```json
  {
    "name": "example",
    "enabled": true
  }
  ```

## Errors

Document the main error conditions, what they mean, and what a user should check next. Keep remediation brief and link out to task guides if the fix is procedural.

- Example: `403 forbidden` — user lacks the required role for this action.
- Example: `invalid_option` — supplied value is outside the allowed set listed above.

## Index

End with a quick index if the page is long or covers multiple surfaces. This helps readers return to the exact entry they need after scanning an example or error section.

- Example: alphabetical item list with anchor links.
- Example: group by command family, page section, or object type.

## Related documents

- [README.md](README.md) — explains when reference is the right documentation mode.
- [../how-to/01-perform-a-common-task-TEMPLATE.md](../how-to/01-perform-a-common-task-TEMPLATE.md) — use this for step-by-step task completion using the surface documented here.
- [../tutorials/01-getting-started-TEMPLATE.md](../tutorials/01-getting-started-TEMPLATE.md) — use this for first-run learning paths that introduce the surface gradually.
- [../explanation/01-concept-explanation-TEMPLATE.md](../explanation/01-concept-explanation-TEMPLATE.md) — use this for concepts, architecture, or tradeoffs behind the feature.
- [../release-notes/2026-05-07-template-bootstrap.md](../release-notes/2026-05-07-template-bootstrap.md) — record user-visible surface changes in release-specific notes.
