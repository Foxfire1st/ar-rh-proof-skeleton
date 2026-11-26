# audit/provenance.json

## Governing Overview

[Repository overview](../overview.md)

## Purpose

Stores the machine-generated, deterministic Phase 0 provenance snapshot pinning the audited upstream Lean proof repository, optional replay metadata, comparator configuration(s) and tracked documentation/verification note digests.

## Code Commentary

Serialized as canonical sorted-key JSON (`schema_version` `"1.0.0"`, zero wall-clock timestamps) matching `ProvenanceRecord`. Records the pinned `riemann-hypothesis` commit (`41c2716658c42ba08eb0648994a2efe83082a95c`, branch `main`), Lean toolchain (`leanprover/lean4:v4.37.0-rc1`), Mathlib revision (`31ce06eaadf4a200c0e1407c5f10953cf606b8f8`), the single root `comparator.json` configuration (`Challenge`/`Main`, 5 theorems, permitting `propext`, `Quot.sound` and `Classical.choice`, `enable_nanoda: true`, empty `definition_names` and `external_kernels`), empty `comparator_file_digests`, sorted SHA-256 digests of the five upstream documentation files (`CONTENTS.md`, `GUIDE.md`, `INDEX.md`, `README.md`, `REFERENCES.md`), and the `alpoge-rh-replay` pin (`c367a847a52c70c6b84e94ccb4698897db559b5f`, Lean `4.33.1`, 175,187 modules, `parts.sha256` digest). Never hand-edit; regenerate with `rh-audit` so regeneration stays byte-identical.

## Relationships

Conforms to `ProvenanceRecord` in [audit models](../src/rh_pipeline/models/audit.py.md), is written atomically by [status emission](../src/rh_pipeline/audit/status.py.md) via the [audit CLI](../src/rh_pipeline/cli/audit.py.md) alongside [verification-status.json](verification-status.json.md), and is read as the default pinned baseline by [upstream triage](../src/rh_pipeline/audit/triage.py.md) via the [check-upstream CLI](../src/rh_pipeline/cli/check_upstream.py.md). Described at a high level by `audit/README.md`.

## Evidence

- Pins empty comparator_file_digests and the single root comparator.json configuration (Challenge/Main, 5 theorems, 3 permitted axioms, enable_nanoda true). [1]
- Pins the five upstream Markdown note digests, alpoge-rh-replay metadata (c367a847a52c), schema_version 1.0.0, and riemann-hypothesis commit (41c2716658c4) and toolchain. [2]
