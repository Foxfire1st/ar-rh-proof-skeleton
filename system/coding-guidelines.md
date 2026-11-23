# Code Ownership and Quality Guidelines

## Core Rule

Every meaningful concept gets one clear home. Files stay small enough that a human or agent can understand a module's responsibility without reading half the repository.

Before implementing any task, name the exact owning package and module for every new behavior in the design discussion and task document. Never append unrelated behavior to an existing file merely because it is nearby.

## Package Ownership (`rh-proof-skeleton`)

- `src/rh_pipeline/models/` — Immutable dataclasses and JSON schemas (provenance, verification status, DAG nodes, concept cards). No filesystem or subprocess side effects.
- `src/rh_pipeline/audit/` — Phase 0: upstream archive/repo pinning, static scans, comparator/replay receipt ingestion, `audit/verification-status.json`.
- `src/rh_pipeline/lean/` — Phase 1A: `.lean` declaration parsing, `.olean`/Lean environment adapters, reachability pruning, provenance-stub emission.
- `src/rh_pipeline/graph/` — Phase 1B: DAG condensation, landmark selection, deterministic `dag/stage-map.json` owner.
- `src/rh_pipeline/validate/` — Deterministic quality gates (file-size rail, schema checks, Lean anchor resolution, concept-slug and source-citation checks, jargon-order lint).
- `src/rh_pipeline/site/` — Phase 4: static VitePress site compiler (inline concept reveals, hover cards, zoom levels, interactive DAG pages).
- `src/rh_pipeline/cli/` — Thin command wiring only; zero business logic.
- `skeleton/` — Generated deterministic `.lean` stubs grouped by stage (`00_statement/` … `20_grh/`). Never hand-edited; always regenerated cleanly from upstream + `dag/stage-map.json`.
- `dag/` — Authoritative `stage-map.json` and generated `dag.json`.
- `audit/` — Machine-generated `provenance.json` and `verification-status.json`.
- `site/` — VitePress shell, design tokens, and reusable interactive components.

## File and Function Size Budgets

Enforced deterministically by `python -m rh_pipeline.validate.file_size` across `src/` and `tests/`:

| Band | Lines | Required Behavior |
|---|---:|---|
| Healthy | 0–300 | Normal edits allowed. |
| Watchful | 301–600 | New logic only if it fits the single responsibility. |
| Refactor pressure | 601–900 | Prefer extraction; justify keeping logic in the file. |
| Soft limit exceeded | 901–1200 | Do not add new feature logic without extracting. |
| Hard limit exceeded | >1200 | Gate fails; split before adding behavior. |

Function targets: ≤40 lines healthy (≤80 hard pressure), ≤5 normal args (use a frozen dataclass/config object otherwise), ≤3 nesting levels, cyclomatic complexity ≤10.

## Source Comment Scope

Comments explain technical **why**, invariants, or mathematical/Lean constraints for a future maintainer reading only this repository. Keep task IDs, review rounds, local paths, and chat provenance out of source comments (use Git history, task documents, and onboarding cards instead).
