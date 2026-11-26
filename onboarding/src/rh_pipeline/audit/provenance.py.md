# src/rh_pipeline/audit/provenance.py

## Governing Overview

[Repository overview](../../../overview.md)

## Purpose

Collects deterministic, read-only upstream and optional replay provenance metadata into a `ProvenanceRecord` while failing closed with `AuditInputError`.

## Code Commentary

`collect_provenance` inspects a read-only upstream Git repository root using only `git -C <root> rev-parse` (`--show-toplevel`, `--verify HEAD`, `--abbrev-ref HEAD`), refusing subdirectories or non-Git directories and recording detached HEAD cleanly as `"HEAD"`. Reads `lean-toolchain`, the `mathlib` package `rev` from `lake-manifest.json`, root `comparator.json` (enforcing required keys, rejecting unknown keys outside the allowed set and requiring at least one of `external_kernels`, `definition_names` or `enable_nanoda`), optional `comparator/formal-conjectures.json` plus recursive sorted SHA-256 digests of every file under `comparator/` when `comparator/` is present (failing closed if `comparator` is a non-directory or `comparator/formal-conjectures.json` is missing, and recording empty `comparator_file_digests` when absent; root `comparator.json` is parsed into `comparators`, not `comparator_file_digests`), and sorted SHA-256 digests of existing tracked upstream documentation and verification files (`CONTENTS.md`, `GUIDE.md`, `INDEX.md`, `README.md`, `REFERENCES.md`, `verification/NOTES.md`, `verification/static-scan.txt`, requiring both verification files whenever `verification/` exists and requiring at least `verification/NOTES.md` or `GUIDE.md`). When `replay_root` is provided, `_collect_replay_pin` reads its Git root HEAD, hashes `parts.sha256` and validates `lean`, `lean_commit`, `mathlib_commit` and `module_count` from `tracker/summary.json` into a `ReplayPin`. Missing roots or required files, non-directory `comparator`/`verification` paths, Git failures, invalid UTF-8 or JSON, missing/unknown/incomplete comparator keys or model validation errors raise `AuditInputError`.

## Relationships

Constructs immutable models from [audit models](../models/audit.py.md). Its `AuditInputError` is reused by [static scanning](static_scan.py.md) and [status assembly](status.py.md), and `collect_provenance` is invoked by the [audit CLI](../cli/audit.py.md). Verified by [provenance unit tests](../../../tests/unit/test_audit_provenance.py.md).

## Evidence

- Enforces Git repository root identity and reads HEAD and branch read-only via git rev-parse. [1]
- Raises AuditInputError when required upstream or replay roots, files, Git metadata, JSON or model fields fail validation. [3]

- Reads lean-toolchain, mathlib rev, root comparator config (plus optional comparator/ config and recursive digests), tracked documentation/verification note digests, and optional ReplayPin. [2]
