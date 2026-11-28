# tests/unit/test_validate_concepts.py

## Governing Overview

[Repository overview](../../overview.md)

## Purpose

Verifies `ValidationFinding`, pure `ConceptCard` YAML frontmatter parsing and `validate_concept_corpus` sourcing, child-graph, Lean-anchor caching and `visual_ref` rules.

## Code Commentary

Tests `ValidationFinding` formatting, canonical sorting and runtime constructor guards, `ConceptSource`/`ConceptCard` invariants, valid LF/CRLF frontmatter parsing with exact `body_line_offset`, missing/unclosed/malformed YAML, frontmatter `body_markdown` and unknown-key refusal, slug/filename and `visual_ref` path safety, corpus missing/empty/non-directory handling, synthetic mutual-cycle fixtures, real Stage 00 skeleton anchor resolution, sourcing rules (`sources` or `lean_anchors`), `visual_ref` existence and symlink-escape refusal, all Lean anchor error classes, per-file `extract_declarations` caching, direct self-cycle vs mutual-cycle child rules, unreadable files, duplicate slugs, `find_plain_text_latex` and `concept-plain-text-latex` negative/positive cases (rejecting paired `$...$`, `$$`, `\(`, `\[`, `\command` on `term`/`aliases`/`inline_glimpse` while allowing lone currency `$`, escaped `\$` and Level 2/3 body math), and `concept_variants` same-card deduplication vs deterministic cross-card `concept-term-collision` findings. Owns the stable contract of the synthetic corpus fixtures under `tests/fixtures/validate/concepts/` (`riemann-zeta.md`, `critical-strip.md`, `zeta-strip.svg`, `sample.lean`). Limits: exercises only synthetic corpora and read-only repository `skeleton/00_statement/` files.

## Relationships

Tests [ValidationFinding](../../src/rh_pipeline/models/validation.py.md), [Concept Card models](../../src/rh_pipeline/models/concepts.py.md) and [Concept Card corpus validation](../../src/rh_pipeline/validate/concepts.py.md).

## Evidence

- ValidationFinding formatting, sorting and constructor guards and pure parse_concept_card frontmatter and body_line_offset rules are verified. [1]
- Synthetic mutual-cycle fixtures under tests/fixtures/validate/concepts/ and real Stage 00 skeleton anchors validate cleanly. [2]
- Lean anchor format, containment, parse-error, missing-declaration and per-file parser caching rules are enforced. [3]

- Verifies `find_plain_text_latex` and `concept-plain-text-latex` negative/positive cases plus `concept_variants` same-card deduplication and cross-card `concept-term-collision` findings. [4]
