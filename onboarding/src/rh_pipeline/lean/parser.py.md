# src/rh_pipeline/lean/parser.py

## Governing Overview

[Repository overview](../../../overview.md)

## Purpose

Owns the closed-world Lean 4 top-level declaration and import extractor (`extract_declarations`) and re-exports the typed refusal error (`UnsupportedLeanFormError`) defined in `_lexer.py`.

## Code Commentary

`extract_declarations` normalizes `\r\n`/`\r` to `\n`, delegates tokenization to `LeanLexer.tokenize()` from `_lexer.py`, and parses the resulting depth-annotated token stream via `_Parser.parse()`. Top-level parsing admits `module`, `import`/`public import` (collected into sorted, deduplicated `imports`), `[public|noncomputable]* section` (optionally preceded by `@[expose]` only, allowing unclosed sections to EOF), balanced nested `namespace`/`end`, standalone admitted commands (`open`, `variable`, `include`, `omit`, `export`, `universe`, `attribute`, `grind_pattern`, and `local grind_pattern`), scoped `<kw> ... in` prefixes (`open`, `omit`, `seal`, `unseal`, `variable`, `include`, stripped before or after docs/attributes and also allowed before `attribute`, `grind_pattern`, or `local grind_pattern`), single/multiline `@[...]` attributes (including `local`/`scoped` attributes), modifiers (`public`, `private`, `protected`, `noncomputable`, `partial`, `unsafe`, plus `local` on `instance` only), and all ten declaration kinds. Column-0 span continuation allows `:`/`:=`/closing delimiters always, `|` on `_BAR_CONTINUATION_KINDS` (`structure`, `inductive`, `class`, `def`, `abbrev`, `theorem`, `lemma`, `instance`), `where` on `_DERIVING_KINDS` (`structure`, `inductive`, `class`) and pre-body `_BODY_KINDS`, and `deriving` (excluding `deriving instance`) on `_DERIVING_KINDS`. `_find_body_delim` locates depth-0 `:=`, `where`, or line-initial equation-compiler `|` (before any `match`, requiring a depth-0 `=>` before `:=`/`where` so norm bars `|x|` remain in signatures); `_qualify_name`, `_sig_edits`, and `_apply_sig_edits` override active namespaces on `_root_.` prefixes, synthesize sequential `_instance_N` names for colon-typed anonymous instances, excise skipped mid-prefix `in` spans, and rewrite only the target name span. `opaque` accepts either no body or an explicit depth-0 `:=` body; `_NO_BODY_KINDS` (`structure`, `inductive`, `class`, `axiom`) set `body=""` and `body_lines=0`; `_BODY_KINDS` (`theorem`, `lemma`, `def`, `abbrev`, `instance`) require a non-empty body (normalizing `:=` bodies and preserving `where`/`|` bodies with their leading delimiter/indentation) and compute `body_lines` (minimum 1) and UTF-8 `body_sha256`. Fails closed with `UnsupportedLeanFormError(rel_path, line, form)` on unlisted commands (`mutual`, `macro`, `set_option`, `syntax`, `notation`, `example`, `#check`, `universes`, `variables`, standalone `seal`/`unseal`, etc.), `scoped` modifiers, non-`instance` `local` modifiers, depth-0 `termination_by`/`decreasing_by`, anonymous instances lacking a depth-0 `:`, unbalanced `namespace`/`end` or stray/mismatched `end`, orphan docs/attributes/scoped prefixes/modifiers, missing/empty bodies on `_BODY_KINDS`, and lexer-raised lexical errors. Known limitation: `_scan_body_delim` stops at the first depth-0 `:=` without tracking `let ... :=` bindings inside declaration return types, truncating 20 extracted theorem/lemma stubs across 10 skeleton files (tracked as an open weakness and documented as an explicit exclusion on `INV-0WN2N4`).

## Relationships

Delegates lexing to and re-exports `UnsupportedLeanFormError` from [the Lean 4 lexer](_lexer.py.md), and constructs `ExtractedModule` and `LeanDeclaration` from [Lean models](../models/lean.py.md). Invoked by [the stage-map extractor](../graph/stage_map.py.md) and [the concept validator](../validate/concepts.py.md), caught by [the `rh-extract` CLI](../cli/extract.py.md) to exit 2, and verified alongside the synthetic `tests/fixtures/lean_parser/*.lean` fixtures by [parser unit tests](../../../tests/unit/test_lean_parser.py.md) and [stub-emitter unit tests](../../../tests/unit/test_stub_emitter.py.md).

## Evidence

- Normalizes CRLF/CR line endings and delegates delimiter- and comment-aware lexing to LeanLexer before running _Parser. [1]
- Admits headers, noncomputable/public/expose sections, standalone commands, scoped in prefixes, local instances and _root_. overrides while enforcing balanced namespaces and rejecting stray or mismatched end. [2]
- Scans declaration spans with column-0 continuation rules for pipe, pre-body where and deriving clauses and rejects depth-0 termination_by and decreasing_by. [3]
- Splits signatures and bodies at depth-0 assign, where or equation-arm pipe tokens, handles optional-body opaque, synthesizes anonymous instance names, excises skipped in prefixes and refuses missing or empty bodies. [4]
