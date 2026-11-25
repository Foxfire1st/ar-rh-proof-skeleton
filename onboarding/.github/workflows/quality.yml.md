# .github/workflows/quality.yml

## Governing Overview

[Repository overview](../../overview.md)

## Purpose

Runs the Python quality checks for pull requests and pushes to main.

## Code Commentary

The job checks out the candidate, selects Python 3.12, and runs the worktree bootstrap before the quality runner.

## Relationships

The [runner](../../scripts/check-quality.sh.md) owns the gate order and handles individual tool failures.

## Evidence

- The job calls both worktree scripts directly after setting up Python. [1]
