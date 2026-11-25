# src/rh_pipeline/validate/concepts.py

## Governing Overview

[Repository overview](../../../overview.md)

## Purpose

Loads `<memory>/docs/concepts/*.md` and validates Concept Card sourcing, child-concept graph references, cached Lean skeleton declaration anchors and optional `visual_ref` files.

## Code Commentary

`validate_concept_corpus(docs_root, *, skeleton_root)` returns `({}, ())` when `<docs_root>/concepts` is absent (and not a symlink), emits `concept-dir` if it is not a directory, reads `*.md` files (`concept-read` on non-regular/unreadable files), parses each via `parse_concept_card` and rejects duplicate slugs (`concept-duplicate-slug`). For every parsed card, `_validate_card` requires at least one entry in `sources` or `lean_anchors` (`concept-unsourced`), `_validate_child_concepts` rejects direct self-cycles (`concept-self-cycle`) and dangling slugs (`concept-missing-child`) while allowing distinct mutual cycles, `_validate_one_anchor` checks `<skeleton_rel_path>#<DeclarationName>` syntax (`lean-anchor-format`, rejecting leading `/`, `..` and `skeleton/` prefix) and resolves targets through `_AnchorResolver` (enforcing resolved containment under `skeleton_root` against symlink escapes and caching `extract_declarations` per skeleton file for `lean-anchor-missing-file`, `lean-anchor-parse-error` and `lean-anchor-missing-decl`), and `_validate_visual_ref` verifies any set `visual_ref` resolves inside `docs_root` (`concept-missing-visual`). Limits: does not scan prose directives or Level-1 jargon order (owned by `validate/jargon.py`).

## Relationships

Consumes [pure Concept Card models](../models/concepts.py.md), [closed-world Lean 4 parser](../lean/parser.py.md) and [ValidationFinding](../models/validation.py.md). Invoked by [the validator CLI](../cli/validate.py.md) and verified by [Concept Card corpus tests](../../../tests/unit/test_validate_concepts.py.md).

## Evidence

- validate_concept_corpus loads docs/concepts/*.md and enforces sourcing, child-concept resolution, Lean skeleton anchor checks and optional visual_ref containment. [1]
- _AnchorResolver enforces resolved skeleton_root containment and caches extract_declarations results per relative skeleton path. [2]
- _validate_child_concepts rejects self-cycles and missing child slugs while permitting mutual cycles between distinct concepts. [3]
