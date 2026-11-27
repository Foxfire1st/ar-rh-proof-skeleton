---
slug: kummer-chart-cup-orthogonality
term: Kummer chart cup orthogonality
aliases: ["Kummer chart", "cup exponent", "cup orthogonality", "pairing side", "Frobenius safe-move walk", "tame inertial chart"]
inline_glimpse: "Bounding the power p^e that pushes crystalline cocycles of a p-adic Galois lattice L into the cup-product annihilator of congruent dual lattices across four weight/prime regimes using Kisin-module Frobenius-orbit walks on a Kummer chart."
visual_ref: assets/stage07-kummer-chart-defect-regimes.svg
where_analogy_breaks: "Thinking of Kummer chart cup orthogonality as a classical Fontaine–Laffaille exactness theorem (which requires all Hodge–Tate weights to stay below p - 1) misses the whole point of Lifting/CupOrthogonalityBounds.lean: Stage 07's far-window weights reach b = a/x, which vastly exceeds small and medium primes p, so exact vanishing (dualDefect = 0) fails below b and must be replaced by explicit cup exponents built from Frobenius-orbit safe-move walks and Behrend-sparse b-letters."
sources:
  - title: "On the lifting of crystalline cohomology classes"
    citation: "A. Iovita & A. Marmora, Adv. Math. 270 (2015), 255–290."
    kind: paper
  - title: "Loose crystalline lifts and overconvergence of étale (φ, τ)-modules"
    citation: "H. Gao & T. Liu, Compos. Math. 160 (2024), 835–872."
    kind: paper
  - title: "Local models for Galois deformation rings and applications"
    citation: "D. Le, B. V. Le Hung, B. Levin & S. Morra, Invent. Math. 231 (2023), 1277–1488."
    kind: paper
lean_anchors:
  - 07_congruences/cup_orthogonality_bounds.lean#New.Lift.KummerChart.universalSide
  - 07_congruences/cup_orthogonality_bounds.lean#New.Lift.KummerChart.dualDefect_eq_zero_and_dualDefect_le
  - 07_congruences/cup_orthogonality_bounds.lean#New.Lift.KummerChart.hasCupExponent_any_pattern
  - 07_congruences/cup_orthogonality_bounds.lean#New.Lift.KummerChart.hasCupExponent_sparse_thick
  - 07_congruences/cup_orthogonality_bounds.lean#New.Lift.hasCupExponent_of_isTameType
  - 07_congruences/cup_orthogonality_bounds.lean#New.Lift.IsTameType
child_concepts:
  - prime-number
  - arithmetic-height
  - frobenius-cycle
  - galois-main-summand
---

# Kummer Chart Cup Orthogonality

## Level 1 — First-Principles

In Ribet's method, congruences between modular forms produce *torsion* Galois cocycles modulo $\ell^M$. To compare those torsion cocycles against a global crystalline Selmer group, Stage 07 needs to know how closely a torsion cocycle lifts to a true crystalline cocycle above a {{concept:prime-number|prime}} $p$ (note that upstream `New/Congruences/Lifting/` uses "lifting" strictly for this $p$-adic Hodge / Kisin-module lifting problem, never for theta lifting).

When all Hodge–Tate weights of a Galois lattice are smaller than $p - 1$, classical integral $p$-adic Hodge theory says there is zero obstruction: every crystalline cocycle of the dual lattice is automatically orthogonal under local cup products to every crystalline cocycle of congruent lattices. In this proof, however, the {{concept:galois-main-summand|Hodge–Tate jump windows}} of a main summand sit out near $b = a/x$, far larger than small and medium primes $p$.

**Kummer chart cup orthogonality** (`Lifting/CupOrthogonalityBounds.lean`) measures this obstruction quantitatively by transferring Galois lattices onto a **Kummer chart** $k$—a power-series ring $W(\mathbb{F}_{p^f})\llbracket z \rrbracket \subset A_{\mathrm{inf}}$ adapted to $\Gamma \le G_p$, whose finite-field Teichmüller "letters" $i \in k.\mathrm{Letter}$ cycle under the $p$-th-power map `letterFrob : i ↦ i^p` (much like Stage 03's {{concept:frobenius-cycle|cyclic Frobenius orbits}}). Packaging a lattice $L$, its crystalline cocycles $Z_f(L)$, and its letter-by-letter jump sets into a **pairing side** (`KummerChart.universalSide`) lets Kisin-module **Frobenius safe-move walks** (`SafeMoves`, `CutExponentBounds`, `FewLowJumps`) bound the smallest power $p^e$—the **cup exponent** (`HasCupExponent`)—for which $p^e x$ lies in `cupOrthogonal` across four exact regimes:

1. **Generic large primes with no small negative jump:** For $p \ge 5$ avoiding multiples near $0$ and $\pm b_i$, if $U, U'$ avoid $[-a, 0)$ (or $U' \subseteq [-\eta, 0]$), then `dualDefect = 0`—exact **cup orthogonality** with zero loss.
2. **Few low jumps:** Under the same generic separation, if $L$ has at most $m$ low jumps in $[-\eta, \eta]$, `dualDefect` is at most $11 f (m(a + 1) + \mathrm{rank}\,L\cdot(W + 1))\,\mathrm{len}(O/p)$.
3. **Two-size far windows (general and Behrend-sparse):** For smaller primes ($p \ge 7$ or constant CM sign) with far windows at $b$ on `b`-letters and $G$ elsewhere, any pattern gives **cup exponent** $10^4(K + \lfloor\log_p(2(b+M))\rfloor)$ (`hasCupExponent_any_pattern`), while forbidding three `b`-letters along a Frobenius orbit with both gaps in $[1, J]$—directly consuming Stage 01's Behrend sparsity of `D.bPlaces`!—drops the **cup exponent** to the rank-independent bound $10^3(\delta + (J + 1)M)$ (`hasCupExponent_sparse_thick`).
4. **Tame inertial types at level primes (`IsTameType`, `hasCupExponent_of_isTameType`):** At the level primes $p_0, p_1$, inertia acts through a **tame inertial chart** (`TameChart`) of ramification index $e = \mathrm{lcm}_{1 \le i \le r}(p^i - 1) \le p^{r^2}$. Descending along the chart produces a **cup exponent** $3 r^2(N + 1)^2 + 150(\eta + W + 1)$ completely independent of $e$!

### Limits of the intuition

Thinking of the **cup exponent** as a single uniform formula misses how sharply the four regimes trade off prime size against weight geometry. At tiny primes $\ell \le kt$, general patterns would blow up the {{concept:arithmetic-height|valuation budget}} unless three consecutive `b`-letters along `letterFrob` are excluded—that is the exact downstream reason Stage 01 built `D.bPlaces` via Behrend's progression-free sphere construction!

## Level 2 — Undergrad Bridge

`src/New/Congruences/Lifting/CupOrthogonalityBounds.lean` (25 declarations, 0 stub truncations) translates Kisin-module bounds on `PairingSide`s into lattice-level cup-orthogonality theorems:

1. **Sides, letter jumps, and opposite windows (`universalSide`, `mem_letterJumps`, `fitsWindows_letterJumps`, `FitsWindows.neg_add_le_of_opposite`, `sum_card_algHom_le_finrank`, `pow_smul_mem_cupOrthogonal_of_isScaledCupCoboundary`):**
   Using a root `ζ : k.LetterRoot O` ($\zeta^{p^f - 1} = 1$), each embedding $g : O \to \mathbb{C}_p$ maps to at most one letter $i$ via $g(\zeta) = \theta(\omega(i))$. If `U` and `U'` sit in opposite windows $[-\eta, \eta] \cup \mp \varepsilon_i [b_i, b_i + W]$, Tate-dual tensor jumps `-(v + v') - 1` stay bounded by $\max(2\eta, b_i + W + \eta)$.
2. **Generic primes & few low jumps (`crysCocycles_le_cupOrthogonal_of_no_small_neg_jump`, `dualDefect_le_of_few_low_weights`, `dualDefect_eq_zero_and_dualDefect_le`):**
   For unramified $k$, $p \ge 5$, $\eta \ge 1$, $b_i + W + \eta \le (p - 3)a$, $6(a + \eta + W) < b_i$, and `NoMultipleNear p (6(a + η + W)) (b i)`:
   - **(a)** `dualDefect (InClass ⊤ ⊥ U) L = 0` if $U, U'$ avoid $[-a, 0)$ or $U' \subseteq [-\eta, 0]$;
   - **(b)** `dualDefect ≤ 11 * k.f * (m * (a + 1) + finrank O L * (W + 1)) * length O (O / p)` when $\sum_{u \in [-\eta, \eta]} \mathrm{jump}(L, g, u) \le m$.
3. **Two-size far windows (`hasCupExponent_any_pattern`, `hasCupExponent_sparse_thick`):**
   - Arbitrary `IsBLetter` ($p \ge 7$, $V + 4\eta \le M$, $0 < G \le b < K p^2$, $2G < K p$, $M \le K$): `HasCupExponent (InClass ⊤ ⊥ U) L (10^4 * (K + p.log (2 * (b + M))))`.
   - Sparse `IsBLetter` ($G + V \le \eta$, $J \ge 1$, no $i$ with `IsBLetter` at $i$, $\varphi^s(i)$, $\varphi^{s+t}(i)$ for $s, t \in [1, J]$, plus depth $2e(b + 5\eta) \le p^{J+1}\delta$ and $20(J + 1 + d)\eta + \lfloor\log_p(2(b + 5\eta))\rfloor \le M$): `HasCupExponent (InClass ⊤ ⊥ U) L (10^3 * (δ + (J + 1) * M))`.
4. **Tame inertial descent (`IsTameType`, `lcm_pow_sub_one_le`, `TameChart.inClass_of_pow_sub_one`, `exists_tameChart_e_eq_lcm`, `hasCupExponent_of_isTameType`):**
   For $p > b + 6(W + \eta) + r^2(N + 1)^2 + 2$ and inertia killed by $X^e - 1$ ($e = \mathrm{lcm}_{1 \le i \le r}(p^i - 1) \le p^{r^2}$) and a monic degree-$\le N$ polynomial over $\mathbb{Z}_p$, descent along `TameChart` yields `HasCupExponent (InClass F K U) L (3 * r^2 * (N + 1)^2 + 150 * (η + W + 1))`.

## Level 3 — Formal Lean Contract

All six anchors extract completely untruncated in `skeleton/07_congruences/cup_orthogonality_bounds.lean` (25 declarations, 0 stub truncations):

- `07_congruences/cup_orthogonality_bounds.lean#New.Lift.IsTameType` (upstream `CupOrthogonalityBounds.lean:76-80`).
- `07_congruences/cup_orthogonality_bounds.lean#New.Lift.KummerChart.universalSide` (upstream `CupOrthogonalityBounds.lean:89-106`).
- `07_congruences/cup_orthogonality_bounds.lean#New.Lift.KummerChart.dualDefect_eq_zero_and_dualDefect_le` (upstream `CupOrthogonalityBounds.lean:346-376`).
- `07_congruences/cup_orthogonality_bounds.lean#New.Lift.KummerChart.hasCupExponent_any_pattern` (upstream `CupOrthogonalityBounds.lean:378-397`).
- `07_congruences/cup_orthogonality_bounds.lean#New.Lift.KummerChart.hasCupExponent_sparse_thick` (upstream `CupOrthogonalityBounds.lean:399-429`).
- `07_congruences/cup_orthogonality_bounds.lean#New.Lift.hasCupExponent_of_isTameType` (upstream `CupOrthogonalityBounds.lean:513-589`):
  ```lean
  lemma New.Lift.hasCupExponent_of_isTameType [TopologicalSpace Γ] (ho : IsOpenEmbedding Γ.overGp.toGp)
      (h_surj : Surjective Γ.overGp.toGp) {F : Filter Γ} {K : Γ → O[X] → Prop}
      (hL : InClass F (fun s : Γ ↦ K s⁻¹) U' L)
      (hK : ∀ s : Γ, IsWeilPair (toGp s) 0 → IsTameType p r N (K s)) {η W b : ℕ} {ε : ℤˣ}
      (hU : ∀ g : O →ₐ[ℤ_[p]] ℂ_[p], FitsWindows η (-ε) b W (U g))
      (hU' : ∀ g : O →ₐ[ℤ_[p]] ℂ_[p], FitsWindows η ε b W (U' g))
      (hp : b + 6 * (W + η) + r ^ 2 * (N + 1) ^ 2 + 2 < p) :
      HasCupExponent (InClass F K U) L (3 * (r ^ 2 * (N + 1) ^ 2) + 150 * (η + W + 1))
  ```

See the page's live `<VerificationBanner />` (`audit/verification-status.json`) for the pinned upstream commit and local Lean build status (`build_status: unverified-locally`).
