---
title: Documentation Development Tool - Verification Evidence Index
status: active
record_class: canonical
audience: [internal, manager]
owner: qa-lead
capability: quality
phase: monitoring
cadence: per-release
last_reviewed: 2026-05-08
source_of_truth: repo
---

<!-- markdownlint-disable MD013 -->

## Evidence matrix

| Requirement or check | Priority | Suite ID | Design ref | Test ref | Latest run ID | Result | Evidence link |
| --- | --- | --- | --- | --- | --- | --- | --- |
| AC-003: Q&A base + follow-up flow | P0 | E2E-SMALL-001 | [Building block view (Intake and Clarification Engine)](../03_architecture/01_solution_design.md#5-building-block-view) | E2E-QA-001 | RC-1-E2E-QA | Planned | [../evidence/rc1/e2e-qa-followup.md](../evidence/rc1/e2e-qa-followup.md) |
| AC-004: File necessity assessment (small scope) | P0 | INT-FLOW | [Project Size Classification](../03_architecture/01_solution_design.md#5a-project-size-classification) | INT-ASS-001 | RC-1-INT-ASS-SMALL | Planned | [../evidence/rc1/int-assessment.md](../evidence/rc1/int-assessment.md) |
| AC-004: File necessity assessment (medium scope) | P1 | INT-FLOW | [Project Size Classification](../03_architecture/01_solution_design.md#5a-project-size-classification) | INT-ASS-002 | RC-1-INT-ASS-MEDIUM | Planned | [../evidence/rc1/int-assessment-medium.md](../evidence/rc1/int-assessment-medium.md) |
| AC-004: File necessity assessment (large scope) | P1 | INT-FLOW | [Project Size Classification](../03_architecture/01_solution_design.md#5a-project-size-classification) | INT-ASS-003 | RC-1-INT-ASS-LARGE | Planned | [../evidence/rc1/int-assessment-large.md](../evidence/rc1/int-assessment-large.md) |
| Docs generation and adaptation | P0 | INT-FLOW | [Generation Logic](../03_architecture/01_solution_design.md#5b-generation-logic) | INT-GEN-001 | RC-1-INT-GEN | Planned | [../evidence/rc1/int-generation.md](../evidence/rc1/int-generation.md) |
| Quality review baseline checks | P1 | INT-FLOW | [Quality Review Baseline](../02_product/01_prd.md#quality-review-baseline) | INT-REV-001 | RC-1-INT-REV | Planned | [../evidence/rc1/int-review-baseline.md](../evidence/rc1/int-review-baseline.md) |
| Readiness status query output | P1 | E2E-SMALL-001 | [Readiness Tracking Mechanism](../03_architecture/01_solution_design.md#5c-readiness-tracking-mechanism) | E2E-READ-001 | RC-1-E2E-READ | Planned | [../evidence/rc1/e2e-readiness-status.md](../evidence/rc1/e2e-readiness-status.md) |
| UAT-lite sign-off walkthrough | P0 | UAT-LITE-001 | [Release Plan](../07_delivery/07_release_plan.md) | UAT-LITE-001 | RC-1-UAT-LITE-001 | Planned | [../evidence/rc1/uat-lite-signoff.md](../evidence/rc1/uat-lite-signoff.md) |
| Release rehearsal output and rollback drill | P0 | REL-REHEARSAL-001 | [Release Plan](../07_delivery/07_release_plan.md) | REL-REHEARSAL-001 | RC-1-REL-REHEARSAL | Planned | [../evidence/rc1/release-rehearsal.md](../evidence/rc1/release-rehearsal.md) |
| NFR-002: Performance response-time check | P1 | E2E-SMALL-001 | [Quality requirements - Performance](../03_architecture/01_solution_design.md#10-quality-requirements) | PERF-001 | RC-1-PERF-001 | Planned | [../evidence/rc1/perf-response-time.md](../evidence/rc1/perf-response-time.md) |
| NFR-004: Security handling check | P0 | E2E-SMALL-001 | [Cross-cutting concepts - Security](../03_architecture/01_solution_design.md#8-cross-cutting-concepts) | SEC-001 | RC-1-SEC-001 | Planned | [../evidence/rc1/sec-sensitive-inputs.md](../evidence/rc1/sec-sensitive-inputs.md) |

## Suite-to-evidence mapping

| Strategy suite ID | Evidence test refs |
| --- | --- |
| UNIT-CORE | Reserved for unit-level evidence rows as needed |
| INT-FLOW | INT-ASS-001, INT-ASS-002, INT-ASS-003, INT-GEN-001, INT-REV-001 |
| E2E-SMALL-001 | E2E-QA-001, E2E-READ-001, PERF-001, SEC-001 |
| UAT-LITE-001 | UAT-LITE-001 |
| REL-REHEARSAL-001 | REL-REHEARSAL-001 |

## Evidence acceptance rules

- Release-gate priority rule for `P0`: must be `Pass` or `Pass with accepted risk` before go/no-go.
- Release-gate priority rule for `P1`: may remain non-`Pass` only with documented owner, issue ID, and target date.
- A row is only considered valid for go/no-go when `Result` is `Pass` or `Pass with accepted risk`.
- Every `Fail` row must include a linked issue ID in the evidence file.
- Evidence files must include run date, branch/revision, and executor.
- Superseded evidence remains in history but is not used for current gate decisions.

### Evidence stub metadata hints

| Test ref | Environment hint | Command hint | Artifact pattern hint |
| --- | --- | --- | --- |
| UAT-LITE-001 | Staging-like workspace | Product-owner guided CLI walkthrough | `uat-lite-signoff-<date>.md` |
| REL-REHEARSAL-001 | Staging-like workspace | Full release rehearsal command set | `release-rehearsal-<date>.md` |

## Result taxonomy

- `Planned`: run not executed yet.
- `Pass`: expected behavior confirmed, no blocking defects.
- `Pass with accepted risk`: non-blocking known issues accepted by release owner.
- `Fail`: behavior does not satisfy acceptance for release.

## Update rules

- Replace `Planned` with final outcome after each test cycle.
- Keep run IDs immutable once assigned; add new run IDs for reruns.
- Add evidence URLs or artifact paths for every row before go/no-go.
- Keep the latest valid run as the active row; move superseded rows to a short history section if needed.

## Short history

| Date | Test ref | Prior run ID | Superseded by | Reason |
| --- | --- | --- | --- | --- |
| n/a | n/a | n/a | n/a | No superseded runs yet |

## Open gaps

- UAT-lite sign-off row is linked but still `Planned`.
- Release rehearsal row is linked but still `Planned`.

## Related documents

- [01_test_strategy.md](01_test_strategy.md)
- [../07_delivery/07_release_plan.md](../07_delivery/07_release_plan.md)
- [../07_delivery/06_readiness_tracker.md](../07_delivery/06_readiness_tracker.md)

<!-- markdownlint-enable MD013 -->
