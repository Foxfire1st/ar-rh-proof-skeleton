# skeleton/02_derivatives/count.lean

## Governing Overview

[Repository overview](../../overview.md)

## Purpose

Stubs Stage 02 theta-jet divisibility counting definitions and minor-product norm bound theorems extracted from upstream `Count.lean`.

## Code Commentary

Auto-generated from upstream `src/New/Derivatives/Divisibility/Count.lean` at commit `41c2716658c42ba08eb0648994a2efe83082a95c` with `preserve_defs: false`. Contains 40 declarations (25 `lemma`, 11 `def`, 3 `theorem`, 1 `abbrev`) across 400 lines. All declarations are stubbed with `sorry -- [EXTRACTED_STUB ...]` provenance lines. Key declarations include `New.predictedDivisibility` (lines 210–218, upstream `src/New/Derivatives/Divisibility/Count.lean:411-428`), `New.Parameters.norm_jet_minorProd_le_of_isChargeExponent` (lines 283–308, upstream `src/New/Derivatives/Divisibility/Count.lean:563-639`), `New.Parameters.norm_jet_minorProd_le_of_charges` (lines 310–336, upstream `src/New/Derivatives/Divisibility/Count.lean:641-684`), `New.Parameters.norm_jet_minorProd_le_of_hasNormalForm` (lines 338–360, upstream `src/New/Derivatives/Divisibility/Count.lean:686-713`), `New.exists_norm_schurJet_le_norm_jet_minorProd` (lines 388–400, upstream `src/New/Derivatives/Divisibility/Count.lean:790-821`). Known limitation: the three `New.Parameters.norm_jet_minorProd_le_*` theorem stubs (lines 307, 335, 359) are truncated at their return-type `let N : ℕ :=` binding because `src/rh_pipeline/lean/parser.py::_scan_body_delim` splits at the first depth-0 `:=` (tracked as an open weakness across 20 stubs in 10 skeleton files; follow the upstream `Count.lean` line links for complete theorem statements). Recorded as landmark node `02_derivatives/count` (`is_landmark: true`) with `depends_on: ["02_derivatives/theta_products"]` in `dag/dag.json`. Never hand-edit; regenerate byte-identically with `rh-extract`.

## Relationships

Mapped in [the 8-stage map](../../dag/stage-map.json.md) under stage `02_derivatives` (Derivatives of Theta Functions), rendered by [the stub emitter](../../src/rh_pipeline/lean/stub_emitter.py.md) via [the stage-map extractor](../../src/rh_pipeline/graph/stage_map.py.md) and [`rh-extract`](../../src/rh_pipeline/cli/extract.py.md), indexed as landmark node `02_derivatives/count` in [the proof DAG](../../dag/dag.json.md), and round-trip checked by [stage-map unit tests](../../tests/unit/test_stage_map.py.md).

## Evidence

- Auto-generated header pins upstream src/New/Derivatives/Divisibility/Count.lean at 41c2716658c42ba08eb0648994a2efe83082a95c with Preserve defs false. [1]
- Stubs the three count theorems New.Parameters.norm_jet_minorProd_le_of_isChargeExponent, norm_jet_minorProd_le_of_charges and norm_jet_minorProd_le_of_hasNormalForm with EXTRACTED_STUB markers. [2]
- Stubs New.exists_norm_schurJet_le_norm_jet_minorProd replacing a Schur matrix jet entry by a product of minors (upstream 790-821, body_lines=21). [3]
