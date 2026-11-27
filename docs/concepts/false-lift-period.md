---
slug: false-lift-period
term: false-lift period
aliases: ["hyperbolic-plane descent tower", "small-root Hecke vanishing", "hyperplane sum", "false-lift orthogonality"]
inline_glimpse: "The theta period over a complementary block I_l of an algebraic form Xi that mimics a lift in level and Hecke eigenvalue yet sits orthogonal to true lifts—forced to vanish by climbing a hyperbolic-plane descent tower when 2r + 2N + 3 < n."
visual_ref: assets/stage06-false-lifts-and-rank-one-blocks.svg
where_analogy_breaks: "Thinking of a false lift as an ordinary orthogonal error term whose projections automatically disappear misses why three separate annihilators and a strict rank gap are required: without the hyperplane sums at chosen places, nontrivial tame characters at twist places, small-root Hecke vanishing at a split place w, and the strict inequality 2r + 2N + 3 < n, a form in the kernel of theta descent to W could still have nonzero theta descents higher up the tower W + H^N."
sources:
  - title: "On the Howe duality conjecture"
    citation: "S. Rallis, Compositio Math. 51 (1984), 333–399."
    kind: paper
  - title: "Poles of L-functions and theta liftings for orthogonal groups"
    citation: "D. Ginzburg, D. Jiang & D. Soudry, J. Inst. Math. Jussieu 8 (2009), 693–741."
    kind: paper
  - title: "Poles of Eisenstein series and theta lifts for unitary groups"
    citation: "C. Wu, Pacific J. Math. 317 (2022), 209–244."
    kind: paper
lean_anchors:
  - 06_one_lift/periods_of_false_lifts.lean#New.Rows.descentsVanish_hyperbolicSum
  - 06_one_lift/periods_of_false_lifts.lean#New.Rows.integral_conj_pairTheta_mul_algFormCoeff_eq_zero
  - 06_one_lift/periods_of_false_lifts.lean#New.Rows.stdClassSum_gramForm_eq_zero
  - 06_one_lift/periods_of_false_lifts.lean#New.Rows.stdClassSum_period_eq_zero
  - 06_one_lift/periods_of_false_lifts.lean#New.Rows.DividedSplitting.stdClassSum_inner_productForm_eq_zero
child_concepts:
  - theta-correspondence
  - hecke-operator
  - central-idempotent
---

# False-Lift Period

## Level 1 — First-Principles

Inside Stage 06, we work on a definite unitary space $I_n$ and want to isolate the subspace spanned by true theta lifts from a smaller space $W$ (rank $r < n$). When you project an arbitrary form back onto that lift subspace, unwanted interference can come from a **false lift**: a form $\Xi$ on $U(I_n)$ that looks indistinguishable from a true lift under local tests—it sits inside the image of the {{concept:central-idempotent|good finite-field projection}} $e_0$ and shares the same split-place Hecke eigenvalue as a lift—yet is completely orthogonal to every actual lift from $W$.

Formally, a false lift $\Xi$ is annihilated by three operators simultaneously:
1. **Hyperplane sums at chosen places (`HyperplaneSumsAnnihilate`):** For every nonzero row vector $\lambda_v$ at a chosen place $v$, the group-algebra sum $\sum \chi_v(\det s)^{-1} s$ over invertible matrices $s = 1 + u\lambda_v$ sends $\Xi$ to $0$.
2. **Theta descent to $W$ (`O.normalizedDescent Ξ = 0`):** Because $\Xi$ is orthogonal to all lifts from $W$, pairing $\Xi$ back down to $U(W)$ via {{concept:theta-correspondence|the dual-pair theta kernel}} vanishes identically.
3. **Small-root Hecke vanishing (`heckePolyOnForms`):** At a split place $w$ off the level (of residue cardinality $q$), the polynomial built from {{concept:hecke-operator|unramified local averaging operators}} evaluated at the small root `liftHeckeRoot w` (of modulus $q^{1 - n + r/2}$) kills $\Xi$.

Why does Stage 06 care about a **false-lift period**? When we split $I_n = I_N \oplus I_l$ into a small block $I_N$ and a large complementary block $I_l$, both the gluing maps (in `GluingMaps.lean`) and the test forms built for rank-one character lifts (in `TwistedValueHeights.lean`) pair forms on $I_n$ against theta kernels over $U(I_l)$. To prove **false-lift orthogonality**—that our constructed forms never mix with false lifts—Stage 06 shows that every such **false-lift period** over $U(I_l)$ is strictly zero.

It proves this by climbing a **hyperbolic-plane descent tower**:
$$W \;\longrightarrow\; W \oplus \mathbb{H} \;\longrightarrow\; W \oplus \mathbb{H}^2 \;\longrightarrow\; \cdots \;\longrightarrow\; W \oplus \mathbb{H}^N.$$
At the bottom rung ($N = 0$), the **hyperplane sum** annihilators upgrade vanishing of one harmonic descent (`normalizedDescent Ξ = 0`) to vanishing of *all* theta descents to $W$. At each rung upward, **small-root Hecke vanishing** forces any nonzero descent on $W \oplus \mathbb{H}^{k+1}$ to fail boundedness, collapsing it to zero as long as the strict rank condition
$$2r + 2N + 3 < n$$
holds (and the tame character is nontrivial at every twist place). Once all descents to $W \oplus \mathbb{H}^N$ vanish (`descentsVanish_hyperbolicSum`), taking a Fourier coefficient kills the complementary theta integral over $U(I_l)$, forcing the discrete class sum (`stdClassSum_gramForm_eq_zero`) to vanish identically.

### Limits of the intuition

Picturing the **hyperbolic-plane descent tower** as simple mathematical induction hides why the strict rank margin $2r + 2N + 3 < n$ and nontriviality of `O.tamePart` at every twist place cannot be relaxed. The induction step (`HyperbolicPlaneStep.lean`) compares the exponent $1 - n + r/2$ in `liftHeckeRoot w` against the growth rate of matrix coefficients on $W \oplus \mathbb{H}^{k+1}$; once $2r + 2N + 3$ reaches $n$, small-root Hecke vanishing no longer forces bounded descents to vanish, and false lifts can begin to carry nonzero periods over $U(I_l)$.

## Level 2 — Undergrad Bridge

In Rallis (1984), Ginzburg–Jiang–Soudry (2009), and Wu (2022), the first occurrence index of a representation in a theta tower governs the vanishing of automorphic periods on orthogonal and unitary subgroups. `src/New/OneLift/FalseLifts/PeriodsOfFalseLifts.lean` (13 declarations, 1 `let`-type stub truncation) turns this principle into an explicit class-sum orthogonality machine in five steps:

1. **Bottom of the tower (`lift_polyVector_blockDatum_eq_zero` + `BottomOfTower.lean`):**
   From `O.normalizedDescent Ξ = 0` and archimedean parameter compatibility `O.FitsParams l₀`, convolving with real test functions spreads vanishing from the highest-weight harmonic vector to every Fock polynomial `polyVector P` against `O.blockDatum`. Combined with `HyperplaneSumsAnnihilate Ξ` (`descentsVanish_of_hyperplaneSumsAnnihilate`), this kills all theta descents of `algFormCoeff` to $W$ for data that are the vacuum off `O.places.conjClosure`.
2. **Climbing the hyperbolic-plane tower (`descentsVanish_hyperbolicSum`):**
   Applying `HyperbolicPlaneStep.lean` (`descentsVanish_add_planes`) inductively adds $N$ hyperbolic planes as long as $2\,\mathrm{card}\,r + 2\,\mathrm{card}\,N + 3 < \mathrm{card}\,n$ and `O.tamePart (O.twistPlace i) ≠ 1` for every twist place, proving `DescentsVanish (hyperbolicSum N O.W) (stdSpace E n)` for every vector $\zeta \in \mathrm{WeylIndex}\ n\ O.\mathrm{columnCount} \to \mathbb{C}$.
3. **From vanishing descents to vanishing block integrals (`integral_conj_pairTheta_mul_algFormCoeff_eq_zero`):**
   Given a splitting $\varepsilon : N \oplus l \simeq n$ of $I_n = I_N \oplus I_l$, absorbing the theta kernel of $(W, I_l)$ into the descent to $W \oplus \mathbb{H}^N$ via Fourier coefficients (`integral_blockEmb_eq_zero_of_descentsVanish`) kills
   $$\int_{[U(I_l)]} \overline{\theta_{W, I_l}(\varphi; g, h_2)}\,\mathrm{algFormCoeff}(\zeta, \Xi)\bigl((h_1, h_2)\gamma\bigr)\,dh_2 = 0.$$
4. **Discretization to finite class sums (`stdClassSum_gramForm_eq_zero`, `stdClassSum_gramForm_kernelVector_eq_zero`):**
   For any conjugate-linear $U(I_l)_\infty$-equivariant map $C$ and kernel-coordinate vector $\vartheta(h)$ (in particular `kernelVector`), integrating over the compact group $U(I_l)_\infty$ collapses the adelic quotient integral into a weighted finite double-coset sum `stdClassSum K f = 0` for every stabilizing level $K$.
5. **Two downstream consumers (`stdClassSum_period_eq_zero`, `DividedSplitting.stdClassSum_inner_productForm_eq_zero`):**
   - For **rank-one lifts** ($r = 1$, `hyperplaneSum_apply_eq_zero_of_rank_one`), `stdClassSum_period_eq_zero` proves that class sums of point-form pairings vanish against false lifts—used directly in `TwistedValueHeights.lean::isTestForm_trivialLiftClassSum_pointForm`.
   - For **divided splittings** ($V_j = V'' \oplus V^\circ$), `DividedSplitting.stdClassSum_inner_productForm_eq_zero` proves that $V^\circ$-slices of false lifts against `productForm` vanish—used directly in `GluingMaps.lean::sum_slice_eq_zero_of_mem_falseLiftsAt`.

## Level 3 — Formal Lean Contract

All five anchors live in `skeleton/06_one_lift/periods_of_false_lifts.lean` (13 declarations; 1 declaration—`New.Rows.stdClassSum_gramForm_eq_zero`—is truncated at `let f (h : Gf E l) : ℂ :=` in the skeleton, with its full upstream statement from `PeriodsOfFalseLifts.lean:326-345` quoted verbatim in [Vanishing Periods of False Lifts and Negative Heights of Twisted L-Values](../stages/06_one_lift/false-lifts-and-twisted-l-values.md) and its complete conclusion from `PeriodsOfFalseLifts.lean:343-345` excerpted below):

- `06_one_lift/periods_of_false_lifts.lean#New.Rows.descentsVanish_hyperbolicSum` (upstream `PeriodsOfFalseLifts.lean:115-185`):
  ```lean
  lemma New.Rows.descentsVanish_hyperbolicSum {l₀ : NumberField.InfinitePlace E → r → ℤ} [Nonempty r]
      (h_fits : O.FitsParams l₀) (h_ramified : ∀ i : O.TwistIndex, O.tamePart (O.twistPlace i) ≠ 1)
      {Ξ : O.FormV} (h_hyperplane : O.HyperplaneSumsAnnihilate Ξ)
      (h_theta : O.normalizedDescent Ξ = 0) {w : FinPlace E} {hw : star w ≠ w}
      {hw_off : w ∉ O.places.conjClosure}
      (h_hecke : O.heckePolyOnForms hw hw_off (O.liftHeckeRoot w) Ξ = 0)
      (h_rank : 2 * card r + 2 * card N + 3 < card n) (ζ : WeylIndex n O.columnCount → ℂ) :
      DescentsVanish (hyperbolicSum N O.W) (stdSpace E n) (O.charW * upsilonU1 ^ card N)
        O.places.conjClosureᶜ (star (algFormCoeff 1 (gramForm ζ) Ξ.val))
  ```
- `06_one_lift/periods_of_false_lifts.lean#New.Rows.integral_conj_pairTheta_mul_algFormCoeff_eq_zero` (upstream `PeriodsOfFalseLifts.lean:214-310`).
- `06_one_lift/periods_of_false_lifts.lean#New.Rows.stdClassSum_gramForm_eq_zero` (complete conclusion from upstream `PeriodsOfFalseLifts.lean:343-345`):
  ```lean
  let f (h : Gf E l) : ℂ := gramForm (C (ϑ h)) (Ξ (U.block ε (h₁, h) * x))
  (∀ δ ∈ (stdSpace E l).Γ, ∀ h : Gf E l, f (δ.2 * h) = f h) ∧
    ∀ (K : Subgroup (Gf E l)) [IsLevel K], K ≤ rightStabilizer f → stdClassSum K f = 0
  ```
- `06_one_lift/periods_of_false_lifts.lean#New.Rows.stdClassSum_period_eq_zero` (upstream `PeriodsOfFalseLifts.lean:572-635`) and `06_one_lift/periods_of_false_lifts.lean#New.Rows.DividedSplitting.stdClassSum_inner_productForm_eq_zero` (upstream `PeriodsOfFalseLifts.lean:676-735`).

See the page's live `<VerificationBanner />` (`audit/verification-status.json`) for the pinned upstream commit and local Lean build status (`build_status: unverified-locally`).
