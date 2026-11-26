# pyproject.toml

## Governing Overview

[Repository overview](overview.md)

## Purpose

Defines the `rh-pipeline` Python package metadata, console script entry points and static-quality tool settings.

## Code Commentary

Requires Python 3.12 or newer with `PyYAML>=6.0` as its runtime dependency and declares `pytest`, `ruff`, `pyright`, `radon` and `types-PyYAML>=6.0` under the `dev` extra. Registers six console scripts under `[project.scripts]`: `rh-audit` pointing to `rh_pipeline.cli.audit:main`, `rh-build-site` pointing to `rh_pipeline.cli.build_site:main`, `rh-check-upstream` pointing to `rh_pipeline.cli.check_upstream:main`, `rh-extract` pointing to `rh_pipeline.cli.extract:main`, `rh-file-size-gate` pointing to `rh_pipeline.validate.file_size:main` and `rh-validate` pointing to `rh_pipeline.cli.validate:main`. Configures Ruff (Python 3.12, line length 100, lint rules `E`, `F`, `I`, `UP`, `B`, `SIM`, `RUF`), strict Pyright over `src` and `tests`, and Pytest test discovery under `tests`.

## Relationships

Installed editably by [bootstrap](scripts/bootstrap-venv.sh.md) and checked by [the quality runner](scripts/check-quality.sh.md). Its console scripts target the [Phase 0 audit CLI](src/rh_pipeline/cli/audit.py.md), the [Phase 4 site-builder CLI](src/rh_pipeline/cli/build_site.py.md), the [upstream-freshness triage CLI](src/rh_pipeline/cli/check_upstream.py.md), the [Phase 1 extractor CLI](src/rh_pipeline/cli/extract.py.md), the [file-size rail](src/rh_pipeline/validate/file_size.py.md) and the [validator CLI](src/rh_pipeline/cli/validate.py.md).

## Evidence

- Registers rh-audit, rh-build-site, rh-check-upstream, rh-extract, rh-file-size-gate and rh-validate console scripts. [1]
- Enforces Python 3.12, PyYAML>=6.0 runtime and types-PyYAML>=6.0 dev dependencies, and strict Pyright and Ruff settings over src and tests. [2]
