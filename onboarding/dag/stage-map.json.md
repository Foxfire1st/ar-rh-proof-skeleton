# dag/stage-map.json

## Governing Overview

[Repository overview](../overview.md)

## Purpose

Stores the authoritative versioned 8-stage condensed mapping (`rh-stage-map/v1`) pinning all 41 upstream landmark Lean files and per-file `preserve_defs` flags across Stages 00–07 (`00_statement`–`07_congruences`).

## Code Commentary

Declares `schema_version: "rh-stage-map/v1"` and 8 consecutively ordered non-empty stages (`order` 0 through 7) mapping 41 upstream Lean files in total (`[3, 6, 5, 5, 5, 6, 5, 6]` files per stage). Stage `00_statement` (`order: 0`, `"Formal Statements & Challenge Contracts"`) maps the three live Stage 00 upstream files on condensed `main` using skeleton-relative target paths (`comparator/` formal-conjectures files are omitted pending re-integration): `src/Statements.lean` and root `Challenge.lean` set `preserve_defs: true` so formal statement `def` bodies remain verbatim, while `src/Main.lean` sets `preserve_defs: false` so its bodies are stubbed. Stages `01_reduction` (6 files), `02_derivatives` (5), `03_upper_bound` (5), `04_spanning` (5), `05_determinant` (6), `06_one_lift` (5), and `07_congruences` (6) (`order` 1–7) map 38 landmark files under `src/New/` with `preserve_defs: false` on every file. Formatted in canonical sorted-key JSON with a trailing newline; stage IDs and orders are stable partitions extended within stages rather than renumbered.

## Relationships

Validated by [DAG models](../src/rh_pipeline/models/dag.py.md) and [the stage-map loader](../src/rh_pipeline/graph/stage_map.py.md), consumed by [`rh-extract`](../src/rh_pipeline/cli/extract.py.md) to emit [the generated proof DAG](dag.json.md) and all 41 `skeleton/<stage>/*.lean` files, read by [upstream triage](../src/rh_pipeline/audit/triage.py.md) via [`rh-check-upstream`](../src/rh_pipeline/cli/check_upstream.py.md), summarized by [dag/README.md](README.md.md), and verified by [stage-map unit tests](../tests/unit/test_stage_map.py.md).

## Evidence

- Declares schema_version rh-stage-map/v1 and maps the three Stage 00 upstream files with preserve_defs true on src/Statements.lean and root Challenge.lean and false on src/Main.lean. [1]

- Maps 38 landmark upstream files across condensed proof stages 01_reduction through 07_congruences (orders 1..7; counts 6, 5, 5, 5, 6, 5, 6), all with preserve_defs false. [2]
