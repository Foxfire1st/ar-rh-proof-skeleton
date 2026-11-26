# tests/unit/test_stub_emitter.py

## Governing Overview

[Repository overview](../../overview.md)

## Purpose

Verifies deterministic Lean skeleton rendering, `preserve_defs` routing across all ten declaration kinds, exact verbatim/stub formatting and `DagDeclarationRef` line-span alignment.

## Code Commentary

Constructs a synthetic module covering all ten `VALID_DECLARATION_KINDS` and checks both `preserve_defs=False` and `preserve_defs=True` to verify verbatim `EXTRACTED_DEF` emission (with trailing provenance comment outside `DagDeclarationRef.end_line`), stub `EXTRACTED_STUB` emission (with `sorry` line inside `DagDeclarationRef.end_line`), single-blank-line block separation and single trailing newline. Locks down exact rendered output for inline, space-indented multiline and tab-indented verbatim bodies, verbatim inline/indented `where` and equation-compiler `|` bodies omitting `:=` (versus norm `|x|` and identifier `whereas` bodies keeping `:=`, verified by round-tripping through `extract_declarations` and stubbing under `preserve_defs=False`), stubbed multiline `def` and `theorem` blocks, declaration-free header-only modules, and byte-identical UTF-8 re-rendering determinism.

## Relationships

Tests [the stub emitter](../../src/rh_pipeline/lean/stub_emitter.py.md) alongside [the Lean 4 parser](../../src/rh_pipeline/lean/parser.py.md), [Lean models](../../src/rh_pipeline/models/lean.py.md) and [DAG models](../../src/rh_pipeline/models/dag.py.md). Executed by [the quality runner](../../scripts/check-quality.sh.md).

## Evidence

- Verifies verbatim versus stub routing, provenance markers and exact 1-based line alignment across all ten declaration kinds under both preserve_defs modes. [1]
- Verifies exact full-file layout for inline, space-indented and tab-indented verbatim bodies and stubbed multiline declarations. [2]
- Verifies byte-identical UTF-8 output and identical DagDeclarationRef tuples on repeated rendering. [3]

- Verifies verbatim where and equation-arm bodies omit := while norm and identifier bodies retain :=, round-tripping through extract_declarations and stubbing under preserve_defs=False. [4]
