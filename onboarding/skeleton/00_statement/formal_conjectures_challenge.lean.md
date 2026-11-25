# skeleton/00_statement/formal_conjectures_challenge.lean

## Governing Overview

[Repository overview](../../overview.md)

## Purpose

Stores the generated Stage 00 Lean 4 skeleton extracted from upstream `comparator/FormalConjecturesChallenge.lean` with all four declarations stubbed under `preserve_defs: false`.

## Code Commentary

Auto-generated from upstream `comparator/FormalConjecturesChallenge.lean` at commit `c900e67c2fcd1fdb041685820bbd2b867fbc5a4b` with `preserve_defs: false`. Emits four namespace-qualified declarations with preserved doc comments, all stubbed with `sorry -- [EXTRACTED_STUB ...]`: `instance DirichletCharacter.instDecidablePredOddOfDecidableEq_formalConjecturesForMathlib` (lines 5–11, upstream 62–68), `theorem RiemannHypothesis.riemannHypothesis` (lines 13–23, upstream 74–84), `noncomputable def GRH.trivialZeros` (lines 25–32, upstream 90–99, stubbed because `preserve_defs` is `false`) and `theorem GRH.generalized_riemann_hypothesis` (lines 34–41, upstream 101–108). Recorded as landmark node `00_statement/formal_conjectures_challenge` with `depends_on: []` in `dag/dag.json` because upstream `comparator/FormalConjecturesChallenge.lean` imports only `Mathlib`. Never hand-edit; regenerate byte-identically with `rh-extract`.

## Relationships

Mapped in [the 21-stage map](../../dag/stage-map.json.md), rendered by [the stub emitter](../../src/rh_pipeline/lean/stub_emitter.py.md) via [the stage-map extractor](../../src/rh_pipeline/graph/stage_map.py.md) and [`rh-extract`](../../src/rh_pipeline/cli/extract.py.md), indexed in [the Stage 00 DAG](../../dag/dag.json.md) beside [formal_conjectures_statement.lean](formal_conjectures_statement.lean.md) and round-trip checked by [stage-map unit tests](../../tests/unit/test_stage_map.py.md).

## Evidence

- Auto-generated header pins comparator/FormalConjecturesChallenge.lean at c900e67c2fcd1fdb041685820bbd2b867fbc5a4b with Preserve defs false. [1]
- Stubs the DirichletCharacter decidability instance and RiemannHypothesis.riemannHypothesis with EXTRACTED_STUB lines. [2]
- Stubs noncomputable def GRH.trivialZeros under Preserve defs false and theorem GRH.generalized_riemann_hypothesis with upstream spans. [3]
