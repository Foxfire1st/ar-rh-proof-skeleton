# src/rh_pipeline/lean/_lexer.py

## Governing Overview

[Repository overview](../../../overview.md)

## Purpose

Owns the delimiter- and comment-aware Lean 4 lexer (`LeanLexer`), token representation (`Token`), guillemet-aware dotted-name splitter (`split_dotted_name`), and typed closed-world refusal exception (`UnsupportedLeanFormError`) used by top-level declaration extraction.

## Code Commentary

`LeanLexer.tokenize` scans LF-normalized Lean 4 source into frozen `Token(kind, text, start, end, line, end_line, col, depth)` records with 1-based line numbers, 0-based columns, and exact delimiter nesting depth across four paired delimiters (`()`, `[]`, `{}`, `⦃⦄`). It skips `--` line comments and non-doc block comments (including nested `/- ... -/`, `/-! ... -/`, and `/--- ... -/`), emits `doc` tokens for `/-- ... -/` comments (excluding `/---` and `/--/`), emits `lit` tokens for escaped strings, raw strings (`r#+"..."#+`), and character literals (including `\xHH`, `\uHHHH`, and `\u{...}` escapes), emits `ident` tokens for dotted Unicode/prime/`?`/`!`/guillemet (`«...»`) names, emits `delim` tokens with outer depth on both openers and closers, and classifies `:=`, `=>`, `:`, `|`, `@`, `,` as `op` (and other single characters as `other`). `split_dotted_name` splits dotted names on `.` only outside `«...»` guillemets. Any lexical violation fails closed with `UnsupportedLeanFormError(rel_path, line, form)` formatted as `rel_path:line:form` (`unclosed-block-comment`, `unclosed-string`, `unclosed-raw-string`, `unclosed-guillemet`, `unbalanced-delimiter`, or `unclosed-delimiter` at EOF).

## Relationships

Consumed directly by [the Lean 4 parser](parser.py.md), which re-exports `UnsupportedLeanFormError` for downstream callers including [the `rh-extract` CLI](../cli/extract.py.md) and [the concept validator](../validate/concepts.py.md). Verified through [parser unit tests](../../../tests/unit/test_lean_parser.py.md).

## Evidence

- Defines UnsupportedLeanFormError(rel_path, line, form) formatted canonically as rel_path:line:form. [1]
- Defines frozen Token span/depth records and guillemet-safe split_dotted_name. [2]
- Lexes nested block comments, doc comments, strings, raw strings, char escapes, Unicode/guillemet identifiers and four delimiter pairs while failing closed on unclosed or unbalanced forms. [3]
