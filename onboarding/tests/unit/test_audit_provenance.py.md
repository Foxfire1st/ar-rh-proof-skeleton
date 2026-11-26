# tests/unit/test_audit_provenance.py

## Governing Overview

[Repository overview](../../overview.md)

## Purpose

Verifies read-only upstream and replay provenance collection, detached-HEAD handling, strict model deserialization, canonical timestamp-free JSON determinism and fail-closed `AuditInputError` paths.

## Code Commentary

Constructs temporary synthetic Git repositories in `tmp_path` without touching external upstream checkouts. Covers collecting provenance with and without `replay_root`, detached-HEAD reporting as `"HEAD"`, repeated byte-identical canonical JSON serialization free of timestamp keys, strict `to_dict`/`from_dict` round-trips and rejection of missing keys, extra keys and wrong field types (including rejecting `bool` for `int` and rejecting `int`/`str` for `ComparatorSummary.enable_nanoda`), legacy six-file missing-input parametrization, condensed-edition provenance collection (empty `comparator_file_digests`, top-level doc digests in `note_digests`, optional `ComparatorSummary` fields `external_kernels`/`definition_names`/`enable_nanoda`), non-root or non-Git directory errors, malformed JSON, missing `mathlib` entry in `lake-manifest.json`, missing `GUIDE.md`, partial `verification/` directories, non-directory `comparator`/`verification` paths, unknown or incomplete comparator schemas, missing replay metadata files and boolean `module_count` rejection.

## Relationships

Verifies [audit models](../../src/rh_pipeline/models/audit.py.md) and [provenance collection](../../src/rh_pipeline/audit/provenance.py.md), and runs inside [the quality runner](../../scripts/check-quality.sh.md).

## Evidence

- Verifies provenance collection with and without replay, detached HEAD and canonical timestamp-free JSON determinism. [1]
- Verifies strict model round-trips, key-mismatch and type rejections, and fail-closed AuditInputError paths for missing or malformed upstream and replay inputs. [2]

- Verifies condensed-edition provenance collection, optional ComparatorSummary fields (definition_names and strict bool enable_nanoda) and fail-closed AuditInputError cases for missing docs, partial/non-directory layouts and invalid comparator schemas. [3]
