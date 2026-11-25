# src/rh_pipeline/lean/parser.py

## Governing Overview

[Repository overview](../../../overview.md)

## Purpose

Owns the closed-world, delimiter- and comment-aware Lean 4 top-level declaration and import extractor (`extract_declarations`) and its typed refusal error (`UnsupportedLeanFormError`).

## Code Commentary

Normalizes `\r\n`/`\r` to `\n` and lexes comments (`--`, nested `/- ... -/`, `/-! ... -/`, `/--- ... -/`), `/-- ... -/` doc comments, escaped and raw strings (`r#"..."#`), character literals, Unicode/prime/dotted/guillemet (`«...»`) identifiers and four delimiter pairs (`()`, `[]`, `{}`, `⦃⦄`). Top-level parsing admits `module`, `import`/`public import` (collected into sorted, deduplicated `imports`), `section`/`public section`/`@[expose] [public] section` (allowing unclosed sections to EOF), balanced nested `namespace`/`end`, single/multiline `@[...]` attributes, modifiers (`public`, `private`, `protected`, `noncomputable`, `partial`, `unsafe`), all ten declaration kinds, `where`/`|`/`deriving` continuations restricted to `structure`/`inductive`/`class`, and term or tactic proofs. `_find_top_level_assign` locates `:=` only at delimiter depth 0 (stopping at depth-0 `where`) so nested `:=` inside binder signatures does not split declarations; `_qualify_sig` qualifies only the declared name span with active namespaces, leaving internal signature tokens untouched. `opaque` accepts either no body or an explicit `:=` body; `_NO_BODY_KINDS` (`structure`, `inductive`, `class`, `axiom`) set `body=""` and `body_lines=0`; `theorem`/`lemma`/`def`/`abbrev`/`instance` require a non-empty normalized body and compute `body_lines` (minimum 1) and UTF-8 `body_sha256`. Fails closed with `UnsupportedLeanFormError(rel_path, line, form)` formatted as `rel_path:line:form` on unlisted commands (`mutual`, `open`, `variable`, `macro`, `set_option`, etc.), `local`/`scoped` modifiers or attributes, `termination_by`/`decreasing_by`, anonymous instances, unbalanced `namespace`/`end`, orphan docs/attributes, and unclosed comments, literals or delimiters.

## Relationships

Constructs `ExtractedModule` and `LeanDeclaration` from [Lean models](../models/lean.py.md). Invoked by [the stage-map extractor](../graph/stage_map.py.md), caught by [the `rh-extract` CLI](../cli/extract.py.md) to exit 2, and verified alongside the synthetic `tests/fixtures/lean_parser/*.lean` fixtures by [parser unit tests](../../../tests/unit/test_lean_parser.py.md).

## Evidence

- The lexer tracks nested block comments, strings, raw strings, char literals, guillemet names and four delimiter pairs to compute exact nesting depth. [1]
- Scope handling allows unclosed sections to EOF while failing closed on unbalanced namespaces, stray end or mismatched end. [2]
- Declaration span scanning restricts column-0 where, pipe and deriving continuations to structure, inductive and class and rejects depth-0 termination_by and decreasing_by. [3]
- Signature and body splitting scans depth-0 assign tokens, allows optional-body opaque and refuses missing or empty bodies on proof and def kinds. [4]
