# dag/README.md

## Governing Overview

[Repository overview](../overview.md)

## Purpose

Documents the `dag/` directory's two versioned JSON artifacts (`stage-map.json` and `dag.json`) and their role across the 8 condensed proof stages (`00_statement/` … `07_congruences/`).

## Code Commentary

Designates `stage-map.json` as the authoritative, versioned deterministic mapping from upstream Lean modules and landmarks to the explainer's 8 condensed proof stages (`00_statement/` … `07_congruences/`), and `dag.json` as the generated condensed landmark dependency graph consumed by curation and the static site DAG explorer.

## Relationships

Summarizes [the 8-stage map](stage-map.json.md) and [the generated proof DAG](dag.json.md), paired with the top-level ownership map in [README.md](../README.md.md).

## Evidence

- Designates stage-map.json as the authoritative versioned mapping to the 8 condensed proof stages (00_statement/ through 07_congruences/). [1]
- Designates dag.json as the generated condensed landmark dependency graph consumed by curation and the static site DAG explorer. [2]
