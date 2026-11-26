# skeleton/04_spanning/energy_estimate.lean

## Governing Overview

[Repository overview](../../overview.md)

## Purpose

Stubs Stage 04 Siegel coset-amplitude bounds and the main-term link second-moment theorem `New.Filling.linkSecondMoment_isOpenElement_le`.

## Code Commentary

Auto-generated from upstream `src/New/Spanning/MainTerm/EnergyEstimate.lean` at commit `41c2716658c42ba08eb0648994a2efe83082a95c` with `preserve_defs: false`. Contains 15 declarations (13 `lemma`, 1 `abbrev`, 1 `theorem`) across 235 lines. All declarations are stubbed with `sorry -- [EXTRACTED_STUB ...]` provenance lines. Key declarations include `New.Filling.normalizingConst` (lines 45–49, upstream `src/New/Spanning/MainTerm/EnergyEstimate.lean:141-147`), `New.Filling.IsFillingData.mul_sum_norm_sum_cosetAmplitude_sq_le` (lines 148–181, upstream `src/New/Spanning/MainTerm/EnergyEstimate.lean:465-551`), `New.Filling.sum_norm_sum_cosetAmplitude_sq_le` (lines 183–213, upstream `src/New/Spanning/MainTerm/EnergyEstimate.lean:553-618`), `New.Filling.linkSecondMoment_isOpenElement_le` (lines 215–235, upstream `src/New/Spanning/MainTerm/EnergyEstimate.lean:620-685`). Recorded as landmark node `04_spanning/energy_estimate` (`is_landmark: true`) with `depends_on: ["02_derivatives/integral_vacuum"]` in `dag/dag.json`. Never hand-edit; regenerate byte-identically with `rh-extract`.

## Relationships

Mapped in [the 8-stage map](../../dag/stage-map.json.md) under stage `04_spanning` (Spanning: Second Moment — Theorem B), rendered by [the stub emitter](../../src/rh_pipeline/lean/stub_emitter.py.md) via [the stage-map extractor](../../src/rh_pipeline/graph/stage_map.py.md) and [`rh-extract`](../../src/rh_pipeline/cli/extract.py.md), indexed as landmark node `04_spanning/energy_estimate` in [the proof DAG](../../dag/dag.json.md), and round-trip checked by [stage-map unit tests](../../tests/unit/test_stage_map.py.md).

## Evidence

- Auto-generated header pins upstream src/New/Spanning/MainTerm/EnergyEstimate.lean at 41c2716658c42ba08eb0648994a2efe83082a95c with Preserve defs false. [1]
- Stubs Siegel's step New.Filling.IsFillingData.mul_sum_norm_sum_cosetAmplitude_sq_le bounding coset amplitude squares (upstream 465-551, body_lines=55). [2]
- Stubs the sole theorem New.Filling.linkSecondMoment_isOpenElement_le bounding the main-term link second moment on open elements (upstream 620-685, body_lines=47). [3]
