# skeleton/00_statement/statements.lean

## Governing Overview

[Repository overview](../../overview.md)

## Purpose

Stores the generated Stage 00 Lean 4 skeleton extracted from upstream `src/Statements.lean` with all four formal statement `def` bodies preserved verbatim.

## Code Commentary

Auto-generated from upstream `src/Statements.lean` at commit `41c2716658c42ba08eb0648994a2efe83082a95c` with `preserve_defs: true`. Emits four verbatim `def` declarations with trailing `-- [EXTRACTED_DEF ...]` provenance tags and body SHA-256 digests: `RiemannZetaZerosOnLine` (lines 5–6, upstream 7–8), `DirichletLNonvanishingRightOfHalf` (lines 9–11, upstream 10–12), `DirichletLZerosOnLine` (lines 14–16, upstream 14–16) and `PrimitiveDirichletLZerosOnLine` (lines 19–21, upstream 18–20). Contains no `theorem` or `lemma` declarations (`is_landmark: false` in `dag/dag.json`). Never hand-edit; regenerate byte-identically with `rh-extract`.

## Relationships

Mapped in [the 8-stage map](../../dag/stage-map.json.md), rendered by [the stub emitter](../../src/rh_pipeline/lean/stub_emitter.py.md) via [the stage-map extractor](../../src/rh_pipeline/graph/stage_map.py.md) and [`rh-extract`](../../src/rh_pipeline/cli/extract.py.md), indexed as node `00_statement/statements` in [the Stage 00 DAG](../../dag/dag.json.md) (imported by [challenge.lean](challenge.lean.md)) and round-trip checked by [stage-map unit tests](../../tests/unit/test_stage_map.py.md).

## Evidence

- Auto-generated header pins upstream src/Statements.lean at 41c2716658c42ba08eb0648994a2efe83082a95c with Preserve defs true. [1]
- Preserves RiemannZetaZerosOnLine and DirichletLNonvanishingRightOfHalf verbatim with EXTRACTED_DEF provenance tags. [2]
- Preserves DirichletLZerosOnLine and PrimitiveDirichletLZerosOnLine verbatim with EXTRACTED_DEF provenance tags. [3]
