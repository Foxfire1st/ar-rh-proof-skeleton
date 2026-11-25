# src/rh_pipeline/validate/file_size.py

## Governing Overview

[Repository overview](../../../overview.md)

## Purpose

Owns deterministic file-size classification, measurement and CLI reporting for Python files.

## Code Commentary

The default ceilings are 300 for healthy, 600 for watchful, 900 for refactor pressure and 1200 for the soft limit. Higher counts exceed the hard limit. Measurement reads UTF-8 Python files in sorted order within each supplied root. The CLI reports each non-healthy file and returns one for a hard-limit violation. Its default roots are src and tests, and callers may override the hard limit.

## Relationships

The [quality runner](../../../scripts/check-quality.sh.md) invokes the module. The [file-size tests](../../../tests/unit/test_file_size_gate.py.md) verify classification and the CLI outcomes.

## Evidence

- Classification selects each of the five bands using the default budget constants and supplied limits. [1]
- The CLI reports non-healthy outcomes and fails on a measured hard-limit violation. [2]
