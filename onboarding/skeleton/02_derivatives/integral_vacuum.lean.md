# skeleton/02_derivatives/integral_vacuum.lean

## Governing Overview

[Repository overview](../../overview.md)

## Purpose

Preserves Stage 02 theta-product and vacuum-scale structures verbatim and stubs the integral vacuum construction and Galois-conjugate jet translate lemmas.

## Code Commentary

Auto-generated from upstream `src/New/Derivatives/Vacuum/IntegralVacuum.lean` at commit `41c2716658c42ba08eb0648994a2efe83082a95c` with `preserve_defs: false`. Contains 36 declarations (25 `lemma`, 8 `def`, 2 `structure`, 1 `theorem`) across 324 lines. Even with `preserve_defs: false`, its 2 `structure` declarations are emitted verbatim with trailing `-- [EXTRACTED_DEF ...]` comments while all other declarations are stubbed with `sorry -- [EXTRACTED_STUB ...]` lines. Key declarations include `New.Parameters.IsThetaProduct` (lines 55–81, upstream `src/New/Derivatives/Vacuum/IntegralVacuum.lean:134-160`), `New.Parameters.IsVacuumScale` (lines 140–152, upstream `src/New/Derivatives/Vacuum/IntegralVacuum.lean:299-311`), `New.Parameters.isVacuumScale_vacuumScale` (lines 160–162, upstream `src/New/Derivatives/Vacuum/IntegralVacuum.lean:318-321`), `New.Parameters.intVacuum` (lines 164–166, upstream `src/New/Derivatives/Vacuum/IntegralVacuum.lean:326-327`), `New.Parameters.exists_isJetConjugate_translates` (lines 308–324, upstream `src/New/Derivatives/Vacuum/IntegralVacuum.lean:591-654`). Recorded as landmark node `02_derivatives/integral_vacuum` (`is_landmark: true`) with `depends_on: ["02_derivatives/tight_frames"]` in `dag/dag.json`. Never hand-edit; regenerate byte-identically with `rh-extract`.

## Relationships

Mapped in [the 8-stage map](../../dag/stage-map.json.md) under stage `02_derivatives` (Derivatives of Theta Functions), rendered by [the stub emitter](../../src/rh_pipeline/lean/stub_emitter.py.md) via [the stage-map extractor](../../src/rh_pipeline/graph/stage_map.py.md) and [`rh-extract`](../../src/rh_pipeline/cli/extract.py.md), indexed as landmark node `02_derivatives/integral_vacuum` in [the proof DAG](../../dag/dag.json.md), and round-trip checked by [stage-map unit tests](../../tests/unit/test_stage_map.py.md).

## Evidence

- Auto-generated header pins upstream src/New/Derivatives/Vacuum/IntegralVacuum.lean at 41c2716658c42ba08eb0648994a2efe83082a95c with Preserve defs false. [1]
- Preserves structure New.Parameters.IsVacuumScale verbatim with EXTRACTED_DEF tag and stubs vacuumScale, theorem isVacuumScale_vacuumScale and def intVacuum. [2]
- Stubs New.Parameters.exists_isJetConjugate_translates establishing Galois-conjugate jet translates for every automorphism of C over K (upstream 591-654, body_lines=49). [3]
