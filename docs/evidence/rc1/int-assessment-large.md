---
title: RC1 Evidence - Integration Assessment (Large)
status: active
record_class: supporting
audience: [internal, manager]
owner: qa-lead
capability: execution
phase: monitoring
cadence: per-release
last_reviewed: 2026-05-08
source_of_truth: repo
---

## Run ID

RC-1-INT-ASS-LARGE

## Date

2026-05-12 (planned)

## Branch/Revision

main @ 95eda1f (planning baseline)

## Executor

QA lead

## Environment

Local Linux workspace using repository docs set

## Command or workflow hint

Run markdown lint, then execute large-scope intake and verify complete document set selection including governance and operations depth.

`git ls-files '*.md' | xargs -r npx --yes markdownlint-cli2`

## Artifact pattern

int-assessment-large-YYYY-MM-DD.md

## Result

Planned

## Notes

Planned validation for large project classification and full mandatory record coverage.

## Linked issue IDs

- RC1-EVID-205
