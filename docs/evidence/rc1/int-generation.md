---
title: RC1 Evidence - Integration Generation
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

RC-1-INT-GEN

## Date

2026-05-12 (planned)

## Branch/Revision

main @ 95eda1f (planning baseline)

## Executor

QA lead

## Environment

Local Linux workspace using repository docs set

## Command or workflow hint

Run markdown lint, then run generation workflow from input prompts and compare produced docs against template-required sections.

`git ls-files '*.md' | xargs -r npx --yes markdownlint-cli2`

## Artifact pattern

int-generation-YYYY-MM-DD.md

## Result

Planned

## Notes

Planned generation run to verify required sections are emitted and naming conventions are preserved.

## Linked issue IDs

- RC1-EVID-206
