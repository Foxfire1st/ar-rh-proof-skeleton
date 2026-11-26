# tests/unit/test_lean_parser.py

## Governing Overview

[Repository overview](../../overview.md)

## Purpose

Verifies closed-world Lean 4 declaration extraction (including admitted commands, scoped `in` prefixes, `_root_.` overrides, `where`/equation-arm bodies, anonymous instances, and skeleton round-trips), default-deny refusal fixtures and inline negative cases, lexical/scope edge cases, and `LeanDeclaration`/`ExtractedModule` strict serialization.

## Code Commentary

Covers `extract_declarations` and `UnsupportedLeanFormError` across `tests/fixtures/lean_parser/admitted_full.lean` (all ten declaration kinds, sorted imports, namespace qualification, Unicode/prime/dotted/guillemet names, nested `:=` inside binders and strings, `where` + `deriving`, multiline and single-line term/tactic proofs) and all five `tests/fixtures/lean_parser/refused_*.lean` fixtures (`test_all_refused_fixture_files_are_listed` guards against orphan fixture files). Parametrized `test_additional_closed_world_refusals` exercises 44 inline refusal cases (unlisted commands, `scoped` and non-`instance` `local` modifiers, standalone `seal`/`unseal`, `termination_by`/`decreasing_by`, unbalanced scopes, unclosed comments/literals/guillemets/delimiters, orphan docs/attributes/scoped prefixes, missing bodies and colonless anonymous instances), while `test_refuses_col0_where_after_def` ensures column-0 `where` is not absorbed after `def`. Also tests CRLF normalization, `⦃...⦄` binders, raw strings, char escapes, optional-body `opaque`, comment-only modules, and `LeanDeclaration`/`ExtractedModule` JSON round-trip and validation rejections. Five dedicated tests further verify admitted standalone commands, `noncomputable section`, `local`/`scoped` attributes and `local instance` declarations, stripping of `<kw> ... in` scoped prefixes before and after docs, `_root_.` namespace overrides, `where` bodies and sequential `_instance_N` anonymous instances, equation-compiler `|` arms versus norm bars `|x|`, and round-tripping through `render_skeleton_file` under both `preserve_defs` settings.

## Relationships

Tests [the Lean 4 parser](../../src/rh_pipeline/lean/parser.py.md), [the Lean 4 lexer](../../src/rh_pipeline/lean/_lexer.py.md), [the stub emitter](../../src/rh_pipeline/lean/stub_emitter.py.md) (via round-trip checks), and [Lean models](../../src/rh_pipeline/models/lean.py.md), and owns the stable contract of the six synthetic fixtures under `tests/fixtures/lean_parser/`. Executed by [the quality runner](../../scripts/check-quality.sh.md).

## Evidence

- Verifies extraction of all ten declaration kinds, sorted imports, nested binder assigns and SHA-256 body metadata on admitted_full.lean. [1]
- Verifies LeanDeclaration and ExtractedModule JSON round-trips and strict key, kind, line-bound and bool-as-int rejections. [3]

- Verifies default-deny UnsupportedLeanFormError refusals across all five refusal fixtures, 44 inline cases and column-0 where after def. [2]
- Verifies admitted commands, noncomputable sections, local/scoped attributes, local instances, stripped scoped in prefixes and _root_. namespace overrides. [4]
- Verifies where bodies, sequential _instance_N anonymous instances, equation-compiler bars versus norm bars and render_skeleton_file round-trips. [5]
