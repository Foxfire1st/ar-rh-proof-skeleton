# site/docs/.vitepress/config.mts

## Governing Overview

[Repository overview](../../../overview.md)

## Purpose

Configures VitePress base-path normalization, clean URLs, light-only editorial appearance, build-time MathJax 3 SVG rendering, navigation and sidebar routing for the Stage 00 vertical slice.

## Code Commentary

`normalizeBase` reads `process.env.VITEPRESS_BASE` (defaulting to `'/ar-rh-proof-skeleton/'`), trims whitespace and guarantees leading and trailing slashes. `defineConfig` sets `cleanUrls: true`, locks light mode with `appearance: false`, enables native MathJax 3 build-time math via `markdown: { math: true }`, defines top navigation (`Home`, `Stage 00`, `Concepts`, `Upstream Lean Repo`), wires two sidebar sections (`Stage 00 — Formal Statement` with 4 pages and `Foundational Concepts` with index plus 12 concept routes), sets `[2, 3]` page outlines, and annotates both GitHub `socialLinks` with explicit `ariaLabel` attributes.

## Relationships

Must stay aligned with `SiteBuildConfig.base_url` in [compiler.py](../../../src/rh_pipeline/site/compiler.py.md) (preserved untouched by `_prune_managed_dirs`), clean-URL links emitted by [markdown_transforms.py](../../../src/rh_pipeline/site/markdown_transforms.py.md) and [renderers.py](../../../src/rh_pipeline/site/renderers.py.md), `markdown-it-mathjax3` in [site/package.json](../../package.json.md), and the theme in [theme/index.ts](theme/index.ts.md).

## Evidence

- `normalizeBase` reads `VITEPRESS_BASE` with default `/ar-rh-proof-skeleton/` and enforces leading and trailing slashes. [1]
- `defineConfig` sets `cleanUrls: true`, light-only `appearance: false` and `markdown.math: true` for build-time MathJax 3 SVG math. [2]
- `themeConfig` defines top navigation, Stage 00 (4 pages) and Foundational Concepts (index + 12 slugs) sidebar groups, `[2, 3]` outline and `ariaLabel`-annotated GitHub `socialLinks`. [3]
