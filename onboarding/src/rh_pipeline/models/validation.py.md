# src/rh_pipeline/models/validation.py

## Governing Overview

[Repository overview](../../../overview.md)

## Purpose

Defines the shared immutable `ValidationFinding` diagnostic model and deterministic sorting helper used across all validator gates and the `rh-validate` CLI.

## Code Commentary

`ValidationFinding` is a frozen slotted dataclass with fields `rule`, `path`, `line` and `message`; `__post_init__` rejects empty or non-string text fields and rejects `bool`, negative or non-integer `line` values via runtime helpers. `format()` renders the canonical `rule:path:line:message` string (`line=0` for non-line-specific diagnostics), `sort_key()` orders findings by `(path, line, rule, message)` and `sort_findings()` sorts arbitrary iterables into a deterministic tuple. Limits: performs no filesystem I/O and imposes no rule-name registry; callers own canonical rule identifiers and repository-relative paths.

## Relationships

Pure stdlib-only model imported by [Concept Card models](concepts.py.md), [artifact schema validation](../validate/schemas.py.md), [Concept Card corpus validation](../validate/concepts.py.md), [explainer jargon validation](../validate/jargon.py.md) and [the validator CLI](../cli/validate.py.md). Verified by [Concept Card and model tests](../../../tests/unit/test_validate_concepts.py.md).

## Evidence

- ValidationFinding enforces non-empty text fields and non-negative non-bool line numbers, renders rule:path:line:message and provides canonical (path, line, rule, message) sorting. [1]
- sort_findings sorts arbitrary finding iterables deterministically into a tuple using ValidationFinding.sort_key. [2]
