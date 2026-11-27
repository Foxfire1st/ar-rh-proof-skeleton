# Chain Determinant Factorization and Two-Grid Auxiliary Forms

This page explains the two combinatorial engines built directly on top of `OneSquare.lean`: `src/New/Determinant/Chain/NormOfChain.lean` (`05_determinant/norm_of_chain`, 24 declarations, **1 `let`-type stub truncation documented verbatim below**) and `src/New/Determinant/AuxiliaryForms/SpanningForms.lean` (`05_determinant/spanning_forms`, 29 declarations, 0 stub truncations), backed by `ChainNorms.lean`, `GridPaths.lean`, and `DiagonalTurns.lean`. Return to the [Stage 05 Overview](./overview.md), or revisit [Siegel–Weil Exchange and the Commuting Theta Square](./exchange-and-one-square.md) and continue to [Regular Twist Families and the Assembly of Theorem C](./regular-twists-and-theorem-c.md).

## Level 1 — First-Principles

With the {{concept:theta-square-law|commuting theta square}} in hand, Stage 05 uses it twice—in two completely different geometric configurations—to supply the two halves of Theorem C's input on the indefinite target space $Y_0$:

![Side-by-side geometric diagram comparing the triangular lift array in NormOfChain on the left against the two square grids meeting along the diagonal in SpanningForms on the right](assets/stage05-triangle-and-two-grid-turns.svg)

### Left panel: Sliding lifts across a triangular array (`NormOfChain.lean`)

On the target space $D.\mathrm{targetForms}$ (good forms on $Y_0$), the $m$-step chain map `D.Θ ϑ` (called $\iota_\vartheta$ in upstream comments) lifts target forms along $Y_0 \to Y_1 \to \cdots \to Y_m$ for an {{concept:regular-twist-family|m-fold twist}} $\vartheta$. Measuring how much `D.Θ ϑ` stretches volumes requires its {{concept:gram-determinant|squared Gram determinant}} $Z(\vartheta) = \det(\Theta_\vartheta^* \Theta_\vartheta)$ (`D.chainDeterminant ϑ`; note that upstream docstrings call `u.normDet ^ 2 = det(u^* u)` the *"volume"* of $u$, while Lean names it *"determinant"*).

Why can we not compute $Z(\vartheta)$ in one step? Because after the very first lift, the chain exits the convergent range of the {{concept:rallis-inner-product|Rallis inner product formula}}. Instead, `ChainNorms.lean` and `NormOfChain.lean` embed the chain inside a {{term:rallis-inner-product|triangle of lifts}} indexed by intervals $(i, n)$:
1. **Slide lifts past descents:** Applying the {{term:theta-square-law}} across every square in the triangle pushes all descents past all lifts until $\Theta_\vartheta^* \Theta_\vartheta$ collapses onto a scalar multiple of the {{term:rallis-inner-product|Rallis product}} $R_\vartheta = R_1 \cdots R_m$ of $m$ first-step spherical {{concept:hecke-operator|Hecke operators}} on $Y_0$:
   $$\Theta_\vartheta^* \Theta_\vartheta = c(\vartheta)\, c_Q^m \, R_\vartheta \qquad (\texttt{exists\_adjoint\_chain\_comp\_eq\_smul\_rallisProduct}).$$
   Watch a crucial honesty point here: the intermediate predicate `HasAbsUpToNormPower` allows the constant $C$ in $\|c(\vartheta)\|^2 = C \cdot \mathrm{Nm}(Q)^k (\prod \mathrm{quotientRatio})^2$ to be $0$. Strict positivity ($C > 0$, `liftDeterminant_pos`, and `chainDeterminant_pos`) is **not** forced by the square law; it is proved separately from the injectivity of the lifts (`lowerChainAtLevel_injective`) in the convergent Rallis range.
2. **Transfer determinants from $Y_0$ to $W_j$:** For each $1 \le j \le m$, the single lift `D.goodLift ϑ j` maps good forms on the definite companion space $W_j$ to $V_j$ (note the argument convention: `goodLift ϑ j` takes a single `TameChar`, whereas `liftDeterminant ϑ j` takes an `MFoldTwist` and evaluates the $j$-th lift at $\vartheta^j$). Borrowing Stage 04's {{concept:trace-comparison|trace comparison across signatures}} (`det_rallisOnGood_eq`) replaces $\det(R_j|_{W_j})$ by $\det(R_j|_{Y_0})$ (`exists_normDet_goodLift_sq_eq`).
3. **Exact factorization and height of the quotient scalar:** Taking determinants and dividing by $\vartheta = 1$ cancels all unknown base constants $C$ and $c_Q$, yielding **`exists_chainDeterminant_pos_and_div_chainDeterminant_one_eq`**:
   $$\frac{Z(\vartheta)}{Z(1)} = \Bigl(\mathrm{Nm}(Q)^{e/2} \prod_{1 \le i < j \le m} \mathrm{quotientRatio}_{i,j}(\vartheta)\Bigr)^d \prod_{j=1}^m \frac{Z_j(\vartheta^j)}{Z_j(1)}.$$
   Under {{term:regular-twist-family|small ratios}}, each of the $\binom{m}{2} \le m^2$ {{concept:hecke-l-value-ratio|quotient ratios}} contributes $t$ inverse critical ratios of {{concept:arithmetic-height|height}} at most $x_1$, so the inverse scalar has height at most $m^2 t x_1$ (`HasSmallRatios.heightLe_inv_rpow_mul_prod_quotientRatio`), which `LongForms.lean` upgrades via `D.le_of_hasLogBound` to $\le b$.

Finally, `NormOfChain.lean` proves `D.range_goodLift_le`: the range of `D.goodLift ϑ j` is spanned by lifts of good Hecke eigenforms landing inside a good polynomial-label idempotent—an interface exported directly to Stages 06 and 07 (`06_one_lift/gluing_maps`, `06_one_lift/lift_volume_height`, `07_congruences/fit_twists_exist`).

### Right panel: Two square grids, $m^2$ turns, and basis-completion lifts (`SpanningForms.lean`)

Theorem C also needs a full spanning family of {{concept:auxiliary-spanning-form|auxiliary spanning forms}} on $Y_0$ whose $m$-step lifts stay integral at every {{concept:prime-number|prime}} $p \nmid \mathrm{Nm}(Q)$ (after one shared twist scalar) and stay bounded in archimedean norm at the trivial twist. Because $Y_0$ is indefinite, Stage 05 cannot construct those forms directly on $Y_0$.

Instead, `GridPaths.lean`, `DiagonalTurns.lean`, and `SpanningForms.lean` lay down **two square grids** of Hermitian spaces, $Y^a_b$ and $\widetilde{Y}^b_a$ ($0 \le a, b \le m$), meeting along the shared diagonal $(l, l)$, and trace monotone {{term:auxiliary-spanning-form|two-grid paths}} from $Y_m$ to the deep-level definite corner $\widetilde{Y}^m_0$:
- **First path:** runs down the actual lower chain $Y_0 \to Y_m$ paired against a surjective restriction map $r$ from deep-level forms $\varphi$ on $\widetilde{Y}^m_0$ onto principal-level forms on $Y_0$.
- **$m^2$ turns across the grids:** Step by step, the path sweeps across $m^2$ squares—off-diagonal turns via the {{term:theta-square-law}} and {{term:auxiliary-spanning-form|diagonal turns}} via {{concept:seesaw-duality|seesaw moves on the shared space $Y^{j+1}_j = \widetilde{Y}^{j+1}_j$}}—transferring the first-path period into a {{term:auxiliary-spanning-form|last-path period}} $\Xi$ divided by a scalar $c(\vartheta)$ controlled by $m^2$ {{term:hecke-l-value-ratio|turn ratios}} (`exists_integral_lowerChainUpTo_eq_lastPathPeriod`).
- **Reading values with delta forms:** Along the last path, every space is definite and explicit: clearing kernel denominators (`D.liftDenominator`) and the middle branching pairing yields $p$-integrality up to $\exp(x^{99} b^2)$ (`exists_mul_lastPathPeriod_mem_embValRing`) and archimedean growth at most $\exp(x^{200} b^2)$ (`norm_lastPathPeriod_le`). Testing the first path against a dual-weight {{term:auxiliary-spanning-form|delta form}} $\delta_x(v)$ supported on a single finite class $x$ uses Schur orthogonality (`integral_lastPathConjDelta_mul`) to read off the exact value of $\Theta_\vartheta(r \varphi)$ at $x$ (`exists_dualPairing_eq_mul_lastPathPeriod`).
- **Lifting short deep-level forms:** By Stage 03's short-form machinery (`span_isShort_eq_top_gridLevelForms`), deep-level forms on $\widetilde{Y}^m_0$ are spanned by forms with {{concept:weyl-gauge|p-adic gauge $\le 1$}} and squared norm $\le \exp(x^{99} b^2)$. Pushing them through $r$ yields **`New.Data.exists_span_short_lifts_eq_top`**: principal-level forms on $Y_0$ are spanned by forms whose scaled lifts $(c(\vartheta)/c(1))\,\mathrm{lowerChainAtLevel}\ \vartheta\ y$ are $p$-integral for all $p \nmid \mathrm{Nm}(Q)$ and whose untwisted lifts have squared Weyl norm at most $\exp(x^{202} b^2)$.

### Where this analogy breaks

Comparing the triangle of lifts in `NormOfChain` and the two square grids in `SpanningForms` as "two applications of the same square commutation rule" captures their shared dependency on `OneSquare.lean`, but misses an important structural contrast. In `NormOfChain`, the diagonal is the base of the triangle where the single-step Rallis formula already converges—so sliding moves stop at $m$ explicit Hecke operators whose determinants transfer to $W_j$ via trace comparison. In `SpanningForms`, crossing the diagonal $(l, l)$ between $Y^a_b$ and $\widetilde{Y}^b_a$ is *not* an `OneSquare` move at all: it requires an extra seesaw on the common space $S = Y^{j+1}_j = \widetilde{Y}^{j+1}_j$ (`DiagonalTurns.lean`) that reverses a line kernel and swaps the two grids before reaching the definite corner $\widetilde{Y}^m_0$.

## Level 2 — Undergrad Bridge

### 1. Chain determinant factorization (`NormOfChain.lean`, 24 decls, 1 truncation)

1. **Definitions and positivity (`normDet_sq_eq_pow_mul_det`, `liftDeterminant`, `normDet_goodLift_sq_pos`, `liftDeterminant_pos`, `Θ`, `chainDeterminant`, `chainDeterminant_pos`):**
   - `D.liftDeterminant ϑ j := ((D.goodLift (ϑ.pow j) j).normDet : ℂ) ^ 2` (using $\vartheta^j$);
   - `D.Θ ϑ` restricts `D.lowerChainAtLevel ϑ` to `D.targetForms`, and `D.chainDeterminant ϑ := ((D.Θ ϑ).normDet : ℂ) ^ 2`;
   - Injectivity of single lifts and of `lowerChainAtLevel` (`ChainNorms.lean`) yields `liftDeterminant_pos` and `chainDeterminant_pos`.
2. **Single-lift determinant transfer (`exists_normDet_goodLift_sq_eq`):**
   Combines the single-lift Rallis inner product on $W_j$ with `normDet_sq_eq_pow_mul_det`, `targetDim_eq_finrank_goodForms_spaceW`, and Stage 04's `det_rallisOnGood_eq` to express `((D.goodLift ϑ j).normDet)^2` directly as $(A \cdot c_Q)^{d} \cdot (D.\mathrm{rallisOnGood}\ Y_0\ \vartheta\ j).\mathrm{det}$ for $A \neq 0$ independent of $\vartheta$.
3. **Triangle Gram form and scalar positivity (`rallisProduct`, `exists_inner_weightedChain_eq`, `exists_inner_chain_eq`, `isRamifiedRatio_of_mem_sigma`, `card_sigma_Ioc_le`, `prod_quotientRatio_one`, `prod_quotientRatio_nonneg`, `HasAbsUpToNormPower.exists_norm_div_eq_mul_prod_quotientRatio`, `exists_adjoint_chain_comp_eq_smul_rallisProduct`):**
   `exists_inner_weightedChain_eq` evaluates the Gram form of the weighted triangle chain against `D.rallisProduct ϑ`, and rescaling to `D.Θ ϑ` gives `exists_inner_chain_eq` with a scalar $c(\vartheta)$ obeying `HasAbsUpToNormPower` (which permits $C = 0$). Evaluating at $\vartheta = 1$ where `chainDeterminant_pos` guarantees $c(1) \neq 0$ upgrades this to a strictly positive $C > 0$ in `exists_adjoint_chain_comp_eq_smul_rallisProduct`.
4. **Determinant factorization and height bound (`exists_chainDeterminant_eq_pow_mul_prod_liftDeterminant`, `exists_div_chainDeterminant_one_eq`, `exists_chainDeterminant_pos_and_div_chainDeterminant_one_eq`, `HasSmallRatios.heightLe_inv_rpow_mul_prod_quotientRatio`):**
   Taking determinants in `exists_adjoint_chain_comp_eq_smul_rallisProduct` and substituting `exists_normDet_goodLift_sq_eq` cancels $c_Q^{m d}$; dividing by $\vartheta = 1$ cancels $C^d$. Each pair $1 \le i < j \le m$ (at most $m^2$ pairs by `card_sigma_Ioc_le`) is a ramified ratio of $t$ critical $L$-value ratios, so `HasSmallRatios θ` bounds the inverse scalar height by `D.m * D.m * (D.t * D.x₁)`.
5. **Eigenform spanning interface (`range_tupleIdem_le`, `singleLift_asAlgebraHom`, `range_goodLift_le`):**
   Equivariance of `D.singleLift ϑ j` with `D.chosenGL` (`singleLift_asAlgebraHom`) shows that `(D.goodLift ϑ j).range` is spanned by lifts of good Hecke eigenforms landing in the range of some good `tupleIdem D.RowIndex c`.

### 2. Two-grid turns and short auxiliary lifts (`SpanningForms.lean`, 29 decls, 0 truncations)

1. **Last-path $p$-integrality (`lastPathBound`, `liftDenominator`, `liftDenominator_smul_lastPathEdgeDatum_mem_raises`, `generator_inv_mul_normQ_mul_star_mem`, `norm_mul_det_flipScalar_zpow_le_one`, `exists_norm_mul_lastPathPairing_le_one`, `exists_mul_lastPathPeriod_mem_embValRing`):**
   Clears every edge kernel on the last path at primes $p \nmid \mathrm{Nm}(Q)$ by `D.liftDenominator` (a product of powers of $M$ and $P.d$), controls the flip scalar $u = \overline{t\alpha}/(t\alpha)$ at the middle branching pairing ($\log N \le x^{98} b^2$), and concludes that $0 < N \le \exp(x^{99} b^2)$ clears `lastPathPeriod Ψ y` into `embValRing`.
2. **Last-path archimedean conjugates (`card_stdClasses_lastPathLevel_le`, `sqrt_weylNormSq_lastPathKernel_le`, `norm_lastPathPeriod_le`):**
   At $\vartheta = 1$, class numbers and single-edge kernel operator norms are each bounded by $\exp(x^{99} b^2)$, yielding $\|\sigma(\mathrm{lastPathPeriod}\ \Psi\ y)\| \le \exp(x^{200} b^2)\sqrt{B_{\mathrm{false}} B_{\mathrm{true}}}$ for all $\sigma \in \operatorname{Aut}(\mathbb{C})$.
3. **Period transfer and delta-form readout (`exists_integral_lowerChainUpTo_eq_lastPathPeriod`, `DualVector`, `dualPairing`, `isDualPairing_dualPairing`, `dualHook`, `dualHook_mul_exp_le`, `mem_embValRing_of_dualPairing_eq_dualHook_mul`, `weylNormSq_le_of_dualPairing`, `lastPathDelta`, `lastPathConjDelta`, `lastPathToAlgForm_lastPathConjDelta`, `fixedByOuterLevels_lastPathConjDelta`, `integral_lastPathConjDelta_mul`, `weylGauge_lastPathConjDelta_le_one`, `weylNormSq_lastPathConjDelta_le`, `exists_dualPairing_eq_mul_lastPathPeriod`):**
   `exists_integral_lowerChainUpTo_eq_lastPathPeriod` imports the $m^2$-turn transfer from `DiagonalTurns.lean`. Inserting the dual-weight delta form $\delta_x(v) = \mathrm{lastPathConjDelta}\ \vartheta\ x\ v$ supported on the class of $x$ uses Schur orthogonality (`integral_lastPathConjDelta_mul`, dividing by the weight dimension `card (WeylIndex (D.Y D.m) D.columnsY)`) to express the dual pairing with $((c(\vartheta)/c(1))\,\mathrm{lowerChain}\ \vartheta\ (r\varphi))(x)$ directly as $\beta \cdot \mathrm{lastPathPeriod}$ (`exists_dualPairing_eq_mul_lastPathPeriod`).
4. **Spanning by short lifts (`exists_lowerChain_integral_and_weylNormSq_le`, `span_isShort_eq_top_gridLevelForms`, `exists_span_short_lifts_eq_top`):**
   Duality recovers coordinate-wise $p$-integrality (clearing `D.dualHook`) and `weylNormSq` bounds ($\exp(2 x^{201} b^2) \cdot B$). Feeding the short spanning family on $\widetilde{Y}^m_0$ (`span_isShort_eq_top_gridLevelForms`, bound $\exp(x^{99} b^2)$) through the surjective restriction $r$ establishes **`New.Data.exists_span_short_lifts_eq_top`** with archimedean bound $\exp(x^{202} b^2)$.

## Level 3 — Formal Lean Contract

The extracted skeletons `skeleton/05_determinant/norm_of_chain.lean` (24 declarations, **1 stub truncation**) and `skeleton/05_determinant/spanning_forms.lean` (29 declarations, 0 stub truncations) mirror `src/New/Determinant/Chain/NormOfChain.lean` and `src/New/Determinant/AuxiliaryForms/SpanningForms.lean` at pinned upstream commit `41c2716658c42ba08eb0648994a2efe83082a95c`.

### Critical Honesty Disclosure: One `let u :=` Stub Truncation in `norm_of_chain.lean`

In `skeleton/05_determinant/norm_of_chain.lean`, `New.Data.exists_inner_weightedChain_eq` (L86) introduces a `let u :=` binding inside its return type, causing `rh-extract` to truncate the extracted stub immediately at `let u := by sorry`. Its **exact untruncated statement** from upstream `src/New/Determinant/Chain/NormOfChain.lean:162–171` (`41c2716658c4`) is quoted verbatim below:

```lean
-- Upstream: src/New/Determinant/Chain/NormOfChain.lean:162-171
-- (truncated at `let u :=` in skeleton/05_determinant/norm_of_chain.lean:86)
lemma exists_inner_weightedChain_eq :
    ∃ κ : D.MFoldTwist → ℂ,
      D.HasAbsUpToNormPower κ (∏ i ∈ Ioc 0 D.m, ∏ j ∈ Ioc i D.m, D.quotientRatio i j) ∧
      ∀ (ϑ : D.MFoldTwist) (x y : D.targetForms),
        let u := Exchange.weightedChain (D.triangleGLCard false) (D.triangleLift false) ϑ 0 D.m
        inner ℂ (u x.val.val) (u y.val.val) =
          κ ϑ * (triangleGLCard false ϑ 0 * triangleGLCard false ϑ 1 ^ D.m /
            triangleGLCard false ϑ D.m) /
            Fintype.card (D.Y 1 × D.RowIndex → (D.twistModulus ϑ 1).ResidueRing) ^ D.m *
            inner ℂ x (D.rallisProduct ϑ y)
```

All 23 remaining declarations in `norm_of_chain.lean` and all 29 declarations in `spanning_forms.lean` extract completely untruncated.

### Untruncated Headline Signatures

From `skeleton/05_determinant/norm_of_chain.lean` and `skeleton/05_determinant/spanning_forms.lean`:

```lean
theorem New.Data.exists_chainDeterminant_pos_and_div_chainDeterminant_one_eq :
    ∃ e : ℤ, ∀ ϑ : D.MFoldTwist,
      0 < D.chainDeterminant ϑ ∧
      (∀ j : D.LiftIndex, 0 < D.liftDeterminant ϑ j) ∧
      D.chainDeterminant ϑ / D.chainDeterminant 1 =
        (((ϑ.normQ : ℝ) ^ (e / 2 : ℝ) *
            ∏ i ∈ Ioc 0 D.m, ∏ j ∈ Ioc i D.m, D.quotientRatio i j ϑ : ℝ) : ℂ) ^ D.targetDim *
          ∏ j ∈ Icc 1 D.m, D.liftDeterminant ϑ j / D.liftDeterminant 1 j

lemma New.Data.HasSmallRatios.heightLe_inv_rpow_mul_prod_quotientRatio {D : Data P}
    {θ : PrimePadicIso → D.MFoldTwist} (hθ : D.HasSmallRatios θ) (e : ℤ) :
    HeightLe (D.m * D.m * (D.t * D.x₁)) fun ι : PrimePadicIso ↦
      ((((θ ι).normQ : ℝ) ^ (e / 2 : ℝ) *
        ∏ i ∈ Ioc 0 D.m, ∏ j ∈ Ioc i D.m, D.quotientRatio i j (θ ι) : ℝ) : ℂ)⁻¹

lemma New.Data.exists_span_short_lifts_eq_top : ∃ c : D.MFoldTwist → ℂˣ,
    D.HasAbsUpToNormPower (fun ϑ : D.MFoldTwist ↦ c ϑ)
      (∏ i ∈ range D.m, ∏ j ∈ range D.m, D.turnRatio i j) ∧
    span ℂ {y : D.principalForms (D.spaceY 0) |
      (∀ (ϑ : D.MFoldTwist) (l : ℕ) [Fact l.Prime] (place : PadicPlace l), l.Coprime ϑ.normQ →
        (c ϑ / c 1 : ℂ) • D.lowerChainAtLevel ϑ y ∈ integralForms (l := l) place) ∧
      ∀ (σ : ℂ ≃+* ℂ) (g : Gf D.E (D.Y D.m)),
        weylNormSq (σ ∘ (D.lowerChain .one y).val g) ≤ exp (D.x ^ 202 * D.b ^ 2)} = ⊤
```

- **Verification status (`audit/verification-status.json`):** Local Lean compilation and kernel replay remain **`build_status: unverified-locally`** at pinned commit `41c2716658c42ba08eb0648994a2efe83082a95c` (`41c2716658c4`). Static token scanning has **`scan_scope: entry`**, covering only `Challenge.lean`, `src/Main.lean`, and `src/Statements.lean` (`0` forbidden tokens in solution files, `5` expected `sorry` tokens in `Challenge.lean`). Neither `NormOfChain.lean` nor `SpanningForms.lean` has been locally compiled, kernel-checked, or statically scanned here.

<!-- rh:lean-node:05_determinant/norm_of_chain -->

<!-- rh:lean-node:05_determinant/spanning_forms -->
