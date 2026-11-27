---
slug: auxiliary-spanning-form
term: auxiliary spanning form
aliases: ["auxiliary form", "two-grid path", "diagonal turn", "last-path period", "delta form", "completion cost"]
inline_glimpse: "A target form whose m-step lifts stay p-integral (after a shared twist scalar) and bounded in archimedean norm by exp(2*x_1^2*b^2), completing an almost-spanning family to a full basis of Y_0 at total cost at most d*b."
visual_ref: assets/stage05-triangle-and-two-grid-turns.svg
where_analogy_breaks: "Thinking of auxiliary spanning forms as just picking any cheap basis vectors on Y_0 misses the fundamental obstacle: Y_0 is indefinite, so you cannot read off short integral forms directly on Y_0 or control their lifts across every twist. Stage 05 must pull back short deep-level forms from the definite space Ytilde^m_0 and push their pairings across m^2 off-diagonal and diagonal turns of two square grids—paying a tiny factor b^{-2} times x_1^2 b^2 per missing basis vector only because Theorem B already leaves at most 4d/b^2 dimensions uncovered."
sources:
  - title: "Périodes et isogénies des variétés abéliennes sur les corps de nombres"
    citation: "J.-B. Bost, Astérisque 237 (1996), Exp. 795, 115–161."
    kind: paper
  - title: "Heights in Diophantine Geometry"
    citation: "E. Bombieri & W. Gubler, New Mathematical Monographs 4, Cambridge Univ. Press (2006)."
    kind: book
  - title: "Symplectic local root numbers, central critical L-values, and restriction problems in the representation theory of classical groups"
    citation: "W. T. Gan, B. H. Gross & D. Prasad, Astérisque 346 (2012), 1–109."
    kind: paper
lean_anchors:
  - 05_determinant/long_forms.lean#New.Data.IsAuxiliaryForm
  - 05_determinant/long_forms.lean#New.Data.exists_span_isAuxiliaryForm_eq_top
  - 05_determinant/long_forms.lean#New.Data.mul_completion_cost_le
  - 05_determinant/spanning_forms.lean#New.Data.exists_span_short_lifts_eq_top
  - 05_determinant/spanning_forms.lean#New.Data.exists_dualPairing_eq_mul_lastPathPeriod
child_concepts:
  - twisted-lattice
  - weyl-gauge
  - theta-square-law
  - seesaw-duality
  - hecke-l-value-ratio
  - central-idempotent
---

# Auxiliary Spanning Form

## Level 1 — First-Principles

In Stage 01's {{concept:twisted-lattice|twisted-lattice inequality}} (`TwistedLattice.covolume_add_index_nonneg`), the {{concept:arithmetic-height|global balance law equating archimedean size and prime-by-prime valuations}} applies only to a **full basis** of the $d$-dimensional target space $D.\mathrm{targetForms}$. Yet Theorem B (Stage 04) does not span 100% of $D.\mathrm{targetForms}$—it spans at least $(1 - 4/b^2) d$ dimensions, leaving up to $n' = d - n \le 4 d / b^2$ missing directions. To close the gap without destroying the lower bound, Stage 05 needs a spanning supply of **auxiliary spanning forms** (`New.Data.IsAuxiliaryForm`) that can fill in any $n'$ missing basis slots cleanly.

What makes a target form $y \in D.\mathrm{targetForms}$ a good **auxiliary form**? Two properties, controlled simultaneously across all twists $\vartheta$:
1. **Uniform integrality up to one shared twist scalar:** There is a single scalar function $\gamma(\vartheta) \neq 0$ (with $\gamma(1) = 1$, independent of $y$!) such that $\gamma(\vartheta) \cdot \Theta_\vartheta(y)$ is $\ell$-integral at every prime $\ell \nmid \mathrm{Nm}(Q(\vartheta))$.
2. **Moderate untwisted archimedean size:** Every Galois conjugate of $\Theta_1(y)$ has squared norm at most $\exp(2 x_1^2 b^2)$.

At first sight, $2 x_1^2 b^2$ looks huge—far larger than $b$! Why does it not ruin Theorem C? Because each of the $n'$ missing basis vectors pays a **completion cost** of $2 x_1^2 b^2 + b$ (the archimedean size plus the height cost $b$ of $|\gamma(\theta_\iota)|^{-2}$), and Theorem B guarantees $n' \le 4d / b^2$. Multiplying cancels $b^2$ identically:
$$n' \cdot \bigl(2 x_1^2 b^2 + b\bigr) \le \frac{4d}{b^2}\bigl(2 x_1^2 b^2 + b\bigr) = d \cdot \bigl(8 x_1^2 + 4 b^{-1}\bigr) \le d \cdot b,$$
since $x_1^{10^6} \le b$ (`mul_completion_cost_le`). The tiny fraction $4/b^2$ of uncovered dimensions turns an $O(x_1^2 b^2)$ per-vector cost into a negligible $d b$ total error!

How does Stage 05 *build* a full spanning set of **auxiliary spanning forms** on the indefinite space $Y_0$? You cannot read off short integral forms on $Y_0$ directly. Instead, `SpanningForms.lean` builds two square grids of Hermitian spaces $Y^a_b$ and $\widetilde{Y}^b_a$ meeting along the diagonal $(l, l)$ and defines monotone **two-grid paths** from $Y_m$ to $\widetilde{Y}^m_0$:
- The *first path* runs down the lower chain $\Theta_\vartheta : Y_0 \to Y_m$ paired against a restriction map $r$ from deep-level forms $\varphi$ on $\widetilde{Y}^m_0$ onto $Y_0$.
- By executing $m^2$ turns across squares—off-diagonal turns via the {{concept:theta-square-law|theta square law}} and **diagonal turns** via {{concept:seesaw-duality|seesaw moves on the shared space $Y^{j+1}_j = \widetilde{Y}^{j+1}_j$}}—the period integral along the first path transfers into a **last-path period** $\Xi = \mathrm{lastPathPeriod}(\Psi_\vartheta, (y', \varphi))$ divided by $c(\vartheta)$, where $|c(\vartheta)|$ is governed by $m^2$ {{concept:hecke-l-value-ratio|turn ratios}}.
- Along the last path, every space is definite and explicit: testing against a dual-weight **delta form** $\delta_x(v)$ supported at one finite class $x$ reads off the exact value of $\Theta_\vartheta(r \varphi)$ at $x$ (`exists_dualPairing_eq_mul_lastPathPeriod`), proving both {{concept:weyl-gauge|p-adic gauge integrality}} and archimedean smallness (`exists_span_short_lifts_eq_top`). Finally, `LongForms.lean` projects onto good forms with the {{concept:central-idempotent|good idempotent}} (`goodProjTranslate 1`) to obtain `exists_span_isAuxiliaryForm_eq_top`.

### Limits of the intuition

Calling **auxiliary spanning forms** "cheap filler vectors" is accurate for their *total* cost $n'(2 x_1^2 b^2 + b) \le d b$, but misleading for their *individual* size: individually, each auxiliary form is vastly longer than the Theorem A vectors (having exponent $2 x_1^2 b^2$, whereas $b$ itself is smaller than $x_1^2 b^2$). The whole completion mechanism works only because Theorem B's uncovered fraction $4/b^2$ is quadratic in $1/b$, beating the $b^2$ factor in the exponent with room to spare (`8 x_1^2 + 4 b^{-1} <= b`).

## Level 2 — Undergrad Bridge

Building `exists_span_isAuxiliaryForm_eq_top` connects Stage 03's short-form infrastructure with Stage 05's square commutation law across `SpanningForms.lean` (29 decls) and `LongForms.lean` (first 3 decls):

1. **Last-path integrality and archimedean size (`SpanningForms.lean:63–388`):**
   - `liftDenominator` (a product of powers of $M$ and $P.d$) clears denominators of every theta kernel on the last path at primes $p \nmid \mathrm{Nm}(Q)$ (`liftDenominator_smul_lastPathEdgeDatum_mem_raises`).
   - `norm_mul_det_flipScalar_zpow_le_one` and `exists_norm_mul_lastPathPairing_le_one` clear the middle branching pairing `lastPathPairing` by $N \neq 0$ with $\log N \le x^{98} b^2$, yielding `exists_mul_lastPathPeriod_mem_embValRing` ($0 < N \le \exp(x^{99} b^2)$ clears `lastPathPeriod Ψ y`).
   - For the trivial twist $\vartheta = 1$, bounding class numbers (`card_stdClasses_lastPathLevel_le`, $\le \exp(x^{99} b^2)$) and single-edge kernel norms (`sqrt_weylNormSq_lastPathKernel_le`, $\le \exp(x^{99} b^2)$) bounds every Galois conjugate of the **last-path period** by $\|\sigma \Xi\| \le \exp(x^{200} b^2)\sqrt{B_{\mathrm{false}} B_{\mathrm{true}}}$ (`norm_lastPathPeriod_le`).
2. **From $m^2$ grid turns to delta-form value readout (`SpanningForms.lean:392–659`):**
   - `exists_integral_lowerChainUpTo_eq_lastPathPeriod` packages `DiagonalTurns.lean::exists_transfersPeriods_first_last`: there exist scalars $c(\vartheta)$ satisfying `HasAbsUpToNormPower` with $\prod_{i,j < m} \mathrm{turnRatio}_{i,j}$, path data $\Psi_\vartheta$, a constant $\beta$, and a surjective restriction map $r : D.\mathrm{gridLevelForms}\ m \twoheadrightarrow D.\mathrm{principalForms}\ Y_0$ relating the first-path pairing to $\beta \cdot \mathrm{lastPathPeriod} / c(\vartheta)$.
   - Pairing against the dual-weight **delta form** $\delta_x(v) = \mathrm{lastPathConjDelta}\ \vartheta\ x\ v$ supported on the class of $x$ uses Schur's orthogonality relations (`integral_lastPathConjDelta_mul`) to read off $v^\top \mathrm{dualPairing}\cdot ((c(\vartheta)/c(1))\,\mathrm{lowerChain}\ \vartheta\ (r\varphi))(x)$ directly as $\beta \cdot \mathrm{lastPathPeriod}$ (`exists_dualPairing_eq_mul_lastPathPeriod`).
3. **Lifting short deep-level forms (`SpanningForms.lean:663–796`):**
   - `mem_embValRing_of_dualPairing_eq_dualHook_mul` and `weylNormSq_le_of_dualPairing` convert dual-pairing integrality and bounds into pointwise integrality and `weylNormSq` bounds (`exists_lowerChain_integral_and_weylNormSq_le`, multiplying the squared norm by at most $\exp(2 x^{201} b^2)$ using `dualHook_mul_exp_le`).
   - Because `span_isShort_eq_top_gridLevelForms` (from Stage 03's short-column infrastructure) spans $D.\mathrm{gridLevelForms}\ m$ by forms that are integral and $\exp(x^{99} b^2)$-short, applying $s \cdot r$ yields **`exists_span_short_lifts_eq_top`** with archimedean bound $\exp(x^{202} b^2)$ on $Y_0$.
4. **Projection to `D.targetForms` and completion budget (`LongForms.lean:44–134`):**
   - Applying `D.goodProjTranslate 1` (surjective onto `D.targetForms`, preserving integrality and adding at most $x^{99}$ to the log norm, with $x^{99} + x^{202} b^2 \le 2 x_1^2 b^2$) establishes **`exists_span_isAuxiliaryForm_eq_top`** with $\gamma(\vartheta) = c(\vartheta)/c(1)$, $\gamma(1) = 1$, and $\|\gamma(\vartheta)\| = \mathrm{Nm}(Q)^{e/2} \prod_{i,j < m} \mathrm{turnRatio}_{i,j}(\vartheta)$.
   - Together with `HeightLe D.b` on $|\gamma(\theta_\iota)|^{-2}$ (from squaring `heightLe_inv_rpow_mul_prod_turnRatio` and $2 m^2 t x_1 \le b$) and `mul_completion_cost_le`, the $n' = d - n$ auxiliary basis vectors add at most $d b$ to Theorem C's inequality.

## Level 3 — Formal Lean Contract

Stage 05 anchors auxiliary spanning forms across `skeleton/05_determinant/spanning_forms.lean` (29 decls, 0 truncations) and `skeleton/05_determinant/long_forms.lean` (9 decls, 0 truncations):

- `05_determinant/long_forms.lean#New.Data.IsAuxiliaryForm` (upstream `LongForms.lean:44-54`):
  ```lean
  structure New.Data.IsAuxiliaryForm (γ : D.MFoldTwist → ℂ) (y : D.targetForms) : Prop where
    smul_lift_mem_integralForms (ι : PrimePadicIso) (ϑ : D.MFoldTwist)
      (hϑ : Nat.Coprime ι.1 ϑ.normQ) : γ ϑ • D.Θ ϑ y ∈ integralForms ι.place
    normSqAt_lift_le (σ : ℂ ≃+* ℂ) : (D.Θ 1 y).normSqAt σ ≤ exp (2 * D.x₁ ^ 2 * D.b ^ 2)
  ```
- `05_determinant/long_forms.lean#New.Data.exists_span_isAuxiliaryForm_eq_top` (upstream `LongForms.lean:57-105`): proves there exist $e \in \mathbb{Z}$ and $\gamma : D.\mathrm{MFoldTwist} \to \mathbb{C}$ with $\gamma(\vartheta) \neq 0$, $\gamma(1) = 1$, $\|\gamma(\vartheta)\| = \mathrm{Nm}(Q)^{e/2} \prod_{i,j < m} \mathrm{turnRatio}_{i,j}(\vartheta)$, and `span ℂ {y | D.IsAuxiliaryForm γ y} = ⊤`.
- `05_determinant/long_forms.lean#New.Data.mul_completion_cost_le` (upstream `LongForms.lean:121-134`): proves $n'(2 x_1^2 b^2 + b) \le d b$ whenever $(1 - 4/b^2)d \le n$ and $d = n + n'$.
- Also anchored: `05_determinant/spanning_forms.lean#New.Data.exists_span_short_lifts_eq_top` (upstream `SpanningForms.lean:769-796`) and `05_determinant/spanning_forms.lean#New.Data.exists_dualPairing_eq_mul_lastPathPeriod` (upstream `SpanningForms.lean:632-659`).

See the page's live `<VerificationBanner />` (`audit/verification-status.json`) for the pinned upstream commit and local Lean build status (`build_status: unverified-locally`).
