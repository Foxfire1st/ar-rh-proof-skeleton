# Coding Tools & Verification Commands

Run all commands from the `rh-proof-skeleton` repository root (inside the task's own code worktree and virtual environment).

## Python Quality Gates

```text
ruff check .
ruff format --check .
pyright
radon cc src -s -a -n B
python -m rh_pipeline.validate.file_size
pytest
```

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
