# rh-proof-skeleton — Overview

## What This Repo Is

The repository houses the deterministic Lean proof-skeleton pipeline and its Python validation tools. The worktree quality checks verify this checkout before its changes are handed on.

## Code Ownership

The repository layout assigns each package one responsibility. `validate` owns deterministic gates, including the [file-size rail](src/rh_pipeline/validate/file_size.py.md). `cli` owns command wiring. See the [README card](README.md.md) for the package map.

## Build and Development

Run [bootstrap](scripts/bootstrap-venv.sh.md) and then the [quality runner](scripts/check-quality.sh.md) from the worktree root. [GitHub Actions](.github/workflows/quality.yml.md) calls the same scripts. The file-size rail classifies Python files into five bands, reports non-healthy files and fails when a default scan finds a file above 1200 lines.

## Evidence

- The layout assigns package responsibilities and documents the worktree commands. [1]
- GitHub Actions invokes the bootstrap and runner directly. [2]
