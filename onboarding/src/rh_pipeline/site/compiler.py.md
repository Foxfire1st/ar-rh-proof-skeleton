# src/rh_pipeline/site/compiler.py

## Governing Overview

[Repository overview](../../../overview.md)

## Purpose

Orchestrates deterministic, fail-closed compilation of validated Concept Cards, Stage 00 explainer essays, SVG assets, Lean skeletons, DAG metadata and audit status into a VitePress `docs/` tree without mutating `.vitepress/`.

## Code Commentary

`compile_site` completes all loading, validation and rendering in memory before touching `out_docs_dir`. It normalizes `base_url`, validates `audit/verification-status.json` and `dag/dag.json` schemas and enforces matching `upstream_commit` values, runs `validate_concept_corpus` and `validate_explainer_docs` (reusing `rh-validate` gates), loads one-level `docs/stages/<stage_id>/*.md` (requiring `00_statement/overview.md`), loads `docs/assets/**` and referenced `skeleton/` files with symlink-escape and containment checks, and confirms every Concept Card's `visual_ref` exists. `_rewrite_assets` normalizes Markdown and HTML asset references outside code spans to root-relative `/assets/...` while failing closed on traversal or missing files; `_compile_all_outputs` dual-emits assets into `assets/` and `public/assets/` so both VitePress asset resolution and direct `/assets/...` URLs work. `_extract_title` masks only fenced code so inline backticks in stage H1 titles survive in landing links and backlinks. `_write_outputs_atomically` rejects duplicate targets, stages fsynced temporary files before `os.replace`, and unlinks temps on failure; `_prune_managed_dirs` removes stale entries strictly inside `stages/`, `concepts/`, `assets/` and `public/assets/`, leaving `.vitepress/` intact.

## Relationships

Depends on [audit models](../models/audit.py.md), [concept models](../models/concepts.py.md), [DAG models](../models/dag.py.md), [concept validator](../validate/concepts.py.md), [jargon validator](../validate/jargon.py.md), [markdown_transforms.py](markdown_transforms.py.md) and [renderers.py](renderers.py.md). Re-exported by [the site package facade](__init__.py.md) and invoked by [the site-builder CLI](../cli/build_site.py.md). Compiles the external memory repository's dependent Stage 00 corpus (`docs/stages/00_statement/{overview,main-theorems,statement-definitions,formal-conjectures}.md`, 12 `docs/concepts/*.md` cards and 5 `docs/assets/*.svg` diagrams standing on `skeleton/00_statement/*.lean`, `dag/dag.json` and `audit/verification-status.json`) into gitignored `site/docs/` consumed by [VitePress config](../../../site/docs/.vitepress/config.mts.md). Verified by [the site compiler tests](../../../tests/unit/test_site_compiler.py.md).

## Evidence

- `compile_site` validates base URL, audit/DAG commit parity, Concept Card and explainer gates, Stage 00 overview presence, asset containment and `visual_ref` existence in memory before touching `out_docs_dir`. [1]
- `_rewrite_assets` normalizes Markdown and HTML asset URLs outside code spans to `/assets/...` while rejecting traversal or missing files, and `_compile_all_outputs` dual-emits assets under `assets/` and `public/assets/`. [2]
- `_compile_stage_pages` and `_compile_concept_pages` compose directive expansion, stage markers, zoom tabs, banners, glossaries, backtick-preserving titles and deterministic Stage-before-Concept backlinks. [3]
- `_write_outputs_atomically` stages fsynced temp files before `os.replace` and `_prune_managed_dirs` removes stale entries only inside `stages/`, `concepts/`, `assets/` and `public/assets/` without touching `.vitepress/`. [4]
