# The Big-Cell Kloosterman Sieve and Remaining-Term Link Bounds

This page explains the two analytic engines feeding `Moments.lean` — `src/New/Spanning/MainTerm/EnergyEstimate.lean` (`04_spanning/energy_estimate`, 15 declarations, **3 `let`-in-type stub truncations documented in full below**) and `src/New/Spanning/RemainingTerms/Sum.lean` (`04_spanning/remaining_terms_sum`, 26 declarations, 0 stub truncations) — together with their upstream foundations in `MainTerm/GlobalSieve.lean`, `RemainingTerms/SiegelLemma.lean`, and `RemainingTerms/SchurTest.lean`. Return to the [Stage 04 Overview](./overview.md), or revisit [Target Dimension, Multiplicity Equality & Spanning](./equal-multiplicities-and-spanning.md) and [Relative-Trace Unfolding & Second Moments](./unfolding-and-second-moments.md).

## Level 1 — First-Principles

To prove that the whole-group second moment of $P_s = P^* \rho(s) P$ overshoots the baseline $\mathcal{D}_{\mathrm{main}}$ by no more than $(1 + 9 b^{-4})$, `Moments.lean` cannot afford a loose constant factor anywhere. Instead, it splits all rational elements $\gamma$ outside the Bessel subgroup along a sharp geometric dividing line and feeds each side into its own specialized analytic engine:

![Side-by-side split diagram of the two Stage 04 analytic engines feeding Moments.lean: Left lane Main Term via generic cell reductions and Poisson summation; Right lane Remaining Terms via InSameLinkClass branching and sublattice counting](assets/stage04-sieve-vs-link-lattice-split.svg)

### Left lane: The Main-Term Engine (`EnergyEstimate.lean` + `GlobalSieve.lean`)

When a finite-prime reduction $s \in \bar{G}$ is an {{concept:kloosterman-sieve|open reduction whose leading frame-product determinants do not vanish modulo a chosen place above $p_0$}}, every rational term $\gamma$ sitting over $(c, s, c')$ automatically lands in the generic {{term:kloosterman-sieve|big cell}} (`IsOpenElement.leadingMinor_ne_zero`). In the {{term:kloosterman-sieve|big cell}}, every term factors cleanly as a Weyl head $w_p$ (carrying $\varrho$ rational unit coordinates $p_0, \dots, p_{\varrho - 1}$ and a final coordinate $t$) times a floor-group element $\sigma$.

The {{term:kloosterman-sieve}} bounds the squared norm of these big-cell {{concept:bessel-orbital-integral|cut Bessel orbital amplitude sums}} with sharp leading constant $1 + b^{-4}$ in two moves:

1. **Siegel's step:** At the infinite places, the Gram matrix of weighted cut orbital integrals matches a positive-semidefinite {{term:kloosterman-sieve|band-limited kernel}} `bandLimit n`. At the finite places $T$ (covering the level {{concept:prime-number|primes}} $M, N$ and auxiliary places), Siegel's mass formula turns the sum of squared coset amplitudes into a Hermitian pairing of `bandLimit n` against a finite-place Kloosterman density `lastEntryDensity`. By {{concept:arithmetic-height|the product formula matching archimedean sizes against finite-place moduli on units}}, all powers of the head coordinates $p_a$ cancel cleanly between the infinite and finite factors.
2. **Entry-by-entry one-sided Poisson summation:** Peeling off one head coordinate at a time via {{term:kloosterman-sieve|one-sided Poisson summation}} (`GlobalSieve.lean`) replaces the discrete rational sum by a nested hierarchy of positive-semidefinite integrals plus a tiny nonzero-frequency tail. Collecting all factors (`sieve_constant_le`) bounds the open-cell link second moment by
   $$(1 + b^{-4})\, b^{-6k|S_g|}\, \mathcal{D}_{\mathrm{main}}\, \nu^2 \qquad (\texttt{linkSecondMoment\_isOpenElement\_le}),$$
   gaining an extra factor $b^{-6k}$ for every place in $S_g$ where $s$ fixes a frame vector.

### Right lane: Remaining-Term Sublattice Bounds (`RemainingTerms/Sum.lean`)

The left lane bounds only one link at a time and only on open reductions. Everything left over—cross-terms between different links, terms where the discriminant $\mathsf{g} = |\Delta|^2 - N^2|\Delta'|^2$ vanishes, and reductions $s$ that fail to be open because the frame span meets its $s$-image modulo some prime above $M$—is routed to `RemainingTerms/Sum.lean`.

To every term $\gamma$, Stage 04 attaches its {{concept:link-lattice|Hermitian sublattice built from pairs $(t, \gamma^{-1} t)$ in the middle quotient $X^\perp/X$}}. For any pair $(\gamma, \gamma')$ sitting over the *same* block $(c, s, c')$, their intersection $\Lambda_\gamma \cap \Lambda_{\gamma'}$ is their {{term:link-lattice|common link}}, with {{term:link-lattice|pair discriminant}} $d$ (measuring its {{concept:gram-determinant|squared fundamental volume}}). The engine branches on whether $\gamma$ and $\gamma'$ lie in the same non-degenerate link class (`InSameLinkClass`):

- **Cross-link or $\mathsf{g} = 0$ pairs (`¬InSameLinkClass`):** Sharing the same mod-$M$ reduction $s$ forces a congruence saving $(2/M)^{2if}$ in $d^2$ whenever the links differ in codimension $i \ge 1$ (`pairDiscriminant_sq_le`). Combining the {{term:link-lattice|weighted Schur test}} (bounding each orbit's `termSize` in terms of its level gauge $G^{-k/2}$) with Bombieri–Vaaler {{term:link-lattice|Siegel's lemma}} (counting all sublattices of corank $l \le n^3$ and bounded discriminant) crushes all cross-link and $\mathsf{g} = 0$ pairs below $b^{-kf} \mathcal{D}_{\mathrm{id}}$ (`sum_termSize_mul_not_inSameLinkClass_le` $\Rightarrow$ `secondMoment_remainingTerms_le`).
- **Same-link $\mathsf{g} \neq 0$ pairs on meeting reductions (`InSameLinkClass` with `meetNorm s ≥ z`):** When $s$ fails to be open, the product of norms of places where the frame collides with its $s$-image gives the {{term:link-lattice|frame-meeting norm}} $S.\mathrm{meetNorm}\ s \ge z > 0$. The same lattice-counting engine extracts a factor $(b^{2f}/z)^k$ (`sum_termSize_mul_inSameLinkClass_le` $\Rightarrow$ `linkSecondMoment_le_of_le_meetNorm`), crushing non-open reductions far below $\mathcal{D}_{\mathrm{main}}$.

### Where this analogy breaks

Drawing the main term and remaining terms as two symmetric lanes hides a fundamental asymmetry in how they win. The **Kloosterman sieve** (left lane) must preserve the exact leading constant $1$ (losing at most $b^{-4}$), so it uses genuine Fourier duality (one-sided Poisson summation) on positive-semidefinite band-limited kernels where the zero frequency matches $\mathcal{D}_{\mathrm{main}}$ on the nose. By contrast, **Remaining Terms** (right lane) uses *no* phase cancellation between pairs at all—it applies the triangle inequality to bound every cross-term by positive absolute values `termSize γ * termSize γ'`, winning solely because the congruence and frame-meeting savings $(2/M)^{2if}$ and $z^{-1}$ are raised to the huge rank exponent $k$, overwhelming the lattice-counting zeta sum.

## Level 2 — Undergrad Bridge

### 1. Main Term: Big-Cell Kloosterman Sieve (`EnergyEstimate.lean` + `GlobalSieve.lean`)

1. **Open reductions force the big cell (`leadingMinor_frameGram_map`, `exists_leadingMinor_reduceAt_eq`, `IsOpenElement.leadingMinor_ne_zero`):**
   Leading minors of `frameGram` commute with ring maps, so for any $\gamma \in S.\mathrm{termsOver}\ c\ s\ c'$, the $l$-th leading minor of `S.frameProducts γ` is integral and reduces at every chosen place $w$ to the $l$-th leading minor of `frameGram (s i w)`. If $s$ is open (`S.IsOpenElement s`—all proper leading minors are nonzero at some chosen place above $p_0$), then $\mathrm{leadingMinor}(S.\mathrm{frameProducts}\ \gamma, l) \neq 0$ for all $l < n$.
2. **Archimedean Gram matrix and product-formula cancellation (`prod_bandLimit_eq_gram`, `gramScalar_mul_headDensity`):**
   Fix a star-stable finite set of places $T$ off which $M^{-1}, N^{-1}$, the lattice basis, and head units $T_0 \subset E^\times$ are integral, a floor-group element $\sigma \in \mathrm{floorGroup}\ \varrho$ with last frame product $t$ ($|\Phi_v(t)|^2 > N^2$ for all $v \mid \infty$), and scaled archimedean head coordinates $x_{v, p, a} = \mathrm{frameScaleSq}(v, a)\cdot \Phi_v(\mathrm{padTuple}(p, t)_a)$ with $\mathrm{frameScaleSq}(v, a) = \kappa_v^{2(\varrho - a)} / N$.
   - `prod_bandLimit_eq_gram` identifies the matrix $R_{p,q} = \prod_{v \mid \infty} (S.\mathrm{besselSystem}\ v).\mathrm{bandLimit}\ n\ (x_{v,p})\ (x_{v,q})$ as `Matrix.gram ℂ` of the cut Bessel orbital integrals at $\gamma = w_p \sigma$, weighted by the scalar $\rho(t, p)$.
   - `gramScalar_mul_headDensity` multiplies $\rho(t, p)$ by the finite-place head density `G.headDensity ϱ` at $T$: by the global product formula ($|p_a|_T^{1/2} = \prod_{v \mid \infty} |\Phi_v(p_a)|^{-1}$), every power of $p_a$ cancels identically, leaving $r \cdot \sqrt{\prod_v (|\Phi_v(t)|^2 - N^2)^{k-1}}$ where $r = S.\mathrm{normalizingConst}(k - 1) > 0$ (`normalizingConst_sq_pos`).
3. **Siegel's step (`IsFillingData.mul_sum_norm_sum_cosetAmplitude_sq_le`):**
   Grouping distinct tuples $(c_i, c'_i, \mathrm{red}(n_i), \sigma_i)$ by their last entry $t_i$ and applying `MainTerm/SiegelTopBound.lean` bounds
   $$r^2 \sum_{i} \Bigl\|\sum_{p : \mathrm{Fin}\,\varrho \to T_0} S.\mathrm{cosetAmplitude}\ \mathrm{.cut}\ c_i\ s_i\ c'_i\ (w_{p, t_i}\sigma_i)\Bigr\|^2 \le (1 + b^{-6})\, S.\mathrm{scaledSiegelConst}\cdot \operatorname{Re}\sum_{t', p, q} R_{p,q}(t')\,\frac{G.\mathrm{lastEntryDensity}(y_{p,t'}, y_{q,t'})}{Y.\mathrm{massRatio}}.$$
4. **One-sided Poisson summation (`GlobalSieve.lean::sieve`) and sharp constant assembly (`prod_poissonConst_eq`, `scaledSiegelConst_mul_decayConst_le`, `sieve_constant_le`):**
   The finite-place Kloosterman densities form a `SieveSystem` of mass $\le (1 + b^{-6}) b^{-6k|S_g|}$ when $Y$ fixes a frame vector $D_w$ for each $w \in S_g$ above $p_0$. Multiplying by the archimedean `bandLimit` systems (mass $\le (1 + b^{-10})/\mathrm{deg}_v$) and running entry-by-entry one-sided Poisson summation (`densitySum_succ_le_mul_densitySum_add` $\to$ `sieve`) bounds the right-hand side by
   $$\Bigl(\prod_{i < n} S.\mathrm{poissonConst}\ i\Bigr) \left(\Bigl(\prod_v \frac{1 + b^{-10}}{\mathrm{deg}_v}\Bigr)\frac{1 + b^{-6}}{b^{6k|S_g|}} + b^{2403 n^9 f}\, S.\mathrm{decayConst}\right).$$
   By `prod_poissonConst_eq`, $\prod_{i < n} S.\mathrm{poissonConst}\ i = r^2 \cdot \mathrm{vol}(\hat{\mathcal{O}}_E)^n (|S.\mathrm{BesselCoord}| / \Lambda)^2 / c_G^f$. Absorbing the frequency-decay error into $b^{-10} \mathcal{D}_{\mathrm{main}}$ (`scaledSiegelConst_mul_decayConst_le`) and combining $(1 + b^{-6})((1 + b^{-9})^2(1 + b^{-6}) + b^{-10}) \le 1 + b^{-4}$ (`one_add_inv_pow_ten_pow_le`, `one_add_pow_six_mul_add_pow_ten_le_one_add_pow_four`) gives `sieve_constant_le`. Dividing by $r^2 > 0$ yields `sum_norm_sum_cosetAmplitude_sq_le` and, summing over all links, **`New.Filling.linkSecondMoment_isOpenElement_le`**:
   $$\mathrm{linkSecondMoment}\ \{\gamma \mid \mathsf{g}(\gamma) \neq 0\}\ \{s \in S.\mathrm{fixer}\ S_g\ D \mid S.\mathrm{IsOpenElement}\ s\} \le (1 + b^{-4})\, b^{-6k|S_g|}\, \mathcal{D}_{\mathrm{main}}\, \nu^2.$$

### 2. Remaining Terms: Link Lattices, Siegel's Lemma, and Weighted Schur Test (`RemainingTerms/Sum.lean`)

1. **Near pairs and discriminant compression (`Sum.lean`, lines 59–147):**
   For any near pair $(\gamma, \gamma')$ over an agreeing block $(c, s, c')$ (`IsNearPairOverAgreeingBlock`):
   - $1 \le d := S.\mathrm{pairDiscriminant}\ c\ c'\ \gamma\ \gamma'$ (`one_le_pairDiscriminant`);
   - letting $i = S.\mathrm{linkCorank}\ \gamma\ \gamma'$ be the codimension of `commonLink γ γ'` in each link, **`pairDiscriminant_sq_le`** gives
     $$d^2 \le \Bigl(\frac{2}{M}\Bigr)^{2 i f}\, G(\gamma)\, G(\gamma');$$
   - **`corank_le_and_pairDiscriminant_le`** bounds both the corank $l = k - \dim_E(\mathrm{commonLink}\ \gamma\ \gamma') \le n^3$ and the discriminant $d \le S.\mathrm{discriminantBound} = b^{n^3 f}$ (`mem_boundedDiscNearPairs`).
2. **Weighted Schur test + Bombieri–Vaaler Siegel's lemma (`SchurTest.lean`, `SiegelLemma.lean`, `Sum.lean:149–305`):**
   - `SchurTest.lean::amplitudeMajorant_sq_le` bounds each orbit's amplitude majorant by Cauchy–Schwarz with the inverting `schurWeight` ($\nu_v^{-(k+i)}$), feeding `termSize_sq_le`: $\mathrm{termSize}(c, c', \gamma)^2 \le \tau^4 D / (n_p G(\gamma)^k)$.
   - Whenever $(d \eta)^2 \le G(\gamma) G(\gamma')$ and $\tau^{l+6} D \eta^{-k} \le B$, each pair contributes at most $B\, d^{-k}/(\tau^{l+2} n_p)$ (`termSize_mul_le`, `ofReal_termSize_mul_le`).
   - Summing $d^{-k}$ over all Hermitian sublattices of corank $l \le n^3$ via `SiegelLemma.lean::tsum_sublatticeDiscriminant_inv_le` (Bombieri–Vaaler Siegel's lemma cuts out each codimension-$s$ subspace by $s$ short vectors) and `cube_add_one_le_tailScale` ($n^3 + 1 \le \tau$) proves the master template **`sum_termSize_mul_le`**: total pair-size sum $\le \tau B$.
3. **Branching on `InSameLinkClass` (`Sum.lean`, lines 307–837):**
   - **Not in the same link class (`sum_termSize_mul_not_inSameLinkClass_le`):** If `link γ ≠ link γ'` ($i \ge 1$), take $\eta = (M/2)^{if}$ and $D = \mathcal{D}_{\mathrm{main}}$ (`cellRankSecondMoment_le_mainSecondMoment`, `pow_mul_mainSecondMoment_div_le`); if $\mathsf{g} = 0$ for either term, its cell rank drops below the top cell. Either way, `sum_termSize_mul_le` yields total sum $\le b^{-kf} \mathcal{D}_{\mathrm{id}}$.
   - **Reduction to one link (`enorm_amplitudeSum_mul_le`, `enorm_amplitudeSum_sq_le` $\Rightarrow$ `secondMoment_remainingTerms_le`):** Partitioning any bi-unipotent-stable subset $T \subseteq \Gamma^*$ by `Piece` (each link $\Lambda$ with $\mathsf{g} \neq 0$, plus the $\mathsf{g} = 0$ piece) isolates the diagonal same-link squares from cross-link and $\mathsf{g} = 0$ pairs, proving:
     $$\mathrm{linkSecondMoment}\ \{\gamma \mid \mathsf{g}(\gamma) \neq 0\}\ \Omega \le w \nu^2 \;\Longrightarrow\; \mathrm{secondMoment}\bigl(S.\mathrm{amplitudeSum}\ \mathrm{.cut}\ \Gamma^*\bigr)\ \Omega \le w + \frac{\mathcal{D}_{\mathrm{id}}}{b^{kf}}.$$
   - **Same link class with meeting norm (`sum_termSize_mul_inSameLinkClass_le` $\Rightarrow$ `linkSecondMoment_le_of_le_meetNorm`):** For pairs with `InSameLinkClass γ γ'` over reductions $s \in \Omega$ satisfying `z ≤ S.meetNorm s` ($z \neq 0$), taking $\eta \propto z / b^{2f}$ inside `sum_termSize_mul_le` bounds both the pair sum by $(b^{2f}/z)^k \mathcal{D}_{\mathrm{main}}$ and the link second moment by
     $$\mathrm{linkSecondMoment}\ \{\gamma \mid \mathsf{g}(\gamma) \neq 0\}\ \Omega \le \Bigl(\frac{b^{2f}}{z}\Bigr)^k \mathcal{D}_{\mathrm{main}}\, \nu^2.$$

## Level 3 — Formal Lean Contract

The extracted skeletons `skeleton/04_spanning/energy_estimate.lean` (15 declarations, **3 stub truncations**) and `skeleton/04_spanning/remaining_terms_sum.lean` (26 declarations, 0 stub truncations) mirror `src/New/Spanning/MainTerm/EnergyEstimate.lean` and `src/New/Spanning/RemainingTerms/Sum.lean` at pinned upstream commit `41c2716658c42ba08eb0648994a2efe83082a95c`.

### Critical Honesty Disclosure: Three `let`-in-Type Stub Truncations in `energy_estimate.lean`

`rh-extract` splits declaration signatures from proof bodies at the first top-level (depth-0) `:=` token. Three intermediate lemmas in `src/New/Spanning/MainTerm/EnergyEstimate.lean` use `let ... :=` bindings *inside their return type*, causing the extracted stubs in `skeleton/04_spanning/energy_estimate.lean` at **L127** (`gramScalar_mul_headDensity`), **L145** (`prod_bandLimit_eq_gram`), and **L180** (`IsFillingData.mul_sum_norm_sum_cosetAmplitude_sq_le`) to truncate immediately at that first `let ... :=` binding (replaced by `:= by sorry`). For complete mathematical accuracy, their **exact untruncated statements** from upstream `src/New/Spanning/MainTerm/EnergyEstimate.lean:371–510` (`41c2716658c4`) are quoted verbatim below:

```lean
-- Upstream: src/New/Spanning/MainTerm/EnergyEstimate.lean:371-386
-- (truncated at `let x ... :=` in skeleton/04_spanning/energy_estimate.lean:127)
lemma gramScalar_mul_headDensity
    [S.IsAdmissible] {T : Finset (FinPlace E)} [T.StarStable] {P : Floors.IntegralPlace (LocProd T)}
    {G : Floors.PlaceData (Fin (𝔰.k - (6 * 𝔰.ϱ + 13))) (S.latticeMiddle (LocProd T))
      (𝔰.ϱ + 1 + 2 * 𝔰.ϱ + 5)} {kPred : ℕ} (hk : 𝔰.k = kPred + 1)
    {T₀ : Finset Eˣ} (hT₀ : ∀ x ∈ T₀, x.val ∈ ratIntegralOff T ∧ x⁻¹.val ∈ ratIntegralOff T)
    {t : Eˣ} (h_large : ∀ v : NumberField.InfinitePlace E, S.N ^ 2 < ‖Φ v t.val‖ ^ 2)
    (p : Fin 𝔰.ϱ → T₀) :
    let x (v : NumberField.InfinitePlace E) (a : ℕ) : ℂˣ :=
      unitOrOne (S.frameScaleSq v a * Φ v (padTuple (fun b : Fin 𝔰.ϱ ↦ (p b).val) t a).val)
    (∏ v : NumberField.InfinitePlace E, √𝔰.openCellConst *
        √(‖(x v 𝔰.ϱ).val‖ ^ 2 - 1) ^ (𝔰.k - 1) / Fintype.card (S.besselSystem v).Index *
        ∏ a ∈ Finset.range 𝔰.ϱ, ‖(x v a).val‖ ^ (𝔰.k + 1 + 2 * (𝔰.ϱ - (a + 1)))) *
      (G.headDensity 𝔰.ϱ (Units.map (ratToPlaces T) ∘ padTuple (fun b : Fin 𝔰.ϱ ↦ (p b).val) t)
        1).toReal =
      S.normalizingConst kPred *
        √((∏ v : NumberField.InfinitePlace E, (‖Φ v t.val‖ ^ 2 - S.N ^ 2)) ^ (𝔰.k - 1))

-- Upstream: src/New/Spanning/MainTerm/EnergyEstimate.lean:425-438
-- (truncated at `let x ... :=` in skeleton/04_spanning/energy_estimate.lean:145)
lemma prod_bandLimit_eq_gram {T₀ : Finset Eˣ} {σ : S.G E} {t : Eˣ}
    (hσ : σ ∈ Shape.Form.floorGroup 𝔰.ϱ) (h_tail : S.frameProducts σ 𝔰.top 𝔰.top = t) :
    let x (v : NumberField.InfinitePlace E) (p : Fin 𝔰.ϱ → T₀) (a : ℕ) : ℂˣ :=
      unitOrOne (S.frameScaleSq v a * Φ v (padTuple (fun b : Fin 𝔰.ϱ ↦ (p b).val) t a).val)
    let R : Matrix (Fin 𝔰.ϱ → T₀) (Fin 𝔰.ϱ → T₀) ℂ := fun p q : Fin 𝔰.ϱ → T₀ ↦
      ∏ v : NumberField.InfinitePlace E, (S.besselSystem v).bandLimit 𝔰.n (x v p) (x v q)
    R = Matrix.gram ℂ fun p : Fin 𝔰.ϱ → T₀ ↦
      (∏ v : NumberField.InfinitePlace E, √𝔰.openCellConst *
          √(‖(x v p 𝔰.ϱ).val‖ ^ 2 - 1) ^ (𝔰.k - 1) / Fintype.card (S.besselSystem v).Index *
          ∏ a ∈ Finset.range 𝔰.ϱ, ‖(x v p a).val‖ ^ (𝔰.k + 1 + 2 * (𝔰.ϱ - (a + 1)))) •
        besselOrbitalIntegral volume
          (fun g : NumberField.InfinitePlace E → 𝔰.G₁ ↦ WithLp.toLp 2 (S.besselFn .cut g))
          (S.ratToInf
            (Floors.FloorGroups.weylElem 𝔰.ϱ (padTuple (fun a : Fin 𝔰.ϱ ↦ (p a).val) t) * σ))

-- Upstream: src/New/Spanning/MainTerm/EnergyEstimate.lean:482-510
-- (truncated at `let r : ℝ :=` in skeleton/04_spanning/energy_estimate.lean:180)
lemma IsFillingData.mul_sum_norm_sum_cosetAmplitude_sq_le [S.IsAdmissible] {T : Finset (FinPlace E)}
    [T.StarStable] [Fact (S.ideleM⁻¹.val ∈ integralOff T)] {P : Floors.IntegralPlace (LocProd T)}
    {G : Floors.PlaceData (Fin (𝔰.k - (6 * 𝔰.ϱ + 13))) (S.latticeMiddle (LocProd T))
      (𝔰.ϱ + 1 + 2 * 𝔰.ϱ + 5)} (hG : S.IsFillingData T G) (Y : G.IntermediateGroup)
    {kPred : ℕ} (hk : 𝔰.k = kPred + 1) {ι : Type} [Fintype ι] [DecidableEq Eˣ]
    {c c' : ι → S.Classes} {σ : ι → S.G E} {n : ι → S.latticeStabilizer} {t : ι → Eˣ}
    (h_inj : (fun i : ι ↦ (c i, c' i, S.reduction (n i), σ i)).Injective)
    (hσ : ∀ i : ι, σ i ∈ Shape.Form.floorGroup 𝔰.ϱ)
    (h_tail : ∀ i : ι, S.frameProducts (σ i) 𝔰.top 𝔰.top = t i)
    (h_large : ∀ (i : ι) (v : NumberField.InfinitePlace E), S.N ^ 2 < ‖Φ v (t i).val‖ ^ 2)
    (hn_mem_Y : ∀ i : ι, S.toPlaces T (n i) ∈ Y.val) {T₀ : Finset Eˣ}
    (hT₀ : ∀ x ∈ T₀, x.val ∈ ratIntegralOff T ∧ x⁻¹.val ∈ ratIntegralOff T)
    (hT_basis : S.latticeBasis ∈ intGL 𝔰.BesselIndex (integralOff T))
    (hT_N : (S.N : E)⁻¹ ∈ ratIntegralOff T)
    (h_off : ∀ i : ι, S.IsIntegralOff T (S.translateByClassReps (c i) (c' i) (σ i))) :
    let r : ℝ := S.normalizingConst kPred
    let x (v : NumberField.InfinitePlace E) (p : Fin 𝔰.ϱ → T₀) (t : Eˣ) (a : ℕ) : ℂˣ :=
      unitOrOne (S.frameScaleSq v a * Φ v (padTuple (fun b : Fin 𝔰.ϱ ↦ (p b).val) t a).val)
    let y (p : Fin 𝔰.ϱ → T₀) (t : Eˣ) : ℕ → (LocProd T)ˣ :=
      Units.map (ratToPlaces T) ∘ padTuple (fun a : Fin 𝔰.ϱ ↦ (p a).val) t
    r ^ 2 * ∑ i : ι, ‖∑ p : Fin 𝔰.ϱ → T₀, S.cosetAmplitude .cut (c i) (S.reduction (n i)) (c' i)
        (Floors.FloorGroups.weylElem 𝔰.ϱ (padTuple (fun a : Fin 𝔰.ϱ ↦ (p a).val) (t i)) *
          σ i)‖ ^ 2 ≤
      (1 + S.b⁻¹ ^ 6) * S.scaledSiegelConst *
        (∑ t' ∈ univ.image t, ∑ p : Fin 𝔰.ϱ → T₀, ∑ q : Fin 𝔰.ϱ → T₀,
          (∏ v : NumberField.InfinitePlace E,
            (S.besselSystem v).bandLimit 𝔰.n (x v p t') (x v q t')) *
          (G.lastEntryDensity (S.piNAt T) Y (y p t') (y q t') / Y.massRatio.toReal)).re
```

### Untruncated Headline Signatures

From `skeleton/04_spanning/energy_estimate.lean` (upstream `EnergyEstimate.lean:620–685`) and `skeleton/04_spanning/remaining_terms_sum.lean` (upstream `RemainingTerms/Sum.lean:678–837`):

```lean
theorem New.Filling.linkSecondMoment_isOpenElement_le [S.IsAdmissible] (Sg : Finset (S.chosenPlaces 0))
    (D : ∀ w : S.chosenPlaces 0, S.FrameVector w) :
    linkSecondMoment {γ : S.G E | S.termDisc γ ≠ 0}
      {s : S.ResidueGL | s ∈ S.fixer Sg D ∧ S.IsOpenElement s} ≤
      .ofReal ((1 + (S.b ^ 4)⁻¹) * (S.b ^ (6 * 𝔰.k))⁻¹ ^ #Sg * S.mainSecondMoment *
        S.unipotentCovolume ^ 2)

theorem New.Filling.secondMoment_remainingTerms_le {E : Type} [Field E] [CMType E] {𝔰 : Shape} {S : Filling E 𝔰}
    [HasPrimitiveRoot5040 E] [S.IsAdmissible] {w : ℝ} {Ω : Set S.ResidueGL} (hw : 0 ≤ w)
    (h_linkMoment : linkSecondMoment {γ : S.G E | S.termDisc γ ≠ 0} Ω ≤
      ENNReal.ofReal (w * S.unipotentCovolume ^ 2)) :
    secondMoment (S.amplitudeSum .cut S.remainingTerms) Ω ≤
      w + S.identitySecondMoment / S.b ^ (𝔰.k * numInfPlaces E)

theorem New.Filling.linkSecondMoment_le_of_le_meetNorm {E : Type} [Field E] [CMType E]
    {𝔰 : Shape} {S : Filling E 𝔰} [HasPrimitiveRoot5040 E] [S.IsAdmissible]
    {Ω : Set S.ResidueGL} {z : ℕ} (hz : z ≠ 0)
    (h_meet : ∀ s ∈ Ω, z ≤ S.meetNorm s) :
    linkSecondMoment {γ : S.G E | S.termDisc γ ≠ 0} Ω ≤
      ENNReal.ofReal ((S.b ^ (2 * numInfPlaces E) / z) ^ 𝔰.k * S.mainSecondMoment *
        S.unipotentCovolume ^ 2)
```

- **Verification status (`audit/verification-status.json`):** Local Lean compilation and kernel replay remain **`build_status: unverified-locally`** at pinned commit `41c2716658c42ba08eb0648994a2efe83082a95c` (`41c2716658c4`). Static token scanning has **`scan_scope: entry`**, covering only `Challenge.lean`, `src/Main.lean`, and `src/Statements.lean` (`0` forbidden tokens in solution files, `5` expected `sorry` tokens in `Challenge.lean`). Neither `EnergyEstimate.lean` nor `RemainingTerms/Sum.lean` has been locally compiled, kernel-checked, or statically scanned here.

<!-- rh:lean-node:04_spanning/energy_estimate -->

<!-- rh:lean-node:04_spanning/remaining_terms_sum -->
