# site/docs/.vitepress/theme/components/VerificationBanner.vue

## Governing Overview

[Repository overview](../../../../../overview.md)

## Purpose

Renders an honest, build-time SSR verification status banner driven strictly by `audit/verification-status.json` props without requiring client JavaScript.

## Code Commentary

Declares nine required provenance props (`buildStatus`, `upstreamCommit`, `leanToolchain`, `mathlibRev`, `scanScope`, `solutionForbiddenCount`, `challengeSorryCount`, `permittedAxioms`, `theoremCount`) plus optional `upstreamRepo` defaulting to `https://github.com/leanprover-community/riemann-hypothesis`. Computes truncated 12-character commit and Mathlib identifiers, upstream tree/comparator/verification-notes URLs, and `statusMeta` mapping `'unverified-locally'` (and any non-verified status) to an amber unverified tone paired with explicit status labels so color never carries meaning alone. Renders a static `<aside aria-label="Verification status">` with a semantic `<dl>` fact grid and `noopener noreferrer` upstream links.

## Relationships

Registered globally in [theme/index.ts](../index.ts.md), styled by [tokens.css](../tokens.css.md), and populated strictly from [audit/verification-status.json](../../../../../audit/verification-status.json.md) via `render_verification_banner` in [renderers.py](../../../../../src/rh_pipeline/site/renderers.py.md).

## Evidence

- Declares nine required audit provenance props plus optional `upstreamRepo` and computes truncated commit/Mathlib hashes, upstream URLs and `statusMeta` tone mapping. [1]
- Renders script-off-safe `<aside aria-label="Verification status">` with a `<dl>` facts grid and `noopener noreferrer` upstream links. [2]
