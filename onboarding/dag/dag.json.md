# dag/dag.json

## Governing Overview

[Repository overview](../overview.md)

## Purpose

Stores the generated deterministic 8-stage proof dependency DAG (`rh-dag/v1`) binding pinned upstream provenance, all 8 condensed stage definitions, 41 mapped nodes, 101 nearest-landmark projected import edges, and 970 line-anchored declaration references.

## Code Commentary

Declares `schema_version: "rh-dag/v1"`, pins `upstream_commit: "41c2716658c42ba08eb0648994a2efe83082a95c"`, embeds all 8 populated stages from `dag/stage-map.json`, and records 41 `DagNode`s carrying 970 `DagDeclarationRef` entries (`00_statement`: 3 nodes / 14 refs; `01_reduction`–`07_congruences`: 38 nodes / 956 refs). Forty nodes contain theorems or lemmas and set `is_landmark: true`, while `00_statement/statements` (4 `def`s) is the sole node with `is_landmark: false`. Dependency edges (`depends_on`, 101 directed edges total, acyclic with zero isolated nodes) are computed by nearest-landmark Lean import projection across unmapped upstream `src/` and root `.lean` files: `00_statement/statements` has `depends_on: []`, `00_statement/challenge` depends on `["00_statement/statements"]`, and `00_statement/main` depends on `["01_reduction/new_main"]`, bridging Stage 00 into the full 8-stage proof graph. Never hand-edit; regenerate byte-identically with `rh-extract --upstream <riemann-hypothesis-checkout> --stage-map dag/stage-map.json --out-skeleton skeleton/ --out-dag dag/dag.json`.

## Relationships

Validated by [DAG models](../src/rh_pipeline/models/dag.py.md), generated from [the 8-stage map](stage-map.json.md) by [the stage-map extractor](../src/rh_pipeline/graph/stage_map.py.md), [nearest-landmark import projection](../src/rh_pipeline/graph/import_projection.py.md), and [`rh-extract`](../src/rh_pipeline/cli/extract.py.md), shares `upstream_commit` with [the Phase 0 verification status](../audit/verification-status.json.md), anchors 970 declarations across all 41 `skeleton/<stage>/*.lean` files (including [statements](../skeleton/00_statement/statements.lean.md), [challenge](../skeleton/00_statement/challenge.lean.md), and [main](../skeleton/00_statement/main.lean.md)), and is round-trip checked by [stage-map unit tests](../tests/unit/test_stage_map.py.md).

## Evidence

- Records 41 DagNodes across all 8 stages carrying 970 skeleton and upstream line-anchored declaration references (40 landmark nodes and 1 non-landmark node 00_statement/statements). [1]
- Records nearest-landmark projected import edges (101 total, acyclic, zero isolated nodes), connecting 00_statement/challenge to 00_statement/statements and 00_statement/main through 01_reduction/new_main into Stages 01–07. [2]
- Pins schema_version rh-dag/v1, embeds all 8 populated condensed stage definitions (41 mapped files), and matches upstream_commit 41c2716658c42ba08eb0648994a2efe83082a95c. [3]
