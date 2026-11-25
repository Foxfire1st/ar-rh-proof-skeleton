# src/rh_pipeline/models/concepts.py

## Governing Overview

[Repository overview](../../../overview.md)

## Purpose

Defines immutable Concept Card domain models and a pure, zero-I/O YAML frontmatter parser returning exact body line offsets.

## Code Commentary

`ConceptSource` (frozen/slots: `title`, `citation`, `kind` in `{"book", "paper", "mathlib", "reference"}`) and `ConceptCard` (frozen/slots: `slug`, `term`, `aliases`, `inline_glimpse`, `visual_ref`, `where_analogy_breaks`, `sources`, `lean_anchors`, `child_concepts`, `body_markdown`) enforce slug regex `^[a-z0-9]+(?:-[a-z0-9]+)*$`, non-empty stripped required text and safe `docs/`-relative POSIX `visual_ref` paths (rejecting leading `/`, backslashes, empty, `.` and `..` segments). `parse_concept_card(text, *, expected_slug, rel_path)` normalizes CRLF/CR to LF, splits `---` YAML frontmatter with `yaml.safe_load`, rejects YAML `body_markdown`, unknown keys, missing required keys, filename/slug mismatches and invalid `sources` entries, and returns `(card, sorted_findings, body_line_offset)` where `body_line_offset` equals the 1-based line count through the closing delimiter (`0` if delimiters are missing). Limits: pure text parsing only—does not check filesystem existence of `visual_ref`, Lean skeleton anchors, cross-card `child_concepts` or sourcing requirements (`sources` or `lean_anchors`).

## Relationships

Uses [ValidationFinding](validation.py.md) and PyYAML from [package metadata](../../../pyproject.toml.md). Consumed by [Concept Card corpus validation](../validate/concepts.py.md) and [explainer jargon validation](../validate/jargon.py.md). Verified by [Concept Card and model tests](../../../tests/unit/test_validate_concepts.py.md).

## Evidence

- ConceptSource and ConceptCard enforce closed source kinds, strict slug regex, non-empty required text and safe docs-relative visual_ref invariants without filesystem I/O. [1]
- parse_concept_card parses YAML frontmatter purely from text with exact required/optional keys, rejects YAML body_markdown and unknown keys, and returns exact body_line_offset. [2]
