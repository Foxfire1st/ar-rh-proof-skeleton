# src/rh_pipeline/site/__init__.py

## Governing Overview

[Repository overview](../../../overview.md)

## Purpose

Exposes the public Phase 4 static-site compiler and Markdown-transform API of `rh_pipeline.site`.

## Code Commentary

Re-exports `SiteBuildConfig`, `SiteBuildError`, `SiteBuildReport` and `compile_site` from `compiler.py`, together with `SiteDirectiveError`, `expand_concept_directives`, `normalize_base_url`, `render_page_glossary` and `split_zoom_levels` from `markdown_transforms.py`, listing all nine symbols in `__all__`.

## Relationships

Consumed by [the site-builder CLI](../cli/build_site.py.md) and [the site compiler unit tests](../../../tests/unit/test_site_compiler.py.md). Keeps low-level renderer internals in [renderers.py](renderers.py.md) behind the package facade while surfacing the stable compiler and transform contract.

## Evidence

- Re-exports the Phase 4 compiler dataclasses, `compile_site`, error types and Markdown transform helpers in `__all__`. [1]
