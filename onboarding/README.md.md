# README.md

## Governing Overview

[Repository overview](overview.md)

## Purpose

Introduces the pipeline, its epistemic-status note, its upstream sources, the package ownership map, the six-command CLI reference table and the developer entry points.

## Code Commentary

Start from the task worktree and run the bootstrap before the quality runner. The ownership table identifies where each kind of Python behavior and top-level artifact directory belongs (including the 8-stage `skeleton/` layout `00_statement/` … `07_congruences/`), and the `## CLI Commands` table summarizes all six installed console scripts.

## Relationships

The [CI workflow](.github/workflows/quality.yml.md) invokes the same [bootstrap](scripts/bootstrap-venv.sh.md) and [runner](scripts/check-quality.sh.md); [pyproject.toml](pyproject.toml.md) registers the six CLI entry points summarized here.

## Evidence

- The package map assigns deterministic gates to validate and command wiring to cli. [1]
- The documented worktree commands use the same scripts as CI. [2]

- Summarizes the six installed rh-pipeline console scripts (rh-audit, rh-check-upstream, rh-extract, rh-validate, rh-build-site and rh-file-size-gate). [3]
