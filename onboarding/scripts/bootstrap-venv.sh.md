# scripts/bootstrap-venv.sh

## Governing Overview

[Repository overview](../overview.md)

## Purpose

Creates or verifies the editable Python environment for the checkout containing the script.

## Code Commentary

Without `--replace`, bootstrap checks the existing environment before reuse. Replacement first resolves a selected launcher outside the environment it removes, including when reached through a symlink. When Python lacks ensurepip, bootstrap uses that interpreter’s pip to install into the new environment. An unsuccessful creation removes the partial environment.

## Relationships

The [quality runner](check-quality.sh.md) owns the shared environment verification. Bootstrap confines pip’s install destination to the worktree environment while retaining environment-provided fetch settings.

## Evidence

- Replacement selects an executable interpreter outside the virtual environment before removing it. [1]
- Bootstrap binds the installation destination and removes partial environments on failure. [2]
