# rh-proof-skeleton — Overview

## What This Repo Is

The repository houses the deterministic Lean proof-skeleton pipeline, including its Phase 0 upstream audit emitter, its Phase 1 closed-world Lean 4 declaration extractor and Stage 00 skeleton/DAG generator, and its deterministic schema, Concept Card, Lean-anchor and Level-1 jargon validation suite. The worktree quality checks verify this checkout before its changes are handed on.

## Code Ownership

The repository layout assigns each package one responsibility. `validate` owns deterministic gates: the [file-size rail](src/rh_pipeline/validate/file_size.py.md), [artifact schema validator](src/rh_pipeline/validate/schemas.py.md), [Concept Card corpus validator](src/rh_pipeline/validate/concepts.py.md) and [explainer directive and Level-1 jargon validator](src/rh_pipeline/validate/jargon.py.md). `models` owns immutable data contracts, pure parsers and canonical JSON formatting, including the [Phase 0 audit models](src/rh_pipeline/models/audit.py.md), [Phase 1 Lean models](src/rh_pipeline/models/lean.py.md), [stage-map and DAG models](src/rh_pipeline/models/dag.py.md), [validation-finding models](src/rh_pipeline/models/validation.py.md) and [pure Concept Card models and YAML parser](src/rh_pipeline/models/concepts.py.md); `audit` owns read-only [provenance collection](src/rh_pipeline/audit/provenance.py.md), [Lean lexical static scanning](src/rh_pipeline/audit/static_scan.py.md) and [verification-status assembly and atomic emission](src/rh_pipeline/audit/status.py.md); `lean` owns the [closed-world Lean 4 parser](src/rh_pipeline/lean/parser.py.md) and [deterministic stub emitter](src/rh_pipeline/lean/stub_emitter.py.md); `graph` owns [stage-map validation, import-only DAG assembly and atomic extraction](src/rh_pipeline/graph/stage_map.py.md); and `cli` owns thin command wiring, including [`rh-audit`](src/rh_pipeline/cli/audit.py.md), [`rh-extract`](src/rh_pipeline/cli/extract.py.md) and [`rh-validate`](src/rh_pipeline/cli/validate.py.md). See the [README card](README.md.md) for the package map.

## Build and Development

Run [bootstrap](scripts/bootstrap-venv.sh.md) and then the [quality runner](scripts/check-quality.sh.md) from the worktree root. [GitHub Actions](.github/workflows/quality.yml.md) calls the same scripts. The file-size rail classifies Python files into five bands, reports non-healthy files and fails when a default scan finds a file above 1200 lines. [Package metadata](pyproject.toml.md) registers `rh-audit`, `rh-extract`, `rh-file-size-gate` and `rh-validate` as console scripts; running `rh-audit` against read-only upstream checkouts regenerates deterministic, timestamp-free [provenance](audit/provenance.json.md) and [verification status](audit/verification-status.json.md) snapshots, running `rh-extract` with [the 21-stage map](dag/stage-map.json.md) regenerates deterministic `skeleton/00_statement/*.lean` stubs and [the Stage 00 DAG](dag/dag.json.md), and running `rh-validate` checks repository JSON schemas plus optional Concept Card, Lean-anchor and Level-1 jargon gates under `<memory>/docs/{concepts,stages}/` (never scanning AR `onboarding/`), running inside `scripts/check-quality.sh` before `pytest`.

## Evidence

- The layout assigns package responsibilities and documents the worktree commands. [1]
- GitHub Actions invokes the bootstrap and runner directly. [2]
- The rh-audit CLI runs provenance collection, static scanning, status assembly and atomic artifact emission. [4]
- The rh-extract CLI orchestrates stage-map validation, closed-world Lean extraction, stub rendering, import-only DAG construction and atomic emission. [5]

- Package metadata registers rh-audit, rh-extract, rh-file-size-gate and rh-validate console scripts. [3]
- The rh-validate CLI composes repository schema validation with optional Concept Card, Lean-anchor and Level-1 jargon checks under docs/. [6]
