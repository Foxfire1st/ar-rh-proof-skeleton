# skeleton/07_congruences/crystalline_defect.lean

## Governing Overview

[Repository overview](../../overview.md)

## Purpose

Stubs Stage 07 local crystalline and tame dual-defect bounds plus the verbatim `New.Selmer.IsSummandClass` structure.

## Code Commentary

Auto-generated from upstream `src/New/Congruences/LocalLoss/CrystallineDefect.lean` at commit `41c2716658c42ba08eb0648994a2efe83082a95c` with `preserve_defs: false`. Contains 18 declarations (16 `lemma`, 1 `abbrev`, 1 `structure`) across 301 lines. Even with `preserve_defs: false`, its 1 `structure` declaration is emitted verbatim with trailing `-- [EXTRACTED_DEF ...]` comments while all other declarations are stubbed with `sorry -- [EXTRACTED_STUB ...]` lines. Key declarations include `New.Selmer.eventually_exists_lettersReadThick` (lines 88–107, upstream `src/New/Congruences/LocalLoss/CrystallineDefect.lean:210-293`), `New.Selmer.crystalline_bound_le_defectBound` (lines 143–157, upstream `src/New/Congruences/LocalLoss/CrystallineDefect.lean:348-381`), `New.Selmer.IsSummandClass` (lines 159–182, upstream `src/New/Congruences/LocalLoss/CrystallineDefect.lean:383-406`), `New.Selmer.normLength_dualDefect_le_defectBound_of_isTameType` (lines 223–264, upstream `src/New/Congruences/LocalLoss/CrystallineDefect.lean:467-517`), `New.Selmer.normLength_dualDefect_le_of_isSummandClass` (lines 266–301, upstream `src/New/Congruences/LocalLoss/CrystallineDefect.lean:519-624`). Recorded as landmark node `07_congruences/crystalline_defect` (`is_landmark: true`) with `depends_on: ["02_derivatives/integral_vacuum", "07_congruences/cup_orthogonality_bounds"]` in `dag/dag.json`. Never hand-edit; regenerate byte-identically with `rh-extract`.

## Relationships

Mapped in [the 8-stage map](../../dag/stage-map.json.md) under stage `07_congruences` (Lower Bound: Congruences & Galois Representations — Theorem C, Part 3), rendered by [the stub emitter](../../src/rh_pipeline/lean/stub_emitter.py.md) via [the stage-map extractor](../../src/rh_pipeline/graph/stage_map.py.md) and [`rh-extract`](../../src/rh_pipeline/cli/extract.py.md), indexed as landmark node `07_congruences/crystalline_defect` in [the proof DAG](../../dag/dag.json.md), and round-trip checked by [stage-map unit tests](../../tests/unit/test_stage_map.py.md).

## Evidence

- Auto-generated header pins upstream src/New/Congruences/LocalLoss/CrystallineDefect.lean at 41c2716658c42ba08eb0648994a2efe83082a95c with Preserve defs false. [1]
- Stubs New.Selmer.eventually_exists_lettersReadThick and New.Selmer.crystalline_bound_le_defectBound and preserves structure New.Selmer.IsSummandClass verbatim with EXTRACTED_DEF tag. [2]
- Stubs the tame and general local dual-defect bounds New.Selmer.normLength_dualDefect_le_defectBound_of_isTameType and New.Selmer.normLength_dualDefect_le_of_isSummandClass. [3]
