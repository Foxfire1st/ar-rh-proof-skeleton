---
slug: hecke-l-value-ratio
term: Hecke L-value ratio
aliases: ["critical pair", "ramified pair", "main pair", "quotient ratio", "turn ratio", "partial Hecke L-value"]
inline_glimpse: "The ratio |L^S(z, (nu * theta^s)^-1)| / |L^S(z, nu^-1)| of twisted to untwisted partial Hecke L-values at a critical pair (nu, z), tracking how Siegel–Weil exchange constants scale under twisting across chain quotients and grid turns."
visual_ref: assets/stage05-theorem-c-blueprint.svg
where_analogy_breaks: "Treating a Hecke L-value ratio as a quotient of two ordinary Dirichlet L-values at the same Euler product hides an asymmetric omission of Euler factors: the denominator omits the chosen places and their conjugates, whereas the numerator omits BOTH the chosen places and the twist places Q (and their conjugates). Moreover, Stage 05 never bounds the ratio at a single prime in isolation; it bounds the global arithmetic height of the INVERSE ratio across all prime embeddings iota via HasSmallRatios."
sources:
  - title: "The special values of the zeta functions associated with Hilbert modular forms"
    citation: "G. Shimura, Duke Math. J. 45 (1978), 637–679."
    kind: paper
  - title: "L-functions and periods of polarized regular motives"
    citation: "M. Harris, J. Reine Angew. Math. 483 (1997), 75–161."
    kind: paper
  - title: "On the conjecture of Birch and Swinnerton-Dyer"
    citation: "J. Coates & A. Wiles, Invent. Math. 39 (1977), 223–251."
    kind: paper
lean_anchors:
  - 05_determinant/fit_twists.lean#New.Data.CriticalPair.ratio
  - 05_determinant/fit_twists.lean#New.Data.CriticalPair.IsRamified
  - 05_determinant/fit_twists.lean#New.Data.CriticalPair.IsMainPair
  - 05_determinant/norm_of_chain.lean#New.Data.HasSmallRatios.heightLe_inv_rpow_mul_prod_quotientRatio
  - 05_determinant/long_forms.lean#New.Data.HasSmallRatios.heightLe_inv_rpow_mul_prod_turnRatio
child_concepts:
  - dirichlet-l-function
  - arithmetic-height
  - siegel-weil-exchange
  - regular-twist-family
  - rallis-inner-product
  - auxiliary-spanning-form
---

# Hecke L-Value Ratio

## Level 1 — First-Principles

Earlier in the proof, Stage 00 introduced {{concept:dirichlet-l-function|global $L$-functions built from periodic multiplicative characters over the rational numbers}}. Over the cyclotomic CM field $E$, the same role is played by a **partial Hecke L-value** $L^S(z, \nu)$, built from an automorphic character $\nu$ of $U(1)$ evaluated at a positive integer $z$ while omitting Euler factors at a finite set of places $S$ and their complex conjugates. Whenever Stage 05 applies {{concept:siegel-weil-exchange|complementary-rank exchange}} to commute two theta lifts, the proportionality scalar $c$ changes by a product of **partial Hecke L-values** $|L^S(z, \nu)|$ evaluated at the twist character.

To isolate how a twist $\vartheta$ alters those scalars without ever computing the untwisted archimedean period constants, Stage 05 takes a **Hecke L-value ratio**. Fix a **critical pair** $p = (\nu, z)$ consisting of an integer $1 \le z \le t$ and a character $\nu$ of weight $-c$ (with $z \le c(v) \le 4b$ at every infinite place) whose tame ramification lives on the chosen places. For a tame character $\vartheta$ with ramified place set $Q$ and a sign $s \in \{\pm 1\}$ (default $s = 1$), the ratio is
$$p.\mathrm{ratio}(\vartheta, s) = \frac{\bigl|L^{\mathrm{chosen} \cup Q}\bigl(z, (\nu \vartheta^s)^{-1}\bigr)\bigr|}{\bigl|L^{\mathrm{chosen}}\bigl(z, \nu^{-1}\bigr)\bigr|}.$$
Two subclasses of **critical pairs** govern Theorem C:
- **Ramified pairs (`IsRamified`):** Those whose tame part is nontrivial at *every* chosen place (and trivial off the chosen places). Under {{concept:regular-twist-family|a regular twist family}}, every **ramified pair**'s inverse ratio at $\theta(\iota)^{\pm j}$ ($1 \le j \le m$) has {{concept:arithmetic-height|global arithmetic height}} at most $x_1$.
- **Main pairs (`IsMainPair j`):** A sharper subset for each lift $1 \le j \le m$ with $m < z \le r$, tame part equal to $\omega^j$ (so every **main pair** is automatically a **ramified pair**, `IsMainPair.isRamified`), and weight close to the $j$-th lift weight. These are the pairs whose rank-one theta lifts into a definite space of rank $2z$ control the $L$-value heights in Stage 06.

Why do these ratios appear twice in Stage 05?
1. **Quotient ratios in `NormOfChain.lean`:** Sliding $m$ lifts past descents in the {{concept:rallis-inner-product|triangle of lifts}} multiplies $\binom{m}{2} \le m^2$ **quotient ratios** `D.quotientRatio i j` ($1 \le i < j \le m$), each a product of $t$ critical ratios for $\chi_j / \chi_i$ at $\vartheta^{j-i}$. Their inverse product has height at most $m^2 t x_1 \le b$.
2. **Turn ratios in `LongForms.lean` / `SpanningForms.lean`:** Moving periods across $m^2$ squares of the {{concept:auxiliary-spanning-form|two-grid path}} multiplies $m^2$ **turn ratios** `D.turnRatio i j` ($0 \le i, j < m$). Their inverse product also has height at most $m^2 t x_1$, and after squaring ($2 m^2 t x_1 \le b$) contributes at most $b$ to each auxiliary form's completion cost.

### Limits of the intuition

Calling `CriticalPair.ratio` a simple ratio of two values of the same $L$-function misses two critical details. First, the omitted Euler sets are *not* identical: the numerator omits both the chosen places and the twist places $Q$ (and their conjugates), while the denominator omits only the chosen places (and their conjugates), matching the exact partial $L$-values produced by `lValueProd`. Second, Stage 05 bounds the height of the *inverse* ratio $(p.\mathrm{ratio})^{-1}$—never the ratio itself—because $Z(1)/Z(\vartheta)$ and $|\gamma(\vartheta)|^{-2}$ place the twisted scalars in the denominator inside `TwistedLattice.covolume_add_index_nonneg`.

## Level 2 — Undergrad Bridge

In Shimura (1978), Coates–Wiles (1977), and Harris (1997), critical values of Hecke $L$-functions of algebraic characters on CM fields become algebraic numbers after dividing by CM periods; taking the ratio of a twisted critical value to an untwisted critical value of the same weight cancels the transcendental CM period identically, leaving an algebraic number whose prime-by-prime valuations control its arithmetic height.

1. **Exact definition (`FitTwists.lean:78-84`):**
   `CriticalPair.ratio p ϑ s` is defined as the complex coercion of the real quotient
   $$\frac{\bigl\| (p.\mathrm{character} \cdot \vartheta.\mathrm{toAutChar}^s)^{-1}.L(\{\text{chosen}\} \cup \vartheta.Q, p.z) \bigr\|}{\bigl\| p.\mathrm{character}^{-1}.L(\{\text{chosen}\}, p.z) \bigr\|}.$$
   Inverting the character in the denominator does not change the modulus (`CriticalPair.L_inv`).
2. **Ramified pairs vs. main pairs (`IsRamified`, `IsMainPair`, `IsMainPair.isRamified`):**
   A `CriticalPair` has $1 \le z \le t$ and weight $-c$ with $z \le c(v) \le 4b$. `IsRamified` requires the tame part $\psi_w$ to be nontrivial *iff* $w$ is a chosen place (dividing `D.piM` of norm $p_0 p_1$). `IsMainPair j` adds:
   - `HasLiftRank (RankOneIndex p.z)` (splitting rank $2z$ into blocks $12, 4, 4, 2z - 20$ with $z \ge 2$);
   - $m < z \le r$ and $1 \le j \le m$;
   - `p.tamePart = D.chiMLocal ^ (j : ℤ)` (since `D.chiMLocal` has order $> 8m$ at each chosen place, raising to $j \le m$ stays nontrivial, proving `IsMainPair.isRamified`);
   - $w_j + r - t \le c(v) \le w_j$ at every infinite place $v \in D.\Phi$.
3. **Height aggregation across $m^2$ factors (`heightLe_inv_rpow_mul_prod_quotientRatio`, `heightLe_inv_rpow_mul_prod_turnRatio`):**
   Because `HeightLe` is additive under products and any integer power $(\mathrm{Nm}(Q))^{e/2}$ contributes $0$ to the height at primes $\ell \nmid \mathrm{Nm}(Q)$ (it is an $\ell$-adic unit at every $\iota$ with $\gcd(\ell, \mathrm{Nm}(Q)) = 1$, and archimedean powers cancel with the coprime norm), each `quotientRatio i j` ($1 \le i < j \le m$, at most $m^2$ pairs) and each `turnRatio i j` ($0 \le i, j < m$, exactly $m^2$ pairs) is a product of $t$ ramified ratios of height $\le x_1$. Summing heights yields
   $$\mathrm{HeightLe}\bigl(m^2 t x_1\bigr)$$
   for both inverse products. Because $x_1^{10^6} \le b$ while $m \asymp \sqrt{x}$ and $t = 5r \asymp x^5$, the scale inequality `D.le_of_hasLogBound` crushes both $m^2 t x_1 \le b$ (for the chain determinant scalar in `heightLe_chainDeterminant_one_div`) and $2 m^2 t x_1 \le b$ (for the squared auxiliary scalar $|\gamma(\theta_\iota)|^{-2}$ in `integral_forms_long_with_completion_cost`).

## Level 3 — Formal Lean Contract

Stage 05 anchors the critical-pair ratio contract across `fit_twists.lean`, `norm_of_chain.lean`, and `long_forms.lean` (all 5 anchors below extract untruncated):

- `05_determinant/fit_twists.lean#New.Data.CriticalPair.ratio` (upstream `FitTwists.lean:78-84`):
  ```lean
  noncomputable def New.Data.CriticalPair.ratio (p : D.CriticalPair) (ϑ : D.TameChar) (s : ℤˣ := 1) : ℂ
  ```
- `05_determinant/fit_twists.lean#New.Data.CriticalPair.IsRamified` (upstream `FitTwists.lean:116-120`) and `05_determinant/fit_twists.lean#New.Data.CriticalPair.IsMainPair` (upstream `FitTwists.lean:122-140`).
- `05_determinant/norm_of_chain.lean#New.Data.HasSmallRatios.heightLe_inv_rpow_mul_prod_quotientRatio` (upstream `NormOfChain.lean:437-453`):
  ```lean
  lemma New.Data.HasSmallRatios.heightLe_inv_rpow_mul_prod_quotientRatio {D : Data P}
      {θ : PrimePadicIso → D.MFoldTwist} (hθ : D.HasSmallRatios θ) (e : ℤ) :
      HeightLe (D.m * D.m * (D.t * D.x₁)) fun ι : PrimePadicIso ↦
        ((((θ ι).normQ : ℝ) ^ (e / 2 : ℝ) *
          ∏ i ∈ Ioc 0 D.m, ∏ j ∈ Ioc i D.m, D.quotientRatio i j (θ ι) : ℝ) : ℂ)⁻¹
  ```
- `05_determinant/long_forms.lean#New.Data.HasSmallRatios.heightLe_inv_rpow_mul_prod_turnRatio` (upstream `LongForms.lean:107-119`):
  ```lean
  lemma New.Data.HasSmallRatios.heightLe_inv_rpow_mul_prod_turnRatio {θ : PrimePadicIso → D.MFoldTwist}
      (hθ : D.HasSmallRatios θ) (e : ℤ) :
      HeightLe (D.m * D.m * (D.t * D.x₁)) fun ι : PrimePadicIso ↦
        ((((θ ι).normQ : ℝ) ^ (e / 2 : ℝ) *
          ∏ i ∈ Finset.range D.m, ∏ j ∈ Finset.range D.m, D.turnRatio i j (θ ι) : ℝ) : ℂ)⁻¹
  ```

See the page's live `<VerificationBanner />` (`audit/verification-status.json`) for the pinned upstream commit and local Lean build status (`build_status: unverified-locally`).
