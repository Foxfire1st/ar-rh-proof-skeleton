# site/docs/.vitepress/theme/components/ConceptReveal.vue

## Governing Overview

[Repository overview](../../../../../overview.md)

## Purpose

Renders inline concept-first introductions (`Idea <plain prose> → <term link>`) with a distinct visual boundary and clickable Concept Page link readable identically without JavaScript.

## Code Commentary

Accepts `slug`, `term` and `href` props plus the default `<slot />` for inline prose. `resolvedHref` guards against double-prefixing `site.value.base` when calling VitePress `withBase`, handling pre-prefixed URLs emitted by `rh-build-site`. Marks decorative `Idea` and `→` indicators with `aria-hidden="true"` inside a static inline `<span class="rh-concept-reveal" :data-concept="slug">`.

## Relationships

Registered globally in [theme/index.ts](../index.ts.md), styled by [tokens.css](../tokens.css.md), and emitted by `_render_concept_reveal` in [markdown_transforms.py](../../../../../src/rh_pipeline/site/markdown_transforms.py.md) from `{{concept:<slug>|<prose>}}`.

## Evidence

- Accepts `slug`, `term` and `href` props and guards `resolvedHref` against double-prefixing `site.value.base` through `withBase`. [1]
- Renders a static inline `<span class="rh-concept-reveal">` with `aria-hidden="true"` decorative `Idea`/`→` markers, default prose slot and Concept Page link. [2]
