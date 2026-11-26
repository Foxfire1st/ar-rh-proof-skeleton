# tests/unit/test_stage_map.py

## Governing Overview

[Repository overview](../../overview.md)

## Purpose

Verifies Lean module-name resolution, 8-stage condensed map loading and validation, strict import-only DAG construction, DAG model validation, and round-trip parity of committed `dag/dag.json` and Stage 00 skeletons.

## Code Commentary

Checks `lean_module_name_for_path` across valid and invalid paths, verifies `load_stage_map` against the committed 8-stage `dag/stage-map.json` (`00_statement`..`07_congruences`, 3 Stage 00 files, empty Stages 01–07) plus order sorting, duplicate/gap/path validation errors and malformed JSON/I/O failures. `test_build_dag_uses_strict_import_edges_no_false_challenge_edge` proves `depends_on` edges derive strictly from resolved Lean imports (excluding self-edges and unmapped imports and creating no false `challenge -> statements` edge from shared symbol names) and verifies `is_landmark` and JSON round-tripping. `test_dag_models_strict_validation` checks exact-key, schema-version, path-safety, `skeleton/` prefix, boolean and line-range rejections, while `test_committed_dag_and_stage00_skeleton_round_trip` guards committed `dag/dag.json` canonical round-trip fidelity (8 stages, 3 Stage 00 nodes with `challenge -> statements` and empty `statements`/`main` edges), `upstream_commit` parity with `audit/verification-status.json` and exact declaration line-range parity against all three re-parsed `skeleton/00_statement/*.lean` files.

## Relationships

Tests [the stage-map and DAG builder](../../src/rh_pipeline/graph/stage_map.py.md), [DAG models](../../src/rh_pipeline/models/dag.py.md), [the 8-stage map](../../dag/stage-map.json.md), [the Stage 00 DAG](../../dag/dag.json.md) and all three `skeleton/00_statement/*.lean` files (`statements.lean`, `challenge.lean`, `main.lean`). Executed by [the quality runner](../../scripts/check-quality.sh.md).

## Evidence

- Verifies strict import-only DAG edge derivation without false symbol-name edges and strict DAG model validation rejections. [2]

- Verifies Lean module-name resolution, real 8-stage map loading (00_statement..07_congruences, 3 Stage 00 files, empty Stages 01-07) and StageMapError validation on duplicates, non-consecutive orders and malformed inputs. [1]
- Guards committed dag/dag.json round-trip fidelity (8 stages, 3 Stage 00 nodes with challenge -> statements and empty statements/main edges), upstream_commit parity with verification-status.json and declaration line-range parity across all three Stage 00 skeletons. [3]
