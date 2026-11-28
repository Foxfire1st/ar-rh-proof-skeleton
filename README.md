# ar-rh-proof-skeleton

Content, Concept Cards, diagrams, and Agents Remember memory repository for the **Riemann Hypothesis Proof Explainer**, documenting Levent Alpöge and Claude's 2026 Lean 4 proof of the Riemann Hypothesis and Generalized Riemann Hypothesis (GRH).

- **Live Explainer Site:** [foxfire1st.github.io/ar-rh-proof-skeleton](https://foxfire1st.github.io/ar-rh-proof-skeleton/)
- **Paired Pipeline Repository:** [Foxfire1st/rh-proof-skeleton](https://github.com/Foxfire1st/rh-proof-skeleton)
- **Quick Entry Points:** [Home](https://foxfire1st.github.io/ar-rh-proof-skeleton/) · [Full Proof Journey & 20-Step Map](https://foxfire1st.github.io/ar-rh-proof-skeleton/stages/00_statement/proof-journey) · [Stage 00 Overview](https://foxfire1st.github.io/ar-rh-proof-skeleton/stages/00_statement/overview) · [Stage 01 Overview](https://foxfire1st.github.io/ar-rh-proof-skeleton/stages/01_reduction/overview) · [Stage 02 Overview](https://foxfire1st.github.io/ar-rh-proof-skeleton/stages/02_derivatives/overview) · [Stage 03 Overview](https://foxfire1st.github.io/ar-rh-proof-skeleton/stages/03_upper_bound/overview) · [Stage 04 Overview](https://foxfire1st.github.io/ar-rh-proof-skeleton/stages/04_spanning/overview) · [Stage 05 Overview](https://foxfire1st.github.io/ar-rh-proof-skeleton/stages/05_determinant/overview) · [Stage 06 Overview](https://foxfire1st.github.io/ar-rh-proof-skeleton/stages/06_one_lift/overview) · [Stage 07 Overview](https://foxfire1st.github.io/ar-rh-proof-skeleton/stages/07_congruences/overview) · [Concept Cards](https://foxfire1st.github.io/ar-rh-proof-skeleton/concepts/)

> **Epistemic & Verification Status:** Local Lean compilation and Comparator replay are currently **`unverified-locally`**. This project does not claim that the upstream Lean 4 proof or Comparator checks have been verified locally. Deterministic upstream provenance, pinned commit metadata, and lexical static-scan counts (`scan_scope: entry` — `0` forbidden tokens across `src/Statements.lean` and `src/Main.lean`; `5` expected `sorry` tokens in root `Challenge.lean`) live in [`rh-proof-skeleton/audit/verification-status.json`](https://github.com/Foxfire1st/rh-proof-skeleton/blob/main/audit/verification-status.json) and appear on every published page via `<VerificationBanner />`.

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

- **Live now (all 8 pillars live):** Stage `00_statement` essays ([`overview`](https://foxfire1st.github.io/ar-rh-proof-skeleton/stages/00_statement/overview), [`main-theorems`](https://foxfire1st.github.io/ar-rh-proof-skeleton/stages/00_statement/main-theorems), [`statement-definitions`](https://foxfire1st.github.io/ar-rh-proof-skeleton/stages/00_statement/statement-definitions), [`formal-conjectures`](https://foxfire1st.github.io/ar-rh-proof-skeleton/stages/00_statement/formal-conjectures)), [Full Proof Journey & Alpöge 20-Step Map](https://foxfire1st.github.io/ar-rh-proof-skeleton/stages/00_statement/proof-journey) (`proof-journey`), Stage `01_reduction` essays ([`overview`](https://foxfire1st.github.io/ar-rh-proof-skeleton/stages/01_reduction/overview), [`one-sided-criterion`](https://foxfire1st.github.io/ar-rh-proof-skeleton/stages/01_reduction/one-sided-criterion), [`main-inequality`](https://foxfire1st.github.io/ar-rh-proof-skeleton/stages/01_reduction/main-inequality), [`twisted-lattices`](https://foxfire1st.github.io/ar-rh-proof-skeleton/stages/01_reduction/twisted-lattices)), Stage `02_derivatives` essays ([`overview`](https://foxfire1st.github.io/ar-rh-proof-skeleton/stages/02_derivatives/overview), [`formal-group-and-divisibility`](https://foxfire1st.github.io/ar-rh-proof-skeleton/stages/02_derivatives/formal-group-and-divisibility), [`tight-frames-and-integral-vacuum`](https://foxfire1st.github.io/ar-rh-proof-skeleton/stages/02_derivatives/tight-frames-and-integral-vacuum), [`sharpness-bound-attained`](https://foxfire1st.github.io/ar-rh-proof-skeleton/stages/02_derivatives/sharpness-bound-attained)), Stage `03_upper_bound` essays ([`overview`](https://foxfire1st.github.io/ar-rh-proof-skeleton/stages/03_upper_bound/overview), [`seesaw-identity`](https://foxfire1st.github.io/ar-rh-proof-skeleton/stages/03_upper_bound/seesaw-identity), [`short-columns-and-archimedean-size`](https://foxfire1st.github.io/ar-rh-proof-skeleton/stages/03_upper_bound/short-columns-and-archimedean-size), [`local-divisibility-and-elementary-divisors`](https://foxfire1st.github.io/ar-rh-proof-skeleton/stages/03_upper_bound/local-divisibility-and-elementary-divisors)), Stage `04_spanning` essays ([`overview`](https://foxfire1st.github.io/ar-rh-proof-skeleton/stages/04_spanning/overview), [`equal-multiplicities-and-spanning`](https://foxfire1st.github.io/ar-rh-proof-skeleton/stages/04_spanning/equal-multiplicities-and-spanning), [`unfolding-and-second-moments`](https://foxfire1st.github.io/ar-rh-proof-skeleton/stages/04_spanning/unfolding-and-second-moments), [`main-term-sieve-and-remaining-terms`](https://foxfire1st.github.io/ar-rh-proof-skeleton/stages/04_spanning/main-term-sieve-and-remaining-terms)), Stage `05_determinant` essays ([`overview`](https://foxfire1st.github.io/ar-rh-proof-skeleton/stages/05_determinant/overview), [`exchange-and-one-square`](https://foxfire1st.github.io/ar-rh-proof-skeleton/stages/05_determinant/exchange-and-one-square), [`chain-determinant-and-auxiliary-forms`](https://foxfire1st.github.io/ar-rh-proof-skeleton/stages/05_determinant/chain-determinant-and-auxiliary-forms), [`regular-twists-and-theorem-c`](https://foxfire1st.github.io/ar-rh-proof-skeleton/stages/05_determinant/regular-twists-and-theorem-c)), Stage `06_one_lift` essays ([`overview`](https://foxfire1st.github.io/ar-rh-proof-skeleton/stages/06_one_lift/overview), [`three-exchanges-and-gluing-maps`](https://foxfire1st.github.io/ar-rh-proof-skeleton/stages/06_one_lift/three-exchanges-and-gluing-maps), [`false-lifts-and-twisted-l-values`](https://foxfire1st.github.io/ar-rh-proof-skeleton/stages/06_one_lift/false-lifts-and-twisted-l-values), [`lift-volume-height`](https://foxfire1st.github.io/ar-rh-proof-skeleton/stages/06_one_lift/lift-volume-height)), Stage `07_congruences` essays ([`overview`](https://foxfire1st.github.io/ar-rh-proof-skeleton/stages/07_congruences/overview), [`crystalline-lifting-and-local-defect`](https://foxfire1st.github.io/ar-rh-proof-skeleton/stages/07_congruences/crystalline-lifting-and-local-defect), [`main-summands-and-twisting-theorem`](https://foxfire1st.github.io/ar-rh-proof-skeleton/stages/07_congruences/main-summands-and-twisting-theorem), [`ribet-denominators-and-regular-twists`](https://foxfire1st.github.io/ar-rh-proof-skeleton/stages/07_congruences/ribet-denominators-and-regular-twists)), [54 Concept Cards](https://foxfire1st.github.io/ar-rh-proof-skeleton/concepts/), 27 SVG diagrams, and 41 extracted Lean landmarks across all 8 pillars (3 + 6 + 5 + 5 + 5 + 6 + 5 + 6 = 41).
- **Status:** All 8 pillars of the proof (`00_statement` through `07_congruences`) are now live across 33 stage essays, while upstream freshness triage and future platform enhancements continue.

| Stage | Pillar | Landmarks | Status |
| --- | --- | ---: | --- |
| `00_statement` | [Formal Statements & Challenge Contracts](https://foxfire1st.github.io/ar-rh-proof-skeleton/stages/00_statement/overview) (+ [Full Proof Journey & 20-Step Map](https://foxfire1st.github.io/ar-rh-proof-skeleton/stages/00_statement/proof-journey)) | 3 | Live (4 essays + 20-Step Map) |
| `01_reduction` | [The Theorem and Its Reduction](https://foxfire1st.github.io/ar-rh-proof-skeleton/stages/01_reduction/overview) | 6 | Live (4 essays) |
| `02_derivatives` | [Derivatives of Theta Functions](https://foxfire1st.github.io/ar-rh-proof-skeleton/stages/02_derivatives/overview) | 5 | Live (4 essays) |
| `03_upper_bound` | [Upper Bound: Composite Theta Lift — Theorem A](https://foxfire1st.github.io/ar-rh-proof-skeleton/stages/03_upper_bound/overview) | 5 | Live (4 essays) |
| `04_spanning` | [Spanning: Second Moment — Theorem B](https://foxfire1st.github.io/ar-rh-proof-skeleton/stages/04_spanning/overview) | 5 | Live (4 essays) |
| `05_determinant` | [Lower Bound: The Determinant — Theorem C, Part 1](https://foxfire1st.github.io/ar-rh-proof-skeleton/stages/05_determinant/overview) | 6 | Live (4 essays) |
| `06_one_lift` | [Lower Bound: One Lift — Theorem C, Part 2](https://foxfire1st.github.io/ar-rh-proof-skeleton/stages/06_one_lift/overview) | 5 | Live (4 essays) |
| `07_congruences` | [Lower Bound: Congruences & Galois Representations — Theorem C, Part 3](https://foxfire1st.github.io/ar-rh-proof-skeleton/stages/07_congruences/overview) | 6 | Live (4 essays) |

## Repository Layout

| Path | Responsibility |
| --- | --- |
| `docs/stages/` | Stage explainer essays and the Full Proof Journey, grouped by pillar (`00_statement/` through `07_congruences/` — all 8 pillars live) |
| `docs/concepts/` | Canonical Concept Cards driving inline reveals, hover cards, and drill-down pages (54 live cards) |
| `docs/assets/` | SVG diagrams embedded across stage essays and Concept Cards (27 live diagrams) |
| `knowledge/` | Structured Agents Remember memory records (invariants, families, decisions, dependents, and weaknesses) |
| `onboarding/` | Module onboarding cards orienting contributors across code and documentation |
| `system/` | Authoring rules (`TREATMENT.md`), publication conventions (`task-publication.md`), and repository configuration |
| `.github/workflows/pages.yml` | GitHub Actions workflow validating this repository with `rh-proof-skeleton`, building VitePress, and deploying to GitHub Pages |

## Upstream Sources

- **Community Lean 4 Proof Repository:** [`leanprover-community/riemann-hypothesis`](https://github.com/leanprover-community/riemann-hypothesis)
- **Independent Replay Archive:** [`rohanarun/alpoge-rh-replay`](https://github.com/rohanarun/alpoge-rh-replay)
- **Paired Extraction & Site Pipeline:** [`Foxfire1st/rh-proof-skeleton`](https://github.com/Foxfire1st/rh-proof-skeleton)
