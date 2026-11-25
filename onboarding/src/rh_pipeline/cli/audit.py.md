# src/rh_pipeline/cli/audit.py

## Governing Overview

[Repository overview](../../../overview.md)

## Purpose

Provides the thin `rh-audit` command-line entry point wiring arguments to the Phase 0 audit modules and mapping outcomes to exit codes.

## Code Commentary

`_build_parser` accepts required `--upstream`, optional `--out` (default `audit`), `--scope` (`entry` or `full`, default `entry`) and optional `--replay`. `main` runs `collect_provenance`, `scan_upstream_repo` and `build_verification_status` inside a `try` block before touching disk: `AuditInputError` or `AuditScanError` prints `ERROR: <exc>` to stderr and returns exit code `2` without writing any file. When validation and scanning succeed, `write_audit_artifacts` emits both JSON files, prints a one-line summary to stdout and returns exit code `1` if `solution_forbidden_token_count > 0` (leaving honest JSON on disk) or `0` when zero solution forbidden tokens are found.

## Relationships

Registered as `rh-audit` in [package metadata](../../../pyproject.toml.md). Delegates all domain logic to [provenance collection](../audit/provenance.py.md), [static scanning](../audit/static_scan.py.md) and [status assembly and atomic writing](../audit/status.py.md), emitting [provenance.json](../../../audit/provenance.json.md) and [verification-status.json](../../../audit/verification-status.json.md). Verified by [CLI unit tests](../../../tests/unit/test_audit_cli.py.md).

## Evidence

- Parses --upstream, --out (default audit), --scope (entry or full) and optional --replay arguments. [1]
- Returns exit 2 without writing on AuditInputError or AuditScanError, exit 1 after writing honest JSON on solution findings, and exit 0 when clean. [2]
