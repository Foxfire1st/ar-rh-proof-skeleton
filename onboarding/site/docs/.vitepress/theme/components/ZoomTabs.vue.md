# site/docs/.vitepress/theme/components/ZoomTabs.vue

## Governing Overview

[Repository overview](../../../../../overview.md)

## Purpose

Renders all three explanation zoom levels (`First-Principles | Undergrad | Formal Lean`) stacked in static/script-off/print HTML and upgrades after hydration to keyboard-navigable tabs with shareable URL state.

## Code Commentary

Maps `ZoomLevel` (`'first-principles' | 'undergrad' | 'lean'`, defaulting `activeLevel` to `'first-principles'`) onto `level-1`/`level-2`/`level-3` panels and named slots (`level1`/`level2`/`level3`) with labels `Level 1 · First-Principles`, `Level 2 · Undergrad Bridge` and `Level 3 · Formal Lean Contract`, parameterized by `pageId`. Initializes with `hydrated = false` so SSR and script-off HTML render a static `<nav class="rh-zoom-static-nav">` jump bar and all three `<section>` panels visibly without hiding content or attaching `role="tabpanel"`. On mount (`hydrated = true`), `parseLevelFromUrl` reads `?level=` or `#level-*` hashes (mapping `first-principles` and legacy `?level=abitur`/`#level-abitur` aliases to `'first-principles'`), `syncUrl` updates `?level=` via `history.replaceState`, `onTabKeydown` implements WAI-ARIA `ArrowLeft`/`ArrowRight`/`Home`/`End` navigation with roving `tabindex`, and inactive panels receive `.rh-zoom-panel--hidden` while `@media print` restores all three panels and headings visibly.

## Relationships

Registered globally in [theme/index.ts](../index.ts.md), styled by [tokens.css](../tokens.css.md), and emitted by `split_zoom_levels` in [markdown_transforms.py](../../../../../src/rh_pipeline/site/markdown_transforms.py.md) for pages containing ordered `## Level 1`, `## Level 2` and `## Level 3` sections.

## Evidence

- Defines the three zoom tabs (`first-principles`/`undergrad`/`lean` mapped to `level1`/`level2`/`level3` slots with default `activeLevel = 'first-principles'`), `pageId` prop and pre-hydration `hydrated = false` state. [1]
- Parses `?level=` and `#level-*` deep links including legacy `abitur` aliases (`parseLevelFromUrl`), syncs `?level=` via `history.replaceState` (`syncUrl`) and handles `ArrowLeft`/`ArrowRight`/`Home`/`End` keyboard navigation (`onTabKeydown`). [2]
- Progressively enhances from script-off static jump nav and stacked panels to WAI-ARIA `tablist`/`tabpanel` controls with roving `tabindex` and print-restorable headings. [3]
