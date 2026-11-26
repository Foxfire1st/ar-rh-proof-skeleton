# dag/dag.json

## Governing Overview

[Repository overview](../overview.md)

## Purpose

Stores the generated deterministic Stage 00 proof dependency DAG (`rh-dag/v1`) binding pinned upstream provenance, all 8 condensed stage definitions, 3 Stage 00 nodes, strict import-only edges and 14 line-anchored declaration references.

## Code Commentary

Declares `schema_version: "rh-dag/v1"`, pins `upstream_commit: "41c2716658c42ba08eb0648994a2efe83082a95c"`, embeds all 8 stages from `dag/stage-map.json`, and records 3 Stage 00 `DagNode`s carrying 14 `DagDeclarationRef` entries (`00_statement/statements` 4 defs, `00_statement/challenge` 5 theorems, `00_statement/main` 5 theorems). Nodes containing theorems or lemmas (`00_statement/challenge`, `00_statement/main`) set `is_landmark: true`, while `00_statement/statements` sets `is_landmark: false`. Because `depends_on` derives strictly from resolved direct Lean `import` statements among mapped nodes, only `00_statement/challenge` depends on `["00_statement/statements"]`, whereas both `00_statement/statements` and `00_statement/main` carry `depends_on: []` (upstream `src/Main.lean` does `public import New.Main` rather than importing `Statements` directly). Never hand-edit; regenerate byte-identically with `rh-extract --upstream <riemann-hypothesis-checkout> --stage-map dag/stage-map.json --out-skeleton skeleton/ --out-dag dag/dag.json`.

## Relationships

Validated by [DAG models](../src/rh_pipeline/models/dag.py.md), generated from [the 8-stage map](stage-map.json.md) by [the stage-map extractor](../src/rh_pipeline/graph/stage_map.py.md) and [`rh-extract`](../src/rh_pipeline/cli/extract.py.md), shares `upstream_commit` with [the Phase 0 verification status](../audit/verification-status.json.md), anchors declarations across [statements](../skeleton/00_statement/statements.lean.md), [challenge](../skeleton/00_statement/challenge.lean.md) and [main](../skeleton/00_statement/main.lean.md), and is round-trip checked by [stage-map unit tests](../tests/unit/test_stage_map.py.md).

## Evidence

- Records three Stage 00 DagNodes (00_statement/statements, 00_statement/challenge, 00_statement/main) carrying 14 skeleton and upstream line-anchored declaration references. [1]
- Derives strictly import-only dependency edges so 00_statement/challenge depends on 00_statement/statements while 00_statement/main carries an empty depends_on array. [2]
- Pins schema_version rh-dag/v1, embeds all 8 condensed stage definitions and matches upstream_commit 41c2716658c42ba08eb0648994a2efe83082a95c. [3]
