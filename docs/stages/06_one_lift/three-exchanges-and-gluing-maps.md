# Three Exchanges, the Carrier Form, and Divided-Lattice Gluing

This page explains the two core files of `src/New/OneLift/ComparisonOperator/`: `ThreeExchanges.lean` (`06_one_lift/three_exchanges`, 43 declarations, 1 `let`-type stub truncation) — which strips a twist $\vartheta$ down to a single scalar $c(\vartheta)$ via three consecutive Siegel–Weil exchanges — and `GluingMaps.lean` (`06_one_lift/gluing_maps`, 68 declarations, 0 stub truncations) — which glues forms across $V_j = V'' \oplus V^\circ$ so the twist factors cleanly out of the single-lift adjoint. Return to the [Stage 06 Overview](./overview.md), or continue to [Vanishing Periods of False Lifts and Negative Heights of Twisted L-Values](./false-lifts-and-twisted-l-values.md) and [The Lift Comparison Operator and Single-Lift Volume Height](./lift-volume-height.md).

## Level 1 — First-Principles

To compare the $j$-th twisted theta lift $\Theta_{j,\vartheta} : W_j \to V_j$ against the untwisted lift $\Theta_{j,1}$, Stage 06 needs a linear map into the target space $V_j$ that keeps prime-by-prime integrality away from the twist modulus $Q$, stays controlled in archimedean size at $\vartheta = 1$, and pushes every trace of $\vartheta$ out into one scalar when paired back against $\Theta_{j,\vartheta}$.

That engine is built in two halves: **three exchanges** strip the twist out of a family of class sums on $V''$, and gluing across a split lattice sews those initial block factors onto arbitrary forms on the complementary block $V^\circ$.

![Two-panel diagram showing Left: the three-exchange funnel through the fixed full-level lift on U(2m, m) to twist-free choiceFn and scalar c(theta) controlled by Gamma_j(theta); Right: the split V_j = V'' + V^o, gluing map glue_theta, seesaw identity c'_theta glue_theta^* Theta_{j,theta} = Theta^o, and non-vanishing via periodLift_injective and Stage 04 finrank_targetForms_ne_zero](assets/stage06-three-exchanges-and-gluing.svg)

### Part 1: Three exchanges onto a twist-free lift (`ThreeExchanges.lean`)

Starting from Stage 05's {{concept:siegel-weil-exchange|complementary-rank exchange identity}} (applied via `ExplicitMatching.lean`), Stage 06 runs {{concept:carrier-form|three successive exchanges}} until the twist $\vartheta$ survives only as a scalar multiplier $c(\vartheta)$:

1. **First two exchanges onto the carrier form (`exists_mul_stdClassSum_modKernelStdSpace_eq_one`, `exists_mul_stdClassSum_eq_carrierForm`):**
   - The **first exchange** takes a determinant-weighted class sum of {{concept:theta-correspondence|theta kernels}} on the definite space $(I_m, I_m)$ ($m$ even) and collapses it to a constant multiple $\gamma^{-1}$ of $1$, picking up the first $L$-value factor at cutoff $z_{-1} = m/2$.
   - Combining that with the **second exchange** on signature $(2m, m)$ reduces a twisted class sum against `carrierCoeff` to a scalar multiple of one fixed object: the {{term:carrier-form}} $\varphi^\circ = \mathrm{carrierForm}\ E\ m$, defined as the theta lift of the constant function $1$ from $U(0, m)$ to $U(2m, m)$. Crucially, $\varphi^\circ$ does not depend on the twist $\vartheta$, is rapidly decreasing at full level, is adelically holomorphic (`carrierForm_mem_rapidOfLevel_and_isAdelicHol`), and is strictly nonzero (`carrierForm_ne_zero`).
2. **Third exchange onto twist-free functions (`exists_mul_thirdExchangeLeftSide_eq_choiceFn`, `thirdExchangeLeftSide_eq_kernelClassSum`):**
   Integrating the {{term:carrier-form}} $\varphi^\circ$ against a theta kernel on the enlarged space $W' = W_j \oplus (E^j)^- \oplus E^{3m-j} \oplus W^\flat$ produces twist-free holomorphic functions $\mathsf{F}_i = \mathrm{choiceFn}\ j\ i$ on $U(W_j)$ (`isAdelicHol_choiceFn`). The third exchange equates a double class sum on $U(V'')$—which also rewrites as a single theta series `kernelClassSum` on $U(W_j)$—to $c(\vartheta)^{-1} \mathsf{F}_i$, where the combined scalar satisfies
   $$\|c(\vartheta)\|^2 = \frac{C}{\Gamma_j(\vartheta)}.$$
3. **The raw Hecke value product $\Gamma_j(\vartheta)$ (`exchangePair`, `heckeValueProd`):**
   Here $\Gamma_j(\vartheta) = \mathrm{heckeValueProd}\ j\ \vartheta$ is the **raw, unnormalized** product of `Exchange.lValueProd` (squared {{concept:hecke-l-value-ratio|partial Hecke L-values}} times conductor-norm powers)—explicitly **not** divided by $\Gamma_j(1)$ yet—over three {{term:carrier-form|exchange pairs}} `New.Data.exchangePair j i`:
   - `i = some (-1)` (first exchange): flat pair $\nu_{-1} = \upsilon^{-m/2}\chi^{-j}$, cutoff `signPoint (-1)` $= m/2$, twist sign $-1$;
   - `i = some 1` (second exchange): flat pair $\nu_{+1} = \upsilon^{-m}\chi^{j}$, cutoff `signPoint 1` $= m$, twist sign $+1$;
   - `i = none` (third exchange): main-chain character $\nu_j = \mu_j^0 \upsilon^n$ with $n = j + (t - 11m - 1)/2$, cutoff `mainPairPoint` $= 7(m/2) + k/2$, twist sign $+1$.

Note also a cross-stage export: `06_one_lift/three_exchanges` is consumed not only inside Stage 06, but also forward by [Stage 07 (`07_congruences/twisting_theorem`)](../07_congruences/main-summands-and-twisting-theorem.md) (mediated via `src/New/Congruences/Selmer/ClassSums.lean`).

### Part 2: Splitting the target lattice and isolating the twist (`GluingMaps.lean`)

Next, `GluingMaps.lean` splits the rank-$(r + t)$ definite target space $V_j$ into two orthogonal blocks:
$$V_j = V'' \oplus V^\circ, \qquad \operatorname{rk}(V'') = r + 9m + 1, \quad \operatorname{rk}(V^\circ) = t - 9m - 1.$$
Rather than taking an unglued direct sum of lattices, each choice $i \in \mathrm{ChoiceIndex}\ j$ picks a {{concept:divided-lattice-gluing|divided lattice}} $L_\Lambda = \mathsf{g}\mathcal{O}^{r+t}$ (`glueLattice`) between $\varpi\mathcal{O}^{r+t}$ and $\mathcal{O}^{r+t}$ glued across $V''$ and $V^\circ$ by $\Lambda$ modulo $\varpi^2$ (`splittingV`). Rescaling `leftSideSeries` by one twist-independent constant $c'' = \mathrm{firstFactorScale}\ j \neq 0$ produces the {{term:divided-lattice-gluing|first factor}} $\varphi''_{\vartheta,i}$ on $V''$: integral in {{concept:weyl-gauge|the Weyl representation gauge}} at every {{concept:prime-number|prime}} $\ell \nmid \mathrm{Nm}(Q)$ (`weylGauge_firstFactor_le_one`), with untwisted squared Weyl norm at most $\exp(f m b \log^2 b)$ (`weylNormSq_firstFactor_le`).

Multiplying $\varphi''_{\vartheta,i}(h)$ against any tuple of forms $\zeta \in \mathrm{FormTuples}\ j$ on $V^\circ$ via the Cartan product, summing over classes on $(h, h^\circ)\mathsf{g}$, and scaling by $M^{l_1}$ to clear denominators of $\mathsf{g}$ defines the linear gluing map $\mathrm{glue}_\vartheta : \mathrm{FormTuples}\ j \to \mathrm{FormV}$. It establishes three pillars for Stage 06:

1. **Twist-isolating seesaw identity (`twistFactor_smul_adjoint_glue_comp`):** By {{concept:seesaw-duality|dual-pair adjoint transfer}}, the adjoint $\mathrm{glue}_\vartheta^*$ composed with the single lift $\Theta_{j,\vartheta} = D.\mathrm{goodLift}\ \vartheta\ j$ satisfies
   $$c'_\vartheta \cdot \mathrm{glue}_\vartheta^* \circ \Theta_{j,\vartheta} = \Theta^\circ,$$
   where $\Theta^\circ = \mathrm{periodLift}\ j$ is completely independent of $\vartheta$ and $\|c'_\vartheta\|^2 = C / \Gamma_j(\vartheta)$ (`exists_norm_twistFactor_sq_eq`).
2. **Injectivity and non-vanishing (`periodLift_injective`, `twistFactor_ne_zero`):** Because products of good forms with $\mathsf{F}_i$ and $\theta^\circ$ span (`ProductsSpan.lean`), $\Theta^\circ$ is injective. Combining `periodLift_injective` with [Stage 04](../04_spanning/overview.md)'s `D.finrank_targetForms_ne_zero` (`spanning_theorem`) and [Stage 05](../05_determinant/overview.md)'s `targetDim_eq_finrank_goodForms_spaceW` (`norm_of_chain`) proves **`twistFactor_ne_zero`**: $c'_\vartheta \neq 0$—which also retroactively certifies $C \neq 0$!
3. **Integrality, archimedean bound, and orthogonality to false lifts (`glue_mem_integralForms`, `normSqAt_glue_le`, `isOrthogonalToFalseLifts_glue_periodLift`):** For $\ell \nmid \mathrm{Nm}(Q)$, `glue_ϑ` maps integral tuples on $V^\circ$ to integral forms on $V_j$; at $\vartheta = 1$, it multiplies squared norms by at most $\exp(2 f m b \log^2 b)$; and via {{concept:false-lift-period|vanishing false-lift slices}} (`sum_slice_eq_zero_of_mem_falseLiftsAt`, `thetaKernelPairing_adjoint_glue_eq_zero`), every glued form $\mathrm{glue}_\vartheta(\Theta^\circ y)$ is orthogonal to all false lifts.

### Where this analogy breaks

Describing `ThreeExchanges.lean` and `GluingMaps.lean` as "factoring a twist out of a theta kernel and gluing two blocks together" conceals an important dependency order. At the end of `ThreeExchanges.lean`, the constant $C$ in $\|c(\vartheta)\|^2 = C / \Gamma_j(\vartheta)$ is *not yet proved nonzero*—only after `GluingMaps.lean` invokes `ProductsSpan.lean` (`periodLift_injective`) together with Stage 04's `D.finrank_targetForms_ne_zero` does `twistFactor_ne_zero` rule out $C = 0$. Furthermore, `hookNumberMinus` ($\mathrm{hk}$, the hook number of the Young diagram $\mu^-$ on $V^\circ$) is already introduced in `GluingMaps.lean` (`hookNumberMinus_le`: $\mathrm{hk} \le \exp(x^{999})$) specifically to prepare the tight-frame scaling in `LiftVolumeHeight.lean`.

## Level 2 — Undergrad Bridge

### 1. Three-exchange reduction (`ThreeExchanges.lean`, 43 decls, 1 truncation)

| Step | Key Declarations | Exact Mechanism |
| --- | --- | --- |
| Exchange pairs & raw $\Gamma_j(\vartheta)$ | `signPoint`, `mainPairPoint`, `exchangePair`, `heckeValueProd`, `exponent_add_mainPairPoint_le_liftWeight` | Defines $z_{-1} = m/2$, $z_{+1} = m$, $z_* = 7(m/2) + k/2$, the three `CriticalPair`s `exchangePair j i`, and raw unnormalized $\Gamma_j(\vartheta) = \prod_i \mathrm{lValueProd}(\{\text{chosen}\} \cup Q, (\nu_i \vartheta^{s_i})^{-1}, z_i)$. |
| First two exchanges & carrier form | `exists_mul_stdClassSum_modKernelStdSpace_eq_one`, `exists_stdClassSum_modKernelStdSpace_neg_eq_one`, `exists_mul_stdClassSum_eq_carrierForm`, `carrierForm_mem_rapidOfLevel_and_isAdelicHol`, `fockInner_realPairWeil_variableMinor_pow_eq_self`, `carrierForm_ne_zero` | Collapses definite class sums on $(I_m, I_m)$ and $(I_m^-, I_m)$ to $1$, then evaluates the $(2m, m)$ twisted class sum as $\gamma^{-1}\varphi^\circ$ with $\|\gamma\|^2 = C / (L_*(c^{-1}\upsilon^m, m)\,L_*(c\upsilon^{m/2}, m/2))$. Proves $\varphi^\circ$ is full-level, rapidly decreasing, holomorphic, and nonzero. |
| Third exchange & single theta series | `enlargedSpace`, `invariantTraceChar`, `thirdExchangeLeftSide`, `integral_carrierForm_mul_eq_card_mul_choiceFn`, `exists_mul_thirdExchangeLeftSide_eq_choiceFn`, `stdClassSum_thirdExchangeKernel_productDatum`, `stdClassSum_thirdExchangeKernel_eq_productDatum`, `thirdExchangeLeftSide_eq_kernelClassSum`, `isAdelicHol_choiceFn` | Equates $c(\vartheta)\cdot \mathrm{thirdExchangeLeftSide}\ \vartheta\ j\ i\ g = \mathrm{choiceFn}\ j\ i\ g$ with $\|c(\vartheta)\|^2 = C / \Gamma_j(\vartheta)$, rewrites `thirdExchangeLeftSide` as `kernelClassSum` at `rowFixingLevel` whenever $u w = \Lambda$, and proves `choiceFn j i` is holomorphic of weight `conjHolomorphicWeight j - kernelWeight j`. |

### 2. Divided-lattice gluing (`GluingMaps.lean`, 68 decls, 0 truncations)

1. **Block splitting and congruence gluing (`splittingV`, `gluingLevel`, `glueLattice`, `isGlued_glueLattice`, `classAut_gluingLevel_eq_bot`):**
   Embeds `FirstFactorIndex j` (rank $r + 9m + 1$) and `D.SecondFactorIndex` (rank $t - 9m - 1$) into $V_j$ (`D.RowIndex ⊕ Fin D.t`, rank $6r \le 2 n_m$), verifying `hasColumnsBelow_colsPlus` and `hasColumnsBelow_colsMinus`. Every level `gluingLevel j ϑ Λ` has trivial stabilizers (`classAut_gluingLevel_eq_bot`), and `glueLattice j ϑ i` is chosen once at $\vartheta = 1$ and reused across all twists (`isGlued_splittingV_of_isGlued`).
2. **First factor and twist scalar (`firstFactorScale`, `firstFactor`, `firstFactorPairing`, `twistFactor`, `exists_norm_twistFactor_sq_eq`, `isIntegralAndSmall_firstFactor`):**
   Sets $\varphi''_{\vartheta,i} = \overline{c'' \Upsilon_{\vartheta,i}}$ with $c'' \neq 0$ twist-independent, giving `weylGauge_firstFactor_le_one` ($\ell \nmid \mathrm{Nm}(Q)$), `weylNormSq_firstFactor_le` ($\le \exp(f m b \log^2 b)$ at $\vartheta = 1$), and `twistFactor_mul_firstFactorPairing`: $c'_\vartheta \Xi_{\vartheta, i}(g) = c'' \mathsf{F}_i(g)$ with $\|c'_\vartheta\|^2 = C / \Gamma_j(\vartheta)$.
3. **Gluing map, arithmetic, and seesaw (`glue`, `glue_mem_integralForms`, `M_pow_denominatorExponent_sq_mul_le_exp`, `normSqAt_glue_le`, `conj_normalizedDescent_glue`, `inner_adjoint_glue_goodLift_eq_sum_integral`, `periodLift`, `twistFactor_smul_adjoint_glue_comp`):**
   Defines `glue j ϑ` scaled by $M^{l_1}$ (`D.M ^ denominatorExponent`). Integrality away from $Q$ holds in `glue_mem_integralForms`, and combining `weylNormSq_firstFactor_le` with `M_pow_denominatorExponent_sq_mul_le_exp` yields `(glue j 1 ζ).normSqAt σ ≤ exp (2 * (D.f * D.m * D.b * D.logB ^ 2)) * formTupleNormSq σ ζ` (`normSqAt_glue_le`). Unfolding $\langle \zeta, \mathrm{glue}_\vartheta^* \Theta_{j,\vartheta} y\rangle$ over `congruenceGroup` $\bar{J}^1$ and multiplying by $c'_\vartheta$ collapses the twist to `periodLift j` ($\Theta^\circ$): `twistFactor j ϑ • (glue j ϑ).adjoint ∘ₗ D.goodLift ϑ j = periodLift j`.
4. **False-lift slices, injectivity, and non-vanishing (`sum_slice_eq_zero_of_mem_falseLiftsAt`, `thetaKernelPairing_adjoint_glue_eq_zero`, `isOrthogonalToFalseLifts_glue_periodLift`, `periodLift_injective`, `twistFactor_ne_zero`):**
   Invoking `PeriodsOfFalseLifts.lean::DividedSplitting.stdClassSum_inner_productForm_eq_zero` kills every $V^\circ$-slice of a false lift $\Xi$, so `thetaKernelPairing` of `(glue j ϑ).adjoint Ξ` vanishes and `glue j ϑ (periodLift j y)` is orthogonal to all false lifts. Finally, `ProductsSpan.lean` gives `periodLift_injective`, and `D.finrank_targetForms_ne_zero` (from `04_spanning/spanning_theorem`) + `D.targetDim_eq_finrank_goodForms_spaceW j` (from `05_determinant/norm_of_chain`) force `twistFactor j ϑ ≠ 0`.

## Level 3 — Formal Lean Contract

`skeleton/06_one_lift/three_exchanges.lean` (43 declarations, 1 `let`-type stub truncation) and `skeleton/06_one_lift/gluing_maps.lean` (68 declarations, **0 truncations**) mirror `src/New/OneLift/ComparisonOperator/{ThreeExchanges,GluingMaps}.lean` at pinned upstream commit `41c2716658c42ba08eb0648994a2efe83082a95c`.

### Stub-Truncation Disclosure (1 Truncation in `three_exchanges.lean`; 0 in `gluing_maps.lean`)

In `skeleton/06_one_lift/three_exchanges.lean`, **1 declaration** (`New.Rows.fockInner_realPairWeil_variableMinor_pow_eq_self`, decl L156, truncated at L158) is cut at its `let v :=` binding by `rh-extract`'s depth-0 `:=` splitter. Its exact untruncated signature from `src/New/OneLift/ComparisonOperator/ThreeExchanges.lean:495–500` is:

```lean
-- Verbatim from upstream src/New/OneLift/ComparisonOperator/ThreeExchanges.lean:495-500
lemma fockInner_realPairWeil_variableMinor_pow_eq_self {m : ℕ}
    (u : GInf E (-1 : Fin m → ℤˣ)) :
    let v : PairFock E (-1 : Fin m → ℤˣ) (1 ⊕ᵥ -1 : Fin (2 * m) ⊕ Fin m → ℤˣ) :=
      Fock.polyVector (realSigns E (homSigns (-1) (1 ⊕ᵥ -1)))
        (∏ τ : InfinitePlace E, variableMinor τ inr id ^ (2 * m))
    Fock.fockInner v (realPairWeil 0 0 (u, 1) v) = Fock.fockInner v v
```

All 42 remaining declarations in `three_exchanges.lean` and all **68 declarations** in `gluing_maps.lean` extract completely untruncated.

### Headline Untruncated Signatures

```lean
-- skeleton/06_one_lift/three_exchanges.lean (upstream ThreeExchanges.lean:811-860)
lemma New.Data.exists_mul_thirdExchangeLeftSide_eq_choiceFn : ∃ (C : ℂ) (c : D.Twist → ℂ), ∀ ϑ : D.Twist,
    ‖c ϑ‖ ^ 2 = C / heckeValueProd j ϑ ∧
    ∀ (i : ChoiceIndex j) (g : New.G D.E (D.signsW j)),
      c ϑ * thirdExchangeLeftSide ϑ j i g = choiceFn j i g

-- skeleton/06_one_lift/gluing_maps.lean (upstream GluingMaps.lean:1024-1067)
theorem New.Rows.twistFactor_smul_adjoint_glue_comp :
    twistFactor j ϑ • (glue j ϑ).adjoint ∘ₗ D.goodLift ϑ j = periodLift j

theorem New.Rows.isOrthogonalToFalseLifts_glue_periodLift (y : D.goodForms (D.spaceW j)) :
    IsOrthogonalToFalseLifts (D.descentPair ϑ j) D.e₀ (D.goodLift ϑ j).rangeᗮ
      (glue j ϑ (periodLift j y))

theorem New.Rows.periodLift_injective : Function.Injective (periodLift j)

theorem New.Rows.twistFactor_ne_zero : twistFactor j ϑ ≠ 0
```

- **Verification status (`audit/verification-status.json`):** Local Lean compilation and kernel replay remain **`build_status: unverified-locally`** at pinned commit `41c2716658c42ba08eb0648994a2efe83082a95c` (`41c2716658c4`). Static token scanning has **`scan_scope: entry`**, covering only `Challenge.lean`, `src/Main.lean`, and `src/Statements.lean`. Neither `ThreeExchanges.lean` nor `GluingMaps.lean` has been locally compiled, kernel-checked, or statically scanned here.

<!-- rh:lean-node:06_one_lift/three_exchanges -->

<!-- rh:lean-node:06_one_lift/gluing_maps -->
