# tests/unit/test_file_size_gate.py

## Governing Overview

[Repository overview](../../overview.md)

## Purpose

Verifies the default file-size rail with boundary counts and synthetic Python files.

## Code Commentary

The tests cover both sides of each threshold. Separate filesystem cases verify sorted measurements, rejection of a 1201-line file, and an informational watchful report for 450 lines.

## Relationships

The [file-size module](../../src/rh_pipeline/validate/file_size.py.md) owns classification, scanning and CLI status.

## Evidence

- Boundary cases exercise all five classification bands. [1]
- CLI cases verify failure at 1201 lines and a successful watchful report at 450 lines. [2]
