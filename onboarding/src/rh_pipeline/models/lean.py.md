# src/rh_pipeline/models/lean.py

## Governing Overview

[Repository overview](../../../overview.md)

## Purpose

Defines immutable, slotted domain models (`LeanDeclaration`, `ExtractedModule`) and the closed ten-kind Lean 4 declaration vocabulary with strict exact-key dictionary serialization.

## Code Commentary

Pins `LeanDeclarationKind` and `VALID_DECLARATION_KINDS` to the ten admitted Lean 4 declaration kinds (`theorem`, `lemma`, `def`, `abbrev`, `structure`, `inductive`, `class`, `instance`, `opaque`, `axiom`). Both `LeanDeclaration` and `ExtractedModule` are frozen slotted dataclasses whose `__post_init__` and `from_dict` helpers raise `ValueError` on non-mapping payloads, missing or extra keys, unknown declaration kinds, non-string or empty required fields, boolean values passed as integers, invalid 1-based inclusive line bounds (`end_line < start_line`), or mismatches between `body == ""` and `body_lines == 0`. `ExtractedModule.__post_init__` requires strict tuple fields while `from_dict` coerces lists or tuples into tuples. Limits: `LeanDeclaration` does not validate POSIX path syntax on `upstream_path` or recompute `body_sha256` against `body`, and `ExtractedModule` relies on the parser to sort and deduplicate `imports`.

## Relationships

Provides `VALID_DECLARATION_KINDS` and `LeanDeclarationKind` to [DAG models](dag.py.md). Constructed by [the Lean 4 parser](../lean/parser.py.md), rendered by [the stub emitter](../lean/stub_emitter.py.md), consumed by [the stage-map and DAG builder](../graph/stage_map.py.md) and verified by [parser unit tests](../../../tests/unit/test_lean_parser.py.md).

## Evidence

- Pins the closed ten-kind Lean declaration vocabulary used across models, parser and emitter. [1]
- LeanDeclaration enforces exact ten-key deserialization, bool-rejected 1-based line bounds and the empty-body body_lines invariant. [2]
- ExtractedModule enforces strict tuple storage for imports and declarations with exact four-key dictionary round-tripping. [3]
