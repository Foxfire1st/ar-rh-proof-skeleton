# tests/unit/test_audit_static_scan.py

## Governing Overview

[Repository overview](../../overview.md)

## Purpose

Verifies the Lean 4 lexical static scanner, comment and literal skipping, word boundaries, attribute detection, unclosed-comment error reporting, scope and challenge-vs-solution routing, and synthetic fixtures.

## Code Commentary

Tests pure-string scanning and synthetic repository trees in `tmp_path` alongside the three checked-in synthetic fixtures under `tests/fixtures/lean_audit/` (`clean_comments_and_literals.lean`, `forbidden_tokens.lean`, `unclosed_block_comment.lean`, asserting fixture directory presence so missing fixtures fail closed). Verifies line/doc/nested block comments, strings, raw strings, char literals, primed and Unicode identifiers (`hre'`, `sorry'`, `χsorry`), `.`-qualified names (`Lean.ofReduceBool`), all nine forbidden tokens and `extern`/`implemented_by` attribute forms, `AuditScanError` with `rel_path:line`, `entry` vs `full` scope (excluding `scripts/`), routing of non-`sorry` challenge tokens into `solution_findings`, `AuditInputError` on invalid inputs and `StaticScanFinding`/`StaticScanSummary` round-trips.

## Relationships

Verifies [static scanning](../../src/rh_pipeline/audit/static_scan.py.md) and scan models in [audit models](../../src/rh_pipeline/models/audit.py.md), consumes the synthetic fixtures under `tests/fixtures/lean_audit/`, and runs inside [the quality runner](../../scripts/check-quality.sh.md).

## Evidence

- Verifies comment, nested-block, string, raw-string, char-literal, prime, word-boundary, qualified-name and attribute-form detection. [1]
- Verifies unclosed-block-comment AuditScanError, entry vs full scope, challenge vs solution routing, input validation, model round-trips and synthetic fixtures. [2]
