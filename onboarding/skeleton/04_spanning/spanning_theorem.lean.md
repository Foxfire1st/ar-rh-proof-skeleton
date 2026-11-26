# skeleton/04_spanning/spanning_theorem.lean

## Governing Overview

[Repository overview](../../overview.md)

## Purpose

Stubs Stage 04 second-moment spanning theorems culminating in `New.Data.isSpread_goodPoincare`.

## Code Commentary

Auto-generated from upstream `src/New/Spanning/SecondMoment/SpanningTheorem.lean` at commit `41c2716658c42ba08eb0648994a2efe83082a95c` with `preserve_defs: false`. Contains 21 declarations (10 `lemma`, 6 `theorem`, 5 `def`) across 167 lines. All declarations are stubbed with `sorry -- [EXTRACTED_STUB ...]` provenance lines. Key declarations include `New.Data.targetDim_le_card_mul_besselDegree_mul_mass` (lines 13–17, upstream `src/New/Spanning/SecondMoment/SpanningTheorem.lean:91-102`), `New.Data.hasAmplitudeBlocks_scaledPoincare` (lines 59–71, upstream `src/New/Spanning/SecondMoment/SpanningTheorem.lean:153-173`), `New.Data.isNearIsometry_e₀_comp_scaledPoincare` (lines 111–119, upstream `src/New/Spanning/SecondMoment/SpanningTheorem.lean:247-259`), `New.Data.sum_hilbertSchmidtNorm_scaledPoincare_sq_le` (lines 121–130, upstream `src/New/Spanning/SecondMoment/SpanningTheorem.lean:261-276`), `New.Data.isSpread_goodPoincare` (lines 159–167, upstream `src/New/Spanning/SecondMoment/SpanningTheorem.lean:327-359`). Recorded as landmark node `04_spanning/spanning_theorem` (`is_landmark: true`) with `depends_on: ["02_derivatives/integral_vacuum", "03_upper_bound/short_columns", "04_spanning/equal_multiplicities", "04_spanning/moments"]` in `dag/dag.json`. Never hand-edit; regenerate byte-identically with `rh-extract`.

## Relationships

Mapped in [the 8-stage map](../../dag/stage-map.json.md) under stage `04_spanning` (Spanning: Second Moment — Theorem B), rendered by [the stub emitter](../../src/rh_pipeline/lean/stub_emitter.py.md) via [the stage-map extractor](../../src/rh_pipeline/graph/stage_map.py.md) and [`rh-extract`](../../src/rh_pipeline/cli/extract.py.md), indexed as landmark node `04_spanning/spanning_theorem` in [the proof DAG](../../dag/dag.json.md), and round-trip checked by [stage-map unit tests](../../tests/unit/test_stage_map.py.md).

## Evidence

- Auto-generated header pins upstream src/New/Spanning/SecondMoment/SpanningTheorem.lean at 41c2716658c42ba08eb0648994a2efe83082a95c with Preserve defs false. [1]
- Stubs theorems New.Data.bad_types_rare, New.Data.isNearIsometry_e₀_comp_scaledPoincare and New.Data.sum_hilbertSchmidtNorm_scaledPoincare_sq_le (upstream 234-276). [2]
- Stubs theorem New.Data.isSpread_goodPoincare proving one translate of goodPoincare is spread across the target (upstream 327-359, body_lines=26). [3]
