---
title: RC1 Evidence - Performance Response Time
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

RC-1-PERF-001

## Date

2026-05-13 (planned)

## Branch/Revision

main @ 95eda1f (planning baseline)

## Executor

QA lead

## Environment

Local Linux workspace using repository docs set

## Command or workflow hint

Run markdown lint, then measure CLI response time for representative prompts (cold and warm) and log timings in evidence notes.

`git ls-files '*.md' | xargs -r npx --yes markdownlint-cli2`

## Artifact pattern

perf-response-time-YYYY-MM-DD.md

## Result

Planned

## Notes

Planned benchmark pass for NFR-002 with timing table and threshold comparison.

## Linked issue IDs

- RC1-EVID-208
