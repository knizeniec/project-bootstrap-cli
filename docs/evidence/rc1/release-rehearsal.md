---
title: RC1 Evidence - Release Rehearsal
status: active
record_class: supporting
audience: [internal, manager]
owner: release-manager
capability: execution
phase: monitoring
cadence: per-release
last_reviewed: 2026-05-08
source_of_truth: repo
---

## Run ID

RC-1-REL-REHEARSAL

## Date

2026-05-15 (planned)

## Branch/Revision

main @ 95eda1f (planning baseline)

## Executor

Release manager + operations lead

## Environment

Staging-like workspace

## Command or workflow hint

```bash
cd /home/hexaper/project-bootstrap-cli
git ls-files '*.md' | xargs -r npx --yes markdownlint-cli2
PYTHONPATH=tools/docs_validator/src python3 -m docs_validator.cli docs/evidence/rc1/release-rehearsal.md
grep -n "RC-1-REL-REHEARSAL" docs/05_testing_acceptance/03_verification_evidence_index.md
grep -n "Run final verification suite and update evidence index." docs/07_delivery/07_release_plan.md
```

## Artifact pattern

docs/evidence/rc1/artifacts/RC1-REL-REHEARSAL-2026-05-08.md

## Result

Planned

## Notes

Planned run; record timeline, gate outcomes, rollback decision checkpoint, and follow-up actions.

## Linked issue IDs

- RC1-EVID-211
