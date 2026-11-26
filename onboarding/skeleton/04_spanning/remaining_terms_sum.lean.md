# skeleton/04_spanning/remaining_terms_sum.lean

## Governing Overview

[Repository overview](../../overview.md)

## Purpose

Stubs Stage 04 off-diagonal remaining-term pair bounds and link-by-link second-moment theorems extracted from upstream `Sum.lean`.

## Code Commentary

Auto-generated from upstream `src/New/Spanning/RemainingTerms/Sum.lean` at commit `41c2716658c42ba08eb0648994a2efe83082a95c` with `preserve_defs: false`. Contains 26 declarations (19 `lemma`, 4 `theorem`, 2 `def`, 1 `abbrev`) across 325 lines. All declarations are stubbed with `sorry -- [EXTRACTED_STUB ...]` provenance lines. Key declarations include `New.Filling.sum_termSize_mul_not_inSameLinkClass_le` (lines 147–160, upstream `src/New/Spanning/RemainingTerms/Sum.lean:377-446`), `New.Filling.sum_termSize_mul_inSameLinkClass_le` (lines 162–176, upstream `src/New/Spanning/RemainingTerms/Sum.lean:448-491`), `New.Filling.secondMoment_remainingTerms_le` (lines 289–301, upstream `src/New/Spanning/RemainingTerms/Sum.lean:678-744`), `New.Filling.linkSecondMoment_le_of_le_meetNorm` (lines 303–325, upstream `src/New/Spanning/RemainingTerms/Sum.lean:746-837`). Recorded as landmark node `04_spanning/remaining_terms_sum` (`is_landmark: true`) with `depends_on: []` in `dag/dag.json` (a nearest-landmark dependency root). Never hand-edit; regenerate byte-identically with `rh-extract`.

## Relationships

Mapped in [the 8-stage map](../../dag/stage-map.json.md) under stage `04_spanning` (Spanning: Second Moment — Theorem B), rendered by [the stub emitter](../../src/rh_pipeline/lean/stub_emitter.py.md) via [the stage-map extractor](../../src/rh_pipeline/graph/stage_map.py.md) and [`rh-extract`](../../src/rh_pipeline/cli/extract.py.md), indexed as landmark node `04_spanning/remaining_terms_sum` in [the proof DAG](../../dag/dag.json.md), and round-trip checked by [stage-map unit tests](../../tests/unit/test_stage_map.py.md).

## Evidence

- Auto-generated header pins upstream src/New/Spanning/RemainingTerms/Sum.lean at 41c2716658c42ba08eb0648994a2efe83082a95c with Preserve defs false. [1]
- Stubs pair-bound theorems New.Filling.sum_termSize_mul_not_inSameLinkClass_le and New.Filling.sum_termSize_mul_inSameLinkClass_le (upstream 377-491). [2]
- Stubs theorems New.Filling.secondMoment_remainingTerms_le and New.Filling.linkSecondMoment_le_of_le_meetNorm bounding remaining terms link by link (upstream 678-837). [3]
