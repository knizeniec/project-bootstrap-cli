---
title: Template Workflow Hardening Implementation Plan
status: active
record_class: historical
audience: [internal]
owner: Planning maintainers
capability: knowledge
phase: n/a
cadence: ad-hoc
last_reviewed: 2026-05-07
---

# Template Workflow Hardening Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Make the template workflow clearer and less brittle by separating templates from active documents, adding an explicit spec-refinement step, replacing halt-driven prompt behavior with recovery guidance, and tightening governance/CI validation.

**Architecture:** Treat this as three independent documentation surfaces that can be updated safely in parallel: workflow/navigation docs, prompt definitions, and governance/CI enforcement. Keep the repository documentation-first, avoid feature-code generation, and validate each slice with markdownlint plus targeted shell checks before moving on.

**Tech Stack:** Markdown, GitHub Actions, markdownlint-cli2, grep, git

---

## File Map

- Modify: `README.md`
  Responsibility: primary adopter-facing workflow, terminology, minimal-adoption guidance.
- Modify: `AGENTS.md`
  Responsibility: repo-wide routing and non-AI-team guidance.
- Modify: `CLAUDE.md`
  Responsibility: concise workflow pointer aligned with the primary README.
- Modify: `docs/README.md`
  Responsibility: docs tree entrypoint and active-vs-template navigation.
- Modify: `docs/INDEX.md`
  Responsibility: active docs vs starter templates navigation split.
- Modify: `docs/00-source-of-truth.md`
  Responsibility: canonical ownership terminology and prompt ownership rows.
- Modify: `docs/00-documentation-standards.md`
  Responsibility: required metadata semantics and placeholder policy.
- Modify: `docs/00_governance/README.md`
  Responsibility: active area landing page metadata and template positioning.
- Modify: `docs/02_product/README.md`
  Responsibility: active area landing page metadata and template positioning.
- Modify: `docs/03_architecture/README.md`
  Responsibility: active area landing page metadata and template positioning.
- Modify: `docs/adr/README.md`
  Responsibility: ADR lifecycle and binding behavior.
- Modify: `docs/adr/ADR-000-template.md`
  Responsibility: ADR acceptance checklist fields.
- Modify: `prompts/README.md`
  Responsibility: prompt inventory and explicit run order.
- Modify: `prompts/project-bootstrap.md`
  Responsibility: bootstrap handoff guidance and template-instance language.
- Modify: `prompts/architecture-baseline.md`
  Responsibility: architecture recovery gate and blocking/non-blocking gap handling.
- Modify: `prompts/language-adaptation.md`
  Responsibility: adaptation recovery gate and active-doc terminology.
- Create: `prompts/refine-specs.md`
  Responsibility: explicit Phase 2 spec-refinement prompt.
- Modify: `.github/workflows/ci.yml`
  Responsibility: repository policy enforcement and markdown validation.

### Task 1: Fix Workflow Terminology And Navigation Docs

**Files:**
- Modify: `README.md`
- Modify: `AGENTS.md`
- Modify: `CLAUDE.md`
- Modify: `docs/README.md`
- Modify: `docs/INDEX.md`
- Modify: `docs/00-source-of-truth.md`
- Test: `README.md`, `AGENTS.md`, `CLAUDE.md`, `docs/README.md`, `docs/INDEX.md`, `docs/00-source-of-truth.md`

- [ ] **Step 1: Capture the current template-link mismatches before editing**

```bash
cd /home/hexaper/template
grep -n "_TEMPLATE.md" README.md prompts/README.md prompts/project-bootstrap.md prompts/architecture-baseline.md prompts/language-adaptation.md docs/INDEX.md docs/00-source-of-truth.md
```

Expected: output shows multiple references where active-instance labels still point to `_TEMPLATE.md` files.

- [ ] **Step 2: Update the primary README to use explicit workflow terms and a clearer adoption path**

Use these content rules in `README.md`:

```md
## Project journey

Phase 1 creates active project documents from starter templates.
Phase 2 refines those active documents until architecture decisions can be made.
Phase 3 records the architecture baseline and ADRs.
Phase 4 adapts the repository structure to match the active architecture documents.

## Bootstrap checklist

1. Run `prompts/project-bootstrap.md`.
2. Run `prompts/refine-specs.md` until the active brief and PRD are ready for architecture work.
3. Run `prompts/architecture-baseline.md`.
4. Run `prompts/language-adaptation.md`.

## Minimal adoption path

If you are not using AI prompts, create the active project brief, PRD, and solution design from the starter templates under `docs/`, then adapt the repository structure manually.
```

- [ ] **Step 3: Update repo guidance docs to align on three terms only**

Apply these exact wording rules:

```md
- `template`: a reusable `_TEMPLATE.md` starter file.
- `active document`: a project-specific document that governs current work.
- `historical record`: a non-canonical planning or archive document.
```

Use that language in `AGENTS.md`, `CLAUDE.md`, `docs/README.md`, `docs/INDEX.md`, and `docs/00-source-of-truth.md`. In `CLAUDE.md`, reduce the content to a short pointer back to `README.md` plus the ordered prompt sequence.

- [ ] **Step 4: Run focused validation for the navigation slice**

Run:

```bash
cd /home/hexaper/template
npx --yes markdownlint-cli2 README.md AGENTS.md CLAUDE.md docs/README.md docs/INDEX.md docs/00-source-of-truth.md
```

Expected: PASS with no output.

- [ ] **Step 5: Verify active-document references no longer masquerade as template links**

Run:

```bash
cd /home/hexaper/template
grep -n "_TEMPLATE.md" README.md docs/INDEX.md docs/00-source-of-truth.md AGENTS.md CLAUDE.md
```

Expected: `_TEMPLATE.md` appears only in sections explicitly describing starter templates, not in steps describing active project documents.

- [ ] **Step 6: Commit the workflow-doc changes**

```bash
cd /home/hexaper/template
git add README.md AGENTS.md CLAUDE.md docs/README.md docs/INDEX.md docs/00-source-of-truth.md
git commit -m "docs: clarify template workflow terminology"
```

### Task 2: Add Explicit Spec Refinement And Make Prompts Recoverable

**Files:**
- Create: `prompts/refine-specs.md`
- Modify: `prompts/README.md`
- Modify: `prompts/project-bootstrap.md`
- Modify: `prompts/architecture-baseline.md`
- Modify: `prompts/language-adaptation.md`
- Test: `prompts/refine-specs.md`, `prompts/README.md`, `prompts/project-bootstrap.md`, `prompts/architecture-baseline.md`, `prompts/language-adaptation.md`

- [ ] **Step 1: Create the explicit Phase 2 refinement prompt**

Create `prompts/refine-specs.md` with this structure:

```md
---
name: "Refine Active Project Documents"
description: "Use after bootstrap to close blocking gaps in the active project brief and PRD before architecture work."
argument-hint: "No required arguments. Reads the active project brief and PRD and asks only targeted clarification questions."
agent: "agent"
---

Read the active project brief and PRD.
Classify unresolved items into:
- blocking gaps for architecture
- non-blocking TBDs

Ask only targeted questions for blocking gaps.
Update the active documents in place.
Produce an `Architecture readiness` summary with:
- ready now
- ready with assumptions
- not ready
```

- [ ] **Step 2: Update the prompt inventory and run order**

Update `prompts/README.md` so the inventory table lists:

```md
| [project-bootstrap.md](project-bootstrap.md) | Capture project intent and create active project documents from starter templates. | First |
| [refine-specs.md](refine-specs.md) | Close blocking gaps in the active project brief and PRD before architecture work. | After bootstrap |
| [architecture-baseline.md](architecture-baseline.md) | Decide the technical approach and create the active solution design and ADRs. | After spec refinement |
| [language-adaptation.md](language-adaptation.md) | Adapt the repository structure to the active architecture documents. | After architecture baseline |
```

- [ ] **Step 3: Change bootstrap handoff guidance so it no longer points straight to adaptation**

Update `prompts/project-bootstrap.md` to use this guidance:

```md
Adopter handoff:

- If blocking gaps remain in the active project brief or PRD, recommend `prompts/refine-specs.md` next.
- If the active project brief and PRD are complete enough for technical decisions, recommend `prompts/architecture-baseline.md` next.
- Do not recommend `prompts/language-adaptation.md` until an active solution design exists.
```

- [ ] **Step 4: Replace halt-driven prompt gates with recovery guidance**

Update `prompts/architecture-baseline.md` and `prompts/language-adaptation.md` to use this logic:

```md
Blocking gaps:
- gaps that prevent architecture or structural decisions

Non-blocking gaps:
- unresolved ownership, later-phase delivery details, and optional constraints that do not change the current architectural choice

If blocking gaps exist:
- ask targeted clarification questions or direct the adopter to `prompts/refine-specs.md`
- do not stop on non-blocking TBDs alone
```

Also replace labels like `canonical input documents` where they point to active instance files but link to templates. Use active-document language consistently.

- [ ] **Step 5: Validate the prompts and all referenced paths**

Run:

```bash
cd /home/hexaper/template
npx --yes markdownlint-cli2 prompts/README.md prompts/project-bootstrap.md prompts/architecture-baseline.md prompts/language-adaptation.md prompts/refine-specs.md
grep -n "docs/project/\|REPOSTORY_MAP\|REPOSITORY_MAP" prompts/README.md prompts/project-bootstrap.md prompts/architecture-baseline.md prompts/language-adaptation.md prompts/refine-specs.md
```

Expected: markdownlint passes; grep produces no output.

- [ ] **Step 6: Commit the prompt workflow changes**

```bash
cd /home/hexaper/template
git add prompts/README.md prompts/project-bootstrap.md prompts/architecture-baseline.md prompts/language-adaptation.md prompts/refine-specs.md
git commit -m "prompts: add explicit spec refinement step"
```

### Task 3: Tighten Governance Metadata And CI Enforcement

**Files:**
- Modify: `docs/00-documentation-standards.md`
- Modify: `docs/00_governance/README.md`
- Modify: `docs/02_product/README.md`
- Modify: `docs/03_architecture/README.md`
- Modify: `docs/adr/README.md`
- Modify: `docs/adr/ADR-000-template.md`
- Modify: `.github/workflows/ci.yml`
- Test: `.github/workflows/ci.yml`, `docs/00-documentation-standards.md`, `docs/00_governance/README.md`, `docs/02_product/README.md`, `docs/03_architecture/README.md`, `docs/adr/README.md`, `docs/adr/ADR-000-template.md`

- [ ] **Step 1: Replace placeholder-looking metadata in active area landing pages**

Set the active area README metadata to maintainer-owned values, for example:

```md
Status: Active
Owner: Template maintainers
Purpose: provide the landing page for governance, controls, traceability, and ownership records
Last updated: 2026-05-07
```

Apply the same pattern to `docs/02_product/README.md` and `docs/03_architecture/README.md` with their existing purpose lines.

- [ ] **Step 2: Make the documentation standard reject placeholders in active docs**

Add this guidance to `docs/00-documentation-standards.md`:

```md
Active documents must not retain starter placeholders such as `[Role or team ...]`, `[TBD: ...]` in required metadata fields, or `YYYY-MM-DD`.
Starter templates may keep placeholder metadata when the file is explicitly labeled as a template.
```

- [ ] **Step 3: Make ADR guidance more operational without adding bureaucracy**

Update `docs/adr/README.md` and `docs/adr/ADR-000-template.md` with these additions:

```md
Accepted ADRs are binding for active work until superseded by a later accepted ADR.
Proposed ADRs are not binding.
```

And in the template:

```md
- Acceptance date: YYYY-MM-DD or `Pending`
- Binding statement: [What becomes mandatory once the ADR is accepted]
- Review trigger: [What event forces the ADR to be revisited]
```

- [ ] **Step 4: Update CI so it validates substance and local-only assumptions correctly**

Modify `.github/workflows/ci.yml` to do all of the following:

```bash
# Reject placeholder metadata in active docs
if grep -Eq '^Owner: \[|^Last updated: YYYY-MM-DD$' "$f"; then
  echo "ERROR: $f still contains placeholder metadata"
  missing=1
fi

# Fail safely when the template glob is empty
template_files=$(find docs -mindepth 2 -maxdepth 2 -name '*_TEMPLATE.md' | sort)
if [ -z "$template_files" ]; then
  echo "ERROR: no starter templates found under docs/"
  missing=1
fi
```

Also keep the `Validate required governance files` list free of ignored local-only files and switch markdown lint to tracked Markdown only if the action supports it, otherwise keep the current exclusions explicit.

- [ ] **Step 5: Run the focused policy validation locally**

Run:

```bash
cd /home/hexaper/template
npx --yes markdownlint-cli2 docs/00-documentation-standards.md docs/00_governance/README.md docs/02_product/README.md docs/03_architecture/README.md docs/adr/README.md docs/adr/ADR-000-template.md .github/workflows/ci.yml

missing=0
for f in docs/README.md docs/Architecture.md docs/00-documentation-standards.md docs/00-source-of-truth.md docs/INDEX.md prompts/README.md $(find docs -mindepth 2 -maxdepth 2 -name '*_TEMPLATE.md' | sort); do
  for field in "Status:" "Owner:" "Purpose:" "Last updated:"; do
    grep -q "^$field" "$f" || missing=1
  done
done
test $missing -eq 0
```

Expected: PASS with no output other than successful command completion.

- [ ] **Step 6: Commit the governance and CI changes**

```bash
cd /home/hexaper/template
git add docs/00-documentation-standards.md docs/00_governance/README.md docs/02_product/README.md docs/03_architecture/README.md docs/adr/README.md docs/adr/ADR-000-template.md .github/workflows/ci.yml
git commit -m "ci: tighten documentation governance checks"
```

## Self-Review

- Spec coverage: this plan covers terminology cleanup, explicit spec refinement, prompt recovery behavior, metadata cleanup, ADR guidance, and CI hardening.
- Placeholder scan: no `TBD`, `TODO`, or deferred implementation markers remain in the plan itself.
- Type consistency: uses `template`, `active document`, and `historical record` consistently across all tasks.

## Execution Handoff

Plan complete and saved to `docs/superpowers/plans/2026-05-07-template-workflow-hardening.md`. Two execution options:

**1. Subagent-Driven (recommended)** - I dispatch a fresh subagent per task, review between tasks, fast iteration

**2. Inline Execution** - Execute tasks in this session using executing-plans, batch execution with checkpoints

**Which approach?**
