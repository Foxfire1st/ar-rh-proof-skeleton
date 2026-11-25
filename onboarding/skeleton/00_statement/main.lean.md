# skeleton/00_statement/main.lean

## Governing Overview

[Repository overview](../../overview.md)

## Purpose

Stores the generated Stage 00 Lean 4 skeleton extracted from upstream `src/Main.lean` with all five top-level solution theorems stubbed and linked to their upstream spans.

## Code Commentary

Auto-generated from upstream `src/Main.lean` at commit `c900e67c2fcd1fdb041685820bbd2b867fbc5a4b` with `preserve_defs: false`. Stubs all five top-level `theorem` declarations with `sorry -- [EXTRACTED_STUB ...]` provenance lines and body SHA-256 digests: `LFunction_ne_zero_of_half_lt_re` (lines 5–6, upstream 43–44, `body_lines=1`), `riemannHypothesis` (lines 8–9, upstream 46–47, `body_lines=1`), `riemannZeta_eq_zero_re` (lines 11–12, upstream 49–49, `body_lines=1`), `LFunction_eq_zero_re` (lines 14–15, upstream 51–52, `body_lines=1`) and `LFunction_eq_zero_re_of_isPrimitive` (lines 17–18, upstream 54–56, `body_lines=3`). Recorded as landmark node `00_statement/main` with `depends_on: ["00_statement/statements"]` in `dag/dag.json`. Never hand-edit; regenerate byte-identically with `rh-extract`.

## Relationships

Mapped in [the 21-stage map](../../dag/stage-map.json.md), rendered by [the stub emitter](../../src/rh_pipeline/lean/stub_emitter.py.md) via [the stage-map extractor](../../src/rh_pipeline/graph/stage_map.py.md) and [`rh-extract`](../../src/rh_pipeline/cli/extract.py.md), depends on [statements.lean](statements.lean.md) in [the Stage 00 DAG](../../dag/dag.json.md) and is round-trip checked by [stage-map unit tests](../../tests/unit/test_stage_map.py.md).

## Evidence

- Auto-generated header pins upstream src/Main.lean at c900e67c2fcd1fdb041685820bbd2b867fbc5a4b with Preserve defs false. [1]
- Stubs LFunction_ne_zero_of_half_lt_re, riemannHypothesis and riemannZeta_eq_zero_re with EXTRACTED_STUB provenance markers. [2]
- Stubs LFunction_eq_zero_re and LFunction_eq_zero_re_of_isPrimitive with body line counts and SHA-256 digests. [3]
