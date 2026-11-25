# src/rh_pipeline/validate/jargon.py

## Governing Overview

[Repository overview](../../../overview.md)

## Purpose

Resolves `{{concept:slug|...}}` and `{{term:slug[|display]}}` directives across explainer Markdown under `<memory>/docs/{concepts,stages}/` and enforces Level-1 jargon-before-reveal ordering without scanning AR `onboarding/` cards.

## Code Commentary

`check_page_jargon(markdown, *, rel_path, registry, own_slug=None, line_offset=0)` normalizes CRLF/CR line endings, masks fenced code blocks and inline code spans while preserving newlines so offsets map to exact real file lines (`+ line_offset`), and scans `{{concept:...}}` and `{{term:...}}` directives across all sections outside code and math (splitting concept directives on the first `|`, tracking nested `{...}` and `$...$`/`$$...$$` to locate the closing `}}`, emitting `invalid-directive` or `unknown-concept`, and masking only the wrapper so inline prose is checked with `<slug>` already revealed). After masking directive wrappers, math spans and Markdown link URLs, `_build_term_matcher` matches registered `term`, `aliases` and simple `-s`/`-es` plurals case-insensitively and longest-match-first with `(?<![\w-])...(?![\w-])` boundaries (so hyphenated compounds such as `non-analytic` do not falsely match). `_evaluate_events` enforces `jargon-before-reveal` for raw terms and premature `{{term:...}}` directives strictly on lines flagged Level-1 by `_level1_line_flags` (`^##\s+Level\s+1\b` sections on stage docs, or pre-Level-2/3 body prose on Concept Cards) while exempting `own_slug` and Level-2/3 sections. `validate_explainer_docs(docs_root, *, registry)` scans strictly `<docs_root>/concepts/*.md` and `<docs_root>/stages/<stage_id>/*.md`, emitting `explainer-read` on unreadable stage files and silently skipping unreadable/unparseable concept files already reported by corpus validation. Limits: uses `\w` in its boundary lookaround, so underscore-joined identifiers are not matched as standalone terms; does not scan `onboarding/`, root `docs/*.md`, `stages/*.md` or deeper nested stage subdirectories.

## Relationships

Uses [pure Concept Card models](../models/concepts.py.md) and [ValidationFinding](../models/validation.py.md). Invoked by [the validator CLI](../cli/validate.py.md) with the registry returned by [Concept Card corpus validation](concepts.py.md). Verified by [explainer jargon tests](../../../tests/unit/test_validate_jargon.py.md).

## Evidence

- check_page_jargon masks code, math and link URLs without line drift, resolves concept and term directives in all sections and enforces Level-1 jargon order with line_offset. [1]
- _build_term_matcher compiles a case-insensitive longest-match-first pattern over terms, aliases and -s/-es plurals with non-alphanumeric/non-hyphen boundaries. [2]
- validate_explainer_docs scans strictly docs/concepts/*.md and one-level docs/stages/<stage_id>/*.md while ignoring onboarding/ and non-explainer paths. [3]
