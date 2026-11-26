# skeleton/00_statement/challenge.lean

## Governing Overview

[Repository overview](../../overview.md)

## Purpose

Stores the generated Stage 00 Lean 4 skeleton extracted from root upstream `Challenge.lean`, stubbing its five challenge theorems.

## Code Commentary

Auto-generated from root upstream `Challenge.lean` at commit `41c2716658c42ba08eb0648994a2efe83082a95c` with `preserve_defs: true`. Emits five stubbed `theorem` declarations (`riemannHypothesis`, `riemannZeta_eq_zero_re`, `LFunction_ne_zero_of_half_lt_re`, `LFunction_eq_zero_re`, `LFunction_eq_zero_re_of_isPrimitive` at lines 5–18, upstream `Challenge.lean:7-20`) carrying `sorry -- [EXTRACTED_STUB ...]` markers (`body_lines=2`). Recorded as landmark node `00_statement/challenge` with `depends_on: ["00_statement/statements"]` in `dag/dag.json` because root upstream `Challenge.lean` imports `Statements`. Never hand-edit; regenerate byte-identically with `rh-extract`.

## Relationships

Mapped in [the 8-stage map](../../dag/stage-map.json.md), rendered by [the stub emitter](../../src/rh_pipeline/lean/stub_emitter.py.md) via [the stage-map extractor](../../src/rh_pipeline/graph/stage_map.py.md) and [`rh-extract`](../../src/rh_pipeline/cli/extract.py.md), depends on [statements.lean](statements.lean.md) as landmark node `00_statement/challenge` in [the Stage 00 DAG](../../dag/dag.json.md), and round-trip checked by [stage-map unit tests](../../tests/unit/test_stage_map.py.md).

## Evidence

- Auto-generated header pins root upstream Challenge.lean at 41c2716658c42ba08eb0648994a2efe83082a95c with Preserve defs true. [1]
- Stubs all five challenge theorems with EXTRACTED_STUB sorry lines and upstream line ranges Challenge.lean:7-20. [3]
