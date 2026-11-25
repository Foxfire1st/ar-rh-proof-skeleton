# scripts/check-quality.sh

## Governing Overview

[Repository overview](../overview.md)

## Purpose

Runs the ordered Python quality gates for the checkout containing the script.

## Code Commentary

The shared preflight uses isolated Python to check its version, virtual-environment prefix, package path and editable metadata, then verifies the local tool executables. The runner uses the local executables for Ruff lint and format, Pyright, Radon, the file-size rail and pytest. Radon reports complexity grades on stdout. A tool failure or syntax error stops the sequence.

## Relationships

The [bootstrap](bootstrap-venv.sh.md) sources the preflight for new and reused environments. The [file-size module](../src/rh_pipeline/validate/file_size.py.md) owns the line budgets.

## Evidence

- Isolated preflight verifies the local prefix and editable package before validating the tool executables. [1]
- The runner emits Radon output and stops on tool failure or a syntax diagnostic before continuing to file size and tests. [2]
