# site/docs/.vitepress/config.mts

## Governing Overview

[Repository overview](../../../overview.md)

## Purpose

Configures VitePress base-path normalization, full HTML page title with compact navbar `siteTitle`, clean URLs, light-only editorial appearance, build-time MathJax 3 SVG rendering, top navigation and Stage 00 / Proof Journey / Concept sidebar routing.

## Code Commentary

`normalizeBase` reads `process.env.VITEPRESS_BASE` (defaulting to `'/ar-rh-proof-skeleton/'`), trims whitespace and guarantees leading and trailing slashes. `defineConfig` keeps full HTML `title: 'Riemann Hypothesis Proof Explainer'` while setting compact `themeConfig.siteTitle: 'RH Proof Explainer'` so the navbar brand fits inside VitePress's 272px sidebar header, sets `cleanUrls: true`, locks light mode with `appearance: false`, enables native MathJax 3 build-time math via `markdown: { math: true }`, defines 5 top navigation entries (`Home`, `Proof Journey`, `Stage 00`, `Concepts`, `Upstream Lean Repo`), wires two sidebar sections (`Stage 00 — Formal Statement` with 5 pages including `Full Proof Journey & 20-Step Map` at `/stages/00_statement/proof-journey`, and `Foundational Concepts` with index plus 12 concept routes), sets `[2, 3]` page outlines, and annotates both GitHub `socialLinks` with explicit `ariaLabel` attributes.

## Relationships

Must stay aligned with `SiteBuildConfig.base_url` in [compiler.py](../../../src/rh_pipeline/site/compiler.py.md) (preserved untouched by `_prune_managed_dirs`), clean-URL links emitted by [markdown_transforms.py](../../../src/rh_pipeline/site/markdown_transforms.py.md) and [renderers.py](../../../src/rh_pipeline/site/renderers.py.md), `markdown-it-mathjax3` in [site/package.json](../../package.json.md), the theme in [theme/index.ts](theme/index.ts.md), and defensive `.VPNavBarTitle` framing rules in [theme/tokens.css](theme/tokens.css.md).

## Evidence

- `normalizeBase` reads `VITEPRESS_BASE` with default `/ar-rh-proof-skeleton/` and enforces leading and trailing slashes. [1]
- `defineConfig` sets `cleanUrls: true`, light-only `appearance: false` and `markdown.math: true` for build-time MathJax 3 SVG math. [2]
- `themeConfig` sets compact navbar `siteTitle: 'RH Proof Explainer'`, defines 5 top navigation links (including `Proof Journey`), Stage 00 (5 pages including `Full Proof Journey & 20-Step Map`) and Foundational Concepts (index + 12 slugs) sidebar groups, `[2, 3]` outline and `ariaLabel`-annotated GitHub `socialLinks`. [3]
