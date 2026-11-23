# Memory Settings

- **Primary code repository:** `rh-proof-skeleton` (`Foxfire1st/rh-proof-skeleton`)
- **Paired memory repository:** `ar-rh-proof-skeleton` (`Foxfire1st/ar-rh-proof-skeleton`)
- **Landing workflow:** `pull-request` — both `rh-proof-skeleton` and `ar-rh-proof-skeleton` land changes on `main` through paired pull requests as defined in `system/task-publication.md`.
- **Cross-repository subscription (`crossRepo.allow`):** `riemann-hypothesis` (`leanprover-community/riemann-hypothesis`), branch-locked to `main` (`includeCode: true`, `includeMemory: false`). Read-only upstream proof source; never branch or commit into `riemann-hypothesis` from tasks here.
- **Eligible paths:** Configured in `system/settings.json` (`src/**`, `skeleton/**`, `dag/**`, `audit/**`, `site/**`, `README.md`, `pyproject.toml`, `.github/workflows/**`, including `.lean` and `.py`).
