# skeleton/01_reduction/short_and_enough.lean

## Governing Overview

[Repository overview](../../overview.md)

## Purpose

Stubs Stage 01 Theorem A (`span_short_integralRamified_eq_top`, `indexLe_normalizedPoincare`) and Theorem B (`enough_forms`) assembling the short-and-enough inputs to `star_inequality`.

## Code Commentary

Auto-generated from upstream `src/New/Reduction/ShortAndEnough.lean` at commit `41c2716658c42ba08eb0648994a2efe83082a95c` with `preserve_defs: false`. Contains 24 declarations (13 `lemma`, 6 `def`, 4 `theorem`, 1 `abbrev`) across 181 lines. All declarations are stubbed with `sorry -- [EXTRACTED_STUB ...]` provenance lines. Key declarations include `New.Data.normalizedPoincare` (lines 59–62, upstream `src/New/Reduction/ShortAndEnough.lean:139-142`), `New.Data.span_short_integralRamified_eq_top` (lines 89–105, upstream `src/New/Reduction/ShortAndEnough.lean:180-224`), `New.Data.indexLe_normalizedPoincare` (lines 136–142, upstream `src/New/Reduction/ShortAndEnough.lean:309-318`), `New.Data.exists_le_finrank_iSup_range_normalizedPoincare` (lines 160–169, upstream `src/New/Reduction/ShortAndEnough.lean:344-380`), `New.Data.enough_forms` (lines 171–181, upstream `src/New/Reduction/ShortAndEnough.lean:382-392`). Recorded as landmark node `01_reduction/short_and_enough` (`is_landmark: true`) with `depends_on: ["01_reduction/twisted_lattice", "03_upper_bound/archimedean_size", "03_upper_bound/local_divisibility", "03_upper_bound/seesaw_identity", "04_spanning/spanning_theorem", "05_determinant/norm_of_chain"]` in `dag/dag.json`. Never hand-edit; regenerate byte-identically with `rh-extract`.

## Relationships

Mapped in [the 8-stage map](../../dag/stage-map.json.md) under stage `01_reduction` (The Theorem and Its Reduction), rendered by [the stub emitter](../../src/rh_pipeline/lean/stub_emitter.py.md) via [the stage-map extractor](../../src/rh_pipeline/graph/stage_map.py.md) and [`rh-extract`](../../src/rh_pipeline/cli/extract.py.md), indexed as landmark node `01_reduction/short_and_enough` in [the proof DAG](../../dag/dag.json.md), and round-trip checked by [stage-map unit tests](../../tests/unit/test_stage_map.py.md).

## Evidence

- Auto-generated header pins upstream src/New/Reduction/ShortAndEnough.lean at 41c2716658c42ba08eb0648994a2efe83082a95c with Preserve defs false. [1]
- Stubs Theorem A (a) New.Data.span_short_integralRamified_eq_top through Theorem A (b) New.Data.indexLe_normalizedPoincare with EXTRACTED_STUB markers. [2]
- Stubs New.Data.exists_le_finrank_iSup_range_normalizedPoincare and Theorem B New.Data.enough_forms bounding the dimension spanned by few translates. [3]
