# tests/unit/test_validate_cli.py

## Governing Overview

[Repository overview](../../overview.md)

## Purpose

Verifies the `rh-validate` CLI entry point, optional `--memory` skip/error behavior, sorted cross-gate diagnostics, UTF-8 error resilience and installed console-script execution.

## Code Commentary

Tests `rh-validate --repo <repo>` without `--memory` (validating live repo schemas with explicit skip INFO note and exit 0), missing `--repo` and missing `--memory` directories (`cli-missing-repo` / `cli-missing-memory`, exit 1), `--memory` with absent or empty `docs/concepts/` (explicit INFO skip note, exit 0), end-to-end populated memory passing while ignoring `<memory>/onboarding/`, canonically sorted combined schema and jargon findings (exit 1), clean `concept-term-collision` reporting without raising an unhandled `ValueError` traceback (exit 1), non-UTF-8 concept and stage files emitting `concept-read`/`explainer-read` without crashing, and direct invocation of the installed `.venv/bin/rh-validate` console script. Limits: subprocess console-script test requires `REPO_ROOT/.venv/bin/rh-validate` to be installed and runs it directly without skipping.

## Relationships

Tests [the validator CLI](../../src/rh_pipeline/cli/validate.py.md) and its registration in [package metadata](../../pyproject.toml.md).

## Evidence

- Omitting --memory or passing a memory root without populated docs/concepts prints explicit INFO notes and exits 0 when schemas pass. [1]
- Combined schema and jargon failures print canonically sorted rule:path:line:message findings and return exit code 1. [2]
- Populated memory passes end-to-end while ignoring onboarding/, and installed rh-validate runs cleanly against the repository. [3]

- Verifies `rh-validate` reports cross-card `concept-term-collision` findings cleanly with exit code 1 rather than crashing on `_build_term_matcher`'s `ValueError`. [4]
