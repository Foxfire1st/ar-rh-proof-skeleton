# Relative-Trace Unfolding, Bessel Translates, and the Two Moment Bounds

This page explains the central unfolding hub of Stage 04 — `src/New/Spanning/Unfolding/Moments.lean` (`04_spanning/moments`, 40 declarations, 0 stub truncations) — together with its upstream Bessel-vector foundation in `src/New/Spanning/Bessel/{LargeFunction,SchurBound,CutTailBound}.lean` and the Laplace-method period lower bound in `src/New/Appendix/DiscreteSeries/Periods.lean`. Return to the [Stage 04 Overview](./overview.md), read [Target Dimension, Multiplicity Equality & Spanning](./equal-multiplicities-and-spanning.md), or continue to [Kloosterman Sieve & Remaining Terms](./main-term-sieve-and-remaining-terms.md).

## Level 1 — First-Principles

In the previous essay, `SpanningTheorem.lean` turned two operator inequalities on the scaled Poincaré map $P$—good-forms {{concept:hilbert-schmidt-norm|near isometry}} and the whole-group second moment—into the spreading theorem. Where do those two inequalities come from? They come from opening up the operator $P_s = P^* \rho(s) P$ on the source space and reading its matrix entries directly.

Because $P$ is constructed by averaging a local Bessel function $W$ over rational group elements, pairing $\rho(s) P$ against $P$ unfolds into a block matrix indexed by pairs of double-coset classes $(c, c')$ of the definite subgroup $U(X_0)$. Each block is the complex conjugate of a normalized sum of {{concept:bessel-orbital-integral|archimedean overlap integrals $A_\gamma = \int \beta_c(y)\beta_{c'}(\gamma^{-1} y) W(y) \otimes W(\gamma^{-1} y)^* dy$}} over rational elements $\gamma$ whose finite-prime reduction matches $s$, divided by the {{term:bessel-orbital-integral|unipotent covolume}} $\nu$ (`HasAmplitudeBlocks`).

Once $P_s$ is unfolded into these {{term:bessel-orbital-integral|amplitude blocks}}, `Moments.lean` separates the finite level group $\bar{G} = G_{p_0} \times G_{p_1}$ into three clean regimes:

![Matrix block anatomy of P_s = P* rho(s) P unfolded into Bessel orbital amplitude sums across the Bessel subgroup R_0, the open generic cell, and the remaining-term regimes](assets/stage04-unfolding-block-anatomy.svg)

1. **Near the Bessel subgroup $\bar{R}_0$ (why $P^* P \approx 1$):** Rational elements $\gamma$ inside the Bessel subgroup do not scramble orbits; their block sum builds an explicit unitary {{term:bessel-orbital-integral|Bessel translate}} operator $r_\psi(s)$ on the source space that equals the identity at $s = 1$ and intertwines $\rho|_{\bar{R}_0}$ with $P$ ($\rho(t) P = P r_\psi(t)$). Whenever $s \in \bar{G}$ agrees with $\bar{R}_0$ at one of the two level {{concept:prime-number|primes}}, every term *outside* the Bessel subgroup adds up to at most $b^{-12} \mathcal{D}_{\mathrm{id}}$ (`sum_hilbertSchmidtNorm_sub_besselTranslate_sq_le`). Because $\|P_t - r_\psi(t)\|_{\mathrm{HS}}^2 = \|P^* P - 1\|_{\mathrm{HS}}^2$ is identical across all $t \in \bar{R}_0$, dividing by $\#\bar{R}_0$ immediately forces
   $$\|P^* P - 1\|_{\mathrm{HS}}^2 \le b^{-12} D_X \qquad (\texttt{hilbertSchmidtNorm\_sub\_one\_sq\_le}).$$
2. **Across the whole group $\bar{G}$ (the second moment):** Summing $\|P_s\|_{\mathrm{HS}}^2$ over all $s \in \bar{G}$ combines the Bessel-subgroup contribution, the truncation error between the full and {{term:bessel-orbital-integral|cut Bessel function}}, the big-cell {{concept:kloosterman-sieve|one-sided Poisson summation sieve}} from `EnergyEstimate.lean`, and the {{concept:link-lattice|common-link sublattice and weighted Schur test bounds}} from `RemainingTerms/Sum.lean` to prove
   $$\sum_{s \in \bar{G}} \|P^* \rho(s) P\|_{\mathrm{HS}}^2 \le \bigl(1 + 9 b^{-4}\bigr)\,\mathcal{D}_{\mathrm{main}} \qquad (\texttt{sum\_hilbertSchmidtNorm\_sq\_le}).$$
3. **Why two level primes $p_0, p_1$ make bad types harmless:** Knowing $P^* P \approx 1$ is not enough on its own—Stage 04 needs the *projected* map $e_0 P$ onto good forms to stay nearly isometric, where $e_0 = e_{p_0} \otimes e_{p_1}$ is a tensor product of two prime-level {{concept:central-idempotent|orthogonal projections selecting allowed primitive polynomial labels}}. Because there are **two** primes ($M = p_0 p_1$), Stage 04 splits the complement
   $$1 - e_{p_0} \otimes e_{p_1} = (1 - e_{p_0}) \otimes 1 + e_{p_0} \otimes (1 - e_{p_1})$$
   and bounds each prime $p_i$ while holding the *other* prime $p_{i+1}$ fixed at $1$. Holding $s(i+1) = 1$ forces every contributing Bessel-subgroup element to become purely unipotent (`mem_besselU_of_reduction_eq_one`), so $\operatorname{tr}(r_\psi(s))$ simplifies to $D_X$ times the unipotent character $\psi_U(s)$. Averaging over conjugates replaces $\psi_U$ by its class-function mean $\psi_U^\natural$, which satisfies the sharp $L^2$ bound $\sum_{s \in \bar{G}} |\psi_U^\natural(s)|^2 \le 2$ (`sum_norm_reducedCharMean_sq_le`). Pairing $\sqrt{2}$ with the bad-type share $\sqrt{b^{-8}/2}$ gives at most $b^{-4} D_X$ per prime (plus $b^{-6} D_X$ from non-Bessel remainders), summing across both primes to $\|(1 - e_0) P\|_{\mathrm{HS}}^2 \le 4 b^{-4} D_X$, and combining with $\|P^* P - 1\|_{\mathrm{HS}}^2 \le b^{-12} D_X$ to yield the near-isometry error $5 b^{-4}$ (`isNearIsometry_piTensorElem_comp`).

### Where this analogy breaks

Presenting `Moments.lean` as a clean three-regime decomposition of one block matrix skips an essential archimedean bridge that sits underneath `HasAmplitudeBlocks`. The Poincaré map unfolds natively to the *full* discrete-series Bessel function $W$ (`.full`), which is smooth and integrable but *not* compactly supported modulo the Bessel subgroup. By contrast, both `EnergyEstimate.lean` and `RemainingTerms/Sum.lean` require the compactly supported {{term:bessel-orbital-integral|cut Bessel function}} $W^\flat$ (`.cut`), cut off at Bessel length $b^{6n+2}$. Ensuring that $W$ is non-degenerate at a stationary point (`LargeFunction.lean` + `Appendix/DiscreteSeries/Periods.lean`'s Laplace method), controlling its derivatives via Harish-Chandra Schur orthogonality (`SchurBound.lean`), and proving that $W - W^\flat$ has negligible $L^2$ tail (`CutTailBound.lean`) take 21 upstream files before `Moments.lean` ever runs.

## Level 2 — Undergrad Bridge

### 1. Amplitude blocks and the unitary Bessel translate $r_\psi(s)$ (`Moments.lean`, lines 75–397)

Let $S : \mathrm{Filling}\ E\ \mathfrak{s}$ be admissible filling data over a CM field $E$ carrying `WeilData E` (in particular `HasPrimitiveRoot5040 E`), with finite residue group $\bar{G} = S.\mathrm{ResidueGL} = G_0 \times G_1$ above the two level primes $p_0, p_1$, source space $H_{\mathrm{src}} = \mathrm{EuclideanSpace}\ \mathbb{C}\ (S.\mathrm{Classes} \times S.\mathrm{BesselCoord})$ of dimension $D_X = S.\mathrm{sourceDim}$, and unitary representation $\rho : \bar{G} \to U(V)$.

- **`HasAmplitudeBlocks ρ P`** states that for all $s \in \bar{G}$ and basis vectors $e_{(c, i)}, e_{(c', j)}$, the $(c, c')$ block of $P_s = P^* \rho(s) P$ is $\overline{A^{\mathrm{full}}(c, s, c') / \nu}$, where $A^{\mathrm{full}}(c, s, c') = S.\mathrm{amplitudeSum}\ \mathrm{.full}\ \mathrm{.univ}\ c\ s\ c'$ sums the archimedean orbital integrals over all rational $\gamma \in S.\mathrm{termsOver}\ c\ s\ c'$ and $\nu = S.\mathrm{unipotentCovolume} > 0$. By `hasAmplitudeBlocks_smul_of_inner_eq`, rescaling an unnormalized Poincaré map by $(c\nu)^{-1/2}$ achieves `HasAmplitudeBlocks`, and `hilbertSchmidtNorm_sq_eq_secondMoment` identifies
  $$\|P_s\|_{\mathrm{HS}}^2 = \mathrm{secondMoment}\bigl(S.\mathrm{amplitudeSum}\ \mathrm{.full}\ \mathrm{.univ}\bigr)\ \{s\}.$$
- **`besselTranslate s`** ($r_\psi(s) : H_{\mathrm{src}} \to_{\mathbb{C}} H_{\mathrm{src}}$) is defined by the same block formula summed *only* over $\gamma \in \mathrm{besselS}$ (block unitriangular elements times $U(X_0)$). Its algebraic laws are:
  - $r_\psi(s) = 0$ outside $\bar{R}_0 = S.\mathrm{reducedImage}\ \mathrm{besselS}$ (`besselTranslate_eq_zero`);
  - $r_\psi(1) = 1$, $r_\psi(st) = r_\psi(s) r_\psi(t)$ for $s, t \in \bar{R}_0$, $r_\psi(ts) = r_\psi(t) r_\psi(s)$ for $t \in \bar{R}_0, s \in \bar{G}$, and $r_\psi(s)^* = r_\psi(s^{-1})$ (`besselTranslate_one`, `besselTranslate_mul`, `besselTranslate_mul_left`, `adjoint_besselTranslate`);
  - two-sided equivariance: $P_{st} = P_s r_\psi(t)$, $\rho(t) P = P r_\psi(t)$, and $P_{ts} = r_\psi(t) P_s$ for all $t \in \bar{R}_0$ (`adjoint_comp_rep_mul_right`, `rep_comp_eq_comp_besselTranslate`, `adjoint_comp_rep_mul_left`).

### 2. The two core Hilbert–Schmidt bounds (`Moments.lean`, lines 399–583)

1. **Near-$\bar{R}_0$ bound and $P^* P \approx 1$:**
   Subtracting $r_\psi(s)$ removes the Bessel-subgroup terms: $\|P_s - r_\psi(s)\|_{\mathrm{HS}}^2 = \mathrm{secondMoment}(S.\mathrm{amplitudeSum}\ \mathrm{.full}\ \mathrm{besselS}^c)\ \{s\}$ (`hilbertSchmidtNorm_sub_besselTranslate_sq`). Pulling in `Unfolding/SquareExpansion.lean` (which consumes `RemainingTerms/Sum.lean` and the cut-tail bounds), **`sum_hilbertSchmidtNorm_sub_besselTranslate_sq_le`** bounds the slice where $s$ agrees with $\bar{R}_0$ above the complementary prime $p_{i+1}$:
   $$\sum_{\substack{s \in \bar{G} \\ \exists t \in \bar{R}_0,\, s_{i+1} = t_{i+1}}} \|P_s - r_\psi(s)\|_{\mathrm{HS}}^2 \le b^{-12} \mathcal{D}_{\mathrm{id}}, \qquad \mathcal{D}_{\mathrm{id}} = \#\bar{R}_0 \cdot D_X.$$
   For every $t \in \bar{R}_0$, unitary equivariance gives $\|P_t - r_\psi(t)\|_{\mathrm{HS}}^2 = \|r_\psi(t)(P^* P - 1)\|_{\mathrm{HS}}^2 = \|P^* P - 1\|_{\mathrm{HS}}^2$. Restricting the sum to $t \in \bar{R}_0$ gives $\#\bar{R}_0 \cdot \|P^* P - 1\|_{\mathrm{HS}}^2 \le b^{-12} \mathcal{D}_{\mathrm{id}}$, and dividing by $\#\bar{R}_0$ (`le_sourceDim_div_of_card_nsmul_le`) proves **`hilbertSchmidtNorm_sub_one_sq_le`**:
   $$\|P^* P - 1\|_{\mathrm{HS}}^2 \le \frac{D_X}{b^{12}} \qquad \bigl(\text{hence also } \|P^* P\|_{\mathrm{HS}}^2 \le 2 D_X\bigr).$$
2. **Whole-group second moment (`sum_hilbertSchmidtNorm_sq_le`):**
   Pulling in `Unfolding/MomentBounds.lean`—which partitions $\bar{G}$ into open reductions (`EnergyEstimate.lean::linkSecondMoment_isOpenElement_le`, contributing $(1 + b^{-4})\mathcal{D}_{\mathrm{main}}$), meeting reductions (`RemainingTerms/Sum.lean::linkSecondMoment_le_of_le_meetNorm`), cross-link/$\mathsf{g}=0$ terms (`secondMoment_remainingTerms_le`), Bessel-subgroup terms, and the `.full`-vs-`.cut` tail—proves
   $$\sum_{s \in \bar{G}} \|P^* \rho(s) P\|_{\mathrm{HS}}^2 \le \Bigl(1 + \frac{9}{b^4}\Bigr)\,\mathcal{D}_{\mathrm{main}}, \qquad \mathcal{D}_{\mathrm{main}} = \frac{D_X^2}{\mathrm{deg}\cdot \mathrm{mass}_r}.$$

### 3. Two level primes and the bad-type projection loss (`Moments.lean`, lines 591–1256)

Let $g_i \in \mathbb{C}[G_i]$ ($i \in \{0, 1\}$) be central idempotents above $p_0, p_1$ with $\operatorname{Re}((1 - g_i)(1)) \le b^{-8}/2$, and let $e_0 = g_0 \otimes g_1 = \mathrm{piTensorElem}\ g$. Setting $x_i = 1 - g_i$ (embedded in $\bar{G}$ via `Pi.mulSingle i`, so the *other* prime component is $1$):

1. **Remainder term (`hilbertSchmidtNorm_sum_smul_sub_besselTranslate_sq_le`):**
   Because every $t \in G_i$ embedded via `Pi.mulSingle i` has $(i+1)$-component equal to $1 \in \bar{R}_{0, i+1}$, the sum $\sum_{t \in G_i} x_i(t)(P_t - r_\psi(t))$ lies inside the near-$\bar{R}_0$ slice. By Cauchy–Schwarz and `sum_norm_sq_coeff_of_isCentralProj` ($\sum |x_i(t)|^2 = \operatorname{Re} x_i(1) \le 1$),
   $$\Bigl\|\sum_{t \in G_i} x_i(t)(P_t - r_\psi(t))\Bigr\|_{\mathrm{HS}}^2 \le \frac{D_X}{b^{12}}.$$
2. **Why holding the second prime at $1$ isolates unipotent elements (`mem_besselU_of_reduction_eq_one` $\to$ `trace_besselTranslate_eq_mul_reducedChar`):**
   If $\gamma \in \mathrm{besselS}$ conjugates into $k = h_c^{-1} \gamma h_c \in S.\mathrm{latticeStabilizer}$ and reduces to $1$ above $p_{i+1}$, then the definite $U(X_0)$ factor of $\gamma$ must be $1$ (free level above $p_{i+1}$), forcing $k \in \mathrm{besselU}$. Consequently, $\operatorname{tr}(r_\psi(s)) = D_X \cdot \psi_U(s)$ whenever $s_{i+1} = 1$, where $\psi_U = S.\mathrm{reducedChar}$ is the additive character $u \mapsto \psi_f(-\sigma(u)/(M\varpi))$ extended by $0$ off $\bar{U}_0$.
3. **Induced unipotent character $L^2$ bound (`sum_norm_reducedCharMean_sq_le`):**
   Averaging $\psi_U$ over conjugates in $\bar{G}$ produces the class function $\psi_U^\natural = S.\mathrm{reducedCharMean}$, which matches the local $K$-orbital integral $\operatorname{vol}(K)^{-1} \int_K \chi_M(y l y^{-1})\,dy$ (`extendedBesselChar_toPlaces_eq_reducedChar`, `integral_extendedBesselChar_conj_eq`). Evaluating the $L^2$ sum of conjugate integrals (`sum_sq_norm_integral_extendedBesselChar_conjugate_le`) gives
   $$\sum_{s \in \bar{G}} \bigl|\psi_U^\natural(s)\bigr|^2 \le 2.$$
4. **One prime $\to$ both primes $\to$ near-isometry:**
   Since $x_i$ is a class function, $\operatorname{tr}(r_\psi(x_i)) = D_X \sum_{t \in G_i} x_i(t) \psi_U^\natural(t)$, so Cauchy–Schwarz gives $\operatorname{Re}\,\operatorname{tr}(r_\psi(x_i)) \le D_X \sqrt{b^{-8}/2}\sqrt{2} = b^{-4} D_X$ (`re_sum_coeff_mul_trace_besselTranslate_le`). Adding the remainder contribution $b^{-6} D_X \le b^{-4} D_X$ yields $\|x_i P\|_{\mathrm{HS}}^2 \le 2 b^{-4} D_X$ (`hilbertSchmidtNorm_asAlgebraHom_comp_sq_le`). Applying `hilbertSchmidtNorm_one_sub_mul_comp_sq_le` to $1 - g_0 \otimes g_1$ sums the two prime losses:
   $$\bigl\|(1 - e_0) P\bigr\|_{\mathrm{HS}}^2 \le 4 b^{-4} D_X \qquad (\texttt{hilbertSchmidtNorm\_one\_sub\_piTensorElem\_comp\_sq\_le}).$$
   Finally, `IsNearIsometry.proj_comp` combines `IsNearIsometry P b⁻⁴` (from $b^{-6} \le b^{-4}$) with $\|(1 - e_0) P\|_{\mathrm{HS}}^2 \le 4 b^{-4} D_X$ to prove **`isNearIsometry_piTensorElem_comp`**: `IsNearIsometry (e₀ ∘ₗ P) (5 * b⁻⁴)`.

### 4. Upstream discrete-series Bessel vectors (`.full` vs. `.cut`)

The filling data `S = D.filling` instantiated in `Moments.lean` relies on four upstream archimedean pillars:
- **`src/New/Appendix/DiscreteSeries/Periods.lean` (`IsLaplaceDatum.laplace_method`):** Proves an explicit polynomial-in-$\beta$ lower bound on restriction periods of steep discrete-series matrix coefficients via Laplace's method near the identity (the integrand stays in a cone around the positive real axis up to exponentially small error).
- **`src/New/Spanning/Bessel/LargeFunction.lean` (`IsDiscreteSeriesBessel`, `IsSteep.exists_stationary_point`):** Uses Witt's theorem and `Periods.lean` to construct a stationary point $x$ of bounded Bessel length where the average $W(y) = c \int_R c_\lambda(x^{-1} s y)\pi(s)^{-1} v\,ds$ is large and reproduced under right convolution by the discrete-series idempotent $\mathrm{deg}\cdot c_\lambda$.
- **`src/New/Spanning/Bessel/SchurBound.lean` (`HasSchurBound`):** Proves smoothness and Harish-Chandra Schur-orthogonality $L^2$ bounds for all right derivatives of $W$ of order $\le 2 n^2$.
- **`src/New/Spanning/Bessel/CutTailBound.lean` (`besselCut`, `exists_isCutPair_besselAverage`):** Truncates $W$ to the compactly supported cut Bessel function $W^\flat$ (`besselCut`, supported on `besselLength g ≤ b^{6n+2}`) and bounds $\|W - W^\flat\|_{L^2}$ explicitly, allowing `Moments.lean` to pass safely between `.full` (unfolded Poincaré inner products) and `.cut` (sieve and remaining-term estimates).

## Level 3 — Formal Lean Contract

The extracted skeleton `skeleton/04_spanning/moments.lean` mirrors `src/New/Spanning/Unfolding/Moments.lean` at pinned upstream commit `41c2716658c42ba08eb0648994a2efe83082a95c`, containing **40 declarations and 0 stub truncations**.

### Headline Theorem Signatures

From `skeleton/04_spanning/moments.lean` (upstream `src/New/Spanning/Unfolding/Moments.lean:434–1256`):

```lean
theorem New.Filling.HasAmplitudeBlocks.sum_hilbertSchmidtNorm_sub_besselTranslate_sq_le [HasPrimitiveRoot5040 E]
    (hP : S.HasAmplitudeBlocks ρ P) (i : Fin 2) :
    ∑ s : S.ResidueGL with ∃ t ∈ S.reducedImage besselS, s (i + 1) = t (i + 1),
      hilbertSchmidtNorm (P.adjoint ∘ₗ ρ s ∘ₗ P - S.besselTranslate s) ^ 2 ≤
      (S.b ^ 12)⁻¹ * S.identitySecondMoment

theorem New.Filling.HasAmplitudeBlocks.hilbertSchmidtNorm_sub_one_sq_le [HasPrimitiveRoot5040 E]
    (hP : S.HasAmplitudeBlocks ρ P) :
    hilbertSchmidtNorm (P.adjoint ∘ₗ P - 1) ^ 2 ≤ S.sourceDim / S.b ^ 12

theorem New.Filling.HasAmplitudeBlocks.sum_hilbertSchmidtNorm_sq_le [WeilData E] (hP : S.HasAmplitudeBlocks ρ P)
    (hρ : ∀ (s : S.ResidueGL) (x y : V), ⟪ρ s x, ρ s y⟫_ℂ = ⟪x, y⟫_ℂ) :
    ∑ s : S.ResidueGL, hilbertSchmidtNorm (P.adjoint ∘ₗ ρ s ∘ₗ P) ^ 2 ≤
      (1 + 9 / S.b ^ 4) * S.mainSecondMoment

theorem New.Filling.sum_norm_reducedCharMean_sq_le [ok : S.IsAdmissible] :
    ∑ s : S.ResidueGL, ‖S.reducedCharMean s‖ ^ 2 ≤ 2

theorem New.Filling.HasAmplitudeBlocks.hilbertSchmidtNorm_one_sub_piTensorElem_comp_sq_le [WeilData E]
    (hP : S.HasAmplitudeBlocks ρ P) (hρ : ∀ (s : S.ResidueGL) (x y : V), ⟪ρ s x, ρ s y⟫_ℂ = ⟪x,
      y⟫_ℂ)
    (hg : ∀ i : Fin 2, IsCentralProj (g i))
    (hg_small : ∀ i : Fin 2, ((1 - g i).coeff 1).re ≤ (S.b ^ 8)⁻¹ / 2) :
    hilbertSchmidtNorm ((1 - ρ.asAlgebraHom (piTensorElem g)) ∘ₗ P) ^ 2 ≤
      4 * (S.b ^ 4)⁻¹ * S.sourceDim

theorem New.Filling.HasAmplitudeBlocks.isNearIsometry_piTensorElem_comp [WeilData E]
    (hP : S.HasAmplitudeBlocks ρ P)
    (hρ : ∀ (s : S.ResidueGL) (x y : V), ⟪ρ s x, ρ s y⟫_ℂ = ⟪x, y⟫_ℂ)
    (hg : ∀ i : Fin 2, IsCentralProj (g i))
    (hg_small : ∀ i : Fin 2, ((1 - g i).coeff 1).re ≤ (S.b ^ 8)⁻¹ / 2) :
    IsNearIsometry (ρ.asAlgebraHom (piTensorElem g) ∘ₗ P) (5 * (S.b ^ 4)⁻¹)
```

- **Verification status (`audit/verification-status.json`):** Local Lean compilation and kernel replay remain **`build_status: unverified-locally`** at pinned commit `41c2716658c42ba08eb0648994a2efe83082a95c` (`41c2716658c4`). Static token scanning has **`scan_scope: entry`**, covering only `Challenge.lean`, `src/Main.lean`, and `src/Statements.lean` (`0` forbidden tokens in solution files, `5` expected `sorry` tokens in `Challenge.lean`). `Moments.lean` and the upstream `Bessel/` and `Appendix/DiscreteSeries/` files have not been locally compiled, kernel-checked, or statically scanned here.

<!-- rh:lean-node:04_spanning/moments -->
