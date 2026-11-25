# .github/workflows/quality.yml

## Governing Overview

[Repository overview](../../overview.md)

## Purpose

Runs the Python quality gates and static VitePress site-build check for pull requests and pushes to main.

## Code Commentary

Defines two jobs on `ubuntu-latest`. `python-gates` checks out the candidate, selects Python 3.12, and runs the worktree bootstrap before the quality runner. `site-build-check` sets up Python 3.12 and Node 22 (caching npm via `site/package-lock.json`), bootstraps `.venv`, constructs a synthetic Stage 00 and Concept Card fixture in `/tmp/rh-site-check-memory`, runs `rh-validate` and `rh-build-site --out site/docs --base-url /ar-rh-proof-skeleton/`, and runs `npm ci && npm run docs:build` inside `site/`.

## Relationships

The [runner](../../scripts/check-quality.sh.md) owns the Python gate order, while `site-build-check` exercises [the validator CLI](../../src/rh_pipeline/cli/validate.py.md), [the site-builder CLI](../../src/rh_pipeline/cli/build_site.py.md), [the VitePress package](../../site/package.json.md) and [its lockfile](../../site/package-lock.json.md) on every code PR without depending on external GitHub Pages deployment (which is owned by `ar-rh-proof-skeleton/.github/workflows/pages.yml`).

## Evidence

- The python-gates job calls both worktree scripts directly after setting up Python. [1]
- The site-build-check job validates and compiles a synthetic Stage 00 fixture with rh-validate and rh-build-site, then runs npm ci and npm run docs:build inside site/. [2]
