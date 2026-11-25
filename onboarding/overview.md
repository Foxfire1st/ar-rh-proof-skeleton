# rh-proof-skeleton — Overview

## What This Repo Is

The repository houses the deterministic Lean proof-skeleton pipeline, including its Phase 0 upstream audit emitter, its Phase 1 closed-world Lean 4 declaration extractor and Stage 00 skeleton/DAG generator, and its Python validation tools. The worktree quality checks verify this checkout before its changes are handed on.

## Code Ownership

The repository layout assigns each package one responsibility. `validate` owns deterministic gates, including the [file-size rail](src/rh_pipeline/validate/file_size.py.md). `models` owns immutable data contracts and canonical JSON formatting, including the [Phase 0 audit models](src/rh_pipeline/models/audit.py.md), [Phase 1 Lean models](src/rh_pipeline/models/lean.py.md) and [stage-map and DAG models](src/rh_pipeline/models/dag.py.md); `audit` owns read-only [provenance collection](src/rh_pipeline/audit/provenance.py.md), [Lean lexical static scanning](src/rh_pipeline/audit/static_scan.py.md) and [verification-status assembly and atomic emission](src/rh_pipeline/audit/status.py.md); `lean` owns the [closed-world Lean 4 parser](src/rh_pipeline/lean/parser.py.md) and [deterministic stub emitter](src/rh_pipeline/lean/stub_emitter.py.md); `graph` owns [stage-map validation, import-only DAG assembly and atomic extraction](src/rh_pipeline/graph/stage_map.py.md); and `cli` owns thin command wiring, including [`rh-audit`](src/rh_pipeline/cli/audit.py.md) and [`rh-extract`](src/rh_pipeline/cli/extract.py.md). See the [README card](README.md.md) for the package map.

## Build and Development

Run [bootstrap](scripts/bootstrap-venv.sh.md) and then the [quality runner](scripts/check-quality.sh.md) from the worktree root. [GitHub Actions](.github/workflows/quality.yml.md) calls the same scripts. The file-size rail classifies Python files into five bands, reports non-healthy files and fails when a default scan finds a file above 1200 lines. [Package metadata](pyproject.toml.md) registers `rh-audit`, `rh-extract` and `rh-file-size-gate` as console scripts; running `rh-audit` against read-only upstream checkouts regenerates deterministic, timestamp-free [provenance](audit/provenance.json.md) and [verification status](audit/verification-status.json.md) snapshots, and running `rh-extract` with [the 21-stage map](dag/stage-map.json.md) regenerates deterministic `skeleton/00_statement/*.lean` stubs and [the Stage 00 DAG](dag/dag.json.md).

## Evidence

- The layout assigns package responsibilities and documents the worktree commands. [1]
- GitHub Actions invokes the bootstrap and runner directly. [2]
- The rh-audit CLI runs provenance collection, static scanning, status assembly and atomic artifact emission. [4]

- Package metadata registers rh-audit, rh-extract and rh-file-size-gate console scripts. [3]
- The rh-extract CLI orchestrates stage-map validation, closed-world Lean extraction, stub rendering, import-only DAG construction and atomic emission. [5]
