---
title: Documentation Development Tool - Solution Design
status: active
record_class: canonical
audience: [internal]
owner: architecture-maintainer
capability: architecture
phase: planning
cadence: per-stage
last_reviewed: 2026-05-08
---

## 1. Introduction and goals

The solution provides a modular documentation development tool that helps teams generate implementation-ready docs from a standards baseline. The architecture is designed to support guided intake, scope-aware template selection, and quality review.

Primary goals:

- Deliver a reliable documentation-first workflow with minimal friction.
- Support modular growth without major redesign.
- Enable adaptable documentation generation across project sizes and types.

## 2. Constraints

- Reasonable delivery timeline with phased releases.
- Modular architecture requirement to avoid monolithic complexity.
- High usability expectations for cross-functional teams.
- Compatibility with standard developer environments and repository workflows.

## 3. Context and scope

In scope system responsibilities:

- Read documentation templates and standards from template docs source.
- Collect project context through Q&A and structured prompts.
- Determine required documentation files for a specific project profile.
- Generate/adapt documentation and run quality review checks.

External actors and systems:

- Users: project leads, PMs, architects, delivery leads, and engineers.
- Template source: `/home/hexaper/template/docs`.
- Repository docs target: local project docs folders.

Template source precedence for portability:

- CLI argument (highest priority)
- `DOCS_TEMPLATE_SOURCE` environment variable
- Repository default path: `context/docs`

If no valid source is found, generation must fail fast and return path-resolution guidance.

Out of scope:

- Full project scheduling and resource management tooling.
- Code implementation generation beyond documentation outputs.
- Template customization (deferred to Phase 2+).

## 4. Solution strategy

Adopt a modular monolith architecture for Phase 1:

- Shared domain model for project profile, template metadata, and review findings.
- Isolated modules with explicit interfaces for Q&A, assessment, generation, and review.
- Deterministic, rules-based baseline with extensibility for richer heuristics later.

This strategy balances implementation speed and maintainability for MVP delivery. Service decomposition is deferred to Phase 2+ if performance or scaling requires it.

## 5. Building block view

Core modules:

1. Template Catalog
  Role: index template docs, capabilities, and metadata.
2. Intake and Clarification Engine
  Role: ask base questions (see PRD Q&A Specification) and trigger follow-up clarifications for missing context; exit when Q&A flow is complete (see exit condition in PRD).
3. File Necessity Assessment Engine
  Role: classify docs as required, recommended, optional, or out of scope using project profile; apply project size classification rules (see Project Size Classification section below).
4. Documentation Generation and Adaptation Engine
  Role: copy/adapt templates into project docs using collected answers; variable substitution and structure mapping (see Generation Logic section below).
5. Documentation Review Engine
  Role: validate against quality baseline (completeness, structure, cross-references, frontmatter, content substance, consistency); return prioritized findings with actionable guidance.
6. Orchestration Layer
  Role: execute end-to-end workflow, maintain session state, coordinate module handoffs, track readiness status (see Readiness Tracking Mechanism section below).

## 5a. Project Size Classification

Project size is determined from Q&A answers using these categories:

- **Small:** Single team (≤5 people), internal-only scope, ≤3 core modules/components, no external integrations. Required docs: 4–6 core files (project brief, vision, PRD, solution design).
- **Medium:** 2–3 teams (5–15 people), one external integration or customer dependency, 3–8 core modules, standard compliance (SOC2/ISO). Required docs: 6–10 files (add readiness tracker, operations guide, integration spec).
- **Large:** 4+ teams (15+ people), multiple external integrations or regulated operations, 8+ core modules, advanced compliance (HIPAA/PCI-DSS/FedRAMP). Required docs: 10–15 files (add risk register, detailed deployment plan, incident response, security architecture).

## 5b. Generation Logic

**Variable Binding:**
Templates use `{{variable_name}}` syntax. Mapping from Q&A answers to variables:
- `{{project_name}}` ← Q&A question 1
- `{{project_type}}` ← Q&A question 1
- `{{primary_users}}` ← Q&A question 2
- `{{team_size}}` ← Q&A question 3
- `{{external_integrations}}` ← Q&A question 5 / follow-up
- `{{compliance_frameworks}}` ← Governance follow-up
- (Additional bindings defined per template)

**Structure Mapping:**
For each template file identified as required/recommended by assessment engine, the generation engine applies the variable bindings and copies to project docs folder. Optional sections marked with `<!-- optional if {{condition}} -->` are included/excluded based on Q&A answers.

**Conflict Resolution:**
If generated doc already exists in project folder: (1) rename existing to `.backup`, (2) generate new version, (3) report merge needed if original contains user edits beyond placeholder text.

## 5c. Readiness Tracking Mechanism

**Status Storage:**
Readiness state is stored in `.readiness.json` in the project docs root:
```json
{
  "session_id": "<uuid>",
  "timestamp": "2026-05-08T10:00:00Z",
  "domains": {
    "scope": {"status": "Green", "owner": "product-owner", "last_update": "2026-05-08T10:00:00Z"},
    "design": {"status": "Green", "owner": "architecture-owner", "last_update": "2026-05-08T10:00:00Z"},
    ...
  },
  "artifacts": {
    "qa_responses": "path/to/qa_responses.json",
    "generated_docs": "docs/",
    "review_findings": "path/to/review_findings.json"
  }
}
```

**Status Determination:**
- **Scope Green:** PRD and Project Brief are generated, pass quality review, all required sections non-empty.
- **Design Green:** Solution Design is generated, passes quality review, architecture decisions documented.
- **Build Red/Amber:** Set to Red initially. Moves to Green when implementation backlog is finalized and confirmed by engineering lead.
- **Test Red/Amber:** Set to Red initially. Moves to Green when verification test suite is complete and passes against generated docs.
- **Release Red/Amber:** Set to Red initially. Moves to Green when release checklist and communication plan are finalized and approved by release manager.
- **Ops Red/Amber:** Set to Red initially. Moves to Green when support model and post-launch runbook are finalized and approved by operations lead.

**Update Triggers:**
- Automatic: After review completion, Scope/Design → Green if quality checks pass.
- Manual: Domain owners update other domains via CLI command or API (Phase 2+).

**Query API:**
The tool exposes status via `readiness status [domain]` command returning current state and owner contact.

## 6. Runtime view

Primary flow:

1. User starts documentation session.
2. Intake engine gathers project profile and clarifies ambiguities.
3. Assessment engine determines required documentation set.
4. Generation engine creates/adapts docs in project workspace.
5. Review engine scans output and returns improvement findings.
6. User resolves findings and reruns review until readiness threshold is met.

Failure and recovery behavior:

- Missing template source: fail fast with actionable path guidance.
- Incomplete answers: loop through targeted follow-up questions.
- Review failures: preserve generated docs and emit fix recommendations.

## 7. Deployment view

Initial deployment profile:

- Local CLI-first operation within developer workspace.
- File-system based input/output using repository docs folders.
- Optional future API wrapper for remote workflows.

Environments:

- Local development and test in repository context.
- CI integration for documentation quality checks in later phases.

## 8. Cross-cutting concepts

- Configuration: centralized rules for scope mapping, quality baseline checks, and assessment classification.
- Security: protect sensitive project input and avoid unsafe logging of captured answers.
- Auditability: track what was generated, what was reviewed, and unresolved findings.
- Usability: concise prompts, clear next actions, and progressive disclosure.
- Extensibility: add capability modules without breaking existing workflow.
- Template stability: template docs in `/home/hexaper/template/docs` are treated as stable for Phase 1; generated docs retain their state independently.

## 9. Architectural decisions

Decision summary:

1. Modular monolith first, rather than microservices, to reduce early complexity.
2. Rules-first assessment and review for explainability and deterministic behavior.
3. Template-source compatibility with `/home/hexaper/template/docs` as baseline standard.
4. CLI-first user experience for immediate integration with repository workflows.

## 10. Quality requirements

- Reliability: workflow should complete without data loss and preserve intermediate outputs.
- Performance: interactive operations should provide near-immediate user feedback.
- Security: sensitive inputs should be protected in transit and storage paths.
- Maintainability: module boundaries and contracts must permit independent evolution.
- Compatibility: support common environments and project structures used by target teams.

## 11. Risks and technical debt

Risks:

- Quality baseline rules may flag false positives or miss important gaps.
- Assessment rules may not generalize equally across all project sizes/types.
- Review performance at scale (large docs sets).

Technical debt watchlist:

- Phase 2: Template customization (parameter binding, YAML overrides, comment markers).
- Phase 2: Configurable review rules by organization profile.
- Phase 3: Refactor for API-first (optional remote workflows).
- Phase 3: Service decomposition if performance scaling required.

## 12. Glossary

- Documentation-first: completion of required project documentation before implementation starts.
- Necessity assessment: classification of documentation files by project scope relevance.
- Clarification loop: targeted Q&A used to resolve missing or ambiguous context.
- Readiness gate: documented checkpoint used to approve progression to implementation.

## Related documents

- [../00_governance/00_project_brief.md](../00_governance/00_project_brief.md)
- [../01_strategy/01_product_vision.md](../01_strategy/01_product_vision.md)
- [../02_product/01_prd.md](../02_product/01_prd.md)
- [../07_delivery/06_readiness_tracker.md](../07_delivery/06_readiness_tracker.md)
