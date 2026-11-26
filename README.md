# ar-rh-proof-skeleton

Content, Concept Cards, diagrams, and Agents Remember memory repository for the **Riemann Hypothesis Proof Explainer**, documenting Levent Alpöge and Claude's 2026 Lean 4 proof of the Riemann Hypothesis and Generalized Riemann Hypothesis (GRH).

- **Live Explainer Site:** [foxfire1st.github.io/ar-rh-proof-skeleton](https://foxfire1st.github.io/ar-rh-proof-skeleton/)
- **Paired Pipeline Repository:** [Foxfire1st/rh-proof-skeleton](https://github.com/Foxfire1st/rh-proof-skeleton)
- **Quick Entry Points:** [Home](https://foxfire1st.github.io/ar-rh-proof-skeleton/) · [Full Proof Journey & 20-Step Map](https://foxfire1st.github.io/ar-rh-proof-skeleton/stages/00_statement/proof-journey) · [Stage 00 Overview](https://foxfire1st.github.io/ar-rh-proof-skeleton/stages/00_statement/overview) · [Stage 01 Overview](https://foxfire1st.github.io/ar-rh-proof-skeleton/stages/01_reduction/overview) · [Stage 02 Overview](https://foxfire1st.github.io/ar-rh-proof-skeleton/stages/02_derivatives/overview) · [Concept Cards](https://foxfire1st.github.io/ar-rh-proof-skeleton/concepts/)

> **Epistemic & Verification Status:** Local Lean compilation and Comparator replay are currently **`unverified-locally`**. This project does not claim that the upstream Lean 4 proof or Comparator checks have been verified locally. Deterministic upstream provenance, pinned commit metadata, and entry-point static-scan counts (`0` forbidden tokens across `src/Statements.lean` and `src/Main.lean`; `5` expected `sorry` tokens in root `Challenge.lean`) live in [`rh-proof-skeleton/audit/verification-status.json`](https://github.com/Foxfire1st/rh-proof-skeleton/blob/main/audit/verification-status.json) and appear on every published page via `<VerificationBanner />`.

## Explainer Design

### Concept-First, Name-Second ("Matryoshka") Reading Model

No mathematical term enters Level-1 prose without an immediate plain-language explanation:

1. **Inline plain-language reveal:** The first mention on a page explains the idea in concrete terms inside a highlighted span before revealing the formal name as a clickable link.
2. **Hover recall card:** Subsequent mentions link the term and open a concise recall card on hover or keyboard focus.
3. **Canonical Concept Page:** Clicking through opens the dedicated Concept Page with full context, anchored Lean declarations, canonical math sources, and an explicit *"Where this analogy breaks"* section.

### Three Simultaneous Zoom Levels

Every stage essay presents the same mathematical landmark at three parallel depths:

1. **Level 1 — First-Principles:** High-school calculus, vectors and matrices, determinants, basic complex numbers, and honestly typed black boxes for deeper prerequisites, paired with *"Where this analogy breaks."*
2. **Level 2 — Undergrad Bridge:** Concrete toy analogies, actual spaces, norms, sums, and identities connecting intuition to the proof mechanism.
3. **Level 3 — Formal Lean Contract:** Exact anchored Lean declaration signature, plain-math translation of hypotheses and conclusions, upstream file and line provenance, and verification status inherited from `audit/verification-status.json`.

## 8-Pillar Proof Roadmap

The explainer follows the 8 condensed pillars of the readable edition (`00_statement` through `07_congruences`) alongside the [Full Proof Journey & Alpöge 20-Step Map](https://foxfire1st.github.io/ar-rh-proof-skeleton/stages/00_statement/proof-journey):

- **Live now:** Stage `00_statement` essays ([`overview`](https://foxfire1st.github.io/ar-rh-proof-skeleton/stages/00_statement/overview), [`main-theorems`](https://foxfire1st.github.io/ar-rh-proof-skeleton/stages/00_statement/main-theorems), [`statement-definitions`](https://foxfire1st.github.io/ar-rh-proof-skeleton/stages/00_statement/statement-definitions), [`formal-conjectures`](https://foxfire1st.github.io/ar-rh-proof-skeleton/stages/00_statement/formal-conjectures)), [Full Proof Journey & Alpöge 20-Step Map](https://foxfire1st.github.io/ar-rh-proof-skeleton/stages/00_statement/proof-journey) (`proof-journey`), Stage `01_reduction` essays ([`overview`](https://foxfire1st.github.io/ar-rh-proof-skeleton/stages/01_reduction/overview), [`one-sided-criterion`](https://foxfire1st.github.io/ar-rh-proof-skeleton/stages/01_reduction/one-sided-criterion), [`main-inequality`](https://foxfire1st.github.io/ar-rh-proof-skeleton/stages/01_reduction/main-inequality), [`twisted-lattices`](https://foxfire1st.github.io/ar-rh-proof-skeleton/stages/01_reduction/twisted-lattices)), Stage `02_derivatives` essays ([`overview`](https://foxfire1st.github.io/ar-rh-proof-skeleton/stages/02_derivatives/overview), [`formal-group-and-divisibility`](https://foxfire1st.github.io/ar-rh-proof-skeleton/stages/02_derivatives/formal-group-and-divisibility), [`tight-frames-and-integral-vacuum`](https://foxfire1st.github.io/ar-rh-proof-skeleton/stages/02_derivatives/tight-frames-and-integral-vacuum), [`sharpness-bound-attained`](https://foxfire1st.github.io/ar-rh-proof-skeleton/stages/02_derivatives/sharpness-bound-attained)), [24 Concept Cards](https://foxfire1st.github.io/ar-rh-proof-skeleton/concepts/), 12 SVG diagrams, and 41 extracted Lean landmarks across all 8 pillars (3 + 6 + 5 + 5 + 5 + 6 + 5 + 6 = 41).
- **Upcoming:** Stage essays for `03_upper_bound` through `07_congruences` (Lean skeletons are extracted in [`rh-proof-skeleton/skeleton/`](https://github.com/Foxfire1st/rh-proof-skeleton/tree/main/skeleton); essays are upcoming).

| Stage | Pillar | Landmarks | Status |
| --- | --- | ---: | --- |
| `00_statement` | [Formal Statements & Challenge Contracts](https://foxfire1st.github.io/ar-rh-proof-skeleton/stages/00_statement/overview) (+ [Full Proof Journey & 20-Step Map](https://foxfire1st.github.io/ar-rh-proof-skeleton/stages/00_statement/proof-journey)) | 3 | Live (4 essays + 20-Step Map) |
| `01_reduction` | [The Theorem and Its Reduction](https://foxfire1st.github.io/ar-rh-proof-skeleton/stages/01_reduction/overview) | 6 | Live (4 essays) |
| `02_derivatives` | [Derivatives of Theta Functions](https://foxfire1st.github.io/ar-rh-proof-skeleton/stages/02_derivatives/overview) | 5 | Live (4 essays) |
| `03_upper_bound` | Upper Bound: Composite Theta Lift — Theorem A | 5 | Skeleton extracted; essays upcoming |
| `04_spanning` | Spanning: Second Moment — Theorem B | 5 | Skeleton extracted; essays upcoming |
| `05_determinant` | Lower Bound: The Determinant — Theorem C, Part 1 | 6 | Skeleton extracted; essays upcoming |
| `06_one_lift` | Lower Bound: One Lift — Theorem C, Part 2 | 5 | Skeleton extracted; essays upcoming |
| `07_congruences` | Lower Bound: Congruences & Galois Representations — Theorem C, Part 3 | 6 | Skeleton extracted; essays upcoming |

## Repository Layout

| Path | Responsibility |
| --- | --- |
| `docs/stages/` | Stage explainer essays and the Full Proof Journey, grouped by pillar (`00_statement/`, `01_reduction/`, and `02_derivatives/` live; `03_upper_bound/` through `07_congruences/` upcoming) |
| `docs/concepts/` | Canonical Concept Cards driving inline reveals, hover cards, and drill-down pages (24 live cards) |
| `docs/assets/` | SVG diagrams embedded across stage essays and Concept Cards (12 live diagrams) |
| `knowledge/` | Structured Agents Remember memory records (invariants, families, decisions, dependents, and weaknesses) |
| `onboarding/` | Module onboarding cards orienting contributors across code and documentation |
| `system/` | Authoring rules (`TREATMENT.md`), publication conventions (`task-publication.md`), and repository configuration |
| `.github/workflows/pages.yml` | GitHub Actions workflow validating this repository with `rh-proof-skeleton`, building VitePress, and deploying to GitHub Pages |

## Upstream Sources

- **Community Lean 4 Proof Repository:** [`leanprover-community/riemann-hypothesis`](https://github.com/leanprover-community/riemann-hypothesis)
- **Independent Replay Archive:** [`rohanarun/alpoge-rh-replay`](https://github.com/rohanarun/alpoge-rh-replay)
- **Paired Extraction & Site Pipeline:** [`Foxfire1st/rh-proof-skeleton`](https://github.com/Foxfire1st/rh-proof-skeleton)
