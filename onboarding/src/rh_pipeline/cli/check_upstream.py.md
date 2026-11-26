# src/rh_pipeline/cli/check_upstream.py

## Governing Overview

[Repository overview](../../../overview.md)

## Purpose

Provides the thin `rh-check-upstream` CLI entry point wiring command-line flags to read-only upstream freshness triage and mapping results to canonical JSON or human text output and deterministic exit codes.

## Code Commentary

`_build_parser` configures `rh-check-upstream` with required `--upstream`, optional `--ref` (default `"HEAD"`), `--provenance` (default `audit/provenance.json`), `--stage-map` (default `dag/stage-map.json`), `--replay` (optional), `--replay-ref` (default `"HEAD"`), and `--json`. `main` constructs a `ReplayTarget` when `--replay` is given (ignoring `--replay-ref` when `--replay` is omitted), delegates triage to `check_upstream`, prints `ERROR: <exc>` to stderr returning `EXIT_INPUT_ERROR` (`2`) on `TriageInputError`, and otherwise writes `report.to_json()` (with `--json`) or `format_triage_report(report)` to stdout and returns `report.exit_code` (`0`, `10` or `11`).

## Relationships

Registered as `rh-check-upstream` in [package metadata](../../../pyproject.toml.md). Delegates all Git and schema triage logic to [upstream triage](../audit/triage.py.md), reading [provenance.json](../../../audit/provenance.json.md) and [stage-map.json](../../../dag/stage-map.json.md) by default. Verified by [upstream-triage unit tests](../../../tests/unit/test_check_upstream.py.md).

## Evidence

- Parses --upstream, --ref, --provenance, --stage-map, optional --replay/--replay-ref and --json flags for rh-check-upstream. [1]
- Runs check_upstream, prints ERROR to stderr with exit code 2 on TriageInputError, and writes JSON or text output returning report.exit_code. [2]
