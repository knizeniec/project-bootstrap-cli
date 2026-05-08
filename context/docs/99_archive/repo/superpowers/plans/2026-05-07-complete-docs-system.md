---
title: Complete Documentation System Implementation Plan
status: active
record_class: historical
audience: [internal]
owner: Planning maintainers
capability: knowledge
phase: n/a
cadence: ad-hoc
last_reviewed: 2026-05-07
---

# Complete Documentation System Implementation Plan

> **For agentic workers:** Use superpowers:subagent-driven-development to execute. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Deliver a complete, fillable documentation system for this template repo: the full canonical artifact tree (every section's base file in place with rich descriptions of what goes where), all artifacts born schema-valid, mutual cross-references between related docs, and a CI pipeline that enforces correctness on every PR.

**Architecture:** Three layers shipped together. (1) **Foundation:** Pydantic frontmatter schema + Python validator + GitHub Actions CI. (2) **Canonical scaffolds:** every artifact named in the revised spec exists under `docs/` as a Markdown template with YAML frontmatter, section headers, per-section guidance, and cross-references. The existing folder names (`00_governance/`, `01_strategy/`, etc.) are kept and extended; the spec's missing folders (`00_operating_model/`, `09_user_documentation/`) are added. (3) **End-user surface:** `09_user_documentation/` structured by Diataxis with `tutorials/`, `how-to/`, `reference/`, `explanation/`, plus `CHANGELOG.md` and `release-notes/`.

**Tech Stack:** Python 3.11+, Pydantic v2, `python-frontmatter`, pytest, GitHub Actions, `markdownlint-cli2`, `lychee`.

**Source spec:** `.tmp/docs-lifecycle-redesign-plan.md` (revised version, all 11 review revisions absorbed).

**Folder mapping** (existing → role per revised spec):

| Existing folder | Role | Default audience |
|---|---|---|
| `docs/00_operating_model/` *(new)* | Operating model: lifecycle map, schema, role/audience map | `[internal, manager]` |
| `docs/00_governance/` | Governance & portfolio: brief, business case, PID, board, RAID, change control, gates | `[internal, manager, client]` |
| `docs/01_strategy/` | Strategy: vision, roadmap | `[internal, manager, client]` |
| `docs/02_product/` | Product planning: PRD, requirements, journeys, acceptance | `[internal, manager]` |
| `docs/03_architecture/` | Solution architecture (arc42 + C4) | `[internal]` |
| `docs/04_ai_governance/` | AI governance: model cards, dataset cards, eval reports, prompt registry | `[internal, manager]` |
| `docs/05_testing_acceptance/` | Test strategy, evidence, UAT | `[internal, manager]` |
| `docs/06_security_operations/` | Security baseline, ops, runbook index, incident response | `[internal, manager]` |
| `docs/07_delivery/` | Execution control: integrated plan, status, dependency log | `[internal, manager]` |
| `docs/08_references/` | Standards register, glossary, decision-rights matrix | `[internal, manager]` |
| `docs/09_user_documentation/` *(new)* | End-user docs (Diataxis) + CHANGELOG + release notes | `[end_user]` |
| `docs/adr/` | ADRs (existing) | `[internal]` |
| `docs/99_archive/` | Sealed closure bundles only | varies |

---

## Frontmatter Conventions (used by every scaffold)

Every scaffold below begins with this YAML block, with values filled per the artifact's row in the task tables:

```yaml
---
title: "<title>"
status: draft
record_class: canonical
audience: [<one or more of: internal, manager, client, end_user, auditor>]
owner: "<role>"
capability: <governance|product|architecture|execution|quality|operations|knowledge|references|user_docs|operating_model>
phase: <initiation|planning|execution|monitoring|closure|n/a>
cadence: <ad-hoc|weekly|monthly|per-stage|per-release|one-shot>
last_reviewed: 2026-05-07
---
```

After the frontmatter, every scaffold has this body skeleton:

```markdown
# <Title>

> **Purpose:** <one-sentence purpose>
> **Audience:** <one line on who reads this and why>
> **When to update:** <trigger and cadence>

## How to use this template

- <how to copy / adapt for a real project>
- <which fields are mandatory vs. optional>
- <what to remove before publishing>

## <Section 1 from the section list>

<one or two sentences explaining what to put here, often with example bullets>

## <Section 2>

…

## Related documents

- [<title>](<relative-path>) — <one-line relationship>
- …
```

The "How to use this template" section, the per-section guidance, and the "Related documents" list are *required* for every scaffold — they are what makes the template "ready to be filled."

---

## File Structure (high-level)

| Path | Responsibility |
|---|---|
| `tools/docs_validator/` | Python validator (Pydantic schema + CLI + cross-file checks + tests) |
| `.github/workflows/docs.yml` | CI: frontmatter validation, markdownlint, lychee, ADR coverage |
| `.markdownlint-cli2.yaml` | Markdown lint config |
| `lychee.toml` | Link checker config |
| `docs/README.md` | Root navigation; lifecycle and role/audience entry points |
| `docs/INDEX.md` | Generated lifecycle/role/capability indexes |
| `docs/00_operating_model/*.md` | Lifecycle map, frontmatter schema, role/audience map, source-of-truth map, tailoring matrix, naming rules, audience profile reference, standards register pointer |
| `docs/00_governance/*.md` | Project brief, business case, PID, board terms, stakeholder register, communications plan, RAID, change control log, benefits realization, stage gate checklist, RTM |
| `docs/01_strategy/*.md` | Product vision, roadmap |
| `docs/02_product/*.md` | PRD, requirements catalog, user journeys, backlog policy, acceptance catalog |
| `docs/03_architecture/*.md` | Solution design (arc42), C4 context/container/component, interface control document, data design, deployment, observability, resilience, configuration baseline, ADR index, RFC index, RFC template |
| `docs/04_ai_governance/*.md` | AI use policy (existing), model card, dataset card, evaluation report, prompt registry, AI risk register |
| `docs/05_testing_acceptance/*.md` | Test strategy, quality plan, verification evidence index, UAT plan, defect management |
| `docs/06_security_operations/*.md` | Security baseline, threat model, operating model, support model, access control, incident response, backup and recovery, service acceptance, post-launch review, runbook index |
| `docs/07_delivery/*.md` | Delivery plan (existing), implementation plan (existing), stage plan, status report, dependency log, readiness tracker, release plan, cutover and rollback |
| `docs/08_references/*.md` | Standards register (adopted/adapted/reference-only), glossary, decision-rights matrix (RACI/DACI), policy mappings, lessons-learned index |
| `docs/09_user_documentation/README.md` | Diataxis overview |
| `docs/09_user_documentation/tutorials/*.md` | At least one example tutorial |
| `docs/09_user_documentation/how-to/*.md` | At least one example how-to |
| `docs/09_user_documentation/reference/*.md` | At least one reference scaffold (API/CLI/UI as relevant) |
| `docs/09_user_documentation/explanation/*.md` | At least one concept-explanation scaffold |
| `docs/09_user_documentation/release-notes/2026-05-07-template-bootstrap.md` | Seed release note |
| `docs/09_user_documentation/CHANGELOG.md` | Keep-a-Changelog format |
| `docs/adr/ADR-000-template.md` | Existing ADR template (upgrade frontmatter only) |
| `docs/adr/INDEX.md` | Generated/maintained ADR index |
| `docs/_examples/*.md` | Five frontmatter examples (one per audience class) |

---

## Task 1: Validator scaffolding + Pydantic schema

**Model:** sonnet (mechanical, well-specified).

**Files:**
- Create: `tools/docs_validator/pyproject.toml`
- Create: `tools/docs_validator/Makefile`
- Create: `tools/docs_validator/src/docs_validator/__init__.py`
- Create: `tools/docs_validator/src/docs_validator/schema.py`
- Create: `tools/docs_validator/tests/__init__.py`
- Create: `tools/docs_validator/tests/test_schema.py`

- [ ] **Step 1:** Create `pyproject.toml` with deps `pydantic>=2.6,<3`, `python-frontmatter>=1.1,<2`, dev dep `pytest>=8,<9`. Console script `docs-validator = "docs_validator.cli:main"`. `pythonpath = ["src"]` for pytest.

- [ ] **Step 2:** Create `Makefile` with `install`, `test`, `validate`, `clean` targets.

- [ ] **Step 3:** Create empty `__init__.py` files (package + tests). Set `__version__ = "0.1.0"`.

- [ ] **Step 4:** Write `tests/test_schema.py` BEFORE writing the schema. Cover: minimal valid frontmatter passes; missing required field rejects; empty audience list rejects; unknown audience value rejects; `status: superseded` requires `superseded_by`; `capability: execution` requires `cadence` and `source_of_truth`; `last_reviewed` parses ISO date; supporting record class accepts internal-only audience.

- [ ] **Step 5:** Run `make test` — verify failures (module missing).

- [ ] **Step 6:** Implement `schema.py` with Pydantic v2 BaseModel `Frontmatter`, enums `Status`, `RecordClass`, `Audience`, `Capability`, `Phase`, `Cadence`. Fields exactly as specified in the revised spec Section 7. Use `model_config = ConfigDict(extra="forbid")`. Add `model_validator(mode="after")` for the conditional rules (`superseded_by` when superseded, `cadence`+`source_of_truth` when capability=execution).

- [ ] **Step 7:** Run `make test` — verify all tests pass.

- [ ] **Step 8:** Commit: `feat(docs-validator): scaffold Python project and Pydantic frontmatter schema`.

---

## Task 2: Parser + CLI + cross-file checks

**Model:** sonnet.

**Files:**
- Create: `tools/docs_validator/src/docs_validator/parser.py`
- Create: `tools/docs_validator/src/docs_validator/cli.py`
- Create: `tools/docs_validator/src/docs_validator/checks/__init__.py`
- Create: `tools/docs_validator/src/docs_validator/checks/supersession.py`
- Create: `tools/docs_validator/src/docs_validator/checks/staleness.py`
- Create: `tools/docs_validator/src/docs_validator/checks/adr_coverage.py`
- Create: `tools/docs_validator/tests/test_parser.py`
- Create: `tools/docs_validator/tests/test_cli.py`
- Create: `tools/docs_validator/tests/test_supersession.py`
- Create: `tools/docs_validator/tests/test_staleness.py`
- Create: `tools/docs_validator/tests/test_adr_coverage.py`
- Create: `tools/docs_validator/tests/fixtures/*.md` (valid, no_frontmatter, malformed_yaml, invalid_missing_audience, superseded_ok, replacement, superseded_dangling)

- [ ] **Step 1:** TDD parser. Tests: `parse_file(valid.md)` returns `ParseResult` with metadata + body; missing frontmatter raises `NoFrontmatterError`; malformed YAML raises `MalformedFrontmatterError`; missing file raises `FileNotFoundError`. Implementation uses `python-frontmatter`.

- [ ] **Step 2:** TDD CLI. Tests: exit 0 on valid file; exit 1 on schema-invalid file; exit 1 on missing frontmatter; recurses directories; reports per-file errors; prints `checked N file(s); M error(s)` summary. Implementation: argparse, `discover_markdown` skipping `.git`, `node_modules`, `.venv`, `__pycache__`, `dist`, `build`.

- [ ] **Step 3:** TDD supersession check. `check_supersession_links(parsed, root)` returns one error string per `superseded_by` that does not resolve. Resolves relative to the file's directory, then to root.

- [ ] **Step 4:** TDD staleness check. `is_stale(cadence, last_reviewed)` and `find_stale(parsed)` with thresholds: weekly=14d, monthly=60d, per-stage=90d, ad-hoc=365d. `per-release` and `one-shot` not time-based.

- [ ] **Step 5:** TDD ADR-coverage. `find_orphan_adrs(root, adr_dir)` returns ADR files not referenced by any other Markdown file.

- [ ] **Step 6:** Wire all checks into `cli.main()`. Add `--strict-staleness` flag (warns by default, errors when set). Cross-file checks operate on successfully-parsed files only.

- [ ] **Step 7:** Run `make test` — all tests pass (target ≥25 tests).

- [ ] **Step 8:** Run `docs-validator tools/docs_validator/tests/fixtures` from the repo root — verify it produces expected error report.

- [ ] **Step 9:** Commit: `feat(docs-validator): parser, CLI, and cross-file checks (supersession, staleness, ADR coverage)`.

---

## Task 3: CI workflow + markdownlint + lychee

**Model:** sonnet.

**Files:**
- Create: `.markdownlint-cli2.yaml`
- Create: `lychee.toml`
- Create: `.github/workflows/docs.yml`

- [ ] **Step 1:** Write `.markdownlint-cli2.yaml`. Disable: MD013 (line length), MD041 (first-line top heading; we have frontmatter), MD033 (inline HTML allowed). MD024 sibling-only. Ignore `node_modules`, `.venv`, `tools/docs_validator/tests/fixtures`, `**/CHANGELOG.md`.

- [ ] **Step 2:** Write `lychee.toml`. Reasonable timeouts; cache 7d; accept `[200, 206, 301, 302, 304, 308, 403, 429]`; exclude private hosts and `mailto:`; exclude paths `node_modules`, `.venv`, `tools/docs_validator/tests/fixtures`, `99_archive`.

- [ ] **Step 3:** Write `.github/workflows/docs.yml`. Triggers: push to main and PRs touching `docs/**`, `tools/docs_validator/**`, the workflow itself, lint configs. Jobs: `frontmatter-schema` (setup-python 3.11, install validator with dev extras, run pytest, run `docs-validator docs`); `markdownlint` (setup-node 20, run `npx -y markdownlint-cli2 "docs/**/*.md" "!**/node_modules/**"`); `links` (lycheeverse/lychee-action@v2 with `--config lychee.toml docs/`); `adr-coverage-report` (continue-on-error: true; advisory only; runs `find_orphan_adrs` against `docs/adr`).

- [ ] **Step 4:** Verify YAML parses: `python -c "import yaml; yaml.safe_load(open('.github/workflows/docs.yml'))"`.

- [ ] **Step 5:** Commit: `ci(docs): frontmatter, markdownlint, lychee, and ADR coverage workflow`.

> Note: at this point CI will *fail* against `docs/` because existing templates lack YAML frontmatter. That is fixed in Task 13. The workflow is ready; the docs migrate to it.

---

## Task 4: `00_operating_model/` — the operating layer

**Model:** sonnet.

**Files (all created):**

| File | `audience` | `capability` | Sections (each with prose guidance + examples) |
|---|---|---|---|
| `docs/00_operating_model/README.md` | `[internal, manager]` | `operating_model` | What this folder is; reading order; pointers to lifecycle map and role map |
| `docs/00_operating_model/01_lifecycle_map.md` | `[internal, manager]` | `operating_model` | Overview; phases (initiation/planning/execution/monitoring/closure); per-phase entry/exit/evidence; visual diagram placeholder; tailoring profiles (minimum/standard/high-assurance) |
| `docs/00_operating_model/02_role_audience_map.md` | `[internal, manager]` | `operating_model` | Roles (Executive, PM, Product, Engineer, QA, Ops, Compliance, End User, Client/Sponsor); audience taxonomy (`internal`, `manager`, `client`, `end_user`, `auditor`); per-role primary docs and decision posture; reading paths |
| `docs/00_operating_model/03_source_of_truth_map.md` | `[internal, manager]` | `operating_model` | Per-artifact authoritative location; mirror policy; tool-boundary rules; how to declare `source_of_truth: mirror_of:<system>` |
| `docs/00_operating_model/04_frontmatter_schema.md` | `[internal, manager]` | `operating_model` | Normative schema (required, conditional, optional keys); allowed values; validation rules; how to run the validator locally and in CI; examples linking to `docs/_examples/` |
| `docs/00_operating_model/05_audience_and_export_profiles.md` | `[internal, manager]` | `operating_model` | The three export profiles (`internal-full`, `client-bundle`, `auditor-bundle`); which audience values flow to which profile; redaction rules |
| `docs/00_operating_model/06_naming_and_structure.md` | `[internal, manager]` | `operating_model` | Naming conventions (numeric prefixes only at top level; noun-based artifact names; no `notes`/`misc`/`final`); folder ownership; one-artifact-one-job rule |
| `docs/00_operating_model/07_tailoring_matrix.md` | `[internal, manager]` | `operating_model` | Three profiles (minimum/standard/high-assurance); per-profile canonical artifact list; criteria for selection; how to declare a project's profile in its README |
| `docs/00_operating_model/08_decision_rights_matrix.md` | `[internal, manager]` | `operating_model` | RACI/DACI matrix template; rows = decision classes (funding, scope, design, release, ops); columns = roles; how to instantiate per project |

For each file: full YAML frontmatter, `# Title`, `> **Purpose** / **Audience** / **When to update:**` block, `## How to use this template` section, the listed sections each with 2–4 sentences of guidance and example bullets, and a `## Related documents` section with at least 3 cross-links.

- [ ] **Step 1:** Create all 9 files with valid frontmatter and full body skeleton.
- [ ] **Step 2:** Cross-link: every file in this folder links to the lifecycle map and the frontmatter schema.
- [ ] **Step 3:** Validate: `cd tools/docs_validator && docs-validator ../../docs/00_operating_model` — expect 0 errors.
- [ ] **Step 4:** Commit: `docs(operating-model): scaffold 00_operating_model with lifecycle map, schema, audience profiles, tailoring matrix, decision rights`.

---

## Task 5: Governance + strategy + delivery scaffolds

**Model:** sonnet.

**Files (created or extended):**

`docs/00_governance/`:

| File | Sections |
|---|---|
| `00_project_brief_TEMPLATE.md` (exists; upgrade frontmatter and section guidance) | Summary; Goals & success measures; Scope (in/out); Stakeholders; Constraints/assumptions; Risks/dependencies; Governance & approval; Related |
| `01_business_case_TEMPLATE.md` *(new)* | Problem statement; Strategic alignment; Options considered; Recommended option; Costs (capex/opex); Benefits (financial/non-financial); NPV/payback; Risks; Recommendation |
| `02_project_initiation_document_TEMPLATE.md` *(new)* | Project definition; Approach; Tolerances; Roles & responsibilities; Quality management; Configuration management; Communication management; Project plan summary; Controls |
| `03_board_terms_of_reference_TEMPLATE.md` *(new)* | Purpose; Composition; Decision rights; Cadence; Quorum; Reporting; Escalation |
| `04_stakeholder_register_TEMPLATE.md` *(new)* | Per-stakeholder rows: name/role, interest, influence, communication preference, current sentiment; engagement plan |
| `05_communications_plan_TEMPLATE.md` *(new)* | Audiences; Messages; Channels; Cadence; Owners; Feedback loops; Escalation |
| `06_raid_register_TEMPLATE.md` *(new)* | Risks (probability×impact, mitigation, owner); Assumptions (validity, owner); Issues (status, owner, due); Dependencies (type, owner, status) |
| `07_change_control_log_TEMPLATE.md` *(new)* | Per-change rows: ID, requestor, type (scope/cost/schedule/quality), impact, decision, decision-maker, date |
| `08_benefits_realization_TEMPLATE.md` *(new)* | Benefit map; baseline; target; measurement method; owner; review cadence; realized values over time |
| `09_stage_gate_checklist_TEMPLATE.md` *(new)* | Per-gate: entry criteria, mandatory artifacts, sign-off roles, exit criteria, evidence list |
| `12_requirements_traceability_matrix_TEMPLATE.md` (exists; upgrade) | Per-row: requirement ID, source, design ref, test ref, release ref, status |
| `README.md` (exists; rewrite) | Folder purpose; index of templates; reading order |

`docs/01_strategy/`:

| File | Sections |
|---|---|
| `01_product_vision_TEMPLATE.md` *(new)* | Vision statement; target customer; problem; differentiator; horizon (1y/3y); strategic bets |
| `02_roadmap_TEMPLATE.md` *(new)* | Themes; horizon view (Now/Next/Later); milestones; dependencies; owner |
| `README.md` (exists; rewrite) | Folder purpose; index; reading order |

`docs/07_delivery/`:

| File | Sections |
|---|---|
| `01_delivery_plan_TEMPLATE.md` (exists; upgrade) | Workstreams; milestones; resourcing; dependencies; risks; reporting |
| `02_implementation_plan_TEMPLATE.md` (exists; upgrade) | Approach; sequencing; environments; cutover strategy; rollback; runbook reference |
| `03_stage_plan_TEMPLATE.md` *(new)* | Stage objectives; tolerances; activities; gate criteria; exception triggers |
| `04_status_report_TEMPLATE.md` *(new)* | Period; RAG status (scope/schedule/cost/quality); progress against plan; risks/issues since last; decisions needed; next-period focus. `cadence: weekly`, `source_of_truth: repo` required by schema. |
| `05_dependency_log_TEMPLATE.md` *(new)* | Per-dependency rows: ID, type (in/out), owner, target date, status, blocking? |
| `06_readiness_tracker_TEMPLATE.md` *(new)* | Per-readiness-domain rows: scope/design/build/test/release/ops; status; owner; gating evidence link |
| `07_release_plan_TEMPLATE.md` *(new)* | Release scope; environments; sequence; rollback; comms plan; success criteria |
| `08_cutover_and_rollback_TEMPLATE.md` *(new)* | Cutover runbook (ordered steps, owners, durations); rollback runbook; decision points; comms |
| `README.md` (exists; rewrite) | Folder purpose; index; reading order |

For each file: valid frontmatter (audience appropriate per the folder mapping), full body skeleton, prose section guidance, related-docs cross-links.

- [ ] **Step 1:** Create the new files; upgrade the four existing files (project brief, RTM, delivery plan, implementation plan) to YAML frontmatter while preserving their content; ensure section guidance is rich (≥2 sentences plus example bullets per section).
- [ ] **Step 2:** Cross-link: status report → RAID + change log + readiness tracker; delivery plan → roadmap + business case; release plan → cutover plan + service acceptance.
- [ ] **Step 3:** Validate: `docs-validator docs/00_governance docs/01_strategy docs/07_delivery` — 0 errors.
- [ ] **Step 4:** Commit: `docs(governance,strategy,delivery): canonical artifacts with section guidance and cross-links`.

---

## Task 6: Product + AI governance scaffolds

**Model:** sonnet.

**Files:**

`docs/02_product/`:

| File | Sections |
|---|---|
| `01_prd_TEMPLATE.md` (exists; upgrade) | Problem; users; goals; non-goals; user stories; functional requirements; non-functional requirements; out of scope; acceptance criteria; release plan |
| `02_requirements_catalog_TEMPLATE.md` *(new)* | Per-requirement rows: ID, statement, type (functional/NFR), priority, source, owner, acceptance reference, status |
| `03_user_journeys_TEMPLATE.md` *(new)* | Per-journey: persona, goal, steps, pain points, opportunities, success measure |
| `04_backlog_policy_TEMPLATE.md` *(new)* | What goes in the backlog; intake; prioritization model; refinement cadence; ready/done definitions; tool-boundary statement (where backlog actually lives) |
| `05_acceptance_catalog_TEMPLATE.md` *(new)* | Per-feature: Given/When/Then acceptance scenarios; UAT pass criteria; sign-off roles |
| `README.md` (exists; rewrite) | Folder purpose; index; reading order |

`docs/04_ai_governance/`:

| File | Sections |
|---|---|
| `01_ai_use_policy_TEMPLATE.md` (exists; upgrade) | preserve content + add YAML frontmatter |
| `02_model_card_TEMPLATE.md` *(new)* | Model details (name, version, type); intended use; training data summary; evaluation results; ethical considerations; limitations; caveats; recommendations |
| `03_dataset_card_TEMPLATE.md` *(new)* | Dataset summary; supported tasks; languages; structure (fields, types); data sources; collection process; preprocessing; bias considerations; licensing; citations |
| `04_evaluation_report_TEMPLATE.md` *(new)* | Evaluation date; model+version; eval suites run; metrics (accuracy/F1/BLEU/safety/etc); regression vs prior; failure modes; recommendation (ship/hold) |
| `05_prompt_registry_TEMPLATE.md` *(new)* | Per-prompt rows: ID, version, purpose, model target, prompt text reference, owner, last-eval date, change history pointer |
| `06_ai_risk_register_TEMPLATE.md` *(new)* | Per-risk: hallucination, prompt injection, data leakage, bias, compliance; mitigation; owner; review cadence |
| `README.md` (exists; rewrite) | Folder purpose; AI-governance index; cross-link to evaluation reports under quality |

- [ ] **Step 1:** Create new files; upgrade two existing files; rich section guidance.
- [ ] **Step 2:** Cross-link: PRD ↔ requirements catalog ↔ acceptance catalog ↔ test strategy; model card ↔ dataset card ↔ evaluation report ↔ AI risk register.
- [ ] **Step 3:** Validate: `docs-validator docs/02_product docs/04_ai_governance` — 0 errors.
- [ ] **Step 4:** Commit: `docs(product,ai-governance): canonical artifacts with section guidance and cross-links`.

---

## Task 7: Architecture + ADR + RFC scaffolds

**Model:** sonnet.

**Files:**

`docs/03_architecture/`:

| File | Sections |
|---|---|
| `01_solution_design_TEMPLATE.md` (exists; upgrade — adopt arc42 structure) | Introduction & goals; Constraints; Context & scope; Solution strategy; Building block view; Runtime view; Deployment view; Cross-cutting concepts; Architectural decisions (link to ADRs); Quality requirements; Risks & technical debt; Glossary |
| `02_c4_context_TEMPLATE.md` *(new)* | System context overview; people; external systems; system in scope; key interactions; PlantUML/Mermaid placeholder |
| `03_c4_container_TEMPLATE.md` *(new)* | Containers (apps/services/databases); technology choices; responsibilities; communication paths |
| `04_c4_component_TEMPLATE.md` *(new)* | Components within key containers; responsibilities; collaborators; technology |
| `05_data_design_TEMPLATE.md` *(new)* | Domain model; entities & relationships; storage choices; data lifecycle (retention/archival); migration strategy |
| `06_interface_control_document_TEMPLATE.md` (exists; upgrade) | preserve content + frontmatter |
| `07_deployment_TEMPLATE.md` *(new)* | Environments; topology; infrastructure-as-code reference; secrets management; CI/CD pipeline outline |
| `08_observability_TEMPLATE.md` *(new)* | Logs (structure, levels, retention); metrics (golden signals + SLOs); traces; alerts; dashboards |
| `09_resilience_TEMPLATE.md` *(new)* | Failure modes; redundancy; failover; degradation policy; chaos testing approach |
| `10_configuration_baseline_TEMPLATE.md` *(new)* | Configuration domains; per-environment values policy; secrets; change procedure |
| `11_adr_index_TEMPLATE.md` *(new)* | Generated/maintained ADR list (ID, title, status, date); links to `docs/adr/` |
| `12_rfc_index_TEMPLATE.md` *(new)* | RFC list (ID, title, status, date); links to `docs/03_architecture/rfcs/` |
| `rfcs/RFC-000-template.md` *(new)* | Title, status (draft/proposed/accepted/rejected/superseded), context, proposal, alternatives considered, drawbacks, unresolved questions, security/privacy implications, prior art, references |
| `rfcs/README.md` *(new)* | RFC process: when to write one; lifecycle; review cadence; how to graduate to ADR |
| `README.md` (exists; rewrite) | arc42 + C4 + ADR + RFC overview; reading order |

`docs/adr/`:

| File | Action |
|---|---|
| `ADR-000-template.md` (exists) | Add YAML frontmatter; preserve MADR structure (Title, Context and Problem, Decision Drivers, Considered Options, Decision Outcome, Pros and Cons, More Information) |
| `INDEX.md` *(new)* | Maintained ADR list with status column |
| `README.md` (exists; rewrite) | ADR policy: when to write; status lifecycle; relationship to RFCs |

- [ ] **Step 1:** Create new files; upgrade existing files; arc42 structure for the solution-design template.
- [ ] **Step 2:** Cross-link: solution design ↔ C4 views ↔ ADR index; RFC template references ADR template; deployment ↔ runbook index in `06_security_operations`.
- [ ] **Step 3:** Validate: `docs-validator docs/03_architecture docs/adr` — 0 errors.
- [ ] **Step 4:** Commit: `docs(architecture,adr,rfc): arc42 + C4 + ADR + RFC scaffolds with cross-links`.

---

## Task 8: Testing + operations scaffolds

**Model:** sonnet.

**Files:**

`docs/05_testing_acceptance/`:

| File | Sections |
|---|---|
| `01_test_strategy_TEMPLATE.md` *(new)* | Scope; testing scope (functional/NFR/security/AI eval); levels (unit/integration/E2E/UAT); environments; tools; entry/exit criteria; defect-management cross-link |
| `02_quality_plan_TEMPLATE.md` *(new)* | Quality objectives; metrics (coverage, defect density, escaped defects); reviews; audits; quality gates per phase |
| `03_verification_evidence_index_TEMPLATE.md` *(new)* | Per-requirement: design ref, test ref, run ID, result, evidence link; matrix view |
| `04_uat_plan_TEMPLATE.md` *(new)* | UAT scope; participants; environment; scenarios (link to acceptance catalog); pass criteria; sign-off roles |
| `05_defect_management_TEMPLATE.md` *(new)* | Triage process; severity scale; SLAs; reporting cadence; escape-defect review |
| `README.md` (exists; rewrite) | Folder purpose; index; reading order |

`docs/06_security_operations/`:

| File | Sections |
|---|---|
| `01_security_baseline_TEMPLATE.md` *(new)* | Threat surface; controls (auth/authz/encryption/logging/network); compliance frame (link to standards register); secrets handling; vulnerability management |
| `02_threat_model_TEMPLATE.md` *(new)* | System decomposition; trust boundaries; STRIDE per component; mitigations; residual risks |
| `03_operating_model_TEMPLATE.md` *(new)* | Service catalog; on-call rota; escalation; change windows; capacity planning; cost ownership |
| `04_support_model_TEMPLATE.md` *(new)* | Support tiers (L1/L2/L3); intake channels; SLAs; knowledge base; handover from delivery |
| `05_access_control_TEMPLATE.md` *(new)* | Identity sources; role definitions; least-privilege policy; periodic access review; offboarding |
| `06_incident_response_TEMPLATE.md` *(new)* | Severity classes; declare/contain/eradicate/recover/learn lifecycle; comms templates; post-incident review |
| `07_backup_and_recovery_TEMPLATE.md` *(new)* | Data classes; backup cadence; retention; restore drills; RPO/RTO |
| `08_service_acceptance_TEMPLATE.md` *(new)* | Acceptance criteria; checks (security/operability/observability/runbooks/training); sign-off roles; conditional acceptance |
| `09_post_launch_review_TEMPLATE.md` *(new)* | Outcomes vs goals; benefits status; incidents and learnings; defect leakage; team feedback; actions |
| `10_runbook_index_TEMPLATE.md` *(new)* | Central index linking to per-component runbooks (some may live next to code); per-runbook entry: name, purpose, owner, location |
| `README.md` (exists; rewrite) | Folder purpose; index; reading order |

- [ ] **Step 1:** Create files; rich section guidance.
- [ ] **Step 2:** Cross-link: test strategy ↔ verification evidence index ↔ acceptance catalog; service acceptance ↔ runbook index ↔ release plan; incident response ↔ post-launch review.
- [ ] **Step 3:** Validate: `docs-validator docs/05_testing_acceptance docs/06_security_operations` — 0 errors.
- [ ] **Step 4:** Commit: `docs(quality,operations): canonical artifacts with section guidance and cross-links`.

---

## Task 9: References + standards register

**Model:** sonnet.

**Files:**

`docs/08_references/`:

| File | Sections |
|---|---|
| `01_standards_register_TEMPLATE.md` *(new)* | Adopted (with rationale); Adapted (with adaptation notes); Reference-only (with boundaries); per-row: standard name, scope, surface (manager/engineering/end-user), version, last reviewed |
| `02_glossary_TEMPLATE.md` *(new)* | Per-term: term, definition, examples, canonical source link |
| `03_decision_rights_matrix_TEMPLATE.md` *(new)* | RACI/DACI table; rows = decision classes; columns = roles; per-cell: R/A/C/I or D/A/C/I |
| `04_policy_mappings_TEMPLATE.md` *(new)* | External policy → internal artifact mapping; e.g. ISO 27001 controls → security baseline sections |
| `05_lessons_learned_index_TEMPLATE.md` *(new)* | Per-lesson: project/release, what happened, root cause, action taken, status, link to detail |
| `INDEX.md` (exists; rewrite to be generated-from-frontmatter compatible) | per-area index summary; pointer to root INDEX.md |
| `README.md` (exists; rewrite) | Folder purpose; standards register reading order |

- [ ] **Step 1:** Create files; for the standards register, populate adopted/adapted/reference-only rows from the revised spec Section 5 verbatim (PMBOK, PRINCE2, ISO 21502, arc42, C4, MADR, Diataxis, ISO 31000, ISO 29148, ISO 29119, ITIL, BABOK, NIST CSF, ISO 27001).
- [ ] **Step 2:** Cross-link: standards register ↔ each capability area; decision-rights matrix ↔ governance + change-control log; lessons-learned index ↔ post-launch review.
- [ ] **Step 3:** Validate: `docs-validator docs/08_references` — 0 errors.
- [ ] **Step 4:** Commit: `docs(references): standards register, glossary, decision-rights matrix, lessons-learned index`.

---

## Task 10: `09_user_documentation/` — Diataxis end-user area

**Model:** sonnet.

**Files:**

| File | Diataxis mode | Audience |
|---|---|---|
| `docs/09_user_documentation/README.md` | (overview) | `[end_user, internal]` |
| `docs/09_user_documentation/tutorials/README.md` | tutorials overview | `[end_user]` |
| `docs/09_user_documentation/tutorials/01-getting-started-TEMPLATE.md` | tutorial | `[end_user]` |
| `docs/09_user_documentation/how-to/README.md` | how-to overview | `[end_user]` |
| `docs/09_user_documentation/how-to/01-perform-a-common-task-TEMPLATE.md` | how-to | `[end_user]` |
| `docs/09_user_documentation/reference/README.md` | reference overview | `[end_user]` |
| `docs/09_user_documentation/reference/01-feature-reference-TEMPLATE.md` | reference | `[end_user]` |
| `docs/09_user_documentation/explanation/README.md` | explanation overview | `[end_user]` |
| `docs/09_user_documentation/explanation/01-concept-explanation-TEMPLATE.md` | explanation | `[end_user]` |
| `docs/09_user_documentation/release-notes/2026-05-07-template-bootstrap.md` | release note | `[end_user]` |
| `docs/09_user_documentation/CHANGELOG.md` | changelog (Keep-a-Changelog) | `[end_user]` |

Section structures per Diataxis mode:

- **Tutorials** (learning-oriented): What you'll build; Prerequisites; Steps (numbered, complete code/commands); Verifying; What you learned; Next steps.
- **How-to** (problem-oriented): Goal; Prerequisites; Steps; Verification; Troubleshooting; Related how-tos.
- **Reference** (information-oriented): Surface listing (commands/options/parameters/fields); per-item: signature, description, examples, errors; index.
- **Explanation** (understanding-oriented): Background; the concept; alternatives & tradeoffs; further reading.

Release notes: per release: highlights, breaking changes, new features, fixes, known issues, migration notes, upgrade instructions.

CHANGELOG: Keep-a-Changelog headers (`## [Unreleased]`, `### Added/Changed/Deprecated/Removed/Fixed/Security`).

- [ ] **Step 1:** Create all files with valid frontmatter, Diataxis-mode-appropriate section structure, and rich section guidance.
- [ ] **Step 2:** Cross-link: top README links to all four mode READMEs and to release notes/CHANGELOG; each mode README explains when to write that mode and links to the canonical Diataxis overview at <https://diataxis.fr>.
- [ ] **Step 3:** Validate: `docs-validator docs/09_user_documentation` — 0 errors.
- [ ] **Step 4:** Commit: `docs(user-documentation): Diataxis-structured end-user docs with tutorials, how-to, reference, explanation, release notes, CHANGELOG`.

---

## Task 11: Convert remaining existing files to YAML frontmatter

**Model:** sonnet.

**Files (existing files not yet upgraded by tasks 5–9):**
- `docs/00-documentation-standards.md`
- `docs/00-source-of-truth.md`
- `docs/AGENTS.md`
- `docs/Architecture.md`
- `docs/INDEX.md`
- `docs/README.md`
- `docs/superpowers/AGENTS.md`
- `docs/superpowers/README.md`
- `docs/99_archive/README.md`
- Any others surfaced by `docs-validator docs` reporting "missing frontmatter".

- [ ] **Step 1:** Run `docs-validator docs 2>&1 | grep "missing frontmatter"` to enumerate the backlog.
- [ ] **Step 2:** For each file, add YAML frontmatter at top with appropriate values (use `record_class: supporting` for READMEs and AGENTS files; `record_class: canonical` for documentation-standards and source-of-truth; `record_class: canonical` for Architecture.md). Preserve all existing content.
- [ ] **Step 3:** Validate: `docs-validator docs` should now report 0 errors (warnings about staleness are acceptable).
- [ ] **Step 4:** Commit: `docs: convert remaining files to YAML frontmatter for schema validation`.

---

## Task 12: Root navigation — README, INDEX, AGENTS

**Model:** sonnet.

**Files:**
- Modify: `docs/README.md`
- Modify: `docs/INDEX.md`
- Modify: `docs/AGENTS.md` (only if needed to update folder pointers)

- [ ] **Step 1:** Rewrite `docs/README.md` as the root entry point. Sections: What this repository documents; The audience model (link to operating model); Reading paths (by lifecycle phase / by role / by capability — each path is a bulleted list pointing to canonical artifacts); How to use templates; How CI validates docs; How to contribute.
- [ ] **Step 2:** Rewrite `docs/INDEX.md` as a comprehensive index. Sections: By lifecycle phase (initiation/planning/execution/monitoring/closure — each lists canonical artifacts); By role (Executive/PM/Product/Engineer/QA/Ops/Compliance/EndUser/Client); By capability (the 10 folders, each with their canonical artifact list). At the top, add a note that this file should eventually be generated from frontmatter (see [04_frontmatter_schema](../../00_operating_model/04_frontmatter_schema.md)) — for now it's hand-maintained.
- [ ] **Step 3:** Update `docs/AGENTS.md` if folder pointers have changed (add `00_operating_model/` and `09_user_documentation/`).
- [ ] **Step 4:** Validate: `docs-validator docs/README.md docs/INDEX.md docs/AGENTS.md` — 0 errors.
- [ ] **Step 5:** Commit: `docs: rewrite root README, INDEX, and AGENTS for the new docs system`.

---

## Task 13: Cross-link audit + final validation

**Model:** sonnet.

- [ ] **Step 1:** Run `docs-validator docs` from repo root. Expected: 0 errors. Some staleness warnings acceptable.
- [ ] **Step 2:** Run `npx -y markdownlint-cli2 "docs/**/*.md" "!**/node_modules/**"`. Fix any issues that surface.
- [ ] **Step 3:** Run lychee (locally if Docker available, otherwise rely on CI): `docker run --rm -v "$PWD":/input lycheeverse/lychee --config /input/lychee.toml /input/docs` — fix broken intra-repo links. External-link failures are acceptable (will surface in CI).
- [ ] **Step 4:** Walk every `## Related documents` section in the new scaffolds and verify the relationship makes sense both directions. Add reverse links where missing.
- [ ] **Step 5:** Sanity walk: a new contributor opens `docs/README.md`. Can they reach (a) the lifecycle map, (b) the role map, (c) the frontmatter schema, (d) an example artifact for their role, and (e) the contribution path in ≤5 clicks? If not, fix the navigation.
- [ ] **Step 6:** Add seed examples in `docs/_examples/` (5 files: canonical-manager, canonical-internal, end-user-doc, superseded, replacement) so the validator has known-good fixtures inside the docs tree itself. (These also exist in `tests/fixtures/` for unit tests; the `docs/_examples/` copies are user-facing.)
- [ ] **Step 7:** Final validation pass: `cd tools/docs_validator && make test && docs-validator ../../docs`. Both must succeed.
- [ ] **Step 8:** Commit: `docs: cross-link audit, seed examples, and final validation pass`.

---

## Verification (end-state acceptance)

**Foundation working:**
- `cd tools/docs_validator && make test` → all tests pass.
- `docs-validator docs` → 0 errors.
- Open a PR with a deliberately invalid frontmatter file → CI rejects.

**Docs system fillable:**
- Every folder under `docs/` has a `README.md` explaining its purpose and listing its canonical artifacts.
- Every canonical scaffold has YAML frontmatter, a Purpose/Audience/When-to-update block, "How to use this template" guidance, per-section descriptions of what to put there, and a "Related documents" section with at least 3 cross-links.
- A new contributor reading `docs/README.md` can reach any role-relevant artifact in ≤5 clicks.

**Standards represented:**
- Standards register (`docs/08_references/01_standards_register_TEMPLATE.md`) lists adopted/adapted/reference-only standards exactly as specified in the revised spec Section 5.
- arc42 structure visible in solution-design template.
- C4 (context/container/component) views as separate templates.
- MADR structure preserved in ADR template.
- Diataxis modes structure `09_user_documentation/`.

**Audience model working:**
- Every scaffold declares `audience` in frontmatter.
- Internal-only artifacts (ADRs, RFCs, internal runbooks) are tagged so a future export pipeline can exclude them from client/auditor bundles.
- End-user docs are tagged `[end_user]`, isolating them for the user-bundle.
