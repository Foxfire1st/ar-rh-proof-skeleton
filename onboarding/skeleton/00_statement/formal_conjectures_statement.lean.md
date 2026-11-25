# skeleton/00_statement/formal_conjectures_statement.lean

## Governing Overview

[Repository overview](../../overview.md)

## Purpose

Stores the generated Stage 00 Lean 4 skeleton extracted from upstream `comparator/FormalConjecturesStatement.lean`, stubbing its named `instance` while preserving `GRH.trivialZeros` verbatim.

## Code Commentary

Auto-generated from upstream `comparator/FormalConjecturesStatement.lean` at commit `c900e67c2fcd1fdb041685820bbd2b867fbc5a4b` with `preserve_defs: true`. Emits two namespace-qualified declarations with preserved doc comments: `instance DirichletCharacter.instDecidablePredOddOfDecidableEq_formalConjecturesForMathlib` (lines 5–11, upstream 58–64, stubbed with `EXTRACTED_STUB` after splitting cleanly at the outer depth-0 `:=` past nested binder assignments) and `noncomputable def GRH.trivialZeros` (lines 13–22 plus line 23 `EXTRACTED_DEF` tag, upstream 70–79, preserved verbatim). Recorded as `00_statement/formal_conjectures_statement` (`is_landmark: false`, `depends_on: []`) in `dag/dag.json`. Never hand-edit; regenerate byte-identically with `rh-extract`.

## Relationships

Mapped in [the 21-stage map](../../dag/stage-map.json.md), rendered by [the stub emitter](../../src/rh_pipeline/lean/stub_emitter.py.md) via [the stage-map extractor](../../src/rh_pipeline/graph/stage_map.py.md) and [`rh-extract`](../../src/rh_pipeline/cli/extract.py.md), indexed in [the Stage 00 DAG](../../dag/dag.json.md) beside [formal_conjectures_challenge.lean](formal_conjectures_challenge.lean.md) and round-trip checked by [stage-map unit tests](../../tests/unit/test_stage_map.py.md).

## Evidence

- Auto-generated header pins comparator/FormalConjecturesStatement.lean at c900e67c2fcd1fdb041685820bbd2b867fbc5a4b with Preserve defs true. [1]
- Stubs the named DirichletCharacter decidability instance while preserving its doc comment and nested-assign signature. [2]
- Preserves noncomputable def GRH.trivialZeros verbatim with its doc comment and trailing EXTRACTED_DEF provenance tag. [3]
