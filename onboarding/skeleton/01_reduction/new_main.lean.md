# skeleton/01_reduction/new_main.lean

## Governing Overview

[Repository overview](../../overview.md)

## Purpose

Stubs the Stage 01 condensed entry lemmas `New.zeros_on_line` and `New.zeta_zeros_on_line` imported by `src/Main.lean`.

## Code Commentary

Auto-generated from upstream `src/New/Main.lean` at commit `41c2716658c42ba08eb0648994a2efe83082a95c` with `preserve_defs: false`. Contains 2 declarations (2 `lemma`) across 15 lines. All declarations are stubbed with `sorry -- [EXTRACTED_STUB ...]` provenance lines. Key declarations include `New.zeros_on_line` (lines 5–9, upstream `src/New/Main.lean:29-36`), `New.zeta_zeros_on_line` (lines 11–15, upstream `src/New/Main.lean:38-44`). Recorded as landmark node `01_reduction/new_main` (`is_landmark: true`) with `depends_on: ["01_reduction/one_sided_bound"]` in `dag/dag.json`. Never hand-edit; regenerate byte-identically with `rh-extract`.

## Relationships

Mapped in [the 8-stage map](../../dag/stage-map.json.md) under stage `01_reduction` (The Theorem and Its Reduction), rendered by [the stub emitter](../../src/rh_pipeline/lean/stub_emitter.py.md) via [the stage-map extractor](../../src/rh_pipeline/graph/stage_map.py.md) and [`rh-extract`](../../src/rh_pipeline/cli/extract.py.md), indexed as landmark node `01_reduction/new_main` in [the proof DAG](../../dag/dag.json.md), and round-trip checked by [stage-map unit tests](../../tests/unit/test_stage_map.py.md).

## Evidence

- Auto-generated header pins upstream src/New/Main.lean at 41c2716658c42ba08eb0648994a2efe83082a95c with Preserve defs false. [1]
- Stubs New.zeros_on_line deriving DirichletLZerosOnLine from DirichletLNonvanishingRightOfHalf with EXTRACTED_STUB provenance (upstream 29-36, body_lines=4). [2]
- Stubs New.zeta_zeros_on_line deriving RiemannZetaZerosOnLine from DirichletLNonvanishingRightOfHalf with EXTRACTED_STUB provenance (upstream 38-44, body_lines=4). [3]
