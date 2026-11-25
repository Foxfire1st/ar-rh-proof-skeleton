# scripts/check-quality.sh

## Governing Overview

[Repository overview](../overview.md)

## Purpose

Runs the ordered Python quality gates for the checkout containing the script.

## Code Commentary

The shared preflight uses isolated Python to check its version, virtual-environment prefix, package path and editable metadata, then verifies all five local tool executables (`ruff`, `pyright`, `radon`, `pytest` and `rh-validate`). The runner uses the local executables for Ruff lint and format, Pyright, Radon, the file-size rail, `rh-validate --repo .` (forwarding `--memory "$RH_MEMORY_ROOT"` when `RH_MEMORY_ROOT` is set) and pytest. Radon reports complexity grades on stdout. A tool failure or syntax error stops the sequence.

## Relationships

The [bootstrap](bootstrap-venv.sh.md) sources the preflight for new and reused environments. The [file-size module](../src/rh_pipeline/validate/file_size.py.md) owns the line budgets and the [validator CLI](../src/rh_pipeline/cli/validate.py.md) owns schema, Concept Card and jargon checks.

## Evidence

- Isolated preflight verifies the local prefix, editable package and all five tool executables including rh-validate. [1]
- The runner emits Radon output and stops on tool failure or a syntax diagnostic before continuing through file size, rh-validate (with optional RH_MEMORY_ROOT) and pytest. [2]
