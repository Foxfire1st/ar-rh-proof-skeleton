# site/docs/.vitepress/theme/tokens.css

## Governing Overview

[Repository overview](../../../../overview.md)

## Purpose

Defines the archival-paper light-monograph palette, high-contrast typography, responsive narrow-viewport rules, reduced-motion overrides, print full-stack reveal rules and component styles for the explainer site.

## Code Commentary

Maps `--rh-*` semantic tokens (archival canvas, high-contrast ink, cobalt accent, amber unverified and emerald verified audit tones, terracotta analogy accents, serif/sans/mono stacks) onto VitePress `--vp-*` variables without copying Agents Remember's dark dashboard aesthetic. Enforces `overflow-x: clip`, visible `2px` `:focus-visible` outlines, a `70ch` prose measure (`1.72` line-height), `overflow-wrap: anywhere` for long Lean identifiers, and horizontal scrolling on wide code blocks, tables and display MathJax SVGs. Styles both compiler-emitted static blocks (`.rh-analogy-breaks`, `.rh-page-glossary`, `.rh-stage-minimap`, `.rh-concept-figure`, `.rh-lean-decl`) and all four Vue components (including `--rh-card-shift` popover clamping and `.rh-zoom-panel__heading--sr`). At `<=640px`, converts `.rh-concept-hover__card` into a fixed bottom sheet (`max-height: 60vh`), stacks zoom tabs and collapses banner facts; under `prefers-reduced-motion: reduce`, clamps transitions/animations to `0.001ms`; under `@media print`, hides interactive controls and restores all three zoom panels and headings visibly.

## Relationships

Imported by [theme/index.ts](index.ts.md) and coupled to class names emitted by [markdown_transforms.py](../../../../src/rh_pipeline/site/markdown_transforms.py.md), [renderers.py](../../../../src/rh_pipeline/site/renderers.py.md) and the four Vue components under `components/`. Implements visual rules in `system/design-guidelines.md`.

## Evidence

- Defines `--rh-*` light-monograph tokens and maps them onto VitePress `--vp-*` theme and code variables. [1]
- Enforces `overflow-x: clip`, `:focus-visible` outlines, `70ch` prose measure, Lean identifier wrapping and scrollable MathJax/code/tables. [2]
- Styles compiler-emitted `.rh-analogy-breaks`, `.rh-page-glossary`, `.rh-stage-minimap`, `.rh-concept-figure` and `.rh-lean-decl` blocks plus all four Vue components. [3]
- Provides `<=640px` bottom-sheet/stacked mobile layout, `prefers-reduced-motion: reduce` clamp and `@media print` full-stack zoom-panel visibility. [4]
