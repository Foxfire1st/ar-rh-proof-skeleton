# dag/stage-map.json

## Governing Overview

[Repository overview](../overview.md)

## Purpose

Stores the authoritative versioned 21-stage mapping (`rh-stage-map/v1`) pinning Stage 00 upstream files and per-file `preserve_defs` flags alongside Alpöge's Stages 01–20 placeholders.

## Code Commentary

Declares `schema_version: "rh-stage-map/v1"` and 21 consecutively ordered stages (`order` 0 through 20). Stage `00_statement` (`order: 0`, `"Formal Statements & Challenge Contracts"`) maps all five conforming Stage 00 upstream files using skeleton-relative target paths: `src/Statements.lean`, `src/Challenge.lean` and `comparator/FormalConjecturesStatement.lean` set `preserve_defs: true` so formal statement `def` bodies (including `GRH.trivialZeros`) remain verbatim, while `src/Main.lean` and `comparator/FormalConjecturesChallenge.lean` set `preserve_defs: false` so their bodies are stubbed. Stages `01_stepanov_determinant` through `20_grh` (`order` 1–20) carry empty `files: []` arrays until later masters map them. Formatted in canonical sorted-key JSON with a trailing newline; stage IDs and orders are stable partitions that future masters extend by populating `files` rather than renumbering stages.

## Relationships

Validated by [DAG models](../src/rh_pipeline/models/dag.py.md) and [the stage-map loader](../src/rh_pipeline/graph/stage_map.py.md), consumed by [`rh-extract`](../src/rh_pipeline/cli/extract.py.md) to emit [the generated Stage 00 DAG](dag.json.md) and `skeleton/00_statement/*.lean`, and verified by [stage-map unit tests](../tests/unit/test_stage_map.py.md).

## Evidence

- Declares schema_version rh-stage-map/v1 and maps all five Stage 00 upstream files with preserve_defs true on statements, challenge and formal_conjectures_statement and false on main and formal_conjectures_challenge. [1]
- Defines placeholder proof stages 01_stepanov_determinant through 20_grh (orders 1..20) with empty files arrays. [2]
