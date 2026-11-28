# src/rh_pipeline/markdown_mask.py

## Governing Overview

[Repository overview](../../overview.md)

## Purpose

Owns neutral, zero-domain-dependency low-level Markdown code, math, and directive masking primitives shared beneath both `rh_pipeline.validate` and `rh_pipeline.site` without inverting package ownership.

## Code Commentary

Imports only `re` (zero `ConceptCard` or `SiteBuildError` dependencies). `blank_span` replaces non-newline characters in place while preserving `\n` and `\r` so all masks remain exact same-length across LF and CRLF inputs with zero offset or line-number drift. `mask_fenced_code` blanks backtick and tilde fenced blocks (matching fence character and minimum run length, preserving trailing `\r`), `mask_inline_code` blanks balanced backtick runs without crossing blank-line paragraph breaks (`\r?\n\r?\n`), and `mask_code_spans` composes both. `find_math_end` locates closing `$` or `$$` spans while respecting escaped characters and paragraph boundaries for inline math. `find_directive_close` locates the closing `}}` of a directive body while skipping inline/display math spans, escaped characters, and balanced nested `{...}` braces.

## Relationships

Sits below both `validate` and `site` in the package import graph (realizing `INV-N55EQ7`). Consumed directly by [explainer jargon validation](validate/jargon.py.md) and [Markdown transforms](site/markdown_transforms.py.md) (which re-exports `mask_code_spans` and `mask_fenced_code` for [renderers.py](site/renderers.py.md)). Verified by [markdown-mask golden tests](../../tests/unit/test_markdown_mask.py.md).

## Evidence

- `mask_fenced_code`, `mask_inline_code` and `mask_code_spans` return same-length copies preserving LF and CRLF offsets across fenced blocks, nested backticks and paragraph breaks. [1]
- `find_math_end` and `find_directive_close` locate closing `$`/`$$` math spans and directive `}}` boundaries while skipping escaped characters, math and balanced nested braces. [2]
