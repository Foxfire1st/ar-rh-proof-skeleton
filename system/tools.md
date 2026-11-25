# Coding Tools & Verification Commands

Run the Python checks from the root of the task's own code worktree.

## Python Quality Gates

```sh
scripts/bootstrap-venv.sh
scripts/check-quality.sh
```

Bootstrap creates or verifies the worktree's editable `.venv`. It prefers Python 3.12 and accepts a Python 3.12 or newer override through `PYTHON_BIN`. Run `scripts/bootstrap-venv.sh --replace` to rebuild the environment.

The runner verifies the isolated import and uses the worktree's executables. GitHub Actions runs the same two scripts. For focused verification, invoke the corresponding tool from `.venv/bin/`.

```text
ruff check .
ruff format --check .
pyright
radon cc src -s -a -n B --output-file=
python -m rh_pipeline.validate.file_size
rh-validate --repo .
pytest
```

- Run `rh-validate --repo . --memory <memory-root>` (or set `RH_MEMORY_ROOT=<memory-root>` when calling `scripts/check-quality.sh`) to also run Concept Card, Lean-anchor, directive, and Level-1 jargon-order checks under `<memory>/docs/{concepts,stages}/`.
- Run focused checks during implementation and report exact commands, scope, and any checks not run.
- Preview autofixes with `ruff check --fix --diff .` / `ruff format --diff .` before applying.
- Treat Radon complexity findings as risk maps: simplify or split by responsibility rather than fragmenting into trivial helpers.
- Keep unit tests fast and synthetic using fixtures under `tests/fixtures/`; never copy the 149k-file upstream proof tree into `tests/`.

## Static Site (VitePress)

From `site/`:

```text
npm ci
npm run docs:build
```

## Browser Verification (Playwright)

Frontend and explainer pages must be visually inspected and operated in a real browser (Playwright MCP for interactive step-by-step checks; scripted Playwright for screenshots, `prefers-reduced-motion: reduce`, and narrow widths `560px` / `390px`). Automated checks do not replace looking at rendered typography, math, hover cards, and layout.
