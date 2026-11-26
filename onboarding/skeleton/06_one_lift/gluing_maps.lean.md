# skeleton/06_one_lift/gluing_maps.lean

## Governing Overview

[Repository overview](../../overview.md)

## Purpose

Stubs Stage 06 comparison-operator gluing maps, false-lift orthogonality theorems, and three synthetic namespace-qualified anonymous instances (`New.Rows._instance_1`..`3`).

## Code Commentary

Auto-generated from upstream `src/New/OneLift/ComparisonOperator/GluingMaps.lean` at commit `41c2716658c42ba08eb0648994a2efe83082a95c` with `preserve_defs: false`. Contains 68 declarations (46 `lemma`, 12 `def`, 4 `theorem`, 3 `abbrev`, 3 `instance`) across 524 lines. All declarations are stubbed with `sorry -- [EXTRACTED_STUB ...]` provenance lines. Carries three deterministic synthetic namespace-qualified anonymous instances (`New.Rows._instance_1`, `New.Rows._instance_2`, `New.Rows._instance_3`) that round-trip identically through `extract_declarations`. Key declarations include `New.Rows._instance_1` (lines 13–14, upstream `src/New/OneLift/ComparisonOperator/GluingMaps.lean:69-70`), `New.Rows._instance_2` (lines 16–17, upstream `src/New/OneLift/ComparisonOperator/GluingMaps.lean:72-72`), `New.Rows._instance_3` (lines 19–20, upstream `src/New/OneLift/ComparisonOperator/GluingMaps.lean:74-74`), `New.Rows.glue` (lines 209–215, upstream `src/New/OneLift/ComparisonOperator/GluingMaps.lean:344-385`), `New.Rows.twistFactor_smul_adjoint_glue_comp` (lines 502–507, upstream `src/New/OneLift/ComparisonOperator/GluingMaps.lean:1024-1030`), `New.Rows.isOrthogonalToFalseLifts_glue_periodLift` (lines 509–515, upstream `src/New/OneLift/ComparisonOperator/GluingMaps.lean:1032-1042`), `New.Rows.periodLift_injective` (lines 517–519, upstream `src/New/OneLift/ComparisonOperator/GluingMaps.lean:1046-1059`), `New.Rows.twistFactor_ne_zero` (lines 521–524, upstream `src/New/OneLift/ComparisonOperator/GluingMaps.lean:1061-1067`). Recorded as landmark node `06_one_lift/gluing_maps` (`is_landmark: true`) with `depends_on: ["02_derivatives/integral_vacuum", "04_spanning/spanning_theorem", "05_determinant/norm_of_chain", "06_one_lift/periods_of_false_lifts", "06_one_lift/three_exchanges"]` in `dag/dag.json`. Never hand-edit; regenerate byte-identically with `rh-extract`.

## Relationships

Mapped in [the 8-stage map](../../dag/stage-map.json.md) under stage `06_one_lift` (Lower Bound: One Lift — Theorem C, Part 2), rendered by [the stub emitter](../../src/rh_pipeline/lean/stub_emitter.py.md) via [the stage-map extractor](../../src/rh_pipeline/graph/stage_map.py.md) and [`rh-extract`](../../src/rh_pipeline/cli/extract.py.md), indexed as landmark node `06_one_lift/gluing_maps` in [the proof DAG](../../dag/dag.json.md), and round-trip checked by [stage-map unit tests](../../tests/unit/test_stage_map.py.md).

## Evidence

- Auto-generated header pins upstream src/New/OneLift/ComparisonOperator/GluingMaps.lean at 41c2716658c42ba08eb0648994a2efe83082a95c with Preserve defs false. [1]
- Stubs the three synthetic anonymous instances New.Rows._instance_1, New.Rows._instance_2 and New.Rows._instance_3 alongside cross-namespace abbrev New.Data.FormTuples. [2]
- Stubs the four gluing-map theorems New.Rows.twistFactor_smul_adjoint_glue_comp, New.Rows.isOrthogonalToFalseLifts_glue_periodLift, New.Rows.periodLift_injective and New.Rows.twistFactor_ne_zero. [3]
