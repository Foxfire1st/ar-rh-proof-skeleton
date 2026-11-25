# rh-proof-skeleton — Overview

## What This Repo Is

The repository houses the deterministic Lean proof-skeleton pipeline, its Phase 0 upstream audit emitter and its Python validation tools. The worktree quality checks verify this checkout before its changes are handed on.

## Code Ownership

The repository layout assigns each package one responsibility. `validate` owns deterministic gates, including the [file-size rail](src/rh_pipeline/validate/file_size.py.md). `models` owns immutable data contracts and canonical JSON formatting, including the [Phase 0 audit models](src/rh_pipeline/models/audit.py.md); `audit` owns read-only [provenance collection](src/rh_pipeline/audit/provenance.py.md), [Lean lexical static scanning](src/rh_pipeline/audit/static_scan.py.md) and [verification-status assembly and atomic emission](src/rh_pipeline/audit/status.py.md); and `cli` owns thin command wiring, including [`rh-audit`](src/rh_pipeline/cli/audit.py.md). See the [README card](README.md.md) for the package map.

## Build and Development

Run [bootstrap](scripts/bootstrap-venv.sh.md) and then the [quality runner](scripts/check-quality.sh.md) from the worktree root. [GitHub Actions](.github/workflows/quality.yml.md) calls the same scripts. The file-size rail classifies Python files into five bands, reports non-healthy files and fails when a default scan finds a file above 1200 lines. [Package metadata](pyproject.toml.md) registers `rh-audit` and `rh-file-size-gate` as console scripts; running `rh-audit` against read-only upstream checkouts regenerates deterministic, timestamp-free [provenance](audit/provenance.json.md) and [verification status](audit/verification-status.json.md) snapshots.

## Evidence

- The layout assigns package responsibilities and documents the worktree commands. [1]
- GitHub Actions invokes the bootstrap and runner directly. [2]

- Package metadata registers both rh-audit and rh-file-size-gate console scripts. [3]
- The rh-audit CLI runs provenance collection, static scanning, status assembly and atomic artifact emission. [4]
