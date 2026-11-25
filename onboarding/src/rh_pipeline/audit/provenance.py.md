# src/rh_pipeline/audit/provenance.py

## Governing Overview

[Repository overview](../../../overview.md)

## Purpose

Collects deterministic, read-only upstream and optional replay provenance metadata into a `ProvenanceRecord` while failing closed with `AuditInputError`.

## Code Commentary

`collect_provenance` inspects a read-only upstream Git repository root using only `git -C <root> rev-parse` (`--show-toplevel`, `--verify HEAD`, `--abbrev-ref HEAD`), refusing subdirectories or non-Git directories and recording detached HEAD cleanly as `"HEAD"`. Reads `lean-toolchain`, the `mathlib` package `rev` from `lake-manifest.json`, both `comparator.json` and `comparator/formal-conjectures.json` (projecting required comparator keys while ignoring extra upstream keys), recursive sorted SHA-256 digests of every file under `comparator/` (root `comparator.json` is parsed into `comparators`, not `comparator_file_digests`), and SHA-256 digests of `verification/NOTES.md` and `verification/static-scan.txt`. When `replay_root` is provided, `_collect_replay_pin` reads its Git root HEAD, hashes `parts.sha256` and validates `lean`, `lean_commit`, `mathlib_commit` and `module_count` from `tracker/summary.json` into a `ReplayPin`. Missing roots or files, Git failures, invalid UTF-8 or JSON, missing keys or model validation errors raise `AuditInputError`.

## Relationships

Constructs immutable models from [audit models](../models/audit.py.md). Its `AuditInputError` is reused by [static scanning](static_scan.py.md) and [status assembly](status.py.md), and `collect_provenance` is invoked by the [audit CLI](../cli/audit.py.md). Verified by [provenance unit tests](../../../tests/unit/test_audit_provenance.py.md).

## Evidence

- Enforces Git repository root identity and reads HEAD and branch read-only via git rev-parse. [1]
- Reads lean-toolchain, mathlib rev, dual comparator configs, recursive comparator digests, note digests and optional ReplayPin. [2]
- Raises AuditInputError when required upstream or replay roots, files, Git metadata, JSON or model fields fail validation. [3]
