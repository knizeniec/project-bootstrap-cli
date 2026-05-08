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

2026-05-08

## Branch/Revision

main @ c9a1a6c

## Executor

QA lead

## Environment

Local Linux workspace using repository docs set

## Command or workflow hint

```bash
cd /home/hexaper/project-bootstrap-cli
RUN_DATE="${RUN_DATE:-$(date +%F)}"
artifact="docs/evidence/rc1/artifacts/RC1-PERF-001-${RUN_DATE}.md"
elapsed_file="/tmp/rc1-perf-seconds.txt"
mkdir -p "$(dirname "$artifact")"
/usr/bin/time -f '%e' -o "$elapsed_file" sh -c "git ls-files '*.md' | xargs -r npx --yes markdownlint-cli2 >/tmp/rc1-perf-lint.log 2>&1"
seconds="$(cat "$elapsed_file")"
{
	echo "# RC-1-PERF-001 timing"
	awk -v s="$seconds" 'BEGIN {thr=15; printf "elapsed_seconds=%.2f\nthreshold_seconds=%.2f\nwithin_threshold=%s\n", s, thr, (s<=thr?"yes":"no")} '
	echo "## Lint output sample"
	sed -n '1,40p' /tmp/rc1-perf-lint.log
} | tee "$artifact"
```

## Artifact pattern

docs/evidence/rc1/artifacts/RC1-PERF-001-${RUN_DATE}.md

## Result

Pass

## Notes

Executed on 2026-05-08. Timing result and threshold comparison were captured; check remained within threshold.

## Linked issue IDs

- RC1-EVID-208
