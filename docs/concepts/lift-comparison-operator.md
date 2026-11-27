---
slug: lift-comparison-operator
term: lift comparison operator
aliases: ["lift compression", "compressed comparison operator", "comparison determinant"]
inline_glimpse: "A family of self-adjoint integral operators U_theta = n_Aut * hk^2 * glue_theta * glue_theta^* on V_j whose compression pr U_theta onto the lift subspace satisfies Z_j(theta) * det(pr U_theta) = C * rho_theta^d, turning the lift determinant ratio into a ratio of compressed determinants times inverse Hecke values."
visual_ref: assets/stage06-comparison-operator-volume.svg
where_analogy_breaks: "Viewing U_theta as a simple change-of-basis operator between the untwisted and twisted lift subspaces is misleading: V_j at twist theta and V_j at twist 1 carry different level structures, so U_theta acts separately on each twisted space FormV and links them only through the twist-independent quadratic form U^o on W_j and the determinant identity Z_j(theta) * Delta(theta) = C * rho_theta^d."
sources:
  - title: "Yoshida lifts and Selmer groups"
    citation: "S. Böcherer, N. Dummigan & R. Schulze-Pillot, J. Math. Soc. Japan 64 (2012), 1353–1401."
    kind: paper
  - title: "Special L-values and Selmer groups of Siegel modular forms of genus 2"
    citation: "X. Zhang, arXiv:1811.02031 (2018)."
    kind: paper
  - title: "Congruences of cusp forms and special values of their zeta functions"
    citation: "H. Hida, Invent. Math. 63 (1981), 225–261."
    kind: paper
lean_anchors:
  - 06_one_lift/lift_volume_height.lean#New.Data.IsComparisonOperator
  - 06_one_lift/lift_volume_height.lean#New.Data.comparisonOp
  - 06_one_lift/lift_volume_height.lean#New.Data.isComparisonOperator_comparisonOp
  - 06_one_lift/lift_volume_height.lean#New.Data.liftCompression
  - 06_one_lift/lift_volume_height.lean#New.Data.IsComparisonOperator.exists_normDet_sq_mul_det_eq
child_concepts:
  - gram-determinant
  - tight-frame
  - divided-lattice-gluing
  - false-lift-period
  - regular-twist-family
---

# Lift Comparison Operator

## Level 1 — First-Principles

For a single lift $\Theta_{j,\vartheta} : W_j \to V_j$, we want to bound the ratio $Z_j(1) / Z_j(\vartheta)$ of untwisted to twisted {{concept:gram-determinant|Gram determinants}} $Z_j(\vartheta) = \det(\Theta_{j,\vartheta}^* \Theta_{j,\vartheta})$. Direct comparison is obstructed because the twisted and untwisted target spaces have different congruence levels at $Q$, so you cannot simply multiply one lift matrix by the inverse of the other.

A **lift comparison operator** $U_\vartheta$ resolves this cleanly by acting on the target space $\mathrm{FormV}$ at each twist $\vartheta$ and compressing it onto the $d$-dimensional range of the lift. Start with a pure linear-algebra fact (`normDet_sq_mul_det_compression`): for any injective map $u : U \to V$ between finite-dimensional inner-product spaces and any operator $T$ on $V$, if $\mathrm{pr}$ is the orthogonal projection onto $\mathrm{range}(u)$, then
$$\det(u^* u)\cdot \det\bigl(\mathrm{pr}\,T|_{\mathrm{range}(u)}\bigr) = \det(u^* T u).$$
Here $\mathrm{pr}\,U_\vartheta|_{\mathrm{range}(\Theta_{j,\vartheta})}$ is the **lift compression** (`liftCompression`), also called the **compressed comparison operator**, and $\Delta(\vartheta) = \det(\mathrm{pr}\,U_\vartheta)$ is its **comparison determinant**.

Stage 06 builds the concrete family $U_\vartheta = \mathrm{comparisonOp}\ j\ \vartheta$ directly from {{concept:divided-lattice-gluing|the gluing map}}:
$$U_\vartheta = n_{\mathrm{Aut}}\,\mathrm{hk}^2\,\mathrm{glue}_\vartheta \circ \mathrm{glue}_\vartheta^*,$$
where $n_{\mathrm{Aut}} = D.\mathrm{chosenGroupOrder}$ clears automorphism orders and $\mathrm{hk} = \mathrm{hookNumberMinus}\ j$ is the hook number of the Young diagram $\mu^-$ on $V^\circ$. Why does this operator satisfy the `IsComparisonOperator` contract?
1. **Twist-separated pullback:** Because $c'_\vartheta\,\mathrm{glue}_\vartheta^*\,\Theta_{j,\vartheta} = \Theta^\circ$ is independent of $\vartheta$ and $\|c'_1\|^2 / \|c'_\vartheta\|^2 = \Gamma_j(\vartheta) / \Gamma_j(1) = \rho_\vartheta$, we get
   $$\langle \Theta_{j,\vartheta} y',\, U_\vartheta \Theta_{j,\vartheta} y\rangle = \rho_\vartheta \langle y',\, U^\circ y\rangle$$
   for a fixed positive-definite operator $U^\circ$ independent of $\vartheta$. Taking determinants immediately yields **`exists_normDet_sq_mul_det_eq`**:
   $$Z_j(\vartheta)\cdot \Delta(\vartheta) = C \cdot \rho_\vartheta^d \qquad (C \neq 0 \text{ independent of } \vartheta).$$
   Dividing $\vartheta = 1$ by $\vartheta = \xi_\iota^j$ cancels the unknown constant $C$ identically:
   $$\frac{Z_j(1)}{Z_j(\xi_\iota^j)} = \frac{\Delta(\xi_\iota^j)}{\Delta(1)} \cdot \rho_{\xi_\iota^j}^{-d}.$$
2. **Integrality and smallness:** Expanding $\mathrm{hk}^2\,\mathrm{id}$ on $\mathrm{FormTuples}\ j$ using {{concept:tight-frame|an overcomplete Parseval-style frame}} of integral tuples (`exists_tight_frame_formTuples`) rewrites $U_\vartheta$ as a finite sum of rank-one operators $n_{\mathrm{Aut}}\langle \varphi', \cdot\rangle \varphi'$ over glued integral forms $\varphi' = \mathrm{glue}_\vartheta(f_a)$. That makes $U_\vartheta$ preserve integrality at every prime $\ell \nmid \mathrm{Nm}(Q)$ (`comparisonOp_mem_integralForms`) and caps the operator norm of $U_1$ at $\exp(3 f m b \log^2 b)$ (`normSqAt_comparisonOp_le`).
3. **Orthogonality to false lifts:** Every image $U_\vartheta(\Theta_{j,\vartheta} y)$ is a scalar multiple of $\mathrm{glue}_\vartheta(\Theta^\circ y)$, so by {{concept:false-lift-period|false-lift orthogonality}} it is orthogonal to all false lifts. That ensures the orthogonal projection $\mathrm{pr}$ neither inflates archimedean norms at $\vartheta = 1$ nor blows up $\ell$-adic denominators beyond {{concept:regular-twist-family|projection clearing}} (`ClearsProjection`).

### Limits of the intuition

Treating `IsComparisonOperator` as merely a factorization trick misses why *all five* fields of the structure are locked together. Without `isOrthogonalToFalseLifts`, projecting $U_\vartheta z$ onto $\mathrm{range}(\Theta_{j,\vartheta})$ could leak into false lifts—which would break both `normSqAt_starProjection_range_goodLift_le` (needed for the archimedean bound on $\Delta(1)$) and `ClearsProjection` (needed for the $\ell$-adic bound on $\Delta(\xi_\iota^j)$). And without the **tight frame** expansion over `FormTuples`, `glue_ϑ^*` alone would introduce uncontrolled denominators from the inner product on $V^\circ$.

## Level 2 — Undergrad Bridge

In Hida (1981), Böcherer–Dummigan–Schulze-Pillot (2012), and Zhang (2018), congruence modules and Petersson-norm ratios of individual eigenforms are controlled by pairing against integral forms orthogonal to complementary eigenspaces. `src/New/OneLift/LiftVolumeHeight.lean` (29 declarations, 0 stub truncations) lifts this strategy to an entire $d$-dimensional subspace of good lifts simultaneously:

1. **Abstract compression and determinant bounds (`normDet_sq_mul_det_compression`, `norm_map_det_le_pow`, `exists_smul_mem_integralForms_maximal`):**
   If every eigenvalue $\alpha$ of an endomorphism $u$ on a $d$-dimensional complex space satisfies $\|f(\alpha)\| \le B$ under a ring map $f : \mathbb{C} \to K$, then $\|f(\det u)\| \le B^d$. Scaling any nonzero eigenvector to $\ell$-adic valuation $0$ (`exists_smul_mem_integralForms_maximal`) converts integrality of $\Lambda(\mathrm{pr}\,U_\vartheta)$ into an eigenvalue bound.
2. **Tight-frame expansion of `comparisonOp` (`exists_frame_weightRep_eq_intCast`, `exists_tight_frame_formTuples`, `comparisonOp_eq_sum`):**
   On each class $c$ of $U(V^\circ)$, `exists_frame_weightRep_eq_intCast` supplies a frame $\eta_T$ indexed by `ColumnFilling` of $\mu^-$ with $\sum_T \langle \eta_T, \xi\rangle \eta_T = \mathrm{hk}^2 \xi$, integral after $\delta(c)$, and squared norm $\le \mathrm{hk}^2$. Assembling across `TightFrameIndex j` rewrites
   $$U_\vartheta \varphi = \sum_{a \in \mathrm{TightFrameIndex}\,j} n_{\mathrm{Aut}} \langle \mathrm{glue}_\vartheta(f_a), \varphi\rangle\,\mathrm{glue}_\vartheta(f_a).$$
   Since each $f_a$ is integral and `glue_ϑ` preserves integrality away from $Q$, $U_\vartheta$ preserves `integralForms ι` (`comparisonOp_mem_integralForms`). At $\vartheta = 1$, combining `normSqAt_glue_le` ($\exp(2A)$) with `card_prod_mul_card_tightFrameIndex_mul_sq_le` ($\exp(2A)$, where $A = f m b \log^2 b$) yields `(comparisonOp j 1 φ).normSqAt σ ≤ exp (3 * A) ^ 2 * φ.normSqAt σ` (`normSqAt_comparisonOp_le`).
3. **Verifying the contract and determinant cancellation (`isComparisonOperator_comparisonOp`, `IsComparisonOperator.exists_normDet_sq_mul_det_eq`):**
   Using `heckeValueProd_div_eq` ($\rho_\vartheta = \|c'_1\|^2 / \|c'_\vartheta\|^2$), `inner_goodLift_comparisonOp`, and `isOrthogonalToFalseLifts_comparisonOp` proves `isComparisonOperator_comparisonOp j`. Applying `normDet_sq_mul_det_compression` to `u = D.goodLift ϑ j` then yields
   $$Z_j(\vartheta)\cdot \det\bigl(\mathrm{liftCompression}\ U\ \vartheta\bigr) = C \cdot \rho_\vartheta^d,$$
   with $C = \det(U^\circ) \neq 0$ because $U^\circ$ is positive definite (`periodLift_injective`).

## Level 3 — Formal Lean Contract

All five anchors live in `skeleton/06_one_lift/lift_volume_height.lean` (29 declarations, 0 stub truncations):

- `06_one_lift/lift_volume_height.lean#New.Data.IsComparisonOperator` (upstream `LiftVolumeHeight.lean:172-198`):
  ```lean
  structure New.Data.IsComparisonOperator (j : D.LiftIndex)
      (U : ∀ ϑ : D.Twist, End ℂ (D.descentPair ϑ j).FormV) : Prop where
    ratio_ne_zero (ϑ : D.Twist) : heckeValueProd j ϑ / heckeValueProd j 1 ≠ 0
    exists_inner_eq : ∃ U₀ : End ℂ (D.goodForms (D.spaceW j)),
      (∀ y : D.goodForms (D.spaceW j), y ≠ 0 → 0 < inner ℂ y (U₀ y)) ∧
      ∀ (ϑ : D.Twist) (y' y : D.goodForms (D.spaceW j)),
        inner ℂ (D.goodLift ϑ j y') (U ϑ (D.goodLift ϑ j y)) =
          heckeValueProd j ϑ / heckeValueProd j 1 * inner ℂ y' (U₀ y)
    isOrthogonalToFalseLifts (ϑ : D.Twist) (z : (D.goodLift ϑ j).range) :
      IsOrthogonalToFalseLifts (D.descentPair ϑ j) D.e₀ (D.goodLift ϑ j).rangeᗮ (U ϑ z)
    mem_integralForms (ϑ : D.Twist) (l : ℕ) [Fact l.Prime] (ι : PadicPlace l)
      (hl : l.Coprime ϑ.normQ) : ∀ φ ∈ integralForms ι, U ϑ φ ∈ integralForms ι
    normSqAt_le (φ : (D.descentPair (1 : D.Twist) j).FormV) (σ : ℂ ≃+* ℂ) :
      (U 1 φ).normSqAt σ ≤ exp (3 * (D.f * D.m * D.b * D.logB ^ 2)) ^ 2 * φ.normSqAt σ
  ```
- `06_one_lift/lift_volume_height.lean#New.Data.comparisonOp` (upstream `LiftVolumeHeight.lean:200-206`), `06_one_lift/lift_volume_height.lean#New.Data.isComparisonOperator_comparisonOp` (upstream `LiftVolumeHeight.lean:548-579`), and `06_one_lift/lift_volume_height.lean#New.Data.liftCompression` (upstream `LiftVolumeHeight.lean:612-618`).
- `06_one_lift/lift_volume_height.lean#New.Data.IsComparisonOperator.exists_normDet_sq_mul_det_eq` (upstream `LiftVolumeHeight.lean:622-646`):
  ```lean
  lemma New.Data.IsComparisonOperator.exists_normDet_sq_mul_det_eq {j : D.LiftIndex}
      {U : ∀ ϑ : D.Twist, End ℂ (D.descentPair ϑ j).FormV} (hU : IsComparisonOperator j U) :
      ∃ C : ℂ, C ≠ 0 ∧ ∀ ϑ : D.Twist,
        (D.goodLift ϑ j).normDet ^ 2 * (liftCompression U ϑ).det =
          C * (heckeValueProd j ϑ / heckeValueProd j 1) ^ finrank ℂ (D.goodForms (D.spaceW j))
  ```

See the page's live `<VerificationBanner />` (`audit/verification-status.json`) for the pinned upstream commit and local Lean build status (`build_status: unverified-locally`).
