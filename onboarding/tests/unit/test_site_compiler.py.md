# tests/unit/test_site_compiler.py

## Governing Overview

[Repository overview](../../overview.md)

## Purpose

Unit-tests Phase 4 static-site Markdown transforms, fail-closed compilation, deterministic output, managed-directory stale pruning, `.vitepress/` preservation, backtick-preserving stage titles, concept H1 deduplication and `rh-build-site` CLI exit codes.

## Code Commentary

Constructs an isolated synthetic repository and memory corpus via `_make_fixture` (audit status, Stage 00 DAG, two Lean skeleton files, an SVG asset, two Concept Cards and Stage 00 overview) and collects 16 test cases across 8 test functions. Covers `normalize_base_url`, `expand_concept_directives` (including math, balanced braces, custom display text, HTML escaping, code/math exemptions and 9 parametrized fail-closed malformed/unknown directive cases), `split_zoom_levels`, `render_page_glossary`, end-to-end deterministic `compile_site` with stale pruning and `.vitepress/` survival, 6 fail-closed zero-write error paths (`missing_ver`, `missing_dag`, `commit_mismatch`, `unknown_slug`, `missing_asset`, `bad_node`), `rh-build-site` exit codes `0`/`1`, and stage-title backtick preservation plus concept-body H1 deduplication.

## Relationships

Verifies [markdown_transforms.py](../../src/rh_pipeline/site/markdown_transforms.py.md), [renderers.py](../../src/rh_pipeline/site/renderers.py.md), [compiler.py](../../src/rh_pipeline/site/compiler.py.md) and [the site-builder CLI](../../src/rh_pipeline/cli/build_site.py.md), executed by [the quality runner](../../scripts/check-quality.sh.md).

## Evidence

- Markdown transform tests verify base-URL normalization, directive expansion/escaping/code-and-math exemptions, 9 parametrized fail-closed malformed/unknown directives, `<ZoomTabs>` splitting and static page glossary rendering. [1]
- End-to-end compiler tests verify byte-identical determinism, managed-directory stale pruning with `.vitepress/` preservation, dual asset emission, Lean anchors, backlinks, backtick-preserving stage titles and concept H1 deduplication. [2]
- Fail-closed and CLI tests prove zero-write refusal across six invalid-input cases and `rh-build-site` exit codes 0 and 1. [3]
