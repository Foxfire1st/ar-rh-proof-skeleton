# skeleton/07_congruences/denominator_bound.lean

## Governing Overview

[Repository overview](../../overview.md)

## Purpose

Stubs Stage 07 first-order congruence denominator-bound theorems and preserves structures `IsNonzeroIn` and `IsLiftGenerator` verbatim.

## Code Commentary

Auto-generated from upstream `src/New/Congruences/FirstOrder/DenominatorBound.lean` at commit `41c2716658c42ba08eb0648994a2efe83082a95c` with `preserve_defs: false`. Contains 37 declarations (29 `lemma`, 3 `theorem`, 2 `def`, 2 `structure`, 1 `abbrev`) across 669 lines. Even with `preserve_defs: false`, its 2 `structure` declarations are emitted verbatim with trailing `-- [EXTRACTED_DEF ...]` comments while all other declarations are stubbed with `sorry -- [EXTRACTED_STUB ...]` lines. Key declarations include `New.Selmer.IsNonzeroIn` (lines 41–48, upstream `src/New/Congruences/FirstOrder/DenominatorBound.lean:155-162`), `New.Selmer.IsLiftGenerator` (lines 359–379, upstream `src/New/Congruences/FirstOrder/DenominatorBound.lean:744-764`), `New.Selmer.exists_operator_ellVal_le_liftValuationBound` (lines 593–613, upstream `src/New/Congruences/FirstOrder/DenominatorBound.lean:1116-1181`), `New.Selmer.exists_operator_det_clearingBound` (lines 623–646, upstream `src/New/Congruences/FirstOrder/DenominatorBound.lean:1202-1267`), `New.Selmer.denominatorLe_clearingBound` (lines 648–669, upstream `src/New/Congruences/FirstOrder/DenominatorBound.lean:1269-1293`). Recorded as landmark node `07_congruences/denominator_bound` (`is_landmark: true`) with `depends_on: ["02_derivatives/integral_vacuum", "05_determinant/fit_twists", "07_congruences/crystalline_defect", "07_congruences/cup_orthogonality_bounds"]` in `dag/dag.json`. Never hand-edit; regenerate byte-identically with `rh-extract`.

## Relationships

Mapped in [the 8-stage map](../../dag/stage-map.json.md) under stage `07_congruences` (Lower Bound: Congruences & Galois Representations — Theorem C, Part 3), rendered by [the stub emitter](../../src/rh_pipeline/lean/stub_emitter.py.md) via [the stage-map extractor](../../src/rh_pipeline/graph/stage_map.py.md) and [`rh-extract`](../../src/rh_pipeline/cli/extract.py.md), indexed as landmark node `07_congruences/denominator_bound` in [the proof DAG](../../dag/dag.json.md), and round-trip checked by [stage-map unit tests](../../tests/unit/test_stage_map.py.md).

## Evidence

- Auto-generated header pins upstream src/New/Congruences/FirstOrder/DenominatorBound.lean at 41c2716658c42ba08eb0648994a2efe83082a95c with Preserve defs false. [1]
- Preserves structures New.Selmer.IsNonzeroIn and New.Selmer.IsLiftGenerator verbatim with EXTRACTED_DEF tags alongside intermediate Taylor-Wiles and Ribet lemmas. [2]
- Stubs the three capstone denominator-bound theorems New.Selmer.exists_operator_ellVal_le_liftValuationBound, New.Selmer.exists_operator_det_clearingBound and New.Selmer.denominatorLe_clearingBound. [3]
