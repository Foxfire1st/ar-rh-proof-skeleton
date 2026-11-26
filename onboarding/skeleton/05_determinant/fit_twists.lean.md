# skeleton/05_determinant/fit_twists.lean

## Governing Overview

[Repository overview](../../overview.md)

## Purpose

Defines the fit-twist and denominator-bound contracts (`IsMainPair`, `HasDenominatorsLe`, `HasSmallRatios`, `IsRegularFamily`) for Stage 05 determinant twisting.

## Code Commentary

Auto-generated from upstream `src/New/Determinant/Twist/FitTwists.lean` at commit `41c2716658c42ba08eb0648994a2efe83082a95c` with `preserve_defs: false`. Contains 17 declarations (6 `structure`, 5 `lemma`, 3 `abbrev`, 3 `def`) across 163 lines. Even with `preserve_defs: false`, its 6 `structure` declarations are emitted verbatim with trailing `-- [EXTRACTED_DEF ...]` comments while all other declarations are stubbed with `sorry -- [EXTRACTED_STUB ...]` lines. Key declarations include `New.Data.CriticalPair.ratio` (lines 24–29, upstream `src/New/Determinant/Twist/FitTwists.lean:78-84`), `New.Data.CriticalPair.IsMainPair` (lines 53–71, upstream `src/New/Determinant/Twist/FitTwists.lean:122-140`), `New.Data.HasDenominatorsLe` (lines 102–121, upstream `src/New/Determinant/Twist/FitTwists.lean:177-196`), `New.Data.HasSmallRatios` (lines 144–154, upstream `src/New/Determinant/Twist/FitTwists.lean:216-226`), `New.Data.IsRegularFamily` (lines 157–162, upstream `src/New/Determinant/Twist/FitTwists.lean:228-233`). Recorded as landmark node `05_determinant/fit_twists` (`is_landmark: true`) with `depends_on: ["02_derivatives/integral_vacuum"]` in `dag/dag.json`. Never hand-edit; regenerate byte-identically with `rh-extract`.

## Relationships

Mapped in [the 8-stage map](../../dag/stage-map.json.md) under stage `05_determinant` (Lower Bound: The Determinant — Theorem C, Part 1), rendered by [the stub emitter](../../src/rh_pipeline/lean/stub_emitter.py.md) via [the stage-map extractor](../../src/rh_pipeline/graph/stage_map.py.md) and [`rh-extract`](../../src/rh_pipeline/cli/extract.py.md), indexed as landmark node `05_determinant/fit_twists` in [the proof DAG](../../dag/dag.json.md), and round-trip checked by [stage-map unit tests](../../tests/unit/test_stage_map.py.md).

## Evidence

- Auto-generated header pins upstream src/New/Determinant/Twist/FitTwists.lean at 41c2716658c42ba08eb0648994a2efe83082a95c with Preserve defs false. [1]
- Stubs New.Data.CriticalPair.ratio, New.Data.RankOneIndex, New.Data.goodLift and New.Data.CriticalPair.IsRamified and preserves structure New.Data.CriticalPair.IsMainPair verbatim with EXTRACTED_DEF tag. [2]
- Preserves denominator-bound and twist-family structures New.Data.HasDenominatorsLe, New.Data.IsDenominatorBound, New.Data.IsNegligible, New.Data.HasSmallRatios and New.Data.IsRegularFamily verbatim with EXTRACTED_DEF tags. [3]
