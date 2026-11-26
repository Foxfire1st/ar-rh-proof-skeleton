# src/rh_pipeline/audit/status.py

## Governing Overview

[Repository overview](../../../overview.md)

## Purpose

Assembles the self-contained `VerificationStatus` record from provenance and static-scan results and writes both Phase 0 audit JSON files via same-directory atomic replacement.

## Code Commentary

`build_verification_status` deduplicates `theorem_names` and `permitted_axioms` across all comparators in first-seen order, delegates paired-optional verification digest extraction to `_resolve_verification_digests` (returning `(None, None)` when both `verification/NOTES.md` and `verification/static-scan.txt` are absent from `provenance.note_digests`, and raising `AuditInputError` if only one is present or either digest is empty), hard-codes `build_status="unverified-locally"`, and populates `solution_forbidden_token_count` from `len(scan.solution_findings)`. `VerificationStatus` intentionally carries no `replay_pin` fields (those live in `provenance.json`). `write_audit_artifacts` serializes both JSON payloads before creating `audit_dir`, stages flushed and `os.fsync`-ed temporary files inside `audit_dir`, replaces `provenance.json` and `verification-status.json` via `os.replace`, and unlinks any staged temporary files on `BaseException`.

## Relationships

Consumes `ProvenanceRecord` and `StaticScanSummary` from [audit models](../models/audit.py.md) and `AuditInputError` from [provenance collection](provenance.py.md). Invoked by the [audit CLI](../cli/audit.py.md) to emit [provenance.json](../../../audit/provenance.json.md) and [verification-status.json](../../../audit/verification-status.json.md), and verified by [CLI and status unit tests](../../../tests/unit/test_audit_cli.py.md).

## Evidence

- Stages flushed and fsynced temporary files in audit_dir before os.replace and cleans up staged files on failure. [2]

- Assembles a self-contained VerificationStatus with deduplicated theorems and axioms, paired-optional verification note digests (both present and non-empty, or both null), and hard-coded build_status unverified-locally. [1]
