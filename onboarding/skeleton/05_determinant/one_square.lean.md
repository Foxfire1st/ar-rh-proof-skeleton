# skeleton/05_determinant/one_square.lean

## Governing Overview

[Repository overview](../../overview.md)

## Purpose

Stubs the Stage 05 one-square exchange identity (`New.Exchange.IsSquare` verbatim plus partner-lift exchange theorems).

## Code Commentary

Auto-generated from upstream `src/New/Determinant/Exchange/OneSquare.lean` at commit `41c2716658c42ba08eb0648994a2efe83082a95c` with `preserve_defs: false`. Contains 32 declarations (27 `lemma`, 2 `def`, 2 `theorem`, 1 `structure`) across 564 lines. Even with `preserve_defs: false`, its 1 `structure` declaration is emitted verbatim with trailing `-- [EXTRACTED_DEF ...]` comments while all other declarations are stubbed with `sorry -- [EXTRACTED_STUB ...]` lines. Key declarations include `New.Exchange.IsSquare` (lines 387–425, upstream `src/New/Determinant/Exchange/OneSquare.lean:684-722`), `New.Exchange.exists_sum_pairLift_comp_pairTransposedLift_eq_smul` (lines 428–461, upstream `src/New/Determinant/Exchange/OneSquare.lean:726-852`), `New.Exchange.equivariant_and_exists_sum_eq_of_areMatched` (lines 463–520, upstream `src/New/Determinant/Exchange/OneSquare.lean:854-948`), `New.Exchange.one_square_identity_of_isEdgeFamily` (lines 522–564, upstream `src/New/Determinant/Exchange/OneSquare.lean:950-1045`). Recorded as landmark node `05_determinant/one_square` (`is_landmark: true`) with `depends_on: ["05_determinant/theorem_for_partners"]` in `dag/dag.json`. Never hand-edit; regenerate byte-identically with `rh-extract`.

## Relationships

Mapped in [the 8-stage map](../../dag/stage-map.json.md) under stage `05_determinant` (Lower Bound: The Determinant — Theorem C, Part 1), rendered by [the stub emitter](../../src/rh_pipeline/lean/stub_emitter.py.md) via [the stage-map extractor](../../src/rh_pipeline/graph/stage_map.py.md) and [`rh-extract`](../../src/rh_pipeline/cli/extract.py.md), indexed as landmark node `05_determinant/one_square` in [the proof DAG](../../dag/dag.json.md), and round-trip checked by [stage-map unit tests](../../tests/unit/test_stage_map.py.md).

## Evidence

- Auto-generated header pins upstream src/New/Determinant/Exchange/OneSquare.lean at 41c2716658c42ba08eb0648994a2efe83082a95c with Preserve defs false. [1]
- Preserves structure New.Exchange.IsSquare verbatim with EXTRACTED_DEF tag and stubs theorem New.Exchange.exists_sum_pairLift_comp_pairTransposedLift_eq_smul. [2]
- Stubs theorem New.Exchange.equivariant_and_exists_sum_eq_of_areMatched and lemma New.Exchange.one_square_identity_of_isEdgeFamily for the one-square exchange identity. [3]
