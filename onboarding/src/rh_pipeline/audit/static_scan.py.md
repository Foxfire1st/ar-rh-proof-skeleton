# src/rh_pipeline/audit/static_scan.py

## Governing Overview

[Repository overview](../../../overview.md)

## Purpose

Performs deterministic, comment-and-literal-aware Lean 4 lexical static scanning over upstream proof files and partitions challenge `sorry` findings from solution forbidden tokens.

## Code Commentary

`scan_lean_source` tokenizes Lean 4 text while skipping `--` line comments, arbitrarily nested `/- ... -/` block comments (including `/-!` and `/--` doc comments), escaped double-quoted strings, raw strings `r#"..."#`, and single-line char literals (`'"'`, `'\''`, `'\\'`, Unicode/hex escapes). Uses Unicode-aware `_IDENT_RE` (`[^\W\d][\w']*`) so identifier primes (`hre'`, `sorry'`) and compound names (`my_sorry`, `sorryAx`, `χsorry`) stay intact while `.` splits qualified names such as `Lean.ofReduceBool`. Flags the nine `FORBIDDEN_TOKENS` (`sorry`, `admit`, `axiom`, `opaque`, `unsafe`, `skipKernelTC`, `native_decide`, `ofReduceBool`, `trustCompiler`) anywhere outside comments and literals, plus `ATTRIBUTE_TOKENS` (`extern`, `implemented_by`) at bracket depth 1 inside `@[...]` or non-dot-prefixed `attribute [...]` blocks. Unclosed block comments raise `AuditScanError` naming `rel_path:line` for the innermost unclosed opener. `scan_upstream_repo` resolves exactly one primary challenge file (`Challenge.lean` at repo root or legacy `src/Challenge.lean`, raising `AuditInputError` if neither or both exist) and scans either `src/Statements.lean`, `src/Main.lean`, the resolved primary challenge file and any `comparator/FormalConjectures*.lean` files when `comparator/` exists (`scope="entry"`, matching legacy `ENTRY_SCAN_FILES` on legacy repos) or all `.lean` files under `src/`, the resolved primary challenge file and optional recursive `comparator/**/*.lean` excluding `scripts/` (`scope="full"`), classifying `CHALLENGE_FILES` (`Challenge.lean`, `src/Challenge.lean`, `comparator/FormalConjecturesChallenge.lean`) as `role="challenge"`; only challenge `sorry` hits go to `challenge_findings`, whereas every other forbidden or attribute hit — including non-`sorry` tokens inside challenge files — routes to `solution_findings`, sorted by `(rel_path, line, token)`. Limits: interpolated string expressions (`s!"...{expr}..."`) are skipped as opaque strings, unclosed string literals consume to EOF without raising, and French-quoted identifiers (`«sorry»`) split around guillemets.

## Relationships

Produces `StaticScanFinding` and `StaticScanSummary` from [audit models](../models/audit.py.md), reuses `AuditInputError` from [provenance collection](provenance.py.md) for missing paths or invalid scopes, and feeds [status assembly](status.py.md) via the [audit CLI](../cli/audit.py.md). Verified by [static-scan unit tests](../../../tests/unit/test_audit_static_scan.py.md) using synthetic Lean fixtures under `tests/fixtures/lean_audit/`.

## Evidence

- Tracks nested block comments and raises AuditScanError naming rel_path and innermost opening line if unclosed. [2]
- Tokenizes Lean comments, strings, raw strings, char literals and Unicode primed identifiers before detecting attribute blocks and forbidden tokens. [3]

- Defines three CHALLENGE_FILES (Challenge.lean, src/Challenge.lean, comparator/FormalConjecturesChallenge.lean), primary challenge candidates, required entry files, nine forbidden tokens and two attribute tokens. [1]
- Resolves entry or full scope with single primary challenge resolution and optional comparator/ (excluding scripts/) and routes only challenge sorry hits to challenge_findings while routing all other findings to solution_findings. [4]
