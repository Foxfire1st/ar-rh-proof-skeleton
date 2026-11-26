# skeleton/01_reduction/twisted_lattice.lean

## Governing Overview

[Repository overview](../../overview.md)

## Purpose

Preserves the Stage 01 `New.TwistedLattice` and `New.PrimePadicIso.Carries` structures verbatim and stubs the covolume-plus-index inequality theorem `covolume_add_index_nonneg`.

## Code Commentary

Auto-generated from upstream `src/New/Reduction/Lattices/TwistedLattice.lean` at commit `41c2716658c42ba08eb0648994a2efe83082a95c` with `preserve_defs: false`. Contains 32 declarations (19 `lemma`, 6 `def`, 4 `abbrev`, 2 `structure`, 1 `theorem`) across 281 lines. Even with `preserve_defs: false`, its 2 `structure` declarations are emitted verbatim with trailing `-- [EXTRACTED_DEF ...]` comments while all other declarations are stubbed with `sorry -- [EXTRACTED_STUB ...]` lines. Key declarations include `New.TwistedLattice` (lines 56–85, upstream `src/New/Reduction/Lattices/TwistedLattice.lean:109-138`), `New.TwistedLattice.IndexLe` (lines 97–101, upstream `src/New/Reduction/Lattices/TwistedLattice.lean:160-168`), `New.TwistedLattice.CovolumeLe` (lines 151–155, upstream `src/New/Reduction/Lattices/TwistedLattice.lean:262-266`), `New.TwistedLattice.volume` (lines 193–196, upstream `src/New/Reduction/Lattices/TwistedLattice.lean:334-336`), `New.TwistedLattice.covolume_add_index_nonneg` (lines 259–281, upstream `src/New/Reduction/Lattices/TwistedLattice.lean:444-485`). Recorded as landmark node `01_reduction/twisted_lattice` (`is_landmark: true`) with `depends_on: []` in `dag/dag.json` (a nearest-landmark dependency root). Never hand-edit; regenerate byte-identically with `rh-extract`.

## Relationships

Mapped in [the 8-stage map](../../dag/stage-map.json.md) under stage `01_reduction` (The Theorem and Its Reduction), rendered by [the stub emitter](../../src/rh_pipeline/lean/stub_emitter.py.md) via [the stage-map extractor](../../src/rh_pipeline/graph/stage_map.py.md) and [`rh-extract`](../../src/rh_pipeline/cli/extract.py.md), indexed as landmark node `01_reduction/twisted_lattice` in [the proof DAG](../../dag/dag.json.md), and round-trip checked by [stage-map unit tests](../../tests/unit/test_stage_map.py.md).

## Evidence

- Auto-generated header pins upstream src/New/Reduction/Lattices/TwistedLattice.lean at 41c2716658c42ba08eb0648994a2efe83082a95c with Preserve defs false. [1]
- Preserves structure New.TwistedLattice verbatim with its fields and trailing EXTRACTED_DEF provenance marker (upstream 109-138). [2]
- Stubs the sole theorem New.TwistedLattice.covolume_add_index_nonneg with preserved docstring and EXTRACTED_STUB marker (upstream 444-485, body_lines=21). [3]
