# src/rh_pipeline/models/audit.py

## Governing Overview

[Repository overview](../../../overview.md)

## Purpose

Owns the pure, immutable Phase 0 audit domain models, strict exact-key deserialization and deterministic canonical JSON formatting.

## Code Commentary

Defines `AUDIT_SCHEMA_VERSION` (`"1.0.0"`), literal aliases (`ScanScope`, `ScanRole`, `BuildStatus` restricted to `"unverified-locally"`) and seven `@dataclass(frozen=True, slots=True)` types: `UpstreamRepoPin`, `ReplayPin`, `ComparatorSummary`, `StaticScanFinding`, `StaticScanSummary`, `ProvenanceRecord` and `VerificationStatus`. Every `from_dict` enforces exact key sets (rejecting missing and extra keys with `ValueError`), non-empty strings, non-boolean integers with minimums (`line >= 1`, counts `>= 0`) and exact literal membership. `ComparatorSummary.__post_init__` and `ProvenanceRecord.__post_init__` wrap mapping fields in sorted `MappingProxyType` views. `format_canonical_json` serializes sorted-key JSON with `indent=2`, a trailing newline and no wall-clock timestamps. Limits: `from_dict` checks that `schema_version` is a non-empty string rather than equality with `AUDIT_SCHEMA_VERSION`, and `VerificationStatus.from_dict` does not cross-check that top-level counts agree with its nested `scan` summary.

## Relationships

Uses standard-library modules only with no filesystem or subprocess side effects. Consumed by [provenance collection](../audit/provenance.py.md), [static scanning](../audit/static_scan.py.md), [status assembly](../audit/status.py.md) and the [audit CLI](../cli/audit.py.md), and verified by [provenance tests](../../../tests/unit/test_audit_provenance.py.md), [static-scan tests](../../../tests/unit/test_audit_static_scan.py.md) and [CLI tests](../../../tests/unit/test_audit_cli.py.md).

## Evidence

- Formats deterministic sorted-key JSON with indent=2 and a trailing newline without timestamps. [1]
- Enforces exact key sets, non-empty strings, non-boolean integers and strict literal values during deserialization. [2]
- Freezes mapping fields in sorted MappingProxyType views on ProvenanceRecord and ComparatorSummary. [3]
- Restricts VerificationStatus build_status to unverified-locally and enforces exact-key round-trip serialization. [4]
