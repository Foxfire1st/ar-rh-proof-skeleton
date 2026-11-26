# src/rh_pipeline/site/renderers.py

## Governing Overview

[Repository overview](../../../overview.md)

## Purpose

Renders audit verification banners, verbatim Lean declaration blocks with upstream GitHub line links, layered inline SVG stage mini-maps, Concept Pages with automatic backlinks, the concepts index and the Stage 00 landing page.

## Code Commentary

`render_verification_banner` serializes `VerificationStatus` strictly from `audit/verification-status.json` into `<VerificationBanner />` props — deriving `repo-layout="legacy"` when `status.community_verification_notes_sha256 is not None` and `repo-layout="condensed"` when `None` — and `inject_verification_banner` inserts it idempotently after the first non-code H1. `expand_stage_markers` expands `<!-- rh:lean-node:<id> -->` and `<!-- rh:stage-minimap:<stage_id> -->` comments outside code spans, failing closed with `SiteBuildError` on unknown IDs or invalid skeleton line ranges. `render_lean_declaration` extracts bounded verbatim snippets from `skeleton/` with dynamic backtick fences (`max(3, longest_run + 1)`) and links exact upstream `blob/<commit>/<path>#L<start>-L<end>` anchors. `render_stage_minimap` computes intra-stage longest-path layers, emits an accessible `<figure class="rh-stage-minimap">` + inline `<svg>` with `<title>`/`<desc>` and an ordered Markdown dependency list, preferring the hosting `rh:lean-node` page stem for node links. `render_concept_page` strips duplicate leading body H1s, embeds optional `<figure class="rh-concept-figure">` (`/assets/...`), renders the `## Where this analogy breaks` callout, resolves `lean_anchors` fail-closed against `dag/dag.json`, emits related concepts, Stage-before-Concept backlinks, sources and a self-excluding page glossary. `render_landing_index` renders the Stage 00 entry page with the `00_statement` minimap, landmark pages, alphabetical concept links and pinned upstream commit.

## Relationships

Consumes `VerificationStatus` from [audit models](../models/audit.py.md), `ConceptCard` from [concept models](../models/concepts.py.md), `DagGraph`/`DagNode`/`DagDeclarationRef` from [DAG models](../models/dag.py.md) and helpers from [markdown_transforms.py](markdown_transforms.py.md). Driven by [compiler.py](compiler.py.md) to compile external memory-repo dependent docs (`ar-rh-proof-skeleton/docs/{stages/00_statement/*.md,concepts/*.md,assets/*.svg}` standing on `skeleton/00_statement/*.lean`, `dag/dag.json` and `audit/verification-status.json`) for [VerificationBanner.vue](../../../site/docs/.vitepress/theme/components/VerificationBanner.vue.md) and [tokens.css](../../../site/docs/.vitepress/theme/tokens.css.md). Verified by [the site compiler tests](../../../tests/unit/test_site_compiler.py.md).

## Evidence

- `expand_stage_markers` expands `rh:lean-node` and `rh:stage-minimap` comments outside code spans with bounded verbatim Lean snippets, dynamic backtick fences and layered accessible SVG mini-maps, failing closed on unknown IDs or out-of-range lines. [2]
- `render_concept_page` deduplicates leading body H1s, resolves `lean_anchors` against `dag/dag.json`, omits self-references from the page glossary and renders analogy callouts, backlinks and sources. [3]
- `render_concepts_index` and `render_landing_index` emit deterministic alphabetical concept listings and the Stage 00 landing page with minimap, landmark links and pinned upstream commit. [4]

- render_verification_banner serializes VerificationStatus into <VerificationBanner /> (deriving repo-layout="legacy" when community_verification_notes_sha256 is present, else "condensed") and inject_verification_banner inserts it idempotently after the first non-code H1. [1]
