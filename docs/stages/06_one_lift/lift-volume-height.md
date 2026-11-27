# The Lift Comparison Operator and Single-Lift Volume Height

This page explains `src/New/OneLift/LiftVolumeHeight.lean` (`06_one_lift/lift_volume_height`, 29 declarations, **0 stub truncations**): the culminating Stage 06 theorem `New.Data.heightLe_liftDeterminant_ratio`, which converts divided-lattice gluing and negative twisted $L$-value heights into the exact single-lift volume height feeding [Stage 05 — Regular Twist Families and the Assembly of Theorem C](../05_determinant/regular-twists-and-theorem-c.md) (`05_determinant/long_forms`). Return to the [Stage 06 Overview](./overview.md), or revisit [Three Exchanges, the Carrier Form, and Divided-Lattice Gluing](./three-exchanges-and-gluing-maps.md) and [Vanishing Periods of False Lifts and Negative Heights of Twisted L-Values](./false-lifts-and-twisted-l-values.md).

## Level 1 — First-Principles

Everything in Stage 06 converges inside `LiftVolumeHeight.lean`. From [Gluing Maps](./three-exchanges-and-gluing-maps.md) we have the linear map {{concept:divided-lattice-gluing|$\mathrm{glue}_\vartheta : \mathrm{FormTuples}\ j \to \mathrm{FormV}$}} whose adjoint isolates the twist as $c'_\vartheta\,\mathrm{glue}_\vartheta^*\,\Theta_{j,\vartheta} = \Theta^\circ$, and from [Twisted Value Heights](./false-lifts-and-twisted-l-values.md) we have the negative {{concept:arithmetic-height|arithmetic height}} bound on the inverse {{concept:carrier-form|Hecke value product}} ratio $\rho_{\xi_\iota^j}^{-1} = \Gamma_j(1)/\Gamma_j(\xi_\iota^j)$. Now we turn those two pieces into a negative height bound on the single-lift {{concept:gram-determinant|Gram determinant}} ratio $Z_j(1) / Z_j(\xi_\iota^j)$.

![End-to-end Stage 06 convergence diagram showing gluing maps and tight-frame expansion constructing U_theta = n_Aut hk^2 glue_theta glue_theta^*, the compression identity Z_j(theta) Delta(theta) = C rho_theta^d, intermediate height bounds d(3A + r denominatorBudget) and d(10 m x_1 + k(6 denominatorBudget - directionAmount)), absorption via le_of_hasLogBound into d k (log b denominatorBudget - directionAmount), and handoff into Stage 05 LongForms.lean](assets/stage06-comparison-operator-volume.svg)

The argument runs in four clean steps:

1. **Abstract compression identity (`normDet_sq_mul_det_compression`):**
   For any injective linear map $u : U \to V$ between finite-dimensional inner-product spaces and any operator $T$ on $V$, compressing $T$ onto $\mathrm{range}(u)$ via the orthogonal projection $\mathrm{pr}$ satisfies
   $$\det(u^* u)\cdot \det\bigl(\mathrm{pr}\,T|_{\mathrm{range}(u)}\bigr) = \det(u^* T u).$$
   Applying this to $u = \Theta_{j,\vartheta}$ relates the lift Gram determinant $Z_j(\vartheta) = \det(\Theta_{j,\vartheta}^* \Theta_{j,\vartheta})$ directly to the determinant $\Delta(\vartheta) = \det(\mathrm{pr}\,U_\vartheta)$ of the {{concept:lift-comparison-operator|lift compression}}.
2. **Concrete comparison operator (`comparisonOp`, `exists_tight_frame_formTuples`, `isComparisonOperator_comparisonOp`):**
   Stage 06 sets
   $$U_\vartheta = \mathrm{comparisonOp}\ j\ \vartheta = n_{\mathrm{Aut}}\,\mathrm{hk}^2\,\mathrm{glue}_\vartheta \circ \mathrm{glue}_\vartheta^*,$$
   where $n_{\mathrm{Aut}} = D.\mathrm{chosenGroupOrder}$ and $\mathrm{hk} = \mathrm{hookNumberMinus}\ j$. Expanding $\mathrm{hk}^2\,\mathrm{id}$ on $\mathrm{FormTuples}\ j$ using {{concept:tight-frame|a tight frame of integral tuples}} (`exists_tight_frame_formTuples`) rewrites $U_\vartheta$ as a finite sum of rank-one operators $n_{\mathrm{Aut}}\langle \mathrm{glue}_\vartheta(f_a), \cdot\rangle\,\mathrm{glue}_\vartheta(f_a)$. This proves all five axioms of `IsComparisonOperator`:
   - `ratio_ne_zero`: $\rho_\vartheta = \Gamma_j(\vartheta)/\Gamma_j(1) = \|c'_1\|^2/\|c'_\vartheta\|^2 \neq 0$;
   - `exists_inner_eq`: $\langle \Theta_{j,\vartheta} y',\, U_\vartheta \Theta_{j,\vartheta} y\rangle = \rho_\vartheta \langle y',\, U^\circ y\rangle$ with $U^\circ$ positive definite and independent of $\vartheta$;
   - `isOrthogonalToFalseLifts`: $U_\vartheta$ maps lifts to forms satisfying {{concept:false-lift-period|false-lift orthogonality}};
   - `mem_integralForms`: $U_\vartheta$ preserves integrality at every {{concept:prime-number|prime}} $\ell \nmid \mathrm{Nm}(Q)$ (`comparisonOp_mem_integralForms`);
   - `normSqAt_le`: $U_1$ multiplies squared norms by at most $\exp(3 f m b \log^2 b)^2 = \exp(6 f m b \log^2 b)$ (`normSqAt_comparisonOp_le`), meaning its operator norm is at most $\exp(3 f m b \log^2 b)$.
3. **Determinant cancellation and intermediate height bounds (`exists_normDet_sq_mul_det_eq`, `norm_map_det_liftCompression_le`, `norm_padic_det_liftCompression_le`):**
   Applying the compression identity gives $Z_j(\vartheta)\cdot \Delta(\vartheta) = C \cdot \rho_\vartheta^d$ with $C = \det(U^\circ) \neq 0$ independent of $\vartheta$. Dividing $\vartheta = 1$ by $\vartheta = \xi_\iota^j$ cancels $C$ identically:
   $$\frac{Z_j(1)}{Z_j(\xi_\iota^j)} = \frac{\Delta(\xi_\iota^j)}{\Delta(1)} \cdot \rho_{\xi_\iota^j}^{-d}.$$
   Now we bound the two factors separately—keeping the intermediate bounds strictly distinct from the final absorbed bound:
   - **Compressed determinant ratio $\Delta(\xi_\iota^j)/\Delta(1)$:** At infinity, false-lift orthogonality keeps $\mathrm{pr}$ norm-nonincreasing, so every eigenvalue of $\mathrm{pr}\,U_1$ has modulus $\le \exp(3A)$ ($A = f m b \log^2 b$), giving $\|\sigma(\Delta(1))\| \le \exp(3 d A)$ (`norm_map_det_liftCompression_le`). At each prime $\ell$ of {{concept:regular-twist-family|a regular twist family}} $\theta$, `ClearsProjection` produces $\Lambda$ clearing denominators of $\mathrm{pr}$ with $|\iota(\det\Lambda)| \ge \exp(-d r \beta_\ell)$, forcing $\|\iota(\Delta(\xi_\iota^j))\| \le \exp(d r \beta_\ell)$ (`norm_padic_det_liftCompression_le`). Together, these yield the **intermediate height**
     $$\mathrm{HeightLe}\Bigl(d\bigl(3A + r\,\mathrm{denominatorBudget}\bigr)\Bigr) \quad\text{on}\quad \frac{\Delta(\xi_\iota^j)}{\Delta(1)}.$$
   - **Inverse Hecke value power $\rho_{\xi_\iota^j}^{-d}$:** Raising {{concept:character-theta-lift|Stage 06's twisted $L$-value height}} (`heightLe_inv_heckeValueProd_div`) to the $d$-th power yields the **intermediate height**
     $$\mathrm{HeightLe}\Bigl(d\bigl(10 m x_1 + k(6\,\mathrm{denominatorBudget} - D.\mathrm{directionAmount})\bigr)\Bigr) \quad\text{on}\quad \rho_{\xi_\iota^j}^{-d}.$$
4. **Error absorption and handoff to Stage 05 (`heightLe_liftDeterminant_ratio`):**
   Adding both intermediate heights and invoking the scale bound `D.le_of_hasLogBound`:
   $$3A + r\,\mathrm{denominatorBudget} + 10 m x_1 + 6 k\,\mathrm{denominatorBudget} \;\le\; k \log b \cdot \mathrm{denominatorBudget}$$
   delivers {{concept:idle-direction-repayment|the single-lift volume height}} **`New.Data.heightLe_liftDeterminant_ratio`**:
   $$\mathrm{HeightLe}\Bigl(d \cdot k\bigl(\log b \cdot \mathrm{denominatorBudget} - D.\mathrm{directionAmount}\bigr)\Bigr) \quad\text{on}\quad \frac{Z_j(1)}{Z_j(\theta(\iota)^j)}.$$
   Handed directly to [Stage 05 — Regular Twist Families and the Assembly of Theorem C](../05_determinant/regular-twists-and-theorem-c.md) (`05_determinant/long_forms`), the identity $k \log b \cdot \mathrm{denominatorBudget} = f a \log^4 b / m$ turns this into $d(f a \log^4 b / m - k\,D.\mathrm{directionAmount})$, which sums over $j = 1, \dots, m$ to $d(f a \log^4 b - D.\mathrm{mainAmount})$ and closes Theorem C.

### Where this analogy breaks

It is tempting to conflate the intermediate height $d(3A + r\,\mathrm{denominatorBudget})$ of $\Delta(\xi_\iota^j)/\Delta(1)$ with the final error term $d k \log b \cdot \mathrm{denominatorBudget}$ in `heightLe_liftDeterminant_ratio`. They play different roles: `norm_map_det_liftCompression_le` and `norm_padic_det_liftCompression_le` know nothing about $\log b$ or $k$, whereas `D.le_of_hasLogBound` uses the extra $\log b$ factor to absorb *all four* positive error terms ($3A$, $r\,\mathrm{denominatorBudget}$, $10 m x_1$, and $6 k\,\mathrm{denominatorBudget}$) at once while leaving $-d k\,D.\mathrm{directionAmount}$ completely untouched.

## Level 2 — Undergrad Bridge

Walkthrough of all **29 declarations** in `src/New/OneLift/LiftVolumeHeight.lean` (0 stub truncations):

| Section | Declarations (Count) | Mathematical Role |
| --- | --- | --- |
| Linear algebra & valuation scaling | `normDet_sq_mul_det_compression`, `norm_map_det_le_pow`, `normSqAt_smul`, `normSqAt_nonneg`, `normSqAt_pos`, `exists_smul_mem_integralForms_maximal` (6) | Proves $\det(u^* u)\det(\mathrm{pr}\,T|_{\mathrm{range}\,u}) = \det(u^* T u)$, bounds $\|f(\det u)\| \le B^{\dim L}$ from eigenvalue bounds, and scales any nonzero form to $\ell$-adic valuation $0$ (`exists_smul_mem_integralForms_maximal`). |
| Comparison-operator contract & pullback | `IsComparisonOperator`, `comparisonOp`, `heckeValueProd_div_eq`, `adjoint_glue_goodLift`, `inner_goodLift_comparisonOp`, `isOrthogonalToFalseLifts_comparisonOp` (6) | Defines the 5-axiom structure `IsComparisonOperator` and `comparisonOp j ϑ = n_Aut · hk² · glue_ϑ ∘ glue_ϑ^*`, proving $\langle \Theta_\vartheta y', U_\vartheta \Theta_\vartheta y\rangle = \rho_\vartheta \langle y', U^\circ y\rangle$ with $\rho_\vartheta = \|c'_1\|^2/\|c'_\vartheta\|^2$ and false-lift orthogonality. |
| Tight-frame expansion & arithmetic | `TightFrameIndex`, `exists_frame_weightRep_eq_intCast`, `exists_tight_frame_formTuples`, `comparisonOp_eq_sum`, `comparisonOp_mem_integralForms`, `card_prod_mul_card_tightFrameIndex_mul_sq_le`, `normSqAt_comparisonOp_le`, `isComparisonOperator_comparisonOp` (8) | Builds an integral tight frame of `FormTuples j` with frame constant $\mathrm{hk}^2$ and norm $\le \mathrm{hk}^2$, expands $U_\vartheta \varphi = \sum_a n_{\mathrm{Aut}} \langle \mathrm{glue}_\vartheta(f_a), \varphi\rangle\,\mathrm{glue}_\vartheta(f_a)$ to prove integrality away from $Q$ (`comparisonOp_mem_integralForms`) and squared norm factor $\le \exp(3A)^2 = \exp(6A)$ (`normSqAt_comparisonOp_le`, where $A = f m b \log^2 b$), discharging `isComparisonOperator_comparisonOp j`. |
| Compression, eigenvalue bounds & final height | `finrank_range_goodLift`, `range_goodLift_le_range_inf_ker`, `normSqAt_starProjection_range_goodLift_le`, `liftCompression`, `IsComparisonOperator.exists_normDet_sq_mul_det_eq`, `IsComparisonOperator.norm_padic_det_liftCompression_le`, `IsComparisonOperator.norm_map_det_liftCompression_le`, `mFoldTwist_one_pow`, `heightLe_liftDeterminant_ratio` (9) | Proves $Z_j(\vartheta)\Delta(\vartheta) = C \rho_\vartheta^d$, bounds $\|\sigma(\Delta(1))\| \le \exp(3 d A)$ via `normSqAt_starProjection_range_goodLift_le` and $\|\iota(\Delta(\vartheta))\| \le \exp(d r \beta)$ via `ClearsProjection` + `exists_smul_mem_integralForms_maximal`, forms `HeightLe (d * (3 * A) + d * (D.r * D.denominatorBudget))` on $\Delta(\xi_\iota^j)/\Delta(1)$, multiplies by `(heightLe_inv_heckeValueProd_div hθ j).pow d`, and applies `D.le_of_hasLogBound` to prove `heightLe_liftDeterminant_ratio`. |

### Exact Handoff into Stage 05 (`LongForms.lean`)

Inside `src/New/Determinant/Twist/LongForms.lean:142–154` (`exists_hasSmallRatios_heightLe_liftDeterminant_div`), Stage 05 receives `hθ : D.IsRegularFamily θ` from Stage 07 (`exists_isRegularFamily`), invokes `heightLe_liftDeterminant_ratio hθ j` for each $1 \le j \le m$, and rewrites
$$D.k \cdot D.\mathrm{logB} \cdot D.\mathrm{denominatorBudget} = D.k \cdot D.\mathrm{logB} \cdot \frac{D.f \cdot D.a \cdot D.\mathrm{logB}^3}{D.m \cdot D.k} = \frac{D.f \cdot D.a \cdot D.\mathrm{logB}^4}{D.m}.$$
Summing across $j = 1, \dots, m$ then gives $m \cdot k \cdot D.\mathrm{directionAmount} = D.\mathrm{mainAmount}$ and yields `HeightLe (d * (b + f * a * logB ^ 4 - D.mainAmount))` on $Z(1)/Z(\theta(\iota))$, directly driving Theorem C (`New.Data.integral_forms_long`).

## Level 3 — Formal Lean Contract

The extracted skeleton `skeleton/06_one_lift/lift_volume_height.lean` (**29 declarations, 0 stub truncations**) mirrors `src/New/OneLift/LiftVolumeHeight.lean` at pinned upstream commit `41c2716658c42ba08eb0648994a2efe83082a95c`. Every declaration extracts completely untruncated.

### Headline Signatures

```lean
-- skeleton/06_one_lift/lift_volume_height.lean (upstream LiftVolumeHeight.lean:548-783)
theorem New.Data.isComparisonOperator_comparisonOp (j : D.LiftIndex) :
    IsComparisonOperator j (comparisonOp j)

lemma New.Data.IsComparisonOperator.exists_normDet_sq_mul_det_eq {j : D.LiftIndex}
    {U : ∀ ϑ : D.Twist, End ℂ (D.descentPair ϑ j).FormV} (hU : IsComparisonOperator j U) :
    ∃ C : ℂ, C ≠ 0 ∧ ∀ ϑ : D.Twist,
      (D.goodLift ϑ j).normDet ^ 2 * (liftCompression U ϑ).det =
        C * (heckeValueProd j ϑ / heckeValueProd j 1) ^ finrank ℂ (D.goodForms (D.spaceW j))

lemma New.Data.IsComparisonOperator.norm_padic_det_liftCompression_le {j : D.LiftIndex}
    {U : ∀ ϑ : D.Twist, End ℂ (D.descentPair ϑ j).FormV} (hU : IsComparisonOperator j U)
    (ϑ : D.Twist) (ι : PrimePadicIso) (hι : (ι.prime : ℕ).Coprime ϑ.normQ) {β : ℝ}
    (h_clears : D.ClearsProjection ι.iso ϑ j β) :
    ‖ι.iso (liftCompression U ϑ).det‖ ≤
      exp (finrank ℂ (D.goodForms (D.spaceW j)) * (D.r * β))

lemma New.Data.IsComparisonOperator.norm_map_det_liftCompression_le {j : D.LiftIndex}
    {U : ∀ ϑ : D.Twist, End ℂ (D.descentPair ϑ j).FormV} (hU : IsComparisonOperator j U)
    (σ : ℂ ≃+* ℂ) :
    ‖σ (liftCompression U 1).det‖ ≤
      exp (finrank ℂ (D.goodForms (D.spaceW j)) * (3 * (D.f * D.m * D.b * D.logB ^ 2)))

lemma New.Data.heightLe_liftDeterminant_ratio {θ : PrimePadicIso → D.MFoldTwist} (hθ : D.IsRegularFamily θ)
    (j : D.LiftIndex) :
    HeightLe (D.targetDim * (D.k * (D.logB * D.denominatorBudget - D.directionAmount)))
      fun ι : PrimePadicIso ↦ D.liftDeterminant 1 j / D.liftDeterminant (θ ι) j
```

- **Handoff to Stage 05:** `New.Data.heightLe_liftDeterminant_ratio` is consumed directly by `05_determinant/long_forms` (`New.Data.exists_hasSmallRatios_heightLe_liftDeterminant_div` in [Regular Twist Families and the Assembly of Theorem C](../05_determinant/regular-twists-and-theorem-c.md)).
- **Verification status (`audit/verification-status.json`):** Local Lean compilation and kernel replay remain **`build_status: unverified-locally`** at pinned commit `41c2716658c42ba08eb0648994a2efe83082a95c` (`41c2716658c4`). Static token scanning has **`scan_scope: entry`**, covering only `Challenge.lean`, `src/Main.lean`, and `src/Statements.lean`. `LiftVolumeHeight.lean` has not been locally compiled, kernel-checked, or statically scanned here.

<!-- rh:lean-node:06_one_lift/lift_volume_height -->
