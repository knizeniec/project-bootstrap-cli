---
title: ADR-001 Command-First Template Adoption
status: archived
record_class: historical
audience: [internal]
owner: architecture-maintainer
capability: architecture
phase: planning
cadence: per-stage
last_reviewed: 2026-05-07
---

# ADR-001: Adopt tool-native commands as the primary template initialization workflow

- Status: Archived
- Date: 2026-05-07
- Deciders: Template maintainers
- Consulted: Documentation maintainers
- Informed: Contributors and template adopters
- Tags: [Architecture, Delivery]
- Supersedes: None
- Superseded by: [ADR-002-prompt-adoption-with-tracked-assistant-tooling.md](ADR-002-prompt-adoption-with-tracked-assistant-tooling.md)
- Related documents: [../superpowers/specs/2026-05-07-command-first-template-adoption-design.md](../superpowers/specs/2026-05-07-command-first-template-adoption-design.md), [../superpowers/plans/2026-05-07-command-first-template-adoption.md](../superpowers/plans/2026-05-07-command-first-template-adoption.md)

## Context and problem statement

The repository taught adopters to initialize projects through a prompt-first sequence spread across multiple files. That flow required manual prompt selection, had no shared state across runs, and covered only three of the ten numbered documentation domains. A previous draft of this record proposed a skill-first replacement using `skills/project-initialization/` directories; that proposal was revised before any implementation because repo-included skills are not auto-discovered by any of the three supported tools, and the skill-per-tool files duplicated stage logic without mechanical gating.

## Decision drivers

- Give adopters one auto-discoverable starting point per tool after cloning.
- Enforce one-stage-per-run discipline mechanically, not through agent self-discipline.
- Keep stage content in one tool-neutral source; make tool dirs thin wrappers.
- Support adaptive depth: detailed upfront descriptions skip full interview flows.
- Preserve shared review and concern-handling rules across all supported tools.

## Considered options

### Option 1: Keep prompt-first workflow

- Pros: no new tracked surface; keeps current structure.
- Cons: no cross-run state; manual stage selection; shallow domain coverage.

### Option 2: Skill-first (previous draft of this ADR)

- Pros: richer workflow than prompts; shared contract.
- Cons: skills not auto-discovered from repo paths; tool-specific SKILL.md files duplicate stage logic; no mechanical gating.

### Option 3: Tool-native commands (chosen)

- What it is: seven phase commands per tool dir (`.claude/`, `.copilot/`, `.codex/`), each pointing at shared content in `project-initialization/`. A plan file in `docs/superpowers/plans/` is the state machine. Triage writes a tailored artifact roadmap; phase commands honor it. The repository also keeps `.opencode/` as a tracked assistant-native surface for vendored Superpowers skills and OpenCode bootstrap assets.
- Pros: auto-discovered by each tool's native command loader; mechanical gating via plan-file precheck; single source of stage truth; adaptive depth via per-artifact modes.
- Cons: three sets of command files need parity maintenance; vendored `superpowers` plugin must be manually re-synced when upstream changes.

## Decision outcome

Adopt tool-native phase commands in `.claude/`, `.copilot/`, and `.codex/` as the primary template initialization workflow. `prompts/` is removed. The `project-initialization/` directory at repo root is the single source of stage logic, artifact rubrics, profiles, and the shared contract. Keep `.opencode/` as a first-class tracked assistant-native directory for vendored Superpowers and OpenCode bootstrap assets.

### Consequences

- `README.md`, `AGENTS.md`, and `CLAUDE.md` must point to the tool-specific init-triage command.
- The repository must maintain contract parity across the three tool dirs; CI checks enforce this.
- The full `superpowers` asset set (v5.1.0) is vendored into tracked assistant-native directories and updated manually.

## More information

- An earlier draft of this record (committed 2026-05-07) proposed `skills/project-initialization/` as the primary path. That draft was revised on the same day before any implementation landed. The revision history is visible in git log.
- Vendoring source: `/home/hexaper/.claude/plugins/cache/claude-plugins-official/superpowers/5.1.0`.
- This ADR was superseded once the implemented repository kept `prompts/` as the documented adoption flow and tracked assistant-native integration assets in-repo.
