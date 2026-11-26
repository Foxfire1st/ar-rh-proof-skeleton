# skeleton/03_upper_bound/elementary_divisor_theorem.lean

## Governing Overview

[Repository overview](../../overview.md)

## Purpose

Preserves Stage 03 Frobenius-cycle edge structures verbatim and stubs the elementary divisor theorem `New.FrobeniusCycle.elementary_divisors`.

## Code Commentary

Auto-generated from upstream `src/New/UpperBound/ElementaryDivisors/ElementaryDivisorTheorem.lean` at commit `41c2716658c42ba08eb0648994a2efe83082a95c` with `preserve_defs: false`. Contains 54 declarations (41 `lemma`, 7 `def`, 3 `structure`, 2 `theorem`, 1 `abbrev`) across 593 lines. Even with `preserve_defs: false`, its 3 `structure` declarations are emitted verbatim with trailing `-- [EXTRACTED_DEF ...]` comments while all other declarations are stubbed with `sorry -- [EXTRACTED_STUB ...]` lines. Key declarations include `New.EdgeCoordinates` (lines 129–155, upstream `src/New/UpperBound/ElementaryDivisors/ElementaryDivisorTheorem.lean:342-368`), `New.FrobeniusCycle.IsLargeDegree` (lines 158–172, upstream `src/New/UpperBound/ElementaryDivisors/ElementaryDivisorTheorem.lean:385-399`), `New.FrobeniusCycle.IsEdgeCondition` (lines 280–305, upstream `src/New/UpperBound/ElementaryDivisors/ElementaryDivisorTheorem.lean:618-643`), `New.FrobeniusCycle.exists_isEdgeCondition` (lines 330–352, upstream `src/New/UpperBound/ElementaryDivisors/ElementaryDivisorTheorem.lean:675-727`), `New.FrobeniusCycle.elementary_divisors` (lines 576–593, upstream `src/New/UpperBound/ElementaryDivisors/ElementaryDivisorTheorem.lean:1159-1215`). Recorded as landmark node `03_upper_bound/elementary_divisor_theorem` (`is_landmark: true`) with `depends_on: []` in `dag/dag.json` (a nearest-landmark dependency root). Never hand-edit; regenerate byte-identically with `rh-extract`.

## Relationships

Mapped in [the 8-stage map](../../dag/stage-map.json.md) under stage `03_upper_bound` (Upper Bound: Composite Theta Lift — Theorem A), rendered by [the stub emitter](../../src/rh_pipeline/lean/stub_emitter.py.md) via [the stage-map extractor](../../src/rh_pipeline/graph/stage_map.py.md) and [`rh-extract`](../../src/rh_pipeline/cli/extract.py.md), indexed as landmark node `03_upper_bound/elementary_divisor_theorem` in [the proof DAG](../../dag/dag.json.md), and round-trip checked by [stage-map unit tests](../../tests/unit/test_stage_map.py.md).

## Evidence

- Auto-generated header pins upstream src/New/UpperBound/ElementaryDivisors/ElementaryDivisorTheorem.lean at 41c2716658c42ba08eb0648994a2efe83082a95c with Preserve defs false. [1]
- Preserves structures New.EdgeCoordinates and New.FrobeniusCycle.IsLargeDegree verbatim with trailing EXTRACTED_DEF provenance markers (upstream 342-399). [2]
- Stubs theorem New.FrobeniusCycle.elementary_divisors establishing the elementary divisor bound (upstream 1159-1215, body_lines=41). [3]
