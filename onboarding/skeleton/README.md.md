# skeleton/README.md

## Governing Overview

[Repository overview](../overview.md)

## Purpose

Documents the `skeleton/` directory's generated Lean 4 signature stubs, their 8-stage layout (`00_statement/` … `07_congruences/`) and the provenance boundary separating stubs from verification truth.

## Code Commentary

Defines `skeleton/` as generated, deterministically formatted `.lean` signature stubs grouped by the 8 condensed proof stages (`00_statement/` … `07_congruences/`). Restricts generation exclusively to `rh_pipeline.lean` and `rh_pipeline.graph`, stripping proof bodies to provenance-preserving stubs that point to exact upstream source commits and line ranges while delegating compilation and verification truth to `audit/verification-status.json`.

## Relationships

Describes the generated Stage 00 skeletons ([statements.lean](00_statement/statements.lean.md), [challenge.lean](00_statement/challenge.lean.md) and [main.lean](00_statement/main.lean.md)), points to verification truth in [audit/verification-status.json](../audit/verification-status.json.md), and aligns with [README.md](../README.md.md).

## Evidence

- Defines skeleton/ as generated deterministic .lean signature stubs grouped by the 8 condensed proof stages (00_statement/ through 07_congruences/). [1]
- Restricts stub generation exclusively to rh_pipeline.lean and rh_pipeline.graph and delegates verification truth to audit/verification-status.json. [2]
