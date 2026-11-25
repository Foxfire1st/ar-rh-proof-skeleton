# README.md

## Governing Overview

[Repository overview](overview.md)

## Purpose

Introduces the pipeline, its upstream sources, the package ownership map and the developer entry points.

## Code Commentary

Start from the task worktree and run the bootstrap before the quality runner. The ownership table identifies where each kind of Python behavior belongs.

## Relationships

The [CI workflow](.github/workflows/quality.yml.md) invokes the same [bootstrap](scripts/bootstrap-venv.sh.md) and [runner](scripts/check-quality.sh.md).

## Evidence

- The package map assigns deterministic gates to validate and command wiring to cli. [1]
- The documented worktree commands use the same scripts as CI. [2]
