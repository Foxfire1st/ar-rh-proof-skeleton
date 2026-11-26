# skeleton/05_determinant/long_forms.lean

## Governing Overview

[Repository overview](../../overview.md)

## Purpose

Stubs Stage 05 Theorem C (`New.Data.integral_forms_long`) and auxiliary-form covolume lower bounds extracted from upstream `LongForms.lean`.

## Code Commentary

Auto-generated from upstream `src/New/Determinant/Twist/LongForms.lean` at commit `41c2716658c42ba08eb0648994a2efe83082a95c` with `preserve_defs: false`. Contains 9 declarations (4 `lemma`, 4 `theorem`, 1 `structure`) across 112 lines. Even with `preserve_defs: false`, its 1 `structure` declaration is emitted verbatim with trailing `-- [EXTRACTED_DEF ...]` comments while all other declarations are stubbed with `sorry -- [EXTRACTED_STUB ...]` lines. Key declarations include `New.Data.IsAuxiliaryForm` (lines 5–15, upstream `src/New/Determinant/Twist/LongForms.lean:44-54`), `New.Data.exists_span_isAuxiliaryForm_eq_top` (lines 18–31, upstream `src/New/Determinant/Twist/LongForms.lean:57-105`), `New.Data.exists_hasSmallRatios_heightLe_liftDeterminant_div` (lines 53–60, upstream `src/New/Determinant/Twist/LongForms.lean:139-154`), `New.Data.integral_forms_long_with_completion_cost` (lines 92–100, upstream `src/New/Determinant/Twist/LongForms.lean:211-246`), `New.Data.integral_forms_long` (lines 102–112, upstream `src/New/Determinant/Twist/LongForms.lean:248-266`). Recorded as landmark node `05_determinant/long_forms` (`is_landmark: true`) with `depends_on: ["01_reduction/twisted_lattice", "03_upper_bound/archimedean_size", "05_determinant/norm_of_chain", "05_determinant/spanning_forms", "06_one_lift/lift_volume_height", "07_congruences/fit_twists_exist"]` in `dag/dag.json`. Never hand-edit; regenerate byte-identically with `rh-extract`.

## Relationships

Mapped in [the 8-stage map](../../dag/stage-map.json.md) under stage `05_determinant` (Lower Bound: The Determinant — Theorem C, Part 1), rendered by [the stub emitter](../../src/rh_pipeline/lean/stub_emitter.py.md) via [the stage-map extractor](../../src/rh_pipeline/graph/stage_map.py.md) and [`rh-extract`](../../src/rh_pipeline/cli/extract.py.md), indexed as landmark node `05_determinant/long_forms` in [the proof DAG](../../dag/dag.json.md), and round-trip checked by [stage-map unit tests](../../tests/unit/test_stage_map.py.md).

## Evidence

- Auto-generated header pins upstream src/New/Determinant/Twist/LongForms.lean at 41c2716658c42ba08eb0648994a2efe83082a95c with Preserve defs false. [1]
- Preserves structure New.Data.IsAuxiliaryForm verbatim with EXTRACTED_DEF tag and stubs New.Data.exists_span_isAuxiliaryForm_eq_top and New.Data.exists_hasSmallRatios_heightLe_liftDeterminant_div. [2]
- Stubs the covolume lower-bound theorems New.Data.integral_forms_long_with_completion_cost and New.Data.integral_forms_long. [3]
