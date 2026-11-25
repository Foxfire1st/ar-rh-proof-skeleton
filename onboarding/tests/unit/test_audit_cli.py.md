# tests/unit/test_audit_cli.py

## Governing Overview

[Repository overview](../../overview.md)

## Purpose

Verifies `VerificationStatus` assembly, atomic artifact writing with temp-file cleanup on failure, and `rh-audit` CLI exit codes and output behavior across clean, finding and error paths.

## Code Commentary

Builds temporary synthetic upstream and replay Git repositories in `tmp_path`. Verifies `build_verification_status` deduplication, `build_status="unverified-locally"` enforcement and missing-digest `AuditInputError`; `write_audit_artifacts` determinism and atomic replacement including temp-file cleanup and preservation of prior files when `os.fsync` raises `OSError`; and `audit.main` returning exit `0` on clean repos (`entry` and `full` + `--replay`), exit `1` while still writing honest JSON on solution forbidden tokens, solution `@[extern]` or non-`sorry` challenge tokens, and exit `2` with `ERROR:` on stderr and no output directory created on input or scan failures.

## Relationships

Verifies [status assembly and atomic writing](../../src/rh_pipeline/audit/status.py.md), the [audit CLI](../../src/rh_pipeline/cli/audit.py.md) and `VerificationStatus` in [audit models](../../src/rh_pipeline/models/audit.py.md), and runs inside [the quality runner](../../scripts/check-quality.sh.md).

## Evidence

- Verifies VerificationStatus assembly, strict round-trip, atomic writing and temp-file cleanup on fsync failure. [1]
- Verifies rh-audit CLI exit codes 0, 1 (writing honest JSON) and 2 (writing nothing) across clean, forbidden-token and error scenarios. [2]
