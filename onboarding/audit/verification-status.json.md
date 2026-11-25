# audit/verification-status.json

## Governing Overview

[Repository overview](../overview.md)

## Purpose

Stores the machine-generated, self-contained Phase 0 verification-status snapshot consumed by downstream validators and the public explainer banner.

## Code Commentary

Serialized as canonical sorted-key JSON (`schema_version` `"1.0.0"`, zero wall-clock timestamps) matching `VerificationStatus`. Pins the upstream commit (`c900e67c2fcd1fdb041685820bbd2b867fbc5a4b`), Lean toolchain (`leanprover/lean4:v4.37.0-rc1`), Mathlib revision (`31ce06eaadf4a200c0e1407c5f10953cf606b8f8`), explicit `build_status` `"unverified-locally"`, `scan_scope` `"entry"` over the 7 entry/comparator Lean files, `solution_forbidden_token_count` `0`, `challenge_sorry_count` `7` (lines 84 and 108 in `comparator/FormalConjecturesChallenge.lean` and lines 19, 21, 23, 25 and 27 in `src/Challenge.lean`), the 7 deduplicated theorem names, the 3 permitted axioms and both upstream verification-note SHA-256 digests. Carries no replay pin fields (those live in `provenance.json`). Never hand-edit; regenerate with `rh-audit`.

## Relationships

Conforms to `VerificationStatus` in [audit models](../src/rh_pipeline/models/audit.py.md) and is assembled and written atomically by [status emission](../src/rh_pipeline/audit/status.py.md) via the [audit CLI](../src/rh_pipeline/cli/audit.py.md) alongside [provenance.json](provenance.json.md). Described at a high level by `audit/README.md` (note: `audit/README.md` still overstates replay/axiom-audit/comparator-execution status relative to this Phase 0 schema).

## Evidence

- Records unverified-locally build_status, entry scan scope over 7 files, 0 solution forbidden tokens and 7 challenge sorrys. [1]
- Lists all seven deduplicated theorem names, upstream commit and upstream static-scan digest matching provenance.json. [2]
