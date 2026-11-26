# tests/unit/test_stage_map.py

## Governing Overview

[Repository overview](../../overview.md)

## Purpose

Verifies Lean module-name resolution, 8-stage 41-file map loading and validation, direct and nearest-landmark projected DAG construction, Lean header import scanning, DAG model validation, and round-trip parity of committed `dag/dag.json` and all 41 skeletons.

## Code Commentary

Checks `lean_module_name_for_path` across valid and invalid paths, verifies `load_stage_map` against the committed 8-stage `dag/stage-map.json` (`00_statement`..`07_congruences`, 41 files across `[3, 6, 5, 5, 5, 6, 5, 6]` with `preserve_defs=True` only on `statements.lean` and `challenge.lean` and `False` on `main.lean` and Stages 01–07) plus order sorting, duplicate/gap/path validation errors, and malformed JSON/I/O failures. `test_build_dag_uses_strict_import_edges_no_false_challenge_edge` and `test_build_dag_error_cases` prove direct-import edge derivation (excluding self/unmapped imports and never inferring edges from shared symbol names), `is_landmark` classification, canonical JSON round-tripping, and error rejections, while `test_scan_lean_header_imports_handles_comments_and_module_syntax`, `test_build_dag_with_projected_depends_on_and_validation`, and `test_extract_and_write_nearest_landmark_import_projection` prove Lean header scanning (`module`/`prelude`, nested comments, `public`/`meta`/`all`, header termination), `projected_depends_on` validation/normalization, and end-to-end nearest-landmark BFS projection (multi-hop `src/` and root intermediates, diamonds/cycles, mapped-landmark stop boundary, and blocking of `comparator/`, external imports, and escaping symlinks). `test_dag_models_strict_validation` checks exact-key, schema-version, path-safety, `skeleton/` prefix, boolean, and line-range rejections, while `test_committed_dag_and_skeleton_round_trip` guards committed `dag/dag.json` canonical round-trip fidelity (8 stages, 41 nodes, acyclic, zero isolated nodes, `statements -> ()`, `challenge -> (statements,)`, `01_reduction/new_main` in `main.depends_on`), `upstream_commit` parity with `audit/verification-status.json`, and exact declaration line-range parity across all 41 re-parsed `skeleton/<stage>/*.lean` files.

## Relationships

Tests [the stage-map and DAG builder](../../src/rh_pipeline/graph/stage_map.py.md), [nearest-landmark import projection](../../src/rh_pipeline/graph/import_projection.py.md), [DAG models](../../src/rh_pipeline/models/dag.py.md), [the Lean 4 parser](../../src/rh_pipeline/lean/parser.py.md), [the 8-stage map](../../dag/stage-map.json.md), [the proof DAG](../../dag/dag.json.md), [the Phase 0 verification status](../../audit/verification-status.json.md), and all 41 `skeleton/<stage>/*.lean` files. Executed by [the quality runner](../../scripts/check-quality.sh.md).

## Evidence

- Verifies strict import-only DAG edge derivation without false symbol-name edges and strict DAG model validation rejections. [2]

- Verifies Lean module-name resolution, real 8-stage map loading (00_statement..07_congruences, 41 mapped files across all 8 non-empty stages) and StageMapError validation on duplicates, non-consecutive orders and malformed inputs. [1]
- Guards committed dag/dag.json round-trip fidelity (8 stages, 41 nodes, acyclic, zero isolated nodes, challenge -> statements and 01_reduction/new_main in main.depends_on), upstream_commit parity with verification-status.json, and declaration line-range parity across all 41 skeletons. [3]
- Verifies Lean header import scanning, projected_depends_on validation/normalization, and end-to-end nearest-landmark BFS projection across unmapped src/ and root intermediates while blocking comparator/ files and escaping symlinks. [4]
