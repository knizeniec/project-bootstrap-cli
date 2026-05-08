---
title: 2026-05-07 Template Bootstrap Release Notes
status: active
record_class: canonical
audience: [end_user]
owner: Documentation maintainers
capability: user_docs
phase: closure
cadence: one-shot
last_reviewed: 2026-05-07
---

# 2026-05-07 Template Bootstrap Release Notes

> **Purpose**: provide a seed release-note entry that shows the expected structure for user-facing release communications in this documentation area.
> **Audience**: end users reviewing what changed in a specific release and what actions they may need to take.
> **When to update**: do not update after publication except to correct factual errors; create a new file for each new release.

## How to use this template

Use one file per release and keep the content focused on what changed for end users. Prefer short, scannable bullets with links to the relevant tutorial, how-to, reference, or explanation pages.

- Keep highlights brief and user-visible.
- Call out breaking changes and upgrade steps explicitly.
- Move long histories into [../CHANGELOG.md](../CHANGELOG.md).

## Highlights

Summarize the release in a few bullets so readers can decide whether they need to read further. Lead with the most user-visible improvements.

- Bootstrapped the full end-user documentation area under `docs/09_user_documentation/`.
- Added Diátaxis navigation for tutorials, how-to guides, reference, and explanation.
- Added release-history scaffolding with release notes and a changelog.

## Breaking changes

State anything that would require a user to change behavior before or after upgrading. If there are none, say so explicitly.

- None in this template bootstrap release.

## New features

List newly available user-facing capabilities, flows, or documentation entry points. Keep each bullet outcome-oriented.

- New landing page for end-user docs: [../README.md](../README.md)
- New tutorial scaffold: [../tutorials/01-getting-started-TEMPLATE.md](../tutorials/01-getting-started-TEMPLATE.md)
- New task, reference, and explanation scaffolds for future user docs.

## Fixes

Record user-visible fixes or corrections that matter in practice. Keep the wording factual and easy to scan.

- Added complete internal navigation between all end-user documentation modes.
- Standardized frontmatter for validator-compatible user-doc files.

## Known issues

Call out user-visible limitations, gaps, or follow-up work that readers should know about. This section is especially useful when the release ships useful work but not the full intended surface.

- Templates are scaffolds and still require product-specific content before publication to end users.
- Root-level docs navigation outside this folder may still be wired by follow-on tasks.

## Migration notes

Explain whether users or doc maintainers need to move links, rename bookmarks, or adapt workflows. Keep this separate from upgrade instructions when the impact is mostly informational.

- Existing user-facing content should be mapped into the appropriate Diátaxis mode before broad publication.
- Release-history links should point to this folder for user-facing change summaries.

## Upgrade instructions

List the actions a user or maintainer should take after adopting the release. Keep them short and ordered.

1. Start future end-user docs from the correct template in this folder.
2. Add product-specific content and remove placeholder guidance before publishing.
3. Update [../CHANGELOG.md](../CHANGELOG.md) alongside each new user-visible release.

## Related documents

- [../README.md](../README.md) — top-level navigation for the end-user documentation area introduced by this release.
- [../CHANGELOG.md](../CHANGELOG.md) — rolling history that summarizes changes across releases.
- [../tutorials/README.md](../tutorials/README.md) — tutorial mode guidance referenced by this release.
- [../how-to/README.md](../how-to/README.md) — how-to mode guidance referenced by this release.
- [../reference/README.md](../reference/README.md) — reference mode guidance referenced by this release.
- [../explanation/README.md](../explanation/README.md) — explanation mode guidance referenced by this release.
