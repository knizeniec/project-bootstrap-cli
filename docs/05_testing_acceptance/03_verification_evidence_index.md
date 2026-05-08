---
title: Documentation Development Tool - Verification Evidence Index
status: active
record_class: canonical
audience: [internal, manager]
owner: qa-lead
capability: testing_acceptance
phase: monitoring
cadence: per-release
last_reviewed: 2026-05-08
source_of_truth: repo
---

## Evidence matrix

| Requirement or check | Design ref | Test ref | Latest run ID | Result | Evidence link |
| --- | --- | --- | --- | --- | --- |
| Q&A base + follow-up flow | [../03_architecture/01_solution_design.md](../03_architecture/01_solution_design.md) 5 | E2E-QA-001 | RC-1-E2E-QA | Planned | docs/evidence/rc1/e2e-qa-followup.md |
| File necessity assessment (small scope) | [../03_architecture/01_solution_design.md](../03_architecture/01_solution_design.md) 5a | INT-ASS-001 | RC-1-INT-ASS | Planned | docs/evidence/rc1/int-assessment.md |
| Docs generation and adaptation | [../03_architecture/01_solution_design.md](../03_architecture/01_solution_design.md) 5b | INT-GEN-001 | RC-1-INT-GEN | Planned | docs/evidence/rc1/int-generation.md |
| Quality review baseline checks | [../02_product/01_prd.md](../02_product/01_prd.md) Quality Review Baseline | INT-REV-001 | RC-1-INT-REV | Planned | docs/evidence/rc1/int-review-baseline.md |
| Readiness status query output | [../03_architecture/01_solution_design.md](../03_architecture/01_solution_design.md) 5c | E2E-READ-001 | RC-1-E2E-READ | Planned | docs/evidence/rc1/e2e-readiness-status.md |

## Evidence acceptance rules

- A row is only considered valid for go/no-go when `Result` is `Pass` or `Pass with accepted risk`.
- Every `Fail` row must include a linked issue ID in the evidence file.
- Evidence files must include run date, branch/revision, and executor.
- Superseded evidence remains in history but is not used for current gate decisions.

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

- Evidence files listed above need to be created under `docs/evidence/rc1/` during RC-1 execution.
- UAT-lite sign-off note is not yet linked.
- Release rehearsal output is not yet linked.

## Related documents

- [01_test_strategy.md](01_test_strategy.md)
- [../07_delivery/07_release_plan.md](../07_delivery/07_release_plan.md)
- [../07_delivery/06_readiness_tracker.md](../07_delivery/06_readiness_tracker.md)
