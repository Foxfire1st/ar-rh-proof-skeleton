# src/rh_pipeline/audit/triage.py

## Governing Overview

[Repository overview](../../../overview.md)

## Purpose

Performs deterministic, strictly read-only upstream freshness triage comparing a local `riemann-hypothesis` Git ref (and an optional local replay checkout) against pinned `audit/provenance.json` and `dag/stage-map.json` without mutating any checkout or invoking `git fetch`.

## Code Commentary

Defines `TriageStatus` (`"up-to-date"`, `"unmapped-only"`, `"mapped-drift"`, `"history-switched"`), `ReplayTriageStatus` (`"not-checked"`, `"not-pinned"`, `"up-to-date"`, `"moved"`, `"history-switched"`), deterministic exit constants (`EXIT_UP_TO_DATE = 0`, `EXIT_UNMAPPED_ONLY = 0`, `EXIT_INPUT_ERROR = 2`, `EXIT_MAPPED_DRIFT = 10`, `EXIT_HISTORY_SWITCHED = 11`), `TriageInputError`, and frozen dataclasses `ReplayTarget` and `UpstreamTriageReport` (with timestamp-free sorted-key `to_dict()` / `to_json()`). `check_upstream` loads `audit/provenance.json` via `ProvenanceRecord.from_dict` and `dag/stage-map.json` via `load_stage_map`, verifies Git repository root identity via `git -C <root> rev-parse --show-toplevel`, rejects blank or `-`-prefixed refs to prevent Git option injection, and classifies ancestry: equal commits return `up-to-date` (`0`), a missing or non-ancestor pinned commit returns `history-switched` (`11`, `commit_delta=None`), and fast-forward diffs use `git diff --no-renames --name-only -z <pinned>..<target>` (keeping `--no-renames` so renamed audit or mapped files stay visible under their old paths) partitioned against `_AUDIT_TRACKED_FILES`/`_AUDIT_TRACKED_PREFIXES` (`comparator/`, `verification/`) and mapped stage paths into `mapped-drift` (`10`) or `unmapped-only` (`0`). Optional replay inspection in `_triage_replay` validates `--replay` root and ref (failing closed with `TriageInputError` on bad inputs) and reports `replay_status` strictly informationally without changing the primary upstream `exit_code`. `format_triage_report` renders deterministic multi-line human output.

## Relationships

Imports `ProvenanceRecord` and `format_canonical_json` from [audit models](../models/audit.py.md) and `load_stage_map`/`StageMapError` from [stage-map loader](../graph/stage_map.py.md); reads [provenance.json](../../../audit/provenance.json.md) and [stage-map.json](../../../dag/stage-map.json.md) by default. Invoked by the [check-upstream CLI](../cli/check_upstream.py.md) and verified by [upstream-triage unit tests](../../../tests/unit/test_check_upstream.py.md).

## Evidence

- Defines triage and replay status literals, deterministic exit codes (0, 2, 10, 11) and audit-tracked path allowlists for freshness classification. [1]
- Classifies pinned-vs-target Git ancestry into up-to-date, history-switched, mapped-drift or unmapped-only using --no-renames NUL-delimited diffs. [2]
- Evaluates optional local replay checkouts as informational replay_status without changing the primary upstream exit code. [3]
- Orchestrates provenance and stage-map loading, read-only Git validation, upstream classification and replay triage into an immutable UpstreamTriageReport. [4]
