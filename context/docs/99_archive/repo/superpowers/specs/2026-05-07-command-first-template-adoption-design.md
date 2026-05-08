---
title: Command-First Template Adoption Design
status: draft
record_class: supporting
audience: [internal]
owner: Planning maintainers
capability: knowledge
phase: planning
cadence: ad-hoc
last_reviewed: 2026-05-07
---

# Command-First Template Adoption Design

> **Purpose**: define a tool-native command-first project initialization workflow that replaces both the current prompt-first adoption flow and the previously proposed skill-first variant.
> **Audience**: template maintainers building the initialization workflow surface for Claude, Copilot, and Codex.
> **When to update**: update when the phase catalog, command surface, plan-file contract, or vendoring strategy changes.

## Goal

Ship a repo-included project initialization workflow that drives adopters from a fresh clone through documentation, design, governance, and structural adaptation in mechanically gated phases. Each phase is a tool-native slash command. The workflow is resumable, adaptive in depth based on what the user already knows, and consistent across the three supported AI tools.

## Problem

The repository teaches adopters to initialize projects through a prompt-first sequence (`prompts/project-bootstrap.md` → `refine-specs.md` → `architecture-baseline.md` → `language-adaptation.md`). The flow has three structural weaknesses:

1. Prompt selection is manual; the agent has no state across runs and cannot enforce stage discipline.
2. Stage boundaries, review loops, and concern handling are inconsistent across prompts.
3. The four-prompt sequence covers only ~3 of the 10 numbered docs domains; richer projects (regulated, multi-team, AI-involved) lack first-class artifacts in the flow.

A previously committed ADR-001 proposed a skill-first replacement (`skills/project-initialization/{claude,copilot-codex}/SKILL.md`). On review, the skill-first design carried two flaws: skills are not auto-discoverable from arbitrary repo paths in any of the three target tools, and the per-tool skill files duplicated stage logic without mechanical gating. This design supersedes that proposal in place — ADR-001 is rewritten, not added to.

## Desired outcomes

- A fresh clone has one obvious starting point per tool: `/init-triage`.
- Each phase is a separate command. Phase commands refuse to run if prerequisites are unmet — discipline is mechanical, not aspirational.
- A single tracked plan file (`docs/superpowers/plans/YYYY-MM-DD-project-initialization.md`) is the state machine across runs.
- A triage command up front produces a tailored artifact roadmap — different projects get different artifact sets and different question depths, based on profile + pasted context.
- Stage content lives in one tool-neutral source (`project-initialization/`); each tool dir holds thin commands that point at it. Tool dirs additionally vendor the full `superpowers` plugin for review and orchestration support.
- The workflow is consistent across `.claude/`, `.copilot/`, `.codex/` — only command frontmatter and tool config differs.

## Non-goals

- Auto-loading of any skill or hook on session start. The user must invoke the command explicitly.
- Generation of `docs/09_user_documentation/` content during initialization. User documentation is deliberately deferred to post-initialization.
- A build step that adopters must run before initialization. The repo is usable as-is on clone.
- Tracking upstream `superpowers` changes automatically. Vendoring is manual; updates are deliberate.

## Architecture overview

The template ships **tool-native command surfaces** for Claude, Copilot, and Codex. Each tool dir exposes seven slash commands corresponding to the same seven phases (one triage + six workflow phases). Commands are thin: they load the plan, read shared phase content from a tool-neutral `project-initialization/` directory, run the phase, update the plan, and stop.

Three load-bearing ideas:

1. **Mechanical phase gating via the plan file.** The plan file is a state machine. Each phase command refuses to run if its prerequisite phase is incomplete. One-stage-per-run is enforced by which command was typed and what the plan file says, not by agent self-discipline.
2. **Adaptive depth via triage and per-artifact modes.** `/init-triage` asks routing questions and accepts a pasted project description. From the answers, it writes a tailored artifact list per phase into the plan, with each artifact assigned `mode: interview | confirm | extract`. Phase commands honor the mode at run time. A user who pastes a detailed brief gets `extract` modes; a user starting from scratch gets `interview` modes.
3. **Single source of stage truth.** All stage logic lives in `project-initialization/` at repo root. Tool commands say: read the plan, follow the matching `project-initialization/phases/N-*.md`, stop. No duplication across tools.

## Directory layout

```text
<repo-root>/
├── project-initialization/                     # tool-neutral shared content
│   ├── README.md                               # entrypoint and workflow contract
│   ├── AGENTS.md                               # authoring rules for this dir
│   ├── contract.md                             # stop rules, concern framing, output format
│   ├── plan-template.md                        # initial plan file scaffold
│   ├── review-checklist.md                     # standards + coherence checklist
│   ├── phases/
│   │   ├── 0-triage.md
│   │   ├── 1-intent.md
│   │   ├── 2-specification.md
│   │   ├── 3-design.md
│   │   ├── 4-govern-operate.md
│   │   ├── 5-adapt.md
│   │   └── 6-review.md
│   ├── artifacts/
│   │   ├── project-brief.md
│   │   ├── business-case.md
│   │   ├── prd.md
│   │   ├── requirements-catalog.md
│   │   ├── journeys.md
│   │   ├── solution-design.md
│   │   ├── adr.md
│   │   ├── ai-use-policy.md
│   │   ├── test-strategy.md
│   │   ├── security-baseline.md
│   │   ├── delivery-plan.md
│   │   ├── language-adaptation.md
│   │   └── repo-sync.md
│   └── profiles/
│       ├── product.md
│       ├── internal-tool.md
│       ├── library.md
│       └── platform.md
├── .claude/
│   ├── AGENTS.md
│   ├── README.md
│   ├── commands/
│   │   ├── init-triage.md
│   │   ├── init-intent.md
│   │   ├── init-spec.md
│   │   ├── init-design.md
│   │   ├── init-govern.md
│   │   ├── init-adapt.md
│   │   └── init-review.md
│   ├── agents/
│   │   ├── init-reviewer.md                    # init-specific reviewer
│   │   └── superpowers/                        # vendored superpowers agents
│   ├── skills/
│   │   └── superpowers/                        # vendored superpowers skills (full set)
│   └── (other tool-native config as needed)
├── .copilot/                                   # same shape as .claude/, Copilot frontmatter
├── .codex/                                     # same shape as .claude/, Codex frontmatter
├── docs/
│   ├── adr/
│   │   └── ADR-001-...md                       # rewritten in place
│   └── superpowers/
│       ├── specs/<this file>
│       └── plans/YYYY-MM-DD-project-initialization.md   # plan file (per project)
├── README.md
├── AGENTS.md
├── CLAUDE.md
└── .github/workflows/ci.yml
```

The `prompts/` directory is removed entirely. The doc tree's `_TEMPLATE.md` files under `docs/00_*` through `docs/08_*` remain — they are skeletons that artifact rubrics fill in. `docs/09_user_documentation/` is untouched by initialization.

## Phase and artifact catalog

Six phases plus triage. Each phase has a fixed candidate artifact set; triage decides which become active per project.

| Phase | Command | Candidate artifacts | Output paths |
| --- | --- | --- | --- |
| 0. Triage | `/init-triage` | profile selection, artifact-list write | plan file (Phase Roadmap, Artifact Roadmap) |
| 1. Intent | `/init-intent` | project brief; business case (optional) | `docs/00_governance/00_project_brief.md`; `docs/00_governance/01_business_case.md` |
| 2. Specification | `/init-spec` | PRD; requirements catalog; user journeys; acceptance catalog | `docs/02_product/01_prd.md`, `02_requirements_catalog.md`, `03_user_journeys.md`, `05_acceptance_catalog.md` |
| 3. Design | `/init-design` | solution design; ADR(s); C4 context; C4 container; C4 component | `docs/03_architecture/01_solution_design.md`; `docs/adr/ADR-NNN-*.md`; `docs/03_architecture/02_c4_*.md` |
| 4. Govern & Operate | `/init-govern` | AI use policy; test strategy; security baseline; threat model; delivery plan; standards register | `docs/04_ai_governance/`, `docs/05_testing_acceptance/`, `docs/06_security_operations/`, `docs/07_delivery/`, `docs/08_references/` |
| 5. Adapt | `/init-adapt` | language/stack adaptation; repo sync (`README.md`, `AGENTS.md`, `src/AGENTS.md`, `.gitignore`, `.editorconfig`); routed AGENTS files | repo root and slot directories |
| 6. Final review | `/init-review` | coherence review across all produced artifacts; entry-doc parity check | plan-file `Review Findings`; small fix-up edits |

`docs/09_user_documentation/` is deliberately deferred and never produced during initialization.

### Profile defaults (artifact activations)

| Profile | Phase 1 | Phase 2 | Phase 3 | Phase 4 | Phase 5 |
| --- | --- | --- | --- | --- | --- |
| Library/utility | brief | PRD (lite) | solution design, ADRs | test strategy | language adaptation, repo sync |
| Internal tool | brief | PRD, journeys | solution design, ADRs | test strategy, delivery plan | language adaptation, repo sync |
| Product | brief, business case | PRD, requirements catalog, journeys, acceptance | solution design, ADRs, C4 context | AI policy (if applicable), test strategy, security baseline, delivery plan | language adaptation, repo sync |
| Platform/regulated | brief, business case | full spec set | full design set incl. C4 container | full govern set incl. threat model, standards register | language adaptation, repo sync |

Triage prunes from defaults using rule overrides (e.g., AI involvement = none drops AI policy).

### Boundary rules

- Each phase command runs only artifacts marked active for its phase.
- Phase N command refuses to run if phase N-1 has unresolved blocking artifacts.
- The user can override at triage; overrides are recorded in the plan with rationale.

## Plan file shape

Path: `docs/superpowers/plans/YYYY-MM-DD-project-initialization.md`. Created by `/init-triage`; updated end-of-run by every subsequent phase command. Frontmatter follows the existing `superpowers/plans/` convention.

Body sections, in order:

- `Goal` — one sentence captured during triage.
- `Profile` — selected profile, rationale, and any overrides.
- `Phase Roadmap` — table: phase, status, notes. Statuses: `pending | in-progress | done | active-skipped | fully-skipped | blocked`.
- `Artifact Roadmap` — table: phase, artifact, status, mode (initial), mode (effective), output path. Modes: `interview | confirm | extract | skipped`.
- `Project Facts` — stable facts confirmed across phases. Re-read at the top of every run.
- `Future-Phase Facts` — out-of-phase facts captured during earlier phases, sub-sectioned by target phase/artifact.
- `Open Questions For Current Phase`
- `Parked Questions For Later`
- `Concerns And Recommendations` — five-part advisory entries (concern, why, alternative, trade-off, acceptable-if-intentional).
- `Files Updated This Run`
- `Review Findings` — severity-classified findings from end-of-phase review.
- `Next Recommended Step`
- `Resume Context` — active phase, active artifact, last user input, outstanding clarifications.

The plan file is created only by `/init-triage`. Other commands stop and direct to triage if it is missing. Phase commands write updates in one batch at end-of-run, never incrementally. The user is responsible for committing the plan; commands suggest a commit message but do not auto-commit.

## Triage and per-artifact modes

### `/init-triage` flow

1. **Detect existing state.** If the plan file exists, refuse and tell the user how to proceed (continue or back up and re-triage).
2. **Routing questions** (fixed minimal set):
   - Project type (product / internal tool / library / platform / other)
   - AI involvement (none / consumer / core)
   - Regulatory posture (none / light / heavy)
   - Team shape (solo / small team / multi-team)
   - Existing project description (free text, optional)
3. **Profile selection** from type + posture. Edge cases (e.g., library + heavy regulation) get the heavier profile with a note.
4. **Fact extraction** from any pasted description; populate `Project Facts` and `Future-Phase Facts`.
5. **Build Artifact Roadmap.** Start from profile defaults, apply rule-based prunes (e.g., AI = none drops AI policy), assign initial mode per artifact based on extracted facts:
   - `extract` — sufficient facts captured to draft without questions.
   - `confirm` — partial facts; show extracted content, ask gap-filling questions.
   - `interview` — no usable facts; full question flow.
   Show the proposed roadmap with mode reasoning. User can override.
6. **Write the plan file** and stop.

### Run-time mode behavior

- **`interview`** — run the artifact's full question rubric; produce artifact from collected answers + the doc's `_TEMPLATE.md` skeleton.
- **`confirm`** — show extracted facts, ask gap questions only, produce artifact.
- **`extract`** — produce a complete first-draft artifact with no questions; iterate with the user until accepted.

### Mode promotion at run time

When a phase command starts, it re-evaluates effective modes for its own artifacts given accumulated `Future-Phase Facts`. Promotion is announced ("we captured deployment target during Intent — promoting solution-design from interview to confirm; here's what I have"). Demotion is rare but possible when a user explicitly contradicts an extracted fact. Both initial and effective modes are stored in the plan for transparency.

### Override mechanics

Users can override modes or activate/skip artifacts at any time in chat. Overrides are recorded in the plan's `Profile` section under `Overrides applied` with one-line reasons.

## Phase command flow

Generic lifecycle for every `/init-{intent,spec,design,govern,adapt,review}`:

1. **Precheck** — read plan; refuse if missing or if previous phase is incomplete; confirm with user before re-running a `done` phase.
2. **Load context** — read this phase's `Future-Phase Facts`, all `Project Facts`, this phase's active artifacts.
3. **Effective-mode pass** — for each active artifact, re-evaluate mode given accumulated facts; announce changes.
4. **Artifact walk** — for each active artifact in defined order:
   1. Announce `working on <artifact> in <mode> mode`.
   2. Load rubric from `project-initialization/artifacts/<name>.md`.
   3. Run the mode flow.
   4. Classify each user statement: current-artifact / current-phase-other-artifact / future-phase / off-topic. Route facts accordingly. Future-phase facts go to `Future-Phase Facts`.
   5. Produce the artifact (write the doc).
   6. Mark artifact `done` in plan.
   Phases run all active artifacts end-to-end without intra-phase pauses.
5. **End-of-phase review** — dispatch `init-reviewer` agent with artifacts produced this phase, paths, plan file. Apply objective fixes immediately; record advisory findings in `Review Findings`.
6. **Plan update + stop** — update Phase Roadmap, `Files Updated This Run`, `Review Findings`, `Next Recommended Step`, `Resume Context`. Output structured end-of-run summary. Do not auto-advance.

### `/init-review` (Phase 6)

The final phase produces no new artifacts. It runs:

1. Cross-doc coherence check (using `init-reviewer` with the full doc set).
2. Entry-doc parity check (`README.md`, `AGENTS.md`, `CLAUDE.md` match canonical docs).
3. Link and standards pass on all produced docs.
4. Verdict: `pass` marks Phase 6 done and declares init complete; `concerns` records findings and leaves Phase 6 `in-progress`.
5. Suggest one final commit message; stop.

### `init-reviewer` agent contract

Single shared agent vendored per tool dir; same prompt and output format on each.

- **Inputs:** list of file paths produced or modified this phase, plan file path, phase number, review checklist path.
- **Outputs:** structured findings — severity (`critical | important | minor | advisory`), location, finding, reason, suggested fix.
- **Behavior:** reads `project-initialization/review-checklist.md` for the rubric; does not write to files; returns findings for the phase command to apply. Critical/important must be applied or escalated before continuing; minor/advisory recorded and continued.

### Concerns vs. reviews

Reviews catch quality issues end-of-phase. Concerns flag user-choice issues in flight.

- Concerns are surfaced immediately in chat using the five-part frame (concern, why, alternative, trade-off, acceptable-if-intentional) AND recorded in `Concerns And Recommendations` in the plan.
- Concerns never block the phase from completing.
- Reviews run after artifact production at end-of-phase.

### Stop discipline

Hard rule baked into every phase command: after end-of-phase review and plan update, **stop**. Do not begin the next phase or read in preparation for it. Tell the user the next command and wait.

## Vendored `superpowers` plugin

The full `superpowers` plugin is vendored into each tool dir's `skills/` and `agents/` subdirectories — no curation. Source for vendoring: `/home/hexaper/.copilot/superpowers` (currently version 5.0.7, recorded in upstream `package.json`). Each tool dir's `AGENTS.md` records the source path and version pin so future maintainers know what's vendored.

Vendoring scope per tool dir:

- All 14 skills under `skills/superpowers/` (e.g., `brainstorming`, `writing-plans`, `executing-plans`, `requesting-code-review`, `verification-before-completion`, `dispatching-parallel-agents`, `subagent-driven-development`, `using-superpowers`, `using-git-worktrees`, `systematic-debugging`, `test-driven-development`, `finishing-a-development-branch`, `receiving-code-review`, `writing-skills`).
- The `code-reviewer.md` agent under `agents/superpowers/`.
- The three superpowers commands (`brainstorm`, `execute-plan`, `write-plan`) under `commands/superpowers/` — kept available for general post-init use.

Updates are manual: when upstream `superpowers` changes meaningfully, a maintainer re-syncs all three tool dirs from the source path. Drift between tool dirs is bounded by the source being a single upstream copy.

## Migration from current state

### Files to delete

- `prompts/` (entire directory): `project-bootstrap.md`, `refine-specs.md`, `architecture-baseline.md`, `language-adaptation.md`, `README.md`, `AGENTS.md`. Artifact rubrics under `project-initialization/artifacts/` are written from scratch, not migrated from prompts.
- All references to `prompts/` in entry docs, source-of-truth, INDEX, slot READMEs, and CI guards.

### Files to create

- `project-initialization/` tree (~30 files): README, AGENTS, contract, plan-template, review-checklist, 7 phase files, ~13 artifact rubrics, 4 profile files.
- `.claude/` tree: AGENTS, README, 7 init commands, init-reviewer agent, vendored superpowers (skills/agents/commands).
- `.copilot/` tree: same shape, Copilot frontmatter.
- `.codex/` tree: same shape, Codex frontmatter.

### Files to rewrite in place

- `docs/adr/ADR-001-skill-first-template-adoption.md` → retitled and rewritten as "Command-first template adoption." Same file path/number; full body rewrite. Add a one-sentence note to "More information" stating an earlier draft proposed skill-based adoption and was revised before any implementation landed.
- `docs/adr/INDEX.md` — update ADR-001 row title and summary.
- `README.md` — replace four-phase prompt narrative with command-first journey diagram (Triage → Intent → Spec → Design → Govern → Adapt → Review). Bootstrap checklist points to the appropriate `<.tool>/commands/init-triage.md` for the adopter's tool. Top-level layout adds `.claude/`, `.copilot/`, `.codex/`, `project-initialization/`; removes `prompts/`.
- `AGENTS.md` — main directories list adds the four new dirs, drops `prompts/`. Routing pointers updated.
- `CLAUDE.md` — points to `.claude/commands/init-triage.md`.
- `CONTRIBUTING.md` — note that initialization workflow changes must update all three tool dirs, `project-initialization/`, and any affected entry surfaces in the same change.
- `CHANGELOG.md` — `[Unreleased]` entry: replaced prompt-first template adoption with tool-native command-first workflow.
- `docs/00-source-of-truth.md` — initialization ownership row updated.
- `docs/INDEX.md` — navigation entries updated.
- `src/AGENTS.md`, `bin/README.md`, `diagrams/README.md`, `examples/README.md` — slot guidance now says repo sync (Phase 5) fills these.
- `.github/workflows/ci.yml` — required-files check covers new files, drops `prompts/*` checks; canonical-path-consistency check updated.

### Files to leave alone

- `docs/00_*` through `docs/08_*` template skeletons (except slot README updates above).
- `docs/09_user_documentation/` (deliberately out of init scope).
- `docs/superpowers/` (new design and plan land as new files).
- `tools/`, `scripts/`, `tests/`, `config/`.

### Migration ordering (one PR)

1. Write design doc and implementation plan under `docs/superpowers/specs/` and `docs/superpowers/plans/`.
2. Rewrite ADR-001 in place; update `docs/adr/INDEX.md`.
3. Build `project-initialization/` tree.
4. Build `.claude/` tree (commands, init-reviewer, vendored superpowers).
5. Build `.copilot/` and `.codex/` trees with parity.
6. Rewrite entry docs (`README.md`, `AGENTS.md`, `CLAUDE.md`, `CONTRIBUTING.md`, `CHANGELOG.md`, source-of-truth, INDEX). Update slot READMEs.
7. Delete `prompts/`.
8. Update `.github/workflows/ci.yml`.
9. Validate: markdownlint, docs validator, CI required-files, link check, manual adopter walkthrough.

## Risks

- **Breaking change for in-flight adopters.** Anyone who started with `prompts/` is left in limbo by deletion. Mitigation: this is a template repo; adopters fork or clone. Fresh adopters get the new flow; existing forks keep their prompts. CHANGELOG note explains.
- **Vendored superpowers drift.** Manual vendoring means upstream updates require a deliberate re-sync across three tool dirs. Mitigation: each tool dir's `AGENTS.md` records source path and version pin; CI parity check catches divergence between tool dirs.
- **Three-tool parity.** Despite single-source stage content, each tool dir still owns command files with tool-specific frontmatter. Drift risk is bounded by CI parity checks (substring presence of contract phrases across all 21 init command files).
- **Plan-file as Markdown state machine.** Markdown tables are easier to read than JSON but more fragile to parse. Acceptable for ~6 phases × ~13 artifacts; would not scale to thousands of entries. Mitigation: phase commands grep tables in well-defined formats; if drift occurs, plan file is the canonical artifact and parsing logic can be tightened.

## Acceptance criteria

This design is implementation-ready when all of the following are true:

- The phase and artifact catalog matches the canonical doc tree.
- Plan-file shape covers all state needed for resumability across sessions and for mode promotion mid-flight.
- Triage produces a tailored roadmap from a fixed minimal question set.
- Phase commands across all three tools follow the same lifecycle and respect mechanical gating.
- The `init-reviewer` agent contract is concrete enough to implement in one tool dir and replicate.
- Migration ordering lands the change in one PR without leaving the repo in an inconsistent state.

## More information

- Predecessor proposal (skill-first): [`2026-05-07-adoption-orchestrator-skill-design.md`](2026-05-07-adoption-orchestrator-skill-design.md).
- Predecessor implementation plan (skill-first, not executed): [`../plans/2026-05-07-adoption-orchestrator-skill-implementation.md`](../plans/2026-05-07-adoption-orchestrator-skill-implementation.md).
- Decision record: [`../../adr/ADR-001-skill-first-template-adoption.md`](../../adr/ADR-001-skill-first-template-adoption.md) (to be rewritten in place during implementation).
- Vendoring source: `/home/hexaper/.copilot/superpowers` at version 5.0.7.
