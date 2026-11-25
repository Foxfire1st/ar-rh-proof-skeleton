# pyproject.toml

## Governing Overview

[Repository overview](overview.md)

## Purpose

Defines the `rh-pipeline` Python package metadata, console script entry points and static-quality tool settings.

## Code Commentary

Requires Python 3.12 or newer with zero third-party runtime dependencies and declares `pytest`, `ruff`, `pyright` and `radon` under the `dev` extra. Registers three console scripts under `[project.scripts]`: `rh-audit` pointing to `rh_pipeline.cli.audit:main`, `rh-extract` pointing to `rh_pipeline.cli.extract:main` and `rh-file-size-gate` pointing to `rh_pipeline.validate.file_size:main`. Configures Ruff (Python 3.12, line length 100, lint rules `E`, `F`, `I`, `UP`, `B`, `SIM`, `RUF`), strict Pyright over `src` and `tests`, and Pytest test discovery under `tests`.

## Relationships

Installed editably by [bootstrap](scripts/bootstrap-venv.sh.md) and checked by [the quality runner](scripts/check-quality.sh.md). Its console scripts target the [Phase 0 audit CLI](src/rh_pipeline/cli/audit.py.md), the [Phase 1 extractor CLI](src/rh_pipeline/cli/extract.py.md) and the [file-size rail](src/rh_pipeline/validate/file_size.py.md).

## Evidence

- Registers rh-audit, rh-extract and rh-file-size-gate console scripts. [1]
- Enforces Python 3.12, zero runtime dependencies and strict Pyright and Ruff settings over src and tests. [2]
