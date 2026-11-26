# skeleton/02_derivatives/tight_frames.lean

## Governing Overview

[Repository overview](../../overview.md)

## Purpose

Preserves Stage 02 structure `New.Parameters.IsResolutionProduct` verbatim and stubs tight-frame CM-curve definitions and resolution-product existence lemmas.

## Code Commentary

Auto-generated from upstream `src/New/Derivatives/Vacuum/TightFrames.lean` at commit `41c2716658c42ba08eb0648994a2efe83082a95c` with `preserve_defs: false`. Contains 47 declarations (37 `lemma`, 9 `def`, 1 `structure`) across 393 lines. Even with `preserve_defs: false`, its 1 `structure` declaration is emitted verbatim with trailing `-- [EXTRACTED_DEF ...]` comments while all other declarations are stubbed with `sorry -- [EXTRACTED_STUB ...]` lines. Key declarations include `New.Parameters.cmCurve` (lines 5–8, upstream `src/New/Derivatives/Vacuum/TightFrames.lean:85-88`), `New.Parameters.jetScale` (lines 50–53, upstream `src/New/Derivatives/Vacuum/TightFrames.lean:128-130`), `New.Parameters.isTightFrame_zeta_pow` (lines 159–164, upstream `src/New/Derivatives/Vacuum/TightFrames.lean:408-413`), `New.Parameters.IsResolutionProduct` (lines 281–296, upstream `src/New/Derivatives/Vacuum/TightFrames.lean:674-689`), `New.Parameters.exists_isResolutionProduct_of_isTightFrame` (lines 369–393, upstream `src/New/Derivatives/Vacuum/TightFrames.lean:828-911`). Recorded as landmark node `02_derivatives/tight_frames` (`is_landmark: true`) with `depends_on: []` in `dag/dag.json` (a nearest-landmark dependency root). Never hand-edit; regenerate byte-identically with `rh-extract`.

## Relationships

Mapped in [the 8-stage map](../../dag/stage-map.json.md) under stage `02_derivatives` (Derivatives of Theta Functions), rendered by [the stub emitter](../../src/rh_pipeline/lean/stub_emitter.py.md) via [the stage-map extractor](../../src/rh_pipeline/graph/stage_map.py.md) and [`rh-extract`](../../src/rh_pipeline/cli/extract.py.md), indexed as landmark node `02_derivatives/tight_frames` in [the proof DAG](../../dag/dag.json.md), and round-trip checked by [stage-map unit tests](../../tests/unit/test_stage_map.py.md).

## Evidence

- Auto-generated header pins upstream src/New/Derivatives/Vacuum/TightFrames.lean at 41c2716658c42ba08eb0648994a2efe83082a95c with Preserve defs false. [1]
- Preserves structure New.Parameters.IsResolutionProduct verbatim with trailing EXTRACTED_DEF provenance marker (upstream 674-689). [2]
- Stubs New.Parameters.exists_isResolutionProduct_of_isTightFrame constructing a resolution product from a tight frame (upstream 828-911, body_lines=61). [3]
