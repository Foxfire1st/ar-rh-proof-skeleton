# src/rh_pipeline/site/markdown_transforms.py

## Governing Overview

[Repository overview](../../../overview.md)

## Purpose

Implements pure, fail-closed Markdown transforms for VitePress static-site compilation: base-URL normalization, code/math masking, concept/term directive expansion, three-level zoom tab splitting and static page glossary rendering.

## Code Commentary

`normalize_base_url` strips whitespace, rejects empty strings, whitespace, backslashes, `//` and `.`/`..` path segments with `SiteDirectiveError`, and returns `"/"` or `"/{parts}/"`. `mask_fenced_code` and `mask_code_spans` return same-length masked copies preserving newlines so downstream scanners ignore fenced and inline code. `expand_concept_directives` normalizes CRLF/CR to LF, skips fenced code, inline code and `$...$`/`$$...$$` math, tracks balanced braces inside directives, splits `{{concept:<slug>|<prose>}}` on the first `|` (recursively expanding nested directives in `<prose>`), defaults `{{term:<slug>}}` display text to `card.term`, HTML-escapes all Vue attributes, emits `<ConceptReveal>` and `<ConceptHover />` (including `visual-src`/`visual-alt` only when `card.visual_ref` is present), fails closed with `SiteDirectiveError` on unknown slugs or malformed/empty tags, and returns deduplicated slugs in first-appearance order. `split_zoom_levels` wraps ordered `## Level 1`/`2`/`3` headings outside fenced code in `<ZoomTabs>` slots, preserving preamble, epilogue and pages lacking all three levels. `render_page_glossary` validates registry membership and emits an HTML-escaped `<dl>` fallback glossary for script-off and print readers.

## Relationships

Uses `ConceptCard` from [the concept models](../models/concepts.py.md), is re-exported by [the site package facade](__init__.py.md), and is consumed by [renderers.py](renderers.py.md) and [compiler.py](compiler.py.md). Its emitted Vue tags and `.rh-page-glossary` markup bind to [ConceptReveal.vue](../../../site/docs/.vitepress/theme/components/ConceptReveal.vue.md), [ConceptHover.vue](../../../site/docs/.vitepress/theme/components/ConceptHover.vue.md), [ZoomTabs.vue](../../../site/docs/.vitepress/theme/components/ZoomTabs.vue.md) and [tokens.css](../../../site/docs/.vitepress/theme/tokens.css.md). Verified by [the site compiler tests](../../../tests/unit/test_site_compiler.py.md).

## Evidence

- `normalize_base_url` canonicalizes base paths with leading and trailing slashes and rejects whitespace, backslashes, `//` and `.`/`..` traversal with `SiteDirectiveError`. [1]
- `expand_concept_directives` expands `{{concept:...}}` and `{{term:...}}` outside code and math into HTML-escaped `<ConceptReveal>` and `<ConceptHover />` tags while returning deduplicated first-seen slugs and failing closed on malformed or unknown tags. [2]
- `split_zoom_levels` wraps ordered Level 1, Level 2 and Level 3 headings outside fenced code in `<ZoomTabs>` slots while preserving preamble, epilogue and partial-level pages. [3]
- `render_page_glossary` validates referenced slugs against the registry and emits an ordered, HTML-escaped static `<dl>` glossary fallback. [4]
