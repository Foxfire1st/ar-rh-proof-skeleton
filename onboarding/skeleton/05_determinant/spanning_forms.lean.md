# skeleton/05_determinant/spanning_forms.lean

## Governing Overview

[Repository overview](../../overview.md)

## Purpose

Stubs Stage 05 auxiliary-form denominator and short-lift spanning lemmas extracted from upstream `SpanningForms.lean`.

## Code Commentary

Auto-generated from upstream `src/New/Determinant/AuxiliaryForms/SpanningForms.lean` at commit `41c2716658c42ba08eb0648994a2efe83082a95c` with `preserve_defs: false`. Contains 29 declarations (22 `lemma`, 4 `def`, 3 `abbrev`) across 331 lines. All declarations are stubbed with `sorry -- [EXTRACTED_STUB ...]` provenance lines. Key declarations include `New.Data.liftDenominator` (lines 12–17, upstream `src/New/Determinant/AuxiliaryForms/SpanningForms.lean:72-78`), `New.Data.exists_integral_lowerChainUpTo_eq_lastPathPeriod` (lines 131–152, upstream `src/New/Determinant/AuxiliaryForms/SpanningForms.lean:392-428`), `New.Data.exists_lowerChain_integral_and_weylNormSq_le` (lines 281–304, upstream `src/New/Determinant/AuxiliaryForms/SpanningForms.lean:663-740`), `New.Data.span_isShort_eq_top_gridLevelForms` (lines 306–313, upstream `src/New/Determinant/AuxiliaryForms/SpanningForms.lean:742-767`), `New.Data.exists_span_short_lifts_eq_top` (lines 315–331, upstream `src/New/Determinant/AuxiliaryForms/SpanningForms.lean:769-796`). Recorded as landmark node `05_determinant/spanning_forms` (`is_landmark: true`) with `depends_on: ["02_derivatives/count", "02_derivatives/integral_vacuum", "03_upper_bound/short_columns", "05_determinant/fit_twists", "05_determinant/one_square"]` in `dag/dag.json`. Never hand-edit; regenerate byte-identically with `rh-extract`.

## Relationships

Mapped in [the 8-stage map](../../dag/stage-map.json.md) under stage `05_determinant` (Lower Bound: The Determinant — Theorem C, Part 1), rendered by [the stub emitter](../../src/rh_pipeline/lean/stub_emitter.py.md) via [the stage-map extractor](../../src/rh_pipeline/graph/stage_map.py.md) and [`rh-extract`](../../src/rh_pipeline/cli/extract.py.md), indexed as landmark node `05_determinant/spanning_forms` in [the proof DAG](../../dag/dag.json.md), and round-trip checked by [stage-map unit tests](../../tests/unit/test_stage_map.py.md).

## Evidence

- Auto-generated header pins upstream src/New/Determinant/AuxiliaryForms/SpanningForms.lean at 41c2716658c42ba08eb0648994a2efe83082a95c with Preserve defs false. [1]
- Stubs New.Data.liftDenominator and lower-chain period and integrality lemmas culminating in New.Data.exists_integral_lowerChainUpTo_eq_lastPathPeriod. [2]
- Stubs the short auxiliary-form spanning lemmas New.Data.exists_lowerChain_integral_and_weylNormSq_le, New.Data.span_isShort_eq_top_gridLevelForms and New.Data.exists_span_short_lifts_eq_top. [3]
