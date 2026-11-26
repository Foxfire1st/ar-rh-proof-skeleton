# tests/unit/test_check_upstream.py

## Governing Overview

[Repository overview](../../overview.md)

## Purpose

Unit-tests read-only upstream freshness triage (`check_upstream`) and the `rh-check-upstream` CLI across all four primary statuses, deterministic exit codes, rename detection, informational replay statuses and fail-closed input/schema errors.

## Code Commentary

Constructs temporary local Git repositories in `tmp_path` without mutating external upstream checkouts. Covers six test functions: `up-to-date` human and canonical timestamp-free `--json` output with read-only `HEAD`/branch/status assertions (exit `0`), non-`HEAD` `--ref` `unmapped-only` classification without moving `HEAD` (exit `0`), `mapped-drift` across audit-tracked paths, mapped stage edits and `git mv` renames (exit `10`), `history-switched` on diverged/rewritten branches and missing pinned commits (exit `11`), all five informational `replay_status` outcomes (`not-checked`, `not-pinned`, `up-to-date`, `moved`, `history-switched`) preserving primary exit `0`, and ten invalid-input/schema failures exiting `2` with `ERROR:` on stderr.

## Relationships

Verifies [upstream triage](../../src/rh_pipeline/audit/triage.py.md) and the [check-upstream CLI](../../src/rh_pipeline/cli/check_upstream.py.md), executed by [the quality runner](../../scripts/check-quality.sh.md).

## Evidence

- Verifies read-only up-to-date human/canonical-JSON output and non-HEAD --ref unmapped-only classification (exit 0) without mutating upstream HEAD, branch or working tree. [1]
- Verifies mapped-drift (exit 10) across audit paths, mapped stage edits and git mv renames plus history-switched (exit 11) on diverged history or missing pinned commit. [2]
- Verifies informational replay statuses never override exit 0 while ten invalid-input/schema cases exit 2 with ERROR on stderr. [3]
