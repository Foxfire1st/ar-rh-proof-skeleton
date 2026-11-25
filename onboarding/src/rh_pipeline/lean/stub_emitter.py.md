# src/rh_pipeline/lean/stub_emitter.py

## Governing Overview

[Repository overview](../../../overview.md)

## Purpose

Deterministically renders an `ExtractedModule` into a self-contained provenance-annotated Lean 4 skeleton file and exact line-anchored `DagDeclarationRef` records honoring `preserve_defs`.

## Code Commentary

Emits a three-line auto-generated header naming the upstream path, commit and `preserve_defs` setting, followed by single-blank-line-separated declaration blocks ending with one newline (omitting `module` and `import` statements, which remain on `ExtractedModule` for DAG construction). `_should_emit_verbatim` keeps `structure`, `inductive`, `class`, `opaque` and `axiom` always verbatim, keeps `def` and `abbrev` verbatim iff `preserve_defs=True`, and stubs `theorem`, `lemma`, `instance` and non-preserved `def`/`abbrev` bodies. Verbatim blocks format empty, inline (`:= <body>`) and space/tab-indented multiline (`:=\n<body>`) declarations followed by `-- [EXTRACTED_DEF upstream=<path>:<start>-<end> commit=<sha> body_sha256=<hex>]`, with `DagDeclarationRef.end_line` covering the declaration text and excluding that trailing provenance comment line. Stub blocks emit `<signature> := by` and `  sorry -- [EXTRACTED_STUB upstream=<path>:<start>-<end> commit=<sha> body_lines=<n> body_sha256=<hex>]`, with `DagDeclarationRef.end_line` including the `sorry` line.

## Relationships

Consumes `ExtractedModule` and `LeanDeclaration` from [Lean models](../models/lean.py.md) and constructs `DagDeclarationRef` from [DAG models](../models/dag.py.md). Called by [the stage-map extractor](../graph/stage_map.py.md) to generate `skeleton/00_statement/*.lean` and verified by [stub-emitter unit tests](../../../tests/unit/test_stub_emitter.py.md).

## Evidence

- Routes structure, inductive, class, opaque and axiom always verbatim, def and abbrev verbatim only when preserve_defs is true, and theorem, lemma and instance always to stubs. [1]
- Formats verbatim declarations with trailing EXTRACTED_DEF comments outside DagDeclarationRef.end_line. [2]
- Renders stubbed bodies with sorry EXTRACTED_STUB lines inside DagDeclarationRef.end_line and aligns every returned declaration reference. [3]
