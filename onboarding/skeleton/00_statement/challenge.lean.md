# skeleton/00_statement/challenge.lean

## Governing Overview

[Repository overview](../../overview.md)

## Purpose

Stores the generated Stage 00 Lean 4 skeleton extracted from upstream `src/Challenge.lean`, preserving its four `Prop` definitions verbatim and stubbing its five challenge theorems.

## Code Commentary

Auto-generated from upstream `src/Challenge.lean` at commit `c900e67c2fcd1fdb041685820bbd2b867fbc5a4b` with `preserve_defs: true`. Emits four verbatim `def` declarations (`RiemannZetaZerosOnLine`, `DirichletLNonvanishingRightOfHalf`, `DirichletLZerosOnLine`, `PrimitiveDirichletLZerosOnLine` at lines 5–22, upstream 3–16) with `-- [EXTRACTED_DEF ...]` comments, followed by five stubbed `theorem` declarations (`riemannHypothesis`, `riemannZeta_eq_zero_re`, `LFunction_ne_zero_of_half_lt_re`, `LFunction_eq_zero_re`, `LFunction_eq_zero_re_of_isPrimitive` at lines 24–38, upstream 18–27) carrying `sorry -- [EXTRACTED_STUB ...]` markers. Recorded as a landmark node with `depends_on: []` in `dag/dag.json` because upstream `src/Challenge.lean` imports only `Mathlib` rather than `Statements`. Never hand-edit; regenerate byte-identically with `rh-extract`.

## Relationships

Mapped in [the 21-stage map](../../dag/stage-map.json.md), rendered by [the stub emitter](../../src/rh_pipeline/lean/stub_emitter.py.md) via [the stage-map extractor](../../src/rh_pipeline/graph/stage_map.py.md) and [`rh-extract`](../../src/rh_pipeline/cli/extract.py.md), indexed as node `00_statement/challenge` in [the Stage 00 DAG](../../dag/dag.json.md) and round-trip checked by [stage-map unit tests](../../tests/unit/test_stage_map.py.md).

## Evidence

- Auto-generated header pins upstream src/Challenge.lean at c900e67c2fcd1fdb041685820bbd2b867fbc5a4b with Preserve defs true. [1]
- Preserves all four statement Prop definitions verbatim with EXTRACTED_DEF provenance tags. [2]
- Stubs all five challenge theorems with EXTRACTED_STUB sorry lines and upstream line ranges. [3]
