# tests/unit/test_extract_cli.py

## Governing Overview

[Repository overview](../../overview.md)

## Purpose

Verifies end-to-end `rh-extract` CLI execution, byte-identical deterministic regeneration, zero-write refusal on typed errors and atomic temporary-file cleanup on staging failure.

## Code Commentary

Builds synthetic local Git upstream repositories and runs `rh_pipeline.cli.extract.main` and `extract_and_write` end to end. Verifies exit code `0`, canonical timestamp-free `dag.json`, preservation of pre-existing unmapped files (`skeleton/README.md`) and byte-identical outputs across repeated runs (`test_cli_end_to_end_and_byte_identical_determinism`). Injects an `os.fsync` `OSError` to prove sentinel outputs remain untouched and zero `*.tmp` files linger (`test_atomic_write_cleans_up_temp_files_on_failure`). Confirms exit code `2` with `ERROR:` on stderr and zero created output files or directories on missing upstream repos, non-root subdirectories, missing mapped files, symlink escapes and mid-run `UnsupportedLeanFormError`.

## Relationships

Tests [the `rh-extract` CLI](../../src/rh_pipeline/cli/extract.py.md) and [the stage-map extractor](../../src/rh_pipeline/graph/stage_map.py.md). Executed by [the quality runner](../../scripts/check-quality.sh.md).

## Evidence

- Verifies end-to-end rh-extract CLI execution, sentinel preservation, canonical DAG output and byte-identical determinism across repeated runs. [1]
- Verifies injected os.fsync failure preserves existing sentinel outputs and removes all staged temporary files. [2]
- Verifies exit code 2 and zero filesystem writes on StageMapError inputs, symlink escapes and mid-run UnsupportedLeanFormError. [3]
