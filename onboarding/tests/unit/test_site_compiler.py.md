# tests/unit/test_site_compiler.py

## Governing Overview

[Repository overview](../../overview.md)

## Purpose

Unit-tests Phase 4 static-site Markdown transforms, legacy/condensed `VerificationBanner` `repo-layout` rendering, landing-page 8-pillar proof roadmap and conditional `proof-journey` rendering, fail-closed compilation, deterministic output, managed-directory stale pruning, `.vitepress/` preservation, backtick-preserving stage titles, concept H1 deduplication and `rh-build-site` CLI exit codes.

## Code Commentary

Constructs an isolated synthetic repository and memory corpus via `_make_fixture(..., condensed: bool = False)` (audit status, Stage 00 DAG, two Lean skeleton files, an SVG asset, two Concept Cards and Stage 00 overview, plus `_add_roadmap_stages` extending the DAG with Stages 01 and 02) and collects 19 test cases across 11 test functions. Covers `normalize_base_url`, `expand_concept_directives` (including math, balanced braces, custom display text, HTML escaping, code/math exemptions and 9 parametrized fail-closed malformed/unknown directive cases), `split_zoom_levels`, `render_page_glossary`, `render_verification_banner` and end-to-end `compile_site` `repo-layout="legacy"` vs `repo-layout="condensed"` rendering, end-to-end deterministic `compile_site` with stale pruning and `.vitepress/` survival, 6 fail-closed zero-write error paths (`missing_ver`, `missing_dag`, `commit_mismatch`, `unknown_slug`, `missing_asset`, `bad_node`), `rh-build-site` exit codes `0`/`1`, stage-title backtick preservation plus concept-body H1 deduplication, and landing-index 8-pillar roadmap rendering both without `proof-journey.md` (landmark counts, `Skeleton extracted — essay upcoming` / `Upcoming` statuses, zero unpublished-stage links) and with `proof-journey.md` + `main-theorems.md` (intro and roadmap surfacing of `proof-journey`, exclusion of `overview` and `proof-journey` from `## Landmark Pages`).

## Relationships

Verifies [markdown_transforms.py](../../src/rh_pipeline/site/markdown_transforms.py.md), [renderers.py](../../src/rh_pipeline/site/renderers.py.md), [compiler.py](../../src/rh_pipeline/site/compiler.py.md) and [the site-builder CLI](../../src/rh_pipeline/cli/build_site.py.md), executed by [the quality runner](../../scripts/check-quality.sh.md).

## Evidence

- Markdown transform tests verify base-URL normalization, directive expansion/escaping/code-and-math exemptions, 9 parametrized fail-closed malformed/unknown directives, `<ZoomTabs>` splitting and static page glossary rendering. [1]
- End-to-end compiler tests verify byte-identical determinism, repo-layout="legacy" banner rendering, managed-directory stale pruning with .vitepress/ preservation, dual asset emission, Lean anchors, backlinks, backtick-preserving stage titles and concept H1 deduplication. [2]
- Fail-closed and CLI tests prove zero-write refusal across six invalid-input cases and `rh-build-site` exit codes 0 and 1. [3]
- Verifies render_verification_banner and compile_site emit repo-layout="legacy" when verification digests exist and repo-layout="condensed" when they are None. [4]

- Verifies landing-index 8-pillar proof roadmap ordering, extracted landmark counts, unpublished-stage status fallbacks without dead links, and conditional `proof-journey` surfacing in intro/roadmap with `## Landmark Pages` exclusion. [5]
