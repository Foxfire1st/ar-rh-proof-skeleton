# skeleton/06_one_lift/three_exchanges.lean

## Governing Overview

[Repository overview](../../overview.md)

## Purpose

Stubs Stage 06 three-exchange operator identities and kernel class-sum representations extracted from upstream `ThreeExchanges.lean`.

## Code Commentary

Auto-generated from upstream `src/New/OneLift/ComparisonOperator/ThreeExchanges.lean` at commit `41c2716658c42ba08eb0648994a2efe83082a95c` with `preserve_defs: false`. Contains 43 declarations (31 `lemma`, 9 `def`, 3 `abbrev`) across 466 lines. All declarations are stubbed with `sorry -- [EXTRACTED_STUB ...]` provenance lines. Key declarations include `New.Data.exchangePair` (lines 49–58, upstream `src/New/OneLift/ComparisonOperator/ThreeExchanges.lean:138-177`), `New.Data.heckeValueProd` (lines 60–67, upstream `src/New/OneLift/ComparisonOperator/ThreeExchanges.lean:179-187`), `New.Rows.carrierForm_ne_zero` (lines 161–165, upstream `src/New/OneLift/ComparisonOperator/ThreeExchanges.lean:530-558`), `New.Data.exists_mul_thirdExchangeLeftSide_eq_choiceFn` (lines 328–339, upstream `src/New/OneLift/ComparisonOperator/ThreeExchanges.lean:811-860`), `New.Data.exists_choiceFn_eq_mul_kernelClassSum` (lines 448–454, upstream `src/New/OneLift/ComparisonOperator/ThreeExchanges.lean:1135-1143`). Recorded as landmark node `06_one_lift/three_exchanges` (`is_landmark: true`) with `depends_on: ["02_derivatives/count", "02_derivatives/integral_vacuum", "03_upper_bound/short_columns", "05_determinant/fit_twists", "05_determinant/theorem_for_partners"]` in `dag/dag.json`. Never hand-edit; regenerate byte-identically with `rh-extract`.

## Relationships

Mapped in [the 8-stage map](../../dag/stage-map.json.md) under stage `06_one_lift` (Lower Bound: One Lift — Theorem C, Part 2), rendered by [the stub emitter](../../src/rh_pipeline/lean/stub_emitter.py.md) via [the stage-map extractor](../../src/rh_pipeline/graph/stage_map.py.md) and [`rh-extract`](../../src/rh_pipeline/cli/extract.py.md), indexed as landmark node `06_one_lift/three_exchanges` in [the proof DAG](../../dag/dag.json.md), and round-trip checked by [stage-map unit tests](../../tests/unit/test_stage_map.py.md).

## Evidence

- Auto-generated header pins upstream src/New/OneLift/ComparisonOperator/ThreeExchanges.lean at 41c2716658c42ba08eb0648994a2efe83082a95c with Preserve defs false. [1]
- Stubs New.Data.exchangePair, New.Data.heckeValueProd and carrier-form lemmas culminating in New.Rows.carrierForm_ne_zero. [2]
- Stubs the third-exchange identity New.Data.exists_mul_thirdExchangeLeftSide_eq_choiceFn through the theta-series representation New.Data.exists_choiceFn_eq_mul_kernelClassSum. [3]
