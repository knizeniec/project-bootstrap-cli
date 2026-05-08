---
title: ADR-002 Prompt-Based Template Adoption with Tracked Assistant-Native Tooling
status: archived
record_class: historical
audience: [internal]
owner: architecture-maintainer
capability: architecture
phase: planning
cadence: per-stage
last_reviewed: 2026-05-07
---

# ADR-002: Keep prompt-based template adoption and track assistant-native tooling in-repo

- Status: Archived
- Date: 2026-05-07
- Deciders: Template maintainers
- Consulted: Documentation maintainers
- Informed: Contributors and template adopters
- Tags: [Architecture, Delivery]
- Supersedes: [ADR-001-skill-first-template-adoption.md](ADR-001-skill-first-template-adoption.md)
- Superseded by: None
- Related documents: [../../../README.md](../../../README.md), [../../../00-source-of-truth.md](../../../00-source-of-truth.md)

## Context and problem statement

The repository's actual adoption flow still lives in `prompts/` and is the only cross-tool path that is both documented and present in the checked-out template. At the same time, the template now vendors Superpowers 5.1.0 assets for Claude, Copilot, Codex, and OpenCode so each supported harness can read tracked repo-native integration files. ADR-001 described a command-first adoption workflow that does not exist in the current repository state, so the canonical decision record must be updated to match implemented reality.

## Decision drivers

- Keep the documented adoption path accurate to the files that actually exist.
- Preserve the cross-tool prompt workflow that adapts a fresh clone into a real project.
- Track assistant-native integration assets in-repo so supported tools can share the same vendored Superpowers version.
- Use each harness's official repo-native surface where one exists.

## Considered options

### Option 1: Keep prompt-based adoption and track assistant-native tooling (chosen)

- What it is: `prompts/` remains the documented adoption workflow for bootstrap, refinement, architecture, and language adaptation. The repository also tracks `.claude/`, `.copilot/`, `.codex/`, and `.opencode/` vendored assets. Codex uses root `AGENTS.md`, `.codex/hooks.json`, and `.agents/skills/` as its repo-native runtime surface.
- Pros: matches the files that actually exist; works across tools today; keeps vendored harness assets under version control.
- Cons: prompt execution is still manual; vendored `superpowers` content must be re-synced when upstream changes.

### Option 2: Tool-native command-first adoption

- What it is: replace prompts with tracked per-tool command surfaces for repository adoption.
- Pros: one command surface per tool; potentially less manual prompt selection.
- Cons: not implemented in the current repository state; official repo-native surfaces differ by harness; would require additional tracked command files and maintenance.

### Option 3: Skill-first

- What it is: rely primarily on repo-local skill discovery for initialization.
- Pros: richer workflow guidance than raw prompts.
- Cons: repo-local skills are not the same supported surface across all tools; still needs harness-specific integration.

## Decision outcome

Keep `prompts/` as the primary documented template adoption workflow. Also keep `.claude/`, `.copilot/`, `.codex/`, and `.opencode/` as first-class tracked assistant-native directories for vendored Superpowers and harness-specific integration assets. For Codex, use the official repo-native surfaces: root `AGENTS.md`, `.codex/hooks.json`, and `.agents/skills/`.

### Consequences

- `README.md`, `AGENTS.md`, and project-intelligence context files must describe prompt-based adoption and tracked assistant-native tooling consistently.
- Codex skill vendoring has two tracked surfaces that must stay aligned: `.codex/skills/` and `.agents/skills/`.
- The full `superpowers` asset set (v5.1.0) is vendored into tracked assistant-native directories and updated manually.

## Pros and cons of the options

- Chosen option: it matches implemented reality without inventing new entrypoints.
- Rejected option: command-first adoption was rejected because the required tracked command surfaces are not actually present.

## More information

- Earlier drafts discussed command-first and skill-first template initialization approaches. This ADR narrows the decision to the workflow and harness integration model that the repository currently implements.
- Vendoring source: `/home/hexaper/.claude/plugins/cache/claude-plugins-official/superpowers/5.1.0`.
