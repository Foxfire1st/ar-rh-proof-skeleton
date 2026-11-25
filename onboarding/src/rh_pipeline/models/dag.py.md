# src/rh_pipeline/models/dag.py

## Governing Overview

[Repository overview](../../../overview.md)

## Purpose

Defines immutable, slotted stage-map and proof-DAG domain models (`StageFileMapping`, `StageDefinition`, `StageMap`, `DagDeclarationRef`, `DagNode`, `DagGraph`) with strict schema-version, path and key validation plus canonical sorted-key JSON formatting.

## Code Commentary

Pins `STAGE_MAP_SCHEMA_VERSION = "rh-stage-map/v1"` and `DAG_SCHEMA_VERSION = "rh-dag/v1"`, and formats canonical JSON through `format_canonical_json` (`indent=2`, `sort_keys=True`, single trailing newline). Every dataclass validates exact field key sets via `_as_obj` against `dataclasses.fields`, rejects `bool` for integer fields (`_req_int`), requires strict booleans for `preserve_defs` and `is_landmark`, and enforces relative POSIX paths via `_req_path` (forbidding leading `/`, backslashes and empty, `.` or `..` segments). On `StageFileMapping.skeleton_file` and `DagNode.skeleton_file`, `_req_path(..., skeleton=True)` also refuses a leading `skeleton/` prefix. `DagDeclarationRef` validates `kind` against `VALID_DECLARATION_KINDS` and enforces 1-based inclusive skeleton and upstream line ranges. Limits: cross-entry uniqueness and consecutive stage ordering are enforced in `graph/stage_map.py`, not inside these pure data models.

## Relationships

Imports `VALID_DECLARATION_KINDS` and `LeanDeclarationKind` from [Lean models](lean.py.md). Supplies `DagDeclarationRef` to [the stub emitter](../lean/stub_emitter.py.md) and all stage-map/DAG types to [the stage-map and DAG builder](../graph/stage_map.py.md) for loading [the stage map](../../../dag/stage-map.json.md) and emitting [the proof DAG](../../../dag/dag.json.md); verified by [stage-map unit tests](../../../tests/unit/test_stage_map.py.md).

## Evidence

- Pins rh-stage-map/v1 and rh-dag/v1 schema versions and deterministic sorted-key canonical JSON formatting. [1]
- Enforces relative POSIX paths and rejects any leading skeleton/ prefix on skeleton_file fields. [2]
- StageFileMapping and DagDeclarationRef validate strict booleans, skeleton-relative targets and 1-based inclusive skeleton and upstream line spans. [3]
- StageMap and DagGraph enforce exact key sets, pinned schema versions and canonical to_json output. [4]
