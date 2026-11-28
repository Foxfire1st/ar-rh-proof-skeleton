# src/rh_pipeline/cli/validate.py

## Governing Overview

[Repository overview](../../../overview.md)

## Purpose

Provides the thin `rh-validate` command-line entry point composing repository JSON schema validation with optional paired-memory Concept Card, Lean-anchor and Level-1 jargon checks.

## Code Commentary

`_build_parser` defines `rh-validate --repo <path> [--memory <path>]`. `main(argv=None)` fails immediately with `cli-missing-repo` (exit 1) when `--repo` is not a directory, then runs `validate_repo_artifacts(repo_root)` and `_run_memory_checks(repo_root, memory_root)`. When `--memory` is omitted, `_run_memory_checks` prints `INFO: memory checks skipped (no --memory path)` and runs only schema validation; when `--memory` does not exist as a directory, it emits `cli-missing-memory` (exit 1); when `<memory>/docs/concepts` has no regular `*.md` cards, it prints an explicit informational skip note and still runs `validate_explainer_docs(docs_root, registry={})`; otherwise it runs `validate_concept_corpus`, short-circuits before `validate_explainer_docs` whenever any `concept-term-collision` finding is present (so `rh-validate` reports canonical collision diagnostics and exits 1 cleanly instead of raising `ValueError` inside `_build_term_matcher`), and otherwise runs `validate_explainer_docs`. All findings across gates are merged, sorted canonically, printed as `rule:path:line:message` with exit 1, or `OK: rh-validate passed (0 findings).` with exit 0. Limits: read-only CLI that writes no files; if `docs/concepts` is a regular file rather than a directory, `has_cards` evaluates false and prints the unpopulated-corpus INFO note rather than invoking `validate_concept_corpus`'s `concept-dir` error.

## Relationships

Registered as `rh-validate` in [package metadata](../../../pyproject.toml.md) and invoked before `pytest` by [the quality runner](../../../scripts/check-quality.sh.md). Delegates to [artifact schema validation](../validate/schemas.py.md), [Concept Card corpus validation](../validate/concepts.py.md), [explainer jargon validation](../validate/jargon.py.md) and [ValidationFinding](../models/validation.py.md). Verified by [validator CLI tests](../../../tests/unit/test_validate_cli.py.md).

## Evidence

- main parses --repo and optional --memory, runs schema and memory gates, prints canonically sorted rule:path:line:message findings and returns exit code 0 or 1. [1]

- `_run_memory_checks` handles omitted, missing, unpopulated and populated memory roots with explicit INFO notes, strict `docs/` scoping and clean short-circuiting on `concept-term-collision` before jargon matching. [2]
