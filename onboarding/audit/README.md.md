# audit/README.md

## Governing Overview

[Repository overview](../overview.md)

## Purpose

Documents the scope and epistemic boundaries of the machine-generated Phase 0 audit artifacts under `audit/`.

## Code Commentary

Summarizes `provenance.json` (pinned upstream repository commits, archive hashes, Lean toolchain version and Mathlib commit) and `verification-status.json` (Phase 0 lexical static-scan counts with `scan_scope: entry` and local build status `build_status: unverified-locally` rendered into the site's global verification banner, explicitly noting that replay provenance lives in `provenance.json` with no local axiom-audit or comparator-execution claim).

## Relationships

Summarizes [provenance.json](provenance.json.md) and [verification-status.json](verification-status.json.md), emitted by [`rh-audit`](../src/rh_pipeline/cli/audit.py.md) via [status.py](../src/rh_pipeline/audit/status.py.md) and checked by [artifact schema validation](../src/rh_pipeline/validate/schemas.py.md). Tracked by dependent record `DPD-GF9S0R`.

## Evidence

- Summarizes `provenance.json` and describes `verification-status.json` strictly as Phase 0 lexical static-scan counts (`scan_scope: entry`) with `build_status: unverified-locally`, pointing replay provenance to `provenance.json` with no local axiom-audit or comparator-execution claim. [1]
