# site/package.json

## Governing Overview

[Repository overview](../overview.md)

## Purpose

Defines the private Node ESM package, VitePress CLI scripts (with an 8192 MB Node V8 heap cap on `docs:build`) and build-time MathJax 3 dependency for the static explainer site.

## Code Commentary

Declares private ESM package `rh-proof-explainer-site` (`0.1.0`) with `docs:dev` (`vitepress dev docs`), `docs:build` (`node --max-old-space-size=8192 ./node_modules/vitepress/bin/vitepress.js build docs`, raising Node's V8 heap limit above the 4096 MB default so bundling the full eight-pillar MathJax 3 inline-SVG corpus completes without out-of-memory failure) and `docs:preview` (`vitepress preview docs`) scripts targeting `docs`, and pins two `devDependencies`: `markdown-it-mathjax3` (`^4.3.2`) for build-time static SVG LaTeX math and `vitepress` (`^1.6.3`). Zero runtime dependencies are declared.

## Relationships

Paired with [site/package-lock.json](package-lock.json.md) for deterministic `npm ci` runs in [CI `site-build-check`](../.github/workflows/quality.yml.md) and `ar-rh-proof-skeleton/.github/workflows/pages.yml`. Enables `markdown.math: true` in [VitePress config](docs/.vitepress/config.mts.md).

## Evidence

- Defines private ESM package `rh-proof-explainer-site` with `docs:dev`, `docs:build` (invoking VitePress with `node --max-old-space-size=8192`) and `docs:preview` scripts targeting `docs`. [1]

- Declares `markdown-it-mathjax3` (`^4.3.2`) and `vitepress` (`^1.6.3`) as the sole `devDependencies`. [2]
