# site/docs/.vitepress/theme/index.ts

## Governing Overview

[Repository overview](../../../../overview.md)

## Purpose

Registers the custom VitePress editorial theme, loads global design tokens and globally exposes the four interactive explainer Vue components for compiler-emitted Markdown tags.

## Code Commentary

Extends VitePress `DefaultTheme`, imports `./tokens.css` and registers `VerificationBanner`, `ConceptReveal`, `ConceptHover` and `ZoomTabs` in `enhanceApp` so generated Markdown pages can use those component tags directly without per-page script blocks.

## Relationships

Loads [tokens.css](tokens.css.md) and wires [VerificationBanner.vue](components/VerificationBanner.vue.md), [ConceptReveal.vue](components/ConceptReveal.vue.md), [ConceptHover.vue](components/ConceptHover.vue.md) and [ZoomTabs.vue](components/ZoomTabs.vue.md) for tags emitted by [markdown_transforms.py](../../../../src/rh_pipeline/site/markdown_transforms.py.md) and [renderers.py](../../../../src/rh_pipeline/site/renderers.py.md).

## Evidence

- Extends `DefaultTheme`, imports `./tokens.css` and globally registers `VerificationBanner`, `ConceptReveal`, `ConceptHover` and `ZoomTabs` in `enhanceApp`. [1]
