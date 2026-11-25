# audit/provenance.json

## Governing Overview

[Repository overview](../overview.md)

## Purpose

Stores the machine-generated, deterministic Phase 0 provenance snapshot pinning the audited upstream Lean proof repository, optional replay metadata, comparator configurations and verification-note digests.

## Code Commentary

Serialized as canonical sorted-key JSON (`schema_version` `"1.0.0"`, zero wall-clock timestamps) matching `ProvenanceRecord`. Records the pinned `riemann-hypothesis` commit (`c900e67c2fcd1fdb041685820bbd2b867fbc5a4b`, branch `main`), Lean toolchain (`leanprover/lean4:v4.37.0-rc1`), Mathlib revision (`31ce06eaadf4a200c0e1407c5f10953cf606b8f8`), both comparator configurations (`comparator.json` with 5 theorems and `comparator/formal-conjectures.json` with 2 theorems, both permitting `propext`, `Quot.sound` and `Classical.choice` with `Lean paranoid` and `nanoda` external kernels), sorted SHA-256 digests of all 6 files under `comparator/`, SHA-256 digests of `verification/NOTES.md` and `verification/static-scan.txt`, and the `alpoge-rh-replay` pin (`22df13aabb3eeab0c05e6ed928cb5f3d21c7aee5`, 175,187 modules, `parts.sha256` digest). Never hand-edit; regenerate with `rh-audit` so regeneration stays byte-identical.

## Relationships

Conforms to `ProvenanceRecord` in [audit models](../src/rh_pipeline/models/audit.py.md) and is written atomically by [status emission](../src/rh_pipeline/audit/status.py.md) via the [audit CLI](../src/rh_pipeline/cli/audit.py.md) alongside [verification-status.json](verification-status.json.md). Described at a high level by `audit/README.md`.

## Evidence

- Pins both comparator configurations and recursive SHA-256 digests of all six files under comparator/. [1]
- Pins verification note digests, alpoge-rh-replay metadata, schema_version 1.0.0 and riemann-hypothesis commit and toolchain. [2]
