# tests/unit/test_quality_scripts.py

## Governing Overview

[Repository overview](../../overview.md)

## Purpose

Verifies the worktree scripts in isolated fixtures with local package and executable substitutes.

## Code Commentary

The cases cover invalid invocations, foreign and non-editable package installs, installation failures, command order, Radon output, and replacement through aliased launchers. The ordered-run case traces the tool calls, and the Radon output case delegates to real Radon.

## Relationships

The [bootstrap](../../scripts/bootstrap-venv.sh.md) and [runner](../../scripts/check-quality.sh.md) are copied into disposable worktree fixtures.

## Evidence

- The ordered-run case verifies shared preflight reuse, the candidate import and all six tool calls. [1]
- The Radon and replacement cases check report location and selection through a launcher or checkout alias. [2]
