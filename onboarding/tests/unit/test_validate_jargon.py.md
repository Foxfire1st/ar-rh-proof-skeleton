# tests/unit/test_validate_jargon.py

## Governing Overview

[Repository overview](../../overview.md)

## Purpose

Verifies explainer directive resolution (`{{concept:...}}`, `{{term:...}}`), Level-1 jargon-before-reveal ordering, exemptions, `line_offset` alignment and strict `docs/{concepts,stages}/` scoping.

## Code Commentary

Covers conforming Level-1 reveal-then-use prose, exact-line `jargon-before-reveal` findings for premature raw terms, aliases, `-s`/`-es` plurals and `{{term:...}}` directives, same-line offset ordering and multiline whitespace normalization, first-pipe concept directives containing `$|z| < 1$`, LaTeX braces and inline code, unrevealed secondary terms inside another concept's inline explanation, all-level `invalid-directive` and `unknown-concept` checks (including with an empty registry), exemptions for fences, inline code, `$`/`$$` math, link URLs, hyphenated compounds (`non-analytic`) and Level-2/3 sections, Concept Card `own_slug` exemption, compound-term longest-match precedence, CRLF line fidelity, and strict `validate_explainer_docs` scanning that ignores `onboarding/`, root `docs/*.md`, `stages/*.md` and nested stage subdirectories. Limits: uses synthetic Concept Card registries and temporary Markdown trees.

## Relationships

Tests [explainer jargon validation](../../src/rh_pipeline/validate/jargon.py.md) with [Concept Card models](../../src/rh_pipeline/models/concepts.py.md).

## Evidence

- Premature raw terms, aliases, simple plurals and term directives in Level-1 prose emit exact-line jargon-before-reveal findings. [1]
- Fenced/inline code, math, link URLs, hyphenated compounds and Level-2/3 sections are exempt from jargon-before-reveal while directives still resolve everywhere. [2]
- validate_explainer_docs scans strictly docs/concepts/*.md and docs/stages/<stage_id>/*.md and never scans onboarding/. [3]
