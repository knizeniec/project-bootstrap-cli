---
title: User Documentation
status: draft
record_class: canonical
audience: [end_user, internal]
owner: Documentation maintainers
capability: user_docs
phase: n/a
cadence: per-release
last_reviewed: 2026-05-07
---

# User Documentation

> **Purpose**: provide the Diátaxis-structured end-user documentation area for tutorials, task guides, reference material, explanations, and release history.
> **Audience**: end users looking for help and internal teams maintaining a user-facing documentation set.
> **When to update**: update when the user-doc structure, navigation, doc-type guidance, or release-history entry points change.

## How to use this template

Use this landing page to route readers to the right kind of help before writing or extending user-facing docs. Choose the Diátaxis mode that matches the reader's need, then keep the detailed content in that mode's folder.

- Start with the mode guides below before creating a new user-facing page.
- Keep end-user instructions in this area and link to internal-only docs instead of copying them.
- Remove placeholder wording when turning templates into active product documentation.

## Choose the right documentation mode

Diátaxis separates learning, task completion, factual lookup, and understanding so readers can find the right help quickly. Use one mode per page unless a strong user need justifies splitting the content across linked pages.

- [Tutorials](tutorials/README.md) — for guided learning by doing.
- [How-to guides](how-to/README.md) — for solving a specific user task.
- [Reference](reference/README.md) — for precise facts, fields, commands, or options.
- [Explanation](explanation/README.md) — for concepts, mental models, and tradeoffs.

## Reading paths

Different readers arrive with different intents, so route them by need instead of by team structure. These reading paths help contributors choose where new content belongs and help users get to the right answer with fewer clicks.

- New user onboarding: [tutorials/README.md](tutorials/README.md) → [01-getting-started-TEMPLATE.md](tutorials/01-getting-started-TEMPLATE.md)
- User trying to complete a task: [how-to/README.md](how-to/README.md) → [01-perform-a-common-task-TEMPLATE.md](how-to/01-perform-a-common-task-TEMPLATE.md)
- User checking exact behavior: [reference/README.md](reference/README.md) → [01-feature-reference-TEMPLATE.md](reference/01-feature-reference-TEMPLATE.md)
- User asking “why does it work this way?”: [explanation/README.md](explanation/README.md) → [01-concept-explanation-TEMPLATE.md](explanation/01-concept-explanation-TEMPLATE.md)

## Release history

Release history complements the four Diátaxis modes by telling users what changed and what to watch during upgrades. Keep release notes focused on a specific release, and keep the changelog as the rolling summary across releases.

- [Release notes](release-notes/2026-05-07-template-bootstrap.md) — release-specific highlights, fixes, and upgrade notes.
- [CHANGELOG.md](CHANGELOG.md) — Keep a Changelog style summary across releases.

## Which template should I use?

Pick the smallest set of templates that matches your project's risk and scope. Add more only when justified by complexity, regulation, or stakeholder need.

| User-doc maturity | Recommended templates | Skip |
|---|---|---|
| **Alpha** — feature is functional but unstable; internal or design-partner users only; content will change significantly before GA; no public documentation commitment | [tutorials/README.md](tutorials/README.md) and [tutorials/01-getting-started-TEMPLATE.md](tutorials/01-getting-started-TEMPLATE.md) — a single guided walkthrough so early users can reach a first success quickly | [how-to/README.md](how-to/README.md), [reference/README.md](reference/README.md), [explanation/README.md](explanation/README.md) — premature until the interface and behaviour are stable enough to document reliably |
| **Beta** — feature is stable enough for broader testing; external users onboarding; task completion and factual accuracy become important; public changelog expected | Tutorials plus [how-to/README.md](how-to/README.md) and [how-to/01-perform-a-common-task-TEMPLATE.md](how-to/01-perform-a-common-task-TEMPLATE.md) — task-completion guides for the most common user scenarios; [reference/README.md](reference/README.md) and [reference/01-feature-reference-TEMPLATE.md](reference/01-feature-reference-TEMPLATE.md) — precise field, command, or option reference; [CHANGELOG.md](CHANGELOG.md) — rolling change summary for users tracking updates | [explanation/README.md](explanation/README.md) — add only when users ask "why does it work this way?" at scale |
| **GA** — feature is fully released; users include a wide range of technical levels; support load is a concern; long-term maintainability matters | Full set: all beta templates plus [explanation/README.md](explanation/README.md) and [explanation/01-concept-explanation-TEMPLATE.md](explanation/01-concept-explanation-TEMPLATE.md) — concepts and mental models that reduce support queries; release notes for each release in [release-notes/](release-notes/) | Nothing — all Diátaxis modes apply |

**Rules of thumb:**
- Prioritise tutorials first: a user who cannot reach their first success in a tutorial will not read reference material.
- Add explanation pages only when the same conceptual question appears repeatedly in support channels or user feedback — not as a default for every feature.
- Keep each page in one Diátaxis mode; a page that mixes tutorial and reference content serves neither reader well and is harder to maintain as the product changes.

## Related documents

- [tutorials/README.md](tutorials/README.md) — explains when to write learning-oriented tutorial content.
- [how-to/README.md](how-to/README.md) — explains when to write task-oriented procedural guidance.
- [reference/README.md](reference/README.md) — explains when to write factual lookup material.
- [explanation/README.md](explanation/README.md) — explains when to write conceptual background.
- [release-notes/2026-05-07-template-bootstrap.md](release-notes/2026-05-07-template-bootstrap.md) — seeds release-note structure for end-user updates.
- [CHANGELOG.md](CHANGELOG.md) — provides the repository-wide end-user change history format.
