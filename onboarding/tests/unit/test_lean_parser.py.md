# tests/unit/test_lean_parser.py

## Governing Overview

[Repository overview](../../overview.md)

## Purpose

Verifies closed-world Lean 4 declaration extraction, default-deny refusal fixtures and inline negative cases, lexical/scope edge cases, and `LeanDeclaration`/`ExtractedModule` strict serialization.

## Code Commentary

Covers `extract_declarations` and `UnsupportedLeanFormError` across `tests/fixtures/lean_parser/admitted_full.lean` (all ten declaration kinds, sorted imports, namespace qualification, Unicode/prime/dotted/guillemet names, nested `:=` inside binders and strings, `where` + `deriving`, multiline and single-line term/tactic proofs) and all eight `tests/fixtures/lean_parser/refused_*.lean` fixtures (`test_all_refused_fixture_files_are_listed` guards against orphan fixture files). Parametrized `test_additional_closed_world_refusals` exercises 46 inline refusal cases (unlisted commands, `local`/`scoped` modifiers and attributes, `termination_by`/`decreasing_by`, unbalanced scopes, unclosed literals/comments/delimiters, orphan docs/attributes, missing bodies and anonymous instances), while `test_refuses_col0_where_after_def` ensures column-0 `where` is not absorbed after `def`. Also tests CRLF normalization, `⦃...⦄` binders, raw strings, char escapes, optional-body `opaque`, comment-only modules and `LeanDeclaration`/`ExtractedModule` JSON round-trip and validation rejections.

## Relationships

Tests [the Lean 4 parser](../../src/rh_pipeline/lean/parser.py.md) and [Lean models](../../src/rh_pipeline/models/lean.py.md) and owns the stable contract of the nine synthetic fixtures under `tests/fixtures/lean_parser/`. Executed by [the quality runner](../../scripts/check-quality.sh.md).

## Evidence

- Verifies extraction of all ten declaration kinds, sorted imports, nested binder assigns and SHA-256 body metadata on admitted_full.lean. [1]
- Verifies default-deny UnsupportedLeanFormError refusals across all eight refusal fixtures, 46 inline cases and column-0 where after def. [2]
- Verifies LeanDeclaration and ExtractedModule JSON round-trips and strict key, kind, line-bound and bool-as-int rejections. [3]
