# site/docs/.vitepress/theme/components/ZoomTabs.vue

## Governing Overview

[Repository overview](../../../../../overview.md)

## Purpose

Renders all three explanation zoom levels (`First-Principles | Undergrad | Formal Lean`) stacked in static/script-off/print HTML and upgrades after hydration to keyboard-navigable tabs with shareable URL state.

## Code Commentary

Maps `ZoomLevel` (`'first-principles' | 'undergrad' | 'lean'`, defaulting `activeLevel` to `'first-principles'`) onto `level-1`/`level-2`/`level-3` panels and named slots (`level1`/`level2`/`level3`) with labels `Level 1 · First-Principles`, `Level 2 · Undergrad Bridge` and `Level 3 · Formal Lean Contract`, parameterized by `pageId`. Initializes with `hydrated = false` so SSR and script-off HTML render a static `<nav class="rh-zoom-static-nav">` jump bar and all three `<section>` panels visibly without hiding content or attaching `role="tabpanel"`. On mount (`hydrated = true`), `applyLocation` resolves `?level=` (accepting `first-principles`, `undergrad`, `lean`, numeric `1`/`2`/`3` and legacy `abitur`) and `window.location.hash` via `resolveHashTarget` (supporting `LEVEL_ALIAS_HASHES` such as `#level-1`/`#level-abitur` as well as arbitrary heading IDs inside any `.rh-zoom-panel` via `findPanelLevelForElement`), switches `activeLevel` before scheduling `scheduleScroll` (`nextTick` plus double-`requestAnimationFrame` after VitePress's single-`rAF` router scroll), and calls `syncUrl` via `history.replaceState` without clobbering non-level heading hashes (clearing only level-alias hashes). Listens to `hashchange`, `popstate` (restoring Back/Forward level and hash) and bubble-phase document clicks (`onDocumentClick`, intentionally handling same-page `#` anchors even after VitePress's capture-phase router calls `preventDefault` so `On this page` outline clicks switch hidden panels and scroll cleanly), implements WAI-ARIA `ArrowLeft`/`ArrowRight`/`Home`/`End` roving-tabindex navigation (`onTabKeydown`), hides inactive panels with `.rh-zoom-panel--hidden` while `@media print` restores all three panels visibly, and cancels pending timers/rAFs and removes all listeners in `onBeforeUnmount`.

## Relationships

Registered globally in [theme/index.ts](../index.ts.md), styled by [tokens.css](../tokens.css.md), and emitted by `split_zoom_levels` in [markdown_transforms.py](../../../../../src/rh_pipeline/site/markdown_transforms.py.md) for pages containing ordered `## Level 1`, `## Level 2` and `## Level 3` sections.

## Evidence

- Defines the three zoom tabs (`first-principles`/`undergrad`/`lean` mapped to `level1`/`level2`/`level3` slots with default `activeLevel = 'first-principles'`), `LEVEL_ALIAS_HASHES`, `pageId` prop, `rootRef` and pre-hydration `hydrated = false` state. [1]
- Parses `?level=` (`1`/`2`/`3`, named and legacy `abitur`) and heading/alias hashes (`parseQueryLevel`, `resolveHashTarget`), switches panels and scrolls via `nextTick` + double-`rAF` (`scheduleScroll`), syncs `?level=` via `history.replaceState` preserving non-level heading hashes (`syncUrl`), handles `hashchange`, `popstate`, VitePress `On this page` outline clicks (`onDocumentClick`) and roving-tabindex keyboard navigation (`onTabKeydown`), and cleans up timers/rAFs/listeners in `onBeforeUnmount`. [2]
- Progressively enhances from script-off static jump nav and stacked panels to WAI-ARIA `tablist`/`tabpanel` controls bound to `rootRef` with roving `tabindex` and print-restorable headings. [3]
