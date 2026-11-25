# src/rh_pipeline/cli/build_site.py

## Governing Overview

[Repository overview](../../../overview.md)

## Purpose

Provides the thin `rh-build-site` command-line entry point that binds CLI flags to deterministic VitePress static-site compilation without carrying compiler business logic.

## Code Commentary

`_build_parser` defines `rh-build-site` with required `--repo`, `--memory` and `--out` paths plus optional `--base-url` defaulting to `/ar-rh-proof-skeleton/`. `main` constructs `SiteBuildConfig`, invokes `compile_site`, catches `SiteBuildError` and `SiteDirectiveError` to print `ERROR: <exc>` on stderr and return exit code `1` with zero writes, and prints stage, concept and asset counts to stdout with exit code `0` on success.

## Relationships

Registered as `rh-build-site` in [package metadata](../../../pyproject.toml.md), delegates all validation and rendering to [the site compiler](../site/compiler.py.md) via [the site package facade](../site/__init__.py.md), and is invoked by [CI `site-build-check`](../../../.github/workflows/quality.yml.md) and `ar-rh-proof-skeleton/.github/workflows/pages.yml`. Verified by [the site compiler tests](../../../tests/unit/test_site_compiler.py.md).

## Evidence

- `_build_parser` defines `rh-build-site` with required `--repo`, `--memory`, `--out` paths and `--base-url` defaulting to `/ar-rh-proof-skeleton/`. [1]
- `main` delegates to `compile_site`, catches `SiteBuildError` and `SiteDirectiveError` to emit `ERROR:` on stderr with exit code 1, and prints compiled counts with exit code 0 on success. [2]
