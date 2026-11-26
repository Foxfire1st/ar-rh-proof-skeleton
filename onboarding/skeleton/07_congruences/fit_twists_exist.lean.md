# skeleton/07_congruences/fit_twists_exist.lean

## Governing Overview

[Repository overview](../../overview.md)

## Purpose

Stubs the Stage 07 capstone existence of fit regular twist families (`New.exists_isRegularFamily`) and supporting Selmer/denominator lemmas.

## Code Commentary

Auto-generated from upstream `src/New/Congruences/FitTwistsExist.lean` at commit `41c2716658c42ba08eb0648994a2efe83082a95c` with `preserve_defs: false`. Contains 19 declarations (18 `lemma`, 1 `theorem`) across 259 lines. All declarations are stubbed with `sorry -- [EXTRACTED_STUB ...]` provenance lines. Key declarations include `New.Selmer.range_goodLift_le_span_hasLiftHeckePolynomial` (lines 136–160, upstream `src/New/Congruences/FitTwistsExist.lean:320-359`), `New.Selmer.exists_mFoldTwist_selmerLengthLe_inv_ratio_le` (lines 162–190, upstream `src/New/Congruences/FitTwistsExist.lean:361-408`), `New.Selmer.denominatorLe_range_goodLift` (lines 192–213, upstream `src/New/Congruences/FitTwistsExist.lean:410-439`), `New.Selmer.exists_mFoldTwist_hasDenominatorsLe` (lines 236–252, upstream `src/New/Congruences/FitTwistsExist.lean:471-518`), `New.exists_isRegularFamily` (lines 254–259, upstream `src/New/Congruences/FitTwistsExist.lean:520-543`). Recorded as landmark node `07_congruences/fit_twists_exist` (`is_landmark: true`) with `depends_on: ["02_derivatives/integral_vacuum", "03_upper_bound/short_columns", "05_determinant/norm_of_chain", "07_congruences/denominator_bound", "07_congruences/rows_from_eigensystems", "07_congruences/twisting_theorem"]` in `dag/dag.json`. Never hand-edit; regenerate byte-identically with `rh-extract`.

## Relationships

Mapped in [the 8-stage map](../../dag/stage-map.json.md) under stage `07_congruences` (Lower Bound: Congruences & Galois Representations — Theorem C, Part 3), rendered by [the stub emitter](../../src/rh_pipeline/lean/stub_emitter.py.md) via [the stage-map extractor](../../src/rh_pipeline/graph/stage_map.py.md) and [`rh-extract`](../../src/rh_pipeline/cli/extract.py.md), indexed as landmark node `07_congruences/fit_twists_exist` in [the proof DAG](../../dag/dag.json.md), and round-trip checked by [stage-map unit tests](../../tests/unit/test_stage_map.py.md).

## Evidence

- Auto-generated header pins upstream src/New/Congruences/FitTwistsExist.lean at 41c2716658c42ba08eb0648994a2efe83082a95c with Preserve defs false. [1]
- Stubs theorem New.Selmer.range_goodLift_le_span_hasLiftHeckePolynomial and lemmas New.Selmer.exists_mFoldTwist_selmerLengthLe_inv_ratio_le and New.Selmer.denominatorLe_range_goodLift. [2]
- Stubs New.Selmer.exists_mFoldTwist_hasDenominatorsLe and the Stage 07 capstone lemma New.exists_isRegularFamily. [3]
