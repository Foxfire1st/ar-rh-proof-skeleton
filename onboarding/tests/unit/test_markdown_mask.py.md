# tests/unit/test_markdown_mask.py

## Governing Overview

[Repository overview](../../overview.md)

## Purpose

Provides golden unit tests verifying neutral `rh_pipeline.markdown_mask` primitives, `site.markdown_transforms` re-export identity, and exact same-length offset and line preservation across LF and CRLF inputs.

## Code Commentary

Tests re-export identity (`site.markdown_transforms.mask_code_spans is mask_code_spans` and `mask_fenced_code`) and neutral module boundaries (no `ConceptCard` or `SiteBuildError` attributes on `markdown_mask`), CRLF fenced and inline code masking length and newline-index parity, nested and odd unmatched backtick runs plus CRLF paragraph-break boundaries, `$...$`/`$$...$$` math inside inline code and directives inside math, and `find_directive_close` with nested `{...}` braces, inline math, escaped `\}` and term directives carrying braces.

## Relationships

Verifies [markdown_mask.py](../../src/rh_pipeline/markdown_mask.py.md) and re-exports in [markdown_transforms.py](../../src/rh_pipeline/site/markdown_transforms.py.md), executed by [the quality runner](../../scripts/check-quality.sh.md).

## Evidence

- Verifies `site.markdown_transforms` re-exports the exact `markdown_mask` functions and that `markdown_mask` stays neutral with zero `ConceptCard` or `SiteBuildError` attributes. [1]
- Golden tests confirm exact same-length masking and LF/CRLF newline preservation across fenced blocks, nested/odd backtick runs and paragraph breaks. [2]
- Golden tests verify math inside inline code, directives inside inline/display math and directive closing across nested braces and escaped characters. [3]
