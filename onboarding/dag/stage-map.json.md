# dag/stage-map.json

## Governing Overview

[Repository overview](../overview.md)

## Purpose

Stores the authoritative versioned 8-stage condensed mapping (`rh-stage-map/v1`) pinning Stage 00 upstream files and per-file `preserve_defs` flags alongside condensed Stages 01–07 (`01_reduction`–`07_congruences`) placeholders.

## Code Commentary

Declares `schema_version: "rh-stage-map/v1"` and 8 consecutively ordered stages (`order` 0 through 7). Stage `00_statement` (`order: 0`, `"Formal Statements & Challenge Contracts"`) maps the three live Stage 00 upstream files on condensed `main` using skeleton-relative target paths (`comparator/` formal-conjectures files are omitted pending re-integration): `src/Statements.lean` and root `Challenge.lean` set `preserve_defs: true` so formal statement `def` bodies remain verbatim, while `src/Main.lean` sets `preserve_defs: false` so its bodies are stubbed. Stages `01_reduction`, `02_derivatives`, `03_upper_bound`, `04_spanning`, `05_determinant`, `06_one_lift` and `07_congruences` (`order` 1–7) carry empty `files: []` arrays until later leaves map them. Formatted in canonical sorted-key JSON with a trailing newline; stage IDs and orders are stable partitions that future leaves extend by populating `files` rather than renumbering stages.

## Relationships

Validated by [DAG models](../src/rh_pipeline/models/dag.py.md) and [the stage-map loader](../src/rh_pipeline/graph/stage_map.py.md), consumed by [`rh-extract`](../src/rh_pipeline/cli/extract.py.md) to emit [the generated Stage 00 DAG](dag.json.md) and `skeleton/00_statement/*.lean`, read by [upstream triage](../src/rh_pipeline/audit/triage.py.md) via [`rh-check-upstream`](../src/rh_pipeline/cli/check_upstream.py.md), summarized by [dag/README.md](README.md.md), and verified by [stage-map unit tests](../tests/unit/test_stage_map.py.md).

## Evidence

- Declares schema_version rh-stage-map/v1 and maps the three Stage 00 upstream files with preserve_defs true on src/Statements.lean and root Challenge.lean and false on src/Main.lean. [1]
- Defines placeholder condensed proof stages 01_reduction through 07_congruences (orders 1..7) with empty files arrays. [2]
