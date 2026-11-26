# src/rh_pipeline/graph/import_projection.py

## Governing Overview

[Repository overview](../../../overview.md)

## Purpose

Scans leading Lean 4 header imports and projects each mapped node's dependencies across unmapped upstream modules to its nearest mapped landmarks in the proof DAG.

## Code Commentary

`scan_lean_header_imports` normalizes line endings, skips whitespace, `--` line comments, nested `/- ... -/` block comments, and `module`/`prelude` keywords, parses `[public] [meta] import [all] <Module>` forms (checking same-line tokens for `all`), and halts at the first non-header token so unmapped module bodies never need full closed-world parsing; it returns sorted unique module names. `_resolve_unmapped_module` rejects empty, `.`, `..`, or slash/backslash segments, probes `_CANDIDATE_PREFIXES = ("src/", "")` (deliberately omitting `comparator/` so comparator-only files cannot project edges), resolves symlinks, and enforces containment via `is_relative_to(root)` and `is_file()`. `project_nearest_landmarks` runs a shared-cache breadth-first search (`_project_node_dependencies`) from each mapped node's direct imports: hitting any mapped node in `module_to_node` records that dependency (excluding self-edges) and stops traversal along that branch so edges connect only to nearest mapped landmarks, while unmapped intermediates track `visited_unmapped` to terminate cleanly on diamonds and cycles. Limits: unresolvable external imports (e.g., `Mathlib.*`, `Init.*`) and escaping symlinks silently yield empty imports, whereas read/UTF-8 decode errors on resolved unmapped files propagate for `stage_map.extract_and_write` to wrap in `StageMapError`.

## Relationships

Uses `StageDefinition` from [DAG models](../models/dag.py.md) and `ExtractedModule` from [Lean models](../models/lean.py.md). Called by [the stage-map extractor](stage_map.py.md) to compute nearest-landmark edges for [the proof DAG](../../../dag/dag.json.md) and verified by [stage-map unit tests](../../../tests/unit/test_stage_map.py.md).

## Evidence

- Scans leading Lean 4 file headers for sorted unique imports across module/prelude and public/meta/all modifiers while skipping line and nested block comments and stopping at the first non-header token. [1]
- Resolves unmapped upstream modules strictly under src/ or repository root (omitting comparator/) while rejecting unsafe path segments and symlink escapes outside resolved_root. [2]
- Projects each mapped node's imports via cached BFS across unmapped intermediates to nearest mapped landmarks, stopping at mapped nodes, handling diamonds and cycles, and excluding self-edges. [3]
