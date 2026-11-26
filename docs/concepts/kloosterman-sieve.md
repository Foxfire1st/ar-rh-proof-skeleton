---
slug: kloosterman-sieve
term: Kloosterman sieve
aliases: ["one-sided Poisson summation", "Siegel's step", "big cell", "open reduction", "band-limited kernel"]
inline_glimpse: "A global entry-by-entry one-sided Poisson summation engine pairing archimedean band-limited Bessel Gram matrices with finite Kloosterman densities to bound the big-cell second moment by (1 + b⁻⁴) b⁻⁶ᵏ|S_g| D_main ν²."
visual_ref: assets/stage04-sieve-vs-link-lattice-split.svg
where_analogy_breaks: "Picturing the Kloosterman sieve as ordinary Poisson summation over a rectangular grid where the zero frequency gives the main term and all nonzero frequencies cancel misses two decisive mechanisms: first, the sieve operates inductively one Bruhat head coordinate at a time (`densitySum_succ_le_mul_densitySum_add` in `GlobalSieve.lean`) using a positive-semidefinite band-limited majorant rather than an exact equality; second, finite-prime Kloosterman densities enter through Siegel's step (`mul_sum_norm_sum_cosetAmplitude_sq_le`), which requires the product formula to cancel the head powers `p_a` between the archimedean orbital-integral scalars and `G.headDensity`."
sources:
  - title: "Über die analytische Theorie der quadratischen Formen"
    citation: "C. L. Siegel, Ann. of Math. 36 (1935), 527–606."
    kind: paper
  - title: "Smooth transfer of Kloosterman integrals"
    citation: "H. Jacquet, Duke Math. J. 120 (2003), 121–152."
    kind: paper
  - title: "Kloosterman sums and Fourier coefficients of cusp forms"
    citation: "J.-M. Deshouillers & H. Iwaniec, Invent. Math. 70 (1982), 219–288."
    kind: paper
  - title: "On the subconvexity problem for L-functions on GL(3)"
    citation: "V. Blomer & J. Buttcane, Ann. Sci. ÉNS 53 (2020), 1441–1500."
    kind: paper
lean_anchors:
  - 04_spanning/energy_estimate.lean#New.Filling.IsOpenElement.leadingMinor_ne_zero
  - 04_spanning/energy_estimate.lean#New.Filling.sieve_constant_le
  - 04_spanning/energy_estimate.lean#New.Filling.sum_norm_sum_cosetAmplitude_sq_le
  - 04_spanning/energy_estimate.lean#New.Filling.linkSecondMoment_isOpenElement_le
child_concepts:
  - bessel-orbital-integral
  - link-lattice
  - prime-number
  - arithmetic-height
---

# Kloosterman Sieve

## Level 1 — First-Principles

When Stage 04 unfolds the second moment $\sum_{s \in \bar{G}} \|P^* \rho(s) P\|_{\mathrm{HS}}^2$ into {{concept:bessel-orbital-integral|sums of archimedean overlap integrals over rational group elements}}, the bulk of the sum comes from the generic **big cell**: rational elements $\gamma$ whose leading frame-product determinants are all nonzero, forced automatically whenever the finite-prime reduction $s \in \bar{G}$ is an **open reduction** (`IsOpenElement.leadingMinor_ne_zero`). Because Theorem B needs the second moment to overshoot the theoretical baseline $\mathcal{D}_{\mathrm{main}}$ by no more than $1 + O(b^{-4})$—with *no* extra factor of $2$, let alone $X^\varepsilon$—crude triangle inequalities are useless. We need a sharp upper bound that preserves the exact leading constant $1$.

The **Kloosterman sieve** achieves that sharp constant in three coordinated stages:

1. **Siegel's step (`mul_sum_norm_sum_cosetAmplitude_sq_le`):** Every big-cell element factors as a Weyl head $w_p$ (indexed by a tuple $p$ of $\varrho$ units plus a last entry $t$) times an element $\sigma$ of the floor group. At the infinite places, the Gram matrix of weighted cut Bessel orbital integrals is dominated by a positive-semidefinite **band-limited kernel** `bandLimit n` (`prod_bandLimit_eq_gram`). At the finite places $T$ (covering the level {{concept:prime-number|primes}} $M, N$ and auxiliary places), summing over cosets of the floor group via Siegel's mass formula replaces the global sum of squared coset amplitudes by a Hermitian pairing of `bandLimit n` against a finite-place Kloosterman density `G.lastEntryDensity`. By {{concept:arithmetic-height|the product formula equating archimedean sizes with finite-place moduli on units}}, all intermediate powers of the head units $p_a$ cancel cleanly between the archimedean scalar and `G.headDensity` (`gramScalar_mul_headDensity`).
2. **One-sided Poisson summation (`GlobalSieve.lean::sieve`):** Rather than evaluating the coupled sum over all $\varrho + 1$ head coordinates at once, the sieve peels off one coordinate at a time. At each step, the positive-semidefinite kernel lets **one-sided Poisson summation** replace a discrete sum over $T$-integral rational numbers by its zero-frequency integral (the next lower-order kernel in the `SieveSystem` hierarchy) plus a rapidly decaying sum over nonzero frequencies (`frequencyDecay`).
3. **Collecting the sharp constant (`sieve_constant_le` $\Rightarrow$ `linkSecondMoment_isOpenElement_le`):** After all $n = \varrho + 1$ coordinates are removed, the product of the Poisson summation constants cancels the normalizing scalar $r^2$, the infinite-place degree factors give $\prod_v (1 + b^{-10})/\mathrm{deg}_v$, the finite-place mass gives $(1 + b^{-6}) b^{-6k|S_g|}$ (gaining a sharp decay factor for every place in $S_g$ where the reduction $s$ fixes a frame vector), and the nonzero-frequency tail is crushed by $b^{-10}$. The total open-cell second moment lands below $(1 + b^{-4}) b^{-6k|S_g|} \mathcal{D}_{\mathrm{main}} \nu^2$, leaving non-open and cross-link terms to the {{concept:link-lattice|common-link sublattice and weighted Schur test bounds}}.

### Limits of the intuition

Picturing the **Kloosterman sieve** as standard Poisson summation on a box—where the zero mode gives the main term and higher modes oscillate away—captures why the leading constant comes out sharp ($1 + b^{-4}$ instead of $2$ or $X^\varepsilon$), but hides two technical points. First, the sum runs over *units* in a number field $E$ integral off a finite set of places $T$, indexed by Bruhat head tuples $(p_0, \dots, p_{\varrho - 1}, t)$ where the archimedean scaling `frameScaleSq v a = κ_v^{2(ϱ - a)} / N` depends on the coordinate index $a$, so `GlobalSieve.lean` must work with a multiplicative `SieveSystem` of nested positive-semidefinite kernels rather than a single Schwartz function. Second, `linkSecondMoment_isOpenElement_le` bounds only the *cut* Bessel function on open reductions with `termDisc γ ≠ 0`, grouped one link at a time; passing back to the full second moment over all $s \in \bar{G}$ requires `RemainingTerms/Sum.lean` and the cut-tail bounds in `Moments.lean`.

## Level 2 — Undergrad Bridge

Classical spectral large sieves via Kloosterman sums (Deshouillers & Iwaniec 1982; Blomer & Buttcane 2020) and Jacquet's inductive Kloosterman transfer (Jacquet 2003) typically lose a constant factor or $X^\varepsilon$. Stage 04 avoids any constant loss by uniting Siegel's mass formula (Siegel 1935) with an inductive positive-semidefinite `SieveSystem` over the product of all infinite places and a finite set $T$ of finite places (`MainTerm/{SieveSystem,GlobalSieve,SiegelTopBound,EnergyEstimate}.lean`).

### 1. Open reductions and the big Bruhat cell (`IsOpenElement.leadingMinor_ne_zero`)

For a rational element $\gamma \in S.\mathrm{termsOver}\ c\ s\ c'$, the leading minors of `S.frameProducts γ` are integral and reduce modulo each chosen place $w$ to the corresponding leading minors of the frame Gram matrix of $s_{i,w}$ (`exists_leadingMinor_reduceAt_eq`). Whenever $s$ is an **open reduction** (`S.IsOpenElement s`—at some chosen place above $p_0$, none of the proper leading minors vanish), every $\gamma$ over $(c, s, c')$ satisfies $\mathrm{leadingMinor}(S.\mathrm{frameProducts}\ \gamma, l) \neq 0$ for all $l < n$, placing $\gamma$ strictly in the **big cell** where Bruhat-head parametrization $\gamma = u w_p \sigma u'^{-1}$ applies.

### 2. Siegel's step and product-formula cancellation (`EnergyEstimate.lean:371–551`)

Given a finite set $T$ of places (star-stable, off which $M^{-1}, N^{-1}$, the lattice basis, and head units $T_0 \subset E^\times$ are integral), a filling datum $G$ at $T$, and an intermediate congruence group $Y$:
- **`prod_bandLimit_eq_gram`:** For $\sigma \in \mathrm{floorGroup}\ \varrho$ with last frame product $t$, the product $\prod_v (S.\mathrm{besselSystem}\ v).\mathrm{bandLimit}\ n\ (x_{v,p})\ (x_{v,q})$ equals the Gram matrix `Matrix.gram ℂ` of the cut Bessel orbital integrals at $\gamma = w_p \sigma$, each weighted by the archimedean scalar
  $$\rho(t, p) = \prod_{v \mid \infty} \frac{\sqrt{c_G}\,\bigl(|x_{v,p,\varrho}|^2 - 1\bigr)^{(k-1)/2}}{|I_v|} \prod_{a < \varrho} |x_{v,p,a}|^{k + 1 + 2(\varrho - a - 1)},$$
  where $x_{v,p,a} = \mathrm{frameScaleSq}(v, a)\cdot \Phi_v(p_a)$ ($x_{v,p,\varrho} = \Phi_v(t)/N$) and $c_G = \mathrm{openCellConst}$.
- **`gramScalar_mul_headDensity`:** Multiplying $\rho(t, p)$ by the finite-place modulus factor `G.headDensity ϱ` cancels every power of $p_a \in T_0$ via the global product formula ($|p_a|_T^{1/2} = \prod_{v \mid \infty} |\Phi_v(p_a)|^{-1}$), leaving $r \cdot \sqrt{\prod_v (|\Phi_v(t)|^2 - N^2)^{k-1}}$, where $r = S.\mathrm{normalizingConst}(k - 1)$ is independent of $p$ and $t$.
- **`IsFillingData.mul_sum_norm_sum_cosetAmplitude_sq_le` (Siegel's step):** Grouping distinct classes $(c_i, c'_i, \mathrm{red}(n_i), \sigma_i)$ by their last entry $t_i$ (with $|\Phi_v(t_i)|^2 > N^2$) and applying `SiegelTopBound.lean` bounds
  $$r^2 \sum_{i} \Bigl\|\sum_{p : \mathrm{Fin}\,\varrho \to T_0} S.\mathrm{cosetAmplitude}\ \mathrm{.cut}\ c_i\ s_i\ c'_i\ (w_{p, t_i}\sigma_i)\Bigr\|^2 \le (1 + b^{-6})\, S.\mathrm{scaledSiegelConst}\cdot \operatorname{Re}\sum_{t', p, q} \Bigl(\prod_v \mathrm{bandLimit}_v\Bigr) \frac{G.\mathrm{lastEntryDensity}(y_{p,t'}, y_{q,t'})}{Y.\mathrm{massRatio}}.$$

### 3. Global one-sided Poisson summation and constant assembly (`GlobalSieve.lean`, `EnergyEstimate.lean`)

The finite-place densities `G.lastEntryDensity / Y.massRatio` form a `SieveSystem` of mass at most $(1 + b^{-6}) b^{-6k|S_g|}$ when $Y$ fixes a frame vector $D_w$ at each place $w \in S_g$ above $p_0$ (`Frames/FixedLines.lean`, `MainTerm/FiniteSieveSystem.lean`). Multiplying by the archimedean `bandLimit` systems (mass $\le (1 + b^{-10})/\mathrm{deg}_v$) and invoking `GlobalSieve.lean::sieve` yields
$$\Bigl(\prod_{i < n} S.\mathrm{poissonConst}\ i\Bigr) \left(\Bigl(\prod_v \frac{1 + b^{-10}}{\mathrm{deg}_v}\Bigr) \frac{1 + b^{-6}}{b^{6k|S_g|}} + b^{2403 n^9 f}\, S.\mathrm{decayConst}\right).$$
Using `prod_poissonConst_eq` to pull out $r^2$, `scaledSiegelConst_mul_decayConst_le` to absorb the error into $b^{-10} \mathcal{D}_{\mathrm{main}}$, and `one_add_pow_six_mul_add_pow_ten_le_one_add_pow_four` (`sieve_constant_le`), dividing by $r^2 > 0$ gives `sum_norm_sum_cosetAmplitude_sq_le` and, summing over all links, **`New.Filling.linkSecondMoment_isOpenElement_le`**:
$$\mathrm{linkSecondMoment}\ \{\gamma \mid \mathrm{termDisc}\,\gamma \neq 0\}\ \{s \in S.\mathrm{fixer}\ S_g\ D \mid S.\mathrm{IsOpenElement}\ s\} \le (1 + b^{-4})\, b^{-6k|S_g|}\, \mathcal{D}_{\mathrm{main}}\, \nu^2.$$

## Level 3 — Formal Lean Contract

Stage 04 extracts the main-term energy estimate into `skeleton/04_spanning/energy_estimate.lean` (note that three intermediate `let`-in-type lemmas in that skeleton are truncated by `rh-extract`'s depth-0 `:=` splitter and are quoted in full from upstream `src/New/Spanning/MainTerm/EnergyEstimate.lean:371–510` in the Stage 04 essay; all four anchors below are complete, untruncated declarations):

- `04_spanning/energy_estimate.lean#New.Filling.IsOpenElement.leadingMinor_ne_zero` (upstream `src/New/Spanning/MainTerm/EnergyEstimate.lean:91-100`):
  ```lean
  lemma New.Filling.IsOpenElement.leadingMinor_ne_zero {c c' : S.Classes} {s : S.ResidueGL}
      (hs : S.IsOpenElement s) {γ : S.G E} (hγ : γ ∈ S.termsOver c s c') (l : Fin 𝔰.n) :
      leadingMinor (S.frameProducts γ) l.castSucc ≠ 0
  ```
- `04_spanning/energy_estimate.lean#New.Filling.sieve_constant_le` (upstream `src/New/Spanning/MainTerm/EnergyEstimate.lean:264-348`):
  ```lean
  lemma New.Filling.sieve_constant_le [S.IsAdmissible] {kPred numSieved : ℕ} (hk : 𝔰.k = kPred + 1)
      (h_numSieved : numSieved ≤ Core.numInfPlaces E) :
      (1 + S.b⁻¹ ^ 6) * S.scaledSiegelConst *
          ((∏ i ∈ Finset.range 𝔰.n, S.poissonConst i) *
            ((∏ v : NumberField.InfinitePlace E, (1 + S.b⁻¹ ^ 10) / (S.besselSystem v).deg) *
                ((1 + S.b⁻¹ ^ 6) / (S.b ^ (6 * 𝔰.k)) ^ numSieved) +
              S.b ^ (2403 * 𝔰.n ^ 9 * Core.numInfPlaces E) * S.decayConst)) ≤
        S.normalizingConst kPred ^ 2 *
          ((1 + (S.b ^ 4)⁻¹) * (S.b ^ (6 * 𝔰.k))⁻¹ ^ numSieved * S.mainSecondMoment *
            S.unipotentCovolume ^ 2)
  ```
- `04_spanning/energy_estimate.lean#New.Filling.sum_norm_sum_cosetAmplitude_sq_le` (upstream `src/New/Spanning/MainTerm/EnergyEstimate.lean:553-618`):
  ```lean
  lemma New.Filling.sum_norm_sum_cosetAmplitude_sq_le [S.IsAdmissible] (Sg : Finset (S.chosenPlaces 0))
      (D : ∀ w : S.chosenPlaces 0, S.FrameVector w) {ι : Type} [Fintype ι] {c c' : ι → S.Classes}
      {σ : ι → S.G E} {n : ι → S.latticeStabilizer} {t : ι → Eˣ}
      (h_inj : (fun i : ι ↦ (c i, c' i, S.reduction (n i), σ i)).Injective)
      (hσ : ∀ i : ι, σ i ∈ Shape.Form.floorGroup 𝔰.ϱ)
      (h_tail : ∀ i : ι, S.frameProducts (σ i) 𝔰.top 𝔰.top = t i)
      (h_large : ∀ (i : ι) (v : NumberField.InfinitePlace E), S.N ^ 2 < ‖Φ v (t i).val‖ ^ 2)
      (hn_fix : ∀ i : ι, S.reduction (n i) ∈ S.fixer Sg D)
      {T₀ : Finset Eˣ} (ht : ∀ i : ι, t i ∈ T₀) {T : Finset (FinPlace E)} [T.StarStable]
      (hT₀ : ∀ x ∈ T₀, x.val ∈ ratIntegralOff T ∧ x⁻¹.val ∈ ratIntegralOff T)
      [hT_M : Fact (S.ideleM⁻¹.val ∈ integralOff T)]
      (hT_basis : S.latticeBasis ∈ intGL 𝔰.BesselIndex (integralOff T))
      (hT_N : (S.N : E)⁻¹ ∈ ratIntegralOff T)
      (h_off : ∀ i : ι, S.IsIntegralOff T (S.translateByClassReps (c i) (c' i) (σ i))) :
      ∑ i : ι, ‖∑ p : Fin 𝔰.ϱ → T₀, S.cosetAmplitude .cut (c i) (S.reduction (n i)) (c' i)
          (Floors.FloorGroups.weylElem 𝔰.ϱ (padTuple (fun a : Fin 𝔰.ϱ ↦ (p a).val) (t i)) *
            σ i)‖ ^ 2 ≤
        (1 + (S.b ^ 4)⁻¹) * (S.b ^ (6 * 𝔰.k))⁻¹ ^ #Sg * S.mainSecondMoment *
          S.unipotentCovolume ^ 2
  ```
- `04_spanning/energy_estimate.lean#New.Filling.linkSecondMoment_isOpenElement_le` (upstream `src/New/Spanning/MainTerm/EnergyEstimate.lean:620-685`):
  ```lean
  theorem New.Filling.linkSecondMoment_isOpenElement_le [S.IsAdmissible] (Sg : Finset (S.chosenPlaces 0))
      (D : ∀ w : S.chosenPlaces 0, S.FrameVector w) :
      linkSecondMoment {γ : S.G E | S.termDisc γ ≠ 0}
        {s : S.ResidueGL | s ∈ S.fixer Sg D ∧ S.IsOpenElement s} ≤
        .ofReal ((1 + (S.b ^ 4)⁻¹) * (S.b ^ (6 * 𝔰.k))⁻¹ ^ #Sg * S.mainSecondMoment *
          S.unipotentCovolume ^ 2)
  ```

See the page's live `<VerificationBanner />` (`audit/verification-status.json`) for the pinned upstream commit and local Lean build status (`build_status: unverified-locally`).
