# src/rh_pipeline/validate/schemas.py

## Governing Overview

[Repository overview](../../../overview.md)

## Purpose

Validates `audit/provenance.json`, `audit/verification-status.json`, `dag/stage-map.json` and `dag/dag.json` against strict Phase 0 and Phase 1 models and enforces cross-file `upstream_commit` consistency.

## Code Commentary

`validate_repo_artifacts(repo_root)` reads all four JSON artifacts, emitting `schema-missing` (line 0) when absent, `schema-json` (exact JSONDecodeError line or 0 on I/O/UTF-8 failure) on malformed JSON, and `schema-invalid` (line 0) on strict model deserialization errors (unknown keys, missing fields, `bool`-as-`int` and wrong `schema_version`, explicitly enforcing `AUDIT_SCHEMA_VERSION` for Phase 0 records and delegating 21-stage structural invariants to `load_stage_map`). `_check_verification_commit` verifies `verification.upstream_commit == provenance.upstream.head_commit` when both parse, and `_check_dag_commit` verifies `dag.upstream_commit` against `verification.upstream_commit` (falling back to `provenance.upstream.head_commit` when verification is unavailable), emitting `schema-commit-mismatch` on divergence. Limits: stops at the first `ValueError` per JSON artifact and does not validate external memory `docs/` content.

## Relationships

Imports [Phase 0 audit models](../models/audit.py.md), [stage-map and DAG models](../models/dag.py.md), [stage-map loader](../graph/stage_map.py.md) and [ValidationFinding](../models/validation.py.md). Invoked by [the validator CLI](../cli/validate.py.md) and verified by [schema validator tests](../../../tests/unit/test_validate_schemas.py.md).

## Evidence

- validate_repo_artifacts deserializes all four audit and DAG JSON artifacts strictly, enforces AUDIT_SCHEMA_VERSION and stage-map structural invariants, and returns canonically sorted findings. [1]
- _check_dag_commit enforces that dag/dag.json upstream_commit matches audit/verification-status.json or falls back to audit/provenance.json upstream.head_commit when available. [2]
