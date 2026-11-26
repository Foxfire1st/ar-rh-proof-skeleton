# audit/verification-status.json

## Governing Overview

[Repository overview](../overview.md)

## Purpose

Stores the machine-generated, self-contained Phase 0 verification-status snapshot consumed by downstream validators and the public explainer banner.

## Code Commentary

Serialized as canonical sorted-key JSON (`schema_version` `"1.0.0"`, zero wall-clock timestamps) matching `VerificationStatus`. Pins the upstream commit (`41c2716658c42ba08eb0648994a2efe83082a95c`), Lean toolchain (`leanprover/lean4:v4.37.0-rc1`), Mathlib revision (`31ce06eaadf4a200c0e1407c5f10953cf606b8f8`), explicit `build_status` `"unverified-locally"`, `scan_scope` `"entry"` over the 3 condensed entry Lean files (`Challenge.lean`, `src/Main.lean`, `src/Statements.lean`), `solution_forbidden_token_count` `0`, `challenge_sorry_count` `5` (lines 8, 11, 14, 17 and 20 in root `Challenge.lean`), the 5 deduplicated theorem names, the 3 permitted axioms (`propext`, `Quot.sound`, `Classical.choice`) and `null` legacy verification-note digests (`community_verification_notes_sha256` and `upstream_static_scan_sha256`). Carries no replay pin fields (those live in `provenance.json`). Never hand-edit; regenerate with `rh-audit`.

## Relationships

Conforms to `VerificationStatus` in [audit models](../src/rh_pipeline/models/audit.py.md) and is assembled and written atomically by [status emission](../src/rh_pipeline/audit/status.py.md) via the [audit CLI](../src/rh_pipeline/cli/audit.py.md) alongside [provenance.json](provenance.json.md). Described at a high level by `audit/README.md` (note: `audit/README.md` still overstates replay/axiom-audit/comparator-execution status relative to this Phase 0 schema).

## Evidence

- Records unverified-locally build_status, null community_verification_notes_sha256, entry scan scope over 3 files (Challenge.lean, src/Main.lean, src/Statements.lean), 0 solution findings and 5 Challenge.lean sorry hits. [1]
- Records scan_scope entry, schema_version 1.0.0, 0 solution_forbidden_token_count, all five theorem names, upstream commit 41c2716658c4 and null upstream_static_scan_sha256. [2]
