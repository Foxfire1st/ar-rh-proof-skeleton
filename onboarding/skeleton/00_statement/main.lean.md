# skeleton/00_statement/main.lean

## Governing Overview

[Repository overview](../../overview.md)

## Purpose

Stores the generated Stage 00 Lean 4 skeleton extracted from upstream `src/Main.lean` with all five top-level solution theorems stubbed and linked to their upstream spans.

## Code Commentary

Auto-generated from upstream `src/Main.lean` at commit `41c2716658c42ba08eb0648994a2efe83082a95c` with `preserve_defs: false`. Stubs all five top-level `theorem` declarations with preserved leading docstrings, `sorry -- [EXTRACTED_STUB ...]` provenance lines (`body_lines=1` on all five) and body SHA-256 digests: `riemannHypothesis` (lines 5–7, upstream `src/Main.lean:8-9`), `riemannZeta_eq_zero_re` (lines 9–11, upstream `src/Main.lean:11-12`), `LFunction_ne_zero_of_half_lt_re` (lines 13–17, upstream `src/Main.lean:14-17`), `LFunction_eq_zero_re` (lines 19–22, upstream `src/Main.lean:19-21`) and `LFunction_eq_zero_re_of_isPrimitive` (lines 24–27, upstream `src/Main.lean:23-26`). Recorded as landmark node `00_statement/main` with `depends_on: ["01_reduction/new_main"]` in `dag/dag.json` because upstream `src/Main.lean` does `public import New.Main`. Never hand-edit; regenerate byte-identically with `rh-extract`.

## Relationships

Mapped in [the 8-stage map](../../dag/stage-map.json.md), rendered by [the stub emitter](../../src/rh_pipeline/lean/stub_emitter.py.md) via [the stage-map extractor](../../src/rh_pipeline/graph/stage_map.py.md) and [`rh-extract`](../../src/rh_pipeline/cli/extract.py.md), indexed as landmark node `00_statement/main` (depending on [new_main.lean](../01_reduction/new_main.lean.md)) in [the proof DAG](../../dag/dag.json.md) and round-trip checked by [stage-map unit tests](../../tests/unit/test_stage_map.py.md).

## Evidence

- Auto-generated header pins upstream src/Main.lean at 41c2716658c42ba08eb0648994a2efe83082a95c with Preserve defs false. [1]
- Stubs riemannHypothesis, riemannZeta_eq_zero_re and LFunction_ne_zero_of_half_lt_re with preserved docstrings and EXTRACTED_STUB provenance markers (body_lines=1). [2]
- Stubs LFunction_eq_zero_re and LFunction_eq_zero_re_of_isPrimitive with preserved docstrings, body_lines=1 and SHA-256 digests. [3]
