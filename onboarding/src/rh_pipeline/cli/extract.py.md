# src/rh_pipeline/cli/extract.py

## Governing Overview

[Repository overview](../../../overview.md)

## Purpose

Provides the thin `rh-extract` command-line entry point that wires CLI arguments to `extract_and_write` and translates typed domain errors to stderr with exit code 2.

## Code Commentary

`_build_parser` defines `prog="rh-extract"` with required `--upstream` and defaults `--stage-map dag/stage-map.json`, `--out-skeleton skeleton` and `--out-dag dag/dag.json`. `main` delegates directly to `extract_and_write`, catches `StageMapError` and `UnsupportedLeanFormError` to print `ERROR: <exc>` to stderr and return exit code `2` (leaving zero written files because `extract_and_write` fails before disk staging), and otherwise prints `Extracted <N> node(s) across <S> stage(s) at <commit[:12]> -> <out_skeleton>, <out_dag>` to stdout and returns `0`. Limits: unexpected filesystem `OSError` failures during staging or replacement propagate uncaught, and `argparse` usage errors follow standard exit code 2 behavior.

## Relationships

Registered as `rh-extract = "rh_pipeline.cli.extract:main"` in [package metadata](../../../pyproject.toml.md). Calls [the stage-map extractor](../graph/stage_map.py.md), catches `UnsupportedLeanFormError` from [the Lean 4 parser](../lean/parser.py.md), reads [the 8-stage map](../../../dag/stage-map.json.md), regenerates [the Stage 00 DAG](../../../dag/dag.json.md) and `skeleton/00_statement/*.lean`, and is verified by [extract CLI unit tests](../../../tests/unit/test_extract_cli.py.md).

## Evidence

- Defines the rh-extract argument parser with required --upstream and default paths for stage map, skeleton directory and DAG JSON. [1]
- Delegates to extract_and_write, translating StageMapError and UnsupportedLeanFormError to stderr ERROR lines and exit code 2 while printing a summary and returning 0 on success. [2]
