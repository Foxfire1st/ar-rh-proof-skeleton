# site/docs/.vitepress/theme/components/VerificationBanner.vue

## Governing Overview

[Repository overview](../../../../../overview.md)

## Purpose

Renders an honest, build-time SSR verification status banner driven strictly by `audit/verification-status.json` props without requiring client JavaScript.

## Code Commentary

Declares nine required provenance props (`buildStatus`, `upstreamCommit`, `leanToolchain`, `mathlibRev`, `scanScope`, `solutionForbiddenCount`, `challengeSorryCount`, `permittedAxioms`, `theoremCount`) plus two optional props: `upstreamRepo` (defaulting to `https://github.com/leanprover-community/riemann-hypothesis`) and `repoLayout?: 'condensed' | 'legacy'` (defaulting to `'condensed'`). Computes truncated 12-character commit and Mathlib identifiers, `commitTreeUrl`, layout-conditional `upstreamLinks` (`comparator.json`, `GUIDE.md` and `INDEX.md` for `'condensed'`, vs `comparator/` and `verification/NOTES.md` for `'legacy'`), and `statusMeta` mapping `'unverified-locally'` (and any non-verified status) to an amber unverified tone paired with explicit status labels so color never carries meaning alone. Renders a static `<aside aria-label="Verification status">` with a semantic `<dl>` fact grid and `v-for`-rendered `noopener noreferrer` upstream links.

## Relationships

Registered globally in [theme/index.ts](../index.ts.md), styled by [tokens.css](../tokens.css.md), and populated strictly from [audit/verification-status.json](../../../../../audit/verification-status.json.md) via `render_verification_banner` in [renderers.py](../../../../../src/rh_pipeline/site/renderers.py.md).

## Evidence

- Declares nine required audit provenance props plus optional upstreamRepo and repoLayout (default condensed), computing truncated commit/Mathlib hashes, layout-specific upstreamLinks (comparator.json/GUIDE.md/INDEX.md vs comparator/ and verification/NOTES.md) and statusMeta tone mapping. [1]
- Renders script-off-safe aside aria-label Verification status with a semantic dl facts grid and v-for-rendered noopener noreferrer upstream links. [2]
