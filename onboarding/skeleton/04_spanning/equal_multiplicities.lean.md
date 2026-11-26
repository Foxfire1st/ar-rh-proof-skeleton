# skeleton/04_spanning/equal_multiplicities.lean

## Governing Overview

[Repository overview](../../overview.md)

## Purpose

Stubs Stage 04 Hecke trace/charpoly/determinant comparison and equal eigenspace multiplicity theorems extracted from upstream `EqualMultiplicities.lean`.

## Code Commentary

Auto-generated from upstream `src/New/Spanning/Multiplicities/EqualMultiplicities.lean` at commit `41c2716658c42ba08eb0648994a2efe83082a95c` with `preserve_defs: false`. Contains 17 declarations (9 `lemma`, 7 `theorem`, 1 `def`) across 179 lines. All declarations are stubbed with `sorry -- [EXTRACTED_STUB ...]` provenance lines. Key declarations include `New.Data.trace_heckeOnGood_eq` (lines 90–96, upstream `src/New/Spanning/Multiplicities/EqualMultiplicities.lean:273-280`), `New.Data.charpoly_heckeOnGood_eq` (lines 98–104, upstream `src/New/Spanning/Multiplicities/EqualMultiplicities.lean:282-291`), `New.Data.det_rallisOnGood_eq` (lines 137–144, upstream `src/New/Spanning/Multiplicities/EqualMultiplicities.lean:338-351`), `New.Data.finrank_goodForms_inf_eigenPart_eq` (lines 157–165, upstream `src/New/Spanning/Multiplicities/EqualMultiplicities.lean:376-401`), `New.Data.targetDim_eq_finrank_goodForms_spaceW` (lines 175–179, upstream `src/New/Spanning/Multiplicities/EqualMultiplicities.lean:411-415`). Recorded as landmark node `04_spanning/equal_multiplicities` (`is_landmark: true`) with `depends_on: ["02_derivatives/integral_vacuum"]` in `dag/dag.json`. Never hand-edit; regenerate byte-identically with `rh-extract`.

## Relationships

Mapped in [the 8-stage map](../../dag/stage-map.json.md) under stage `04_spanning` (Spanning: Second Moment — Theorem B), rendered by [the stub emitter](../../src/rh_pipeline/lean/stub_emitter.py.md) via [the stage-map extractor](../../src/rh_pipeline/graph/stage_map.py.md) and [`rh-extract`](../../src/rh_pipeline/cli/extract.py.md), indexed as landmark node `04_spanning/equal_multiplicities` in [the proof DAG](../../dag/dag.json.md), and round-trip checked by [stage-map unit tests](../../tests/unit/test_stage_map.py.md).

## Evidence

- Auto-generated header pins upstream src/New/Spanning/Multiplicities/EqualMultiplicities.lean at 41c2716658c42ba08eb0648994a2efe83082a95c with Preserve defs false. [1]
- Stubs theorems New.Data.trace_heckeOnGood_eq, charpoly_heckeOnGood_eq and det_heckeOnGood_eq equating Hecke invariants across hermitian signatures (upstream 273-299). [2]
- Stubs theorems New.Data.finrank_goodForms_inf_eigenPart_eq and New.Data.targetDim_eq_finrank_goodForms_spaceW establishing equal eigenspace multiplicities and target dimension (upstream 376-415). [3]
