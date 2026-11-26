# skeleton/06_one_lift/lift_volume_height.lean

## Governing Overview

[Repository overview](../../overview.md)

## Purpose

Stubs the Stage 06 comparison-operator contract (`New.Data.IsComparisonOperator` verbatim) and one-lift determinant volume-height bound.

## Code Commentary

Auto-generated from upstream `src/New/OneLift/LiftVolumeHeight.lean` at commit `41c2716658c42ba08eb0648994a2efe83082a95c` with `preserve_defs: false`. Contains 29 declarations (24 `lemma`, 2 `def`, 1 `abbrev`, 1 `structure`, 1 `theorem`) across 350 lines. Even with `preserve_defs: false`, its 1 `structure` declaration is emitted verbatim with trailing `-- [EXTRACTED_DEF ...]` comments while all other declarations are stubbed with `sorry -- [EXTRACTED_STUB ...]` lines. Key declarations include `New.Data.IsComparisonOperator` (lines 62–88, upstream `src/New/OneLift/LiftVolumeHeight.lean:172-198`), `New.Data.comparisonOp` (lines 91–96, upstream `src/New/OneLift/LiftVolumeHeight.lean:200-206`), `New.Data.isComparisonOperator_comparisonOp` (lines 245–257, upstream `src/New/OneLift/LiftVolumeHeight.lean:548-579`), `New.Data.IsComparisonOperator.exists_normDet_sq_mul_det_eq` (lines 291–303, upstream `src/New/OneLift/LiftVolumeHeight.lean:622-646`), `New.Data.heightLe_liftDeterminant_ratio` (lines 339–350, upstream `src/New/OneLift/LiftVolumeHeight.lean:726-783`). Recorded as landmark node `06_one_lift/lift_volume_height` (`is_landmark: true`) with `depends_on: ["06_one_lift/gluing_maps", "06_one_lift/twisted_value_heights"]` in `dag/dag.json`. Never hand-edit; regenerate byte-identically with `rh-extract`.

## Relationships

Mapped in [the 8-stage map](../../dag/stage-map.json.md) under stage `06_one_lift` (Lower Bound: One Lift — Theorem C, Part 2), rendered by [the stub emitter](../../src/rh_pipeline/lean/stub_emitter.py.md) via [the stage-map extractor](../../src/rh_pipeline/graph/stage_map.py.md) and [`rh-extract`](../../src/rh_pipeline/cli/extract.py.md), indexed as landmark node `06_one_lift/lift_volume_height` in [the proof DAG](../../dag/dag.json.md), and round-trip checked by [stage-map unit tests](../../tests/unit/test_stage_map.py.md).

## Evidence

- Auto-generated header pins upstream src/New/OneLift/LiftVolumeHeight.lean at 41c2716658c42ba08eb0648994a2efe83082a95c with Preserve defs false. [1]
- Preserves structure New.Data.IsComparisonOperator verbatim with EXTRACTED_DEF tag and stubs def New.Data.comparisonOp. [2]
- Stubs theorem New.Data.isComparisonOperator_comparisonOp, New.Data.IsComparisonOperator.exists_normDet_sq_mul_det_eq and the one-lift cost bound New.Data.heightLe_liftDeterminant_ratio. [3]
