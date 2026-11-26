# src/rh_pipeline/graph/stage_map.py

## Governing Overview

[Repository overview](../../../overview.md)

## Purpose

Loads and validates the versioned stage map (`rh-stage-map/v1`, 8 condensed stages), constructs the deterministic import-driven proof dependency `DagGraph` (with optional nearest-landmark projected dependencies), and orchestrates read-only upstream extraction with full in-memory validation before atomic file replacement.

## Code Commentary

`load_stage_map` parses `rh-stage-map/v1` JSON and fails closed with `StageMapError` on empty stage lists, duplicate `stage_id` or `order`, non-consecutive `0..N-1` ordering, non-`.lean` or empty-stem `skeleton_file`, or duplicate `upstream_path`, `skeleton_file` or derived Lean module names (`lean_module_name_for_path` strips one `src/` or `comparator/` prefix and joins segments with `.`). `build_dag` assigns `id = skeleton_file.removesuffix(".lean")`, `title = Path(upstream_path).stem`, `is_landmark = any(kind in {"theorem", "lemma"})`, refuses blank `upstream_commit`, missing modules, path mismatches, or duplicate declaration names within one skeleton file, and resolves sorted deduplicated `depends_on` edges strictly from direct mapped `module.imports` when `projected_depends_on` is omitted—or from validated, self-edge-stripped `projected_depends_on` overrides when supplied (rejecting unknown node or dependency IDs with `StageMapError`, ignoring external imports, and never cross-matching duplicate symbol names). `extract_and_write` verifies read-only via `git rev-parse --show-toplevel` and `HEAD` that `upstream_root` is an exact Git repository root, blocks symlink or traversal escapes in `_read_upstream_source`, parses all mapped files, renders all skeletons, computes nearest-landmark dependencies via `project_nearest_landmarks` (wrapping unmapped read/decode errors in `StageMapError`), and builds `DagGraph` completely in memory before `_write_outputs_atomically` stages fsynced `.tmp` files (cleaning up all `.tmp` files on `BaseException`) and calls `os.replace`. Limits: per-file `os.replace` cannot roll back already-replaced files on a mid-loop OS failure, and unmapped existing files such as `skeleton/README.md` are left untouched.

## Relationships

Uses [nearest-landmark import projection](import_projection.py.md), [DAG models](../models/dag.py.md), [Lean models](../models/lean.py.md), [the Lean 4 parser](../lean/parser.py.md) and [the stub emitter](../lean/stub_emitter.py.md). Reads [the 8-stage map](../../../dag/stage-map.json.md), emits [the proof DAG](../../../dag/dag.json.md) and all 41 `skeleton/<stage>/*.lean` files, is wired by [the `rh-extract` CLI](../cli/extract.py.md) and [upstream triage](../audit/triage.py.md), and is verified by [stage-map unit tests](../../../tests/unit/test_stage_map.py.md) and [CLI unit tests](../../../tests/unit/test_extract_cli.py.md).

## Evidence

- Resolves Lean module names from src/ and comparator/ paths and validates consecutive 0..N-1 stage ordering and global uniqueness of stages, paths, skeleton targets and modules. [1]
- Verifies the upstream Git repository root read-only and blocks symlink or path escapes when reading mapped source files. [3]

- Builds DagNodes with direct mapped Lean import edges by default or validated, deduplicated, self-edge-stripped projected_depends_on overrides, enforcing unique per-file declaration names and theorem/lemma landmark classification. [2]
- Completes all parsing, stub rendering, nearest-landmark import projection (wrapping unmapped read/decode failures in StageMapError), and DAG construction in memory before staging fsynced temporary files and atomically replacing outputs with cleanup on error. [4]
