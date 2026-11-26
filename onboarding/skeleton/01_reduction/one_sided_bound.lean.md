# skeleton/01_reduction/one_sided_bound.lean

## Governing Overview

[Repository overview](../../overview.md)

## Purpose

Stubs Stage 01 prime-remainder iteration lemmas and the top-level GRH theorems `New.eventually_primeRemainder_le_mul_rpow` and `New.grh`.

## Code Commentary

Auto-generated from upstream `src/New/Reduction/OneSidedBound.lean` at commit `41c2716658c42ba08eb0648994a2efe83082a95c` with `preserve_defs: false`. Contains 9 declarations (7 `lemma`, 2 `theorem`) across 76 lines. All declarations are stubbed with `sorry -- [EXTRACTED_STUB ...]` provenance lines. Key declarations include `New.Data.primeRemainder_le` (lines 5–14, upstream `src/New/Reduction/OneSidedBound.lean:31-71`), `New.eventually_primeRemainder_natCast_le` (lines 36–46, upstream `src/New/Reduction/OneSidedBound.lean:123-159`), `New.eventually_primeRemainder_le_mul_rpow` (lines 65–71, upstream `src/New/Reduction/OneSidedBound.lean:201-235`), `New.grh` (lines 73–76, upstream `src/New/Reduction/OneSidedBound.lean:237-268`). Recorded as landmark node `01_reduction/one_sided_bound` (`is_landmark: true`) with `depends_on: ["00_statement/statements", "01_reduction/criterion", "01_reduction/main_inequality", "02_derivatives/integral_vacuum"]` in `dag/dag.json`. Never hand-edit; regenerate byte-identically with `rh-extract`.

## Relationships

Mapped in [the 8-stage map](../../dag/stage-map.json.md) under stage `01_reduction` (The Theorem and Its Reduction), rendered by [the stub emitter](../../src/rh_pipeline/lean/stub_emitter.py.md) via [the stage-map extractor](../../src/rh_pipeline/graph/stage_map.py.md) and [`rh-extract`](../../src/rh_pipeline/cli/extract.py.md), indexed as landmark node `01_reduction/one_sided_bound` in [the proof DAG](../../dag/dag.json.md), and round-trip checked by [stage-map unit tests](../../tests/unit/test_stage_map.py.md).

## Evidence

- Auto-generated header pins upstream src/New/Reduction/OneSidedBound.lean at 41c2716658c42ba08eb0648994a2efe83082a95c with Preserve defs false. [1]
- Stubs New.Data.primeRemainder_le bounding primeRemainder P.d P.q₀ D.x via D.logB, D.m and primeRemainder at D.x₁ (upstream 31-71, body_lines=33). [2]
- Stubs top-level theorems New.eventually_primeRemainder_le_mul_rpow and New.grh (DirichletLNonvanishingRightOfHalf) with EXTRACTED_STUB markers. [3]
