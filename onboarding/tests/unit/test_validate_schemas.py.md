# tests/unit/test_validate_schemas.py

## Governing Overview

[Repository overview](../../overview.md)

## Purpose

Verifies strict JSON schema validation, stage-map structural checks and cross-file `upstream_commit` consistency across `audit/` and `dag/` artifacts.

## Code Commentary

Covers the real repository artifacts and synthetic valid repos, sorted `schema-missing` findings, malformed JSON exact line numbers and unreadable UTF-8 bytes (`schema-json`), parametrized unknown keys, missing required fields, `bool`-as-`int` rejections and wrong `schema_version` values across all four JSON artifacts, eight `dag/stage-map.json` structural failures via `load_stage_map`, six cross-file `upstream_commit` mismatch arrangements (including fallback from missing `verification-status.json` to `provenance.json`), and JSON `null` payloads. Limits: synthetic artifact tests run inside `tmp_path` without mutating repository JSON files.

## Relationships

Tests [artifact schema validation](../../src/rh_pipeline/validate/schemas.py.md) against [Phase 0 audit models](../../src/rh_pipeline/models/audit.py.md), [stage-map and DAG models](../../src/rh_pipeline/models/dag.py.md) and [the stage-map loader](../../src/rh_pipeline/graph/stage_map.py.md).

## Evidence

- Live repository audit and DAG JSON artifacts pass strict schema and commit-consistency checks with zero findings. [1]
- Unknown keys and bool-where-int values emit schema-invalid across all four repository JSON artifacts. [2]
- Cross-file upstream_commit mismatches between dag.json, verification-status.json and provenance.json emit schema-commit-mismatch. [3]
