# dag/dag.json

## Governing Overview

[Repository overview](../overview.md)

## Purpose

Stores the generated deterministic Stage 00 proof dependency DAG (`rh-dag/v1`) binding pinned upstream provenance, all 21 stage definitions, 5 Stage 00 nodes, strict import-only edges and 24 line-anchored declaration references.

## Code Commentary

Declares `schema_version: "rh-dag/v1"`, pins `upstream_commit: "c900e67c2fcd1fdb041685820bbd2b867fbc5a4b"`, embeds all 21 stages from `dag/stage-map.json`, and records 5 Stage 00 `DagNode`s carrying 24 `DagDeclarationRef` entries (`00_statement/statements` 4 defs, `00_statement/challenge` 4 defs + 5 theorems, `00_statement/main` 5 theorems, `00_statement/formal_conjectures_statement` 1 instance + 1 def, `00_statement/formal_conjectures_challenge` 1 instance + 1 def + 2 theorems). Nodes containing theorems or lemmas (`challenge`, `main`, `formal_conjectures_challenge`) set `is_landmark: true`. Because `depends_on` derives strictly from resolved Lean `import` statements among mapped nodes, only `00_statement/main` depends on `["00_statement/statements"]`; both `00_statement/challenge` and `00_statement/formal_conjectures_challenge` have `depends_on: []` because their upstream files import only `Mathlib` rather than `Statements` or `FormalConjecturesStatement`. Never hand-edit; regenerate byte-identically with `rh-extract --upstream <riemann-hypothesis-checkout> --stage-map dag/stage-map.json --out-skeleton skeleton/ --out-dag dag/dag.json`.

## Relationships

Validated by [DAG models](../src/rh_pipeline/models/dag.py.md), generated from [the 21-stage map](stage-map.json.md) by [the stage-map extractor](../src/rh_pipeline/graph/stage_map.py.md) and [`rh-extract`](../src/rh_pipeline/cli/extract.py.md), shares `upstream_commit` with [the Phase 0 verification status](../audit/verification-status.json.md), anchors declarations across [statements](../skeleton/00_statement/statements.lean.md), [challenge](../skeleton/00_statement/challenge.lean.md), [main](../skeleton/00_statement/main.lean.md), [formal conjectures statement](../skeleton/00_statement/formal_conjectures_statement.lean.md) and [formal conjectures challenge](../skeleton/00_statement/formal_conjectures_challenge.lean.md), and is round-trip checked by [stage-map unit tests](../tests/unit/test_stage_map.py.md).

## Evidence

- Records five Stage 00 DagNodes carrying 24 skeleton and upstream line-anchored declaration references. [1]
- Derives strictly import-only dependency edges so 00_statement/main depends on 00_statement/statements while 00_statement/challenge and 00_statement/formal_conjectures_challenge carry empty depends_on arrays. [2]
- Pins schema_version rh-dag/v1, embeds all 21 stage definitions and matches upstream_commit c900e67c2fcd1fdb041685820bbd2b867fbc5a4b. [3]
