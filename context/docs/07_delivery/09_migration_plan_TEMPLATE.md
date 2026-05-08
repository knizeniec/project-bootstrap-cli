---
title: Migration Plan Template
status: draft
record_class: canonical
audience: [internal, manager]
owner: delivery-lead
capability: execution
phase: planning
cadence: per-release
last_reviewed: 2026-05-07
source_of_truth: repo
---

# Migration Plan Template

> **Purpose:** provide the standard structure for planning a data or system migration — capturing scope, approach, validation, cutover, and rollback in one place so that all stakeholders can review, approve, and execute safely.
> **Audience:** delivery leads, implementation engineers, data engineers, and managers who need to approve or oversee a migration into or out of a production system.
> **When to update:** update when migration scope, approach, or dependencies change. Treat this document as a living plan through the planning phase; freeze it (except for the sign-off section) before the migration begins.

## How to use this template

Copy this file and rename it `09_migration_plan_<projectname>.md`. Complete every mandatory section before requesting approval. Link this plan from the delivery plan and the release plan so stakeholders can find it.

- Mandatory: Migration scope, Approach, Pre-migration checklist, Validation, Cutover plan, Rollback plan, and Stakeholders and approvals.
- Optional: Data classification and volumes (if the migration involves no persistent data), Post-migration review trigger (customize for your release process).
- Write this plan for an implementation engineer who knows the system well but has not been involved in the planning discussions. Every decision should be traceable to either a constraint, an ADR, or a named stakeholder approval.
- Remove all example bullets and placeholder text before the plan is shared for approval.

## What not to include

- **Low-level SQL scripts or shell scripts** — link to version-controlled scripts in the repository rather than pasting them here. Scripts that live only in this document cannot be reviewed, tested, or version-controlled properly. Reference the relevant runbook (`docs/06_security_operations/11_runbook_TEMPLATE.md`) for executable procedures.
- **Architectural rationale for the migration target** — the decision to migrate to a new system or schema should already be recorded in an ADR. Reference the ADR here; do not repeat the reasoning.
- **Customer-facing communications copy** — draft communications belong in the communications plan (`docs/00_governance/05_communications_plan_TEMPLATE.md`). This plan records operational and technical decisions, not messaging.
- **General project status updates** — progress tracking belongs in the status report (`docs/07_delivery/04_status_report_TEMPLATE.md`).

## Frontmatter quick reference

This template's typical frontmatter values. The full schema (with all enums and conditional rules) is at [docs/00_operating_model/04_frontmatter_schema.md](../00_operating_model/04_frontmatter_schema.md).

| Field | Typical value here | Notes |
|---|---|---|
| `status` | `draft` → `active` → `superseded` | Use `superseded` when replaced; required `superseded_by:` link |
| `record_class` | `canonical` | This template defines a canonical artifact |
| `audience` | `[internal, manager]` | Add `client` only when client-export-safe |
| `capability` | `execution` | Fixed for this folder |
| `phase` | `planning` | One of `initiation`, `planning`, `execution`, `monitoring`, `closure`, `n/a` |
| `cadence` | `per-release` | One of `ad-hoc`, `weekly`, `monthly`, `per-stage`, `per-release`, `one-shot` |

> When `capability: execution`, both `cadence` and `source_of_truth` are required by the validator. Add `source_of_truth: repo` (or the appropriate value) to the frontmatter.

---

## Migration scope

Describe what is moving, from where, to where, and why. A reader should understand the full scope of the migration without opening any other document.

Include: the data sets, services, or components being migrated; the source system and its current state; the target system and its expected state after migration; and any explicit out-of-scope items to prevent scope creep.

- Example: "The `accounts` domain database is being migrated from MySQL 5.7 (on-premises) to Aurora MySQL 8.0 (AWS eu-west-1). This covers the `users`, `organisations`, and `permissions` tables. The `audit_log` table is out of scope and will migrate in a separate phase."
- Example: "The file store for the reporting service is being migrated from an NFS mount to S3. Objects created before YYYY-MM-DD are in scope. Objects created after that date are written directly to S3 by the updated service."

## Data classification and volumes

Record the sensitivity classification and volume estimates for all data being migrated. These figures drive retention, encryption, and validation requirements as well as infrastructure sizing.

State the classification using the project's data classification scheme (e.g., public, internal, confidential, restricted). Include row counts, file counts, or byte volumes as appropriate. Acknowledge estimates vs. confirmed measurements.

- Example: "The `users` table contains approximately 2.4 million rows classified as `confidential` (personal data under GDPR Article 4). Estimated export size: 18 GB uncompressed."
- Example: "The file store contains approximately 400,000 objects, 95% of which are classified `internal`. Total size: ~1.2 TB. Confirmed by `aws s3 ls --recursive` on YYYY-MM-DD."

## Approach

Describe the migration strategy and explain why it was chosen over alternatives. The three common patterns are big-bang, phased, and parallel-run — name which applies and tailor the description.

- **Big-bang:** all data migrates in one maintenance window. Fast but highest risk. Use when rollback is straightforward and downtime is acceptable.
- **Phased:** data migrates in batches, domain by domain or date range by date range. Lower risk per batch but requires the old and new systems to coexist.
- **Parallel-run (dual-write):** the system writes to both old and new simultaneously for a period. Allows validation before cutover but increases complexity and cost.

For each approach, document the migration window, expected duration, and dependencies on infrastructure or team availability.

- Example: "We will use a parallel-run approach. The application will dual-write to MySQL and Aurora for a two-sprint validation period. Once reconciliation confirms parity, we will cut read traffic over in a single maintenance window."
- Example: "We will use a phased approach. Phase 1 migrates the `users` table (highest volume). Phase 2 migrates `organisations` and `permissions` in the same release window one week later."

## Pre-migration checklist

List every precondition that must be verified before the migration begins. This checklist is a hard gate — the migration should not start until every item is checked.

Organize items into groups: backups and snapshots, capacity and infrastructure, access and credentials, rollback evidence, and stakeholder approvals.

- [ ] Full backup of source data confirmed and restore-tested within the last 24 hours.
- [ ] Target environment provisioned, sized, and validated by the data team lead.
- [ ] Migration scripts committed to version control and reviewed by [role].
- [ ] Rollback procedure tested in staging environment on YYYY-MM-DD. Outcome: [result].
- [ ] All required access credentials confirmed available for the migration team.
- [ ] Service owners notified of the maintenance window.
- [ ] Monitoring and alerting confirmed active on target environment.
- [ ] Go/no-go approval obtained from [role] on YYYY-MM-DD.

## Migration runbook reference

Link to the runbook that contains the step-by-step execution procedure for this migration. Do not paste the procedure here — keep this document at the planning level and the runbook at the execution level.

- Runbook: [11_<topic>_runbook_<projectname>.md](<relative-path>) — contains the numbered execution steps, expected outcomes, and rollback actions for each step.
- If no runbook exists yet, create one using [../06_security_operations/11_runbook_TEMPLATE.md](../06_security_operations/11_runbook_TEMPLATE.md) before the pre-migration checklist is signed off.

## Validation

Describe how the team will confirm that the migrated data is complete, correct, and consistent. Validation should be automated where possible and must produce evidence that can be attached to the release record.

Include: data integrity checks (row counts, checksums, hash comparisons), reconciliation methods, sampling strategy for large data sets, and the threshold at which a discrepancy is considered a blocking failure.

- Example: "Row count comparison between source and target will be run by `make validate-migration ENV=prod`. A discrepancy of more than 0 rows is a blocking failure. Expected output: `Validation passed: 2,400,000 / 2,400,000 rows matched`."
- Example: "A 1% random sample of migrated records will be spot-checked by the data engineer against source records. Any data mismatch in the sample triggers a full re-validation before the maintenance window closes."
- Example: "SHA-256 checksums of exported and imported files will be compared by the migration script. Any mismatch causes the script to exit non-zero and alert the on-call engineer."

## Cutover plan

Define the sequence of events that transitions traffic and ownership from the old system to the new one. This section must be specific enough to execute during a live maintenance window.

Include: the ordered steps for switching read and write traffic, who executes each step and when, communication points (when to inform stakeholders), and decision points where a human must confirm before proceeding.

- Example: "Step 1 — Freeze writes: enable the application maintenance page and stop all write traffic to the source database. Expected duration: 2 minutes. Owner: release manager."
- Example: "Step 2 — Final delta migration: run `make migrate-delta ENV=prod` to catch changes since the last bulk run. Expected duration: 15 minutes. Owner: data engineer."
- Example: "Decision point: the release manager confirms validation passed (see Validation section) before authorizing Step 3."
- Example: "Step 3 — Switch traffic: update the database connection string in the application configuration to point to Aurora. Deploy via `make deploy-config ENV=prod`. Expected duration: 5 minutes."

## Rollback plan

Define the criteria that trigger a rollback, the full rollback procedure, and the recovery time objective (RTO) — how long rollback is expected to take.

A rollback plan that exists only in principle is not a plan. The rollback procedure must be as detailed as the cutover steps and must have been tested in a non-production environment before the migration window opens.

**Rollback trigger criteria (any one of these triggers rollback):**
- Validation fails with more than [threshold] discrepancies after cutover.
- Application error rate exceeds [threshold] for more than [duration] after cutover.
- The on-call engineer or release manager calls a rollback.

**Rollback procedure:**
1. [Step with exact command, expected outcome, and owner.]
2. [Step with exact command, expected outcome, and owner.]
3. Confirm source system is accepting reads and writes normally.
4. Notify stakeholders that cutover was rolled back and state the next steps.

**Recovery time objective:** [Duration, e.g., "Rollback expected to complete within 30 minutes of trigger."]

## Risks and mitigations

List the significant risks to the migration and the specific actions taken to reduce each risk. Risks without mitigations are warnings; risks with mitigations are managed.

| Risk | Likelihood | Impact | Mitigation |
|---|---|---|---|
| Data loss during export | Low | High | Full backup and restore test within 24 hours of migration start |
| Rollback window exceeds RTO | Medium | High | Rollback rehearsal completed in staging on [date] |
| Target environment capacity insufficient | Low | Medium | Load test completed on [date]; capacity confirmed |
| [Risk] | [L/M/H] | [L/M/H] | [Mitigation] |

## Stakeholders and approvals

Name the roles that must review and approve this plan before the migration begins. Record the approval status for each.

| Role | Name | Approval status | Date |
|---|---|---|---|
| Delivery lead | | Pending / Approved | YYYY-MM-DD |
| Data owner | | Pending / Approved | YYYY-MM-DD |
| Security reviewer | | Pending / Approved | YYYY-MM-DD |
| Operations lead | | Pending / Approved | YYYY-MM-DD |
| Manager / sponsor | | Pending / Approved | YYYY-MM-DD |

## Post-migration review trigger

Define the condition that triggers a formal post-migration review and who is responsible for scheduling it. A review that is not triggered automatically often does not happen.

- Example: "A post-migration review will be scheduled within five working days of the migration completing. Owner: delivery lead. Review will confirm: all validation checks passed, no outstanding incidents are attributable to the migration, and the runbook is updated to reflect any deviations."
- Example: "If any rollback was triggered during the migration, a postmortem will be conducted within three working days using the postmortem template (`docs/06_security_operations/12_incident_postmortem_TEMPLATE.md`)."

## Related documents

- [01_delivery_plan_TEMPLATE.md](01_delivery_plan_TEMPLATE.md) — the delivery plan that includes this migration as a milestone or dependency.
- [08_cutover_and_rollback_TEMPLATE.md](08_cutover_and_rollback_TEMPLATE.md) — the release-level cutover and rollback plan; this migration plan feeds into or aligns with that artifact.
- [../06_security_operations/11_runbook_TEMPLATE.md](../06_security_operations/11_runbook_TEMPLATE.md) — the runbook template used to document the step-by-step execution procedure for this migration.
- [../03_architecture/03_data_design_TEMPLATE.md](../03_architecture/03_data_design_TEMPLATE.md) — the data design that specifies the target schema and data model this migration delivers.
