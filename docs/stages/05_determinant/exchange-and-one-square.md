# Siegel–Weil Exchange and the Commuting Theta Square

This page unpacks the analytic engine at the foundation of Stage 05: `src/New/Determinant/Exchange/TheoremForPartners.lean` (`05_determinant/theorem_for_partners`, 27 declarations, **3 `let`-type stub truncations documented verbatim below**) and `src/New/Determinant/Exchange/OneSquare.lean` (`05_determinant/one_square`, 32 declarations, **5 `let`-type stub truncations including all three headline square theorems, documented verbatim below**). Return to the [Stage 05 Overview](./overview.md), or continue to [Chain Determinant Factorization and Two-Grid Auxiliary Forms](./chain-determinant-and-auxiliary-forms.md) and [Regular Twist Families and the Assembly of Theorem C](./regular-twists-and-theorem-c.md).

## Level 1 — First-Principles

Before Stage 05 can factor an $m$-step chain of theta lifts or sweep periods across two square grids, it needs one fundamental move: **trading a theta lift through a large space for a theta lift through a smaller complementary space**, with exact control over how the proportionality scalar changes when we twist by a tame character $\nu$. That move is built in two stacked steps—first for a single space $W$ (`TheoremForPartners.lean`), and then for a doubled space $W = U_1 \oplus U_2$ (`OneSquare.lean`).

![Side-by-side two-panel diagram contrasting complementary-rank theta exchange on the left with the doubled four-corner square on the right](assets/stage05-exchange-and-one-square.svg)

### Left panel: Complementary-rank theta exchange (`TheoremForPartners.lean`)

Take a Hermitian space $W$ of even rank $|n| = |a| + |b|$ with $q$ negative signs, paired against two partner spaces of {{concept:siegel-weil-exchange|complementary ranks}}: a larger partner $V$ of rank $|a| = |b| + 2e$ and a smaller partner $V'$ of rank $|b|$, whose negative signs add up to $q$. Using {{concept:theta-correspondence|unitary dual-pair theta kernels}}, integrating over $U(V)$ sends finite-prime data $\Phi$ (supported on the last $|a|$ rows of $k^{-1}$ for $k \in \mathrm{GL}_n(\mathcal{O}_E / \mathfrak{m})$) to a function on $U(W)$, while integrating over $U(V')$ sends finite-prime data $\Psi$ (supported on the first $|b|$ columns of $k$) to a function on $U(W)$.

When $\Phi$ and $\Psi$ are {{term:siegel-weil-exchange|matched functions}}—satisfying $\Phi(x) = \chi(\det k)\,\Psi(y)$ for the residue character $\chi$ of a tame character $\nu$ of weight $\mu$—the flagship {{term:siegel-weil-exchange|exchange identity}} (`New.Exchange.exchange_identity`) proves that these two integrals are proportional pointwise on $U(W)$:
$$\int_{[U(V)]} \theta_{W, V}(x \otimes \Phi)(g, h)\,dh = c \int_{[U(V')]} \theta_{V', W}(v \otimes \Psi)(h, g)\,dh,$$
with three crucial guarantees:
1. **Real vector fixed before twisting:** Given a real Schwartz vector $v$ for $(V', W)$, the matching Schwartz vector $x$ for $(W, V)$ and a base real constant $C > 0$ are chosen *once and for all* before picking any modulus $\mathfrak{m}$ or tame character $\nu$ of weight $\mu$.
2. **Exact $L$-value modulus:** The proportionality constant $c \in \mathbb{C}$ (in {{concept:complex-plane|the complex plane}}) depends on $(\mathfrak{m}, \nu, \chi)$, is guaranteed nonzero, and obeys
   $$\|c\|^2 = C \cdot L_*(e) \neq 0, \qquad L_*(e) = \Bigl(\prod_{w \in S} q_w^{e^2}\Bigr) \prod_{z=1}^{e} \bigl|L^S(z, \nu)\bigr|^2,$$
   where $S$ is the set of places dividing $\mathfrak{m}$ and $L^S(z, \nu)$ is the {{concept:hecke-l-value-ratio|partial Hecke L-value omitting $S \cup \bar{S}$}}.
3. **Reusable beyond Stage 05:** Besides feeding `OneSquare.lean` inside Stage 05, `05_determinant/theorem_for_partners` exports `exchange_identity` directly to Stage 06 (`06_one_lift/three_exchanges`, mediated in upstream Lean by `ExplicitMatching.lean`).

How does `TheoremForPartners.lean` pin down $\|c\|^2 = C \cdot L_*(e)$ with $C$ independent of $\nu$? First, `exchange_of_matches` descends from the top of a hyperbolic Witt tower and expresses $C_1 \|c\|^2$ as $L_*(e)$ times an {{term:siegel-weil-exchange|equal-rank theta ratio}} $|I(z)(1)|^2 / L_*(\operatorname{rk}(H \oplus W)/2)$ on $H \oplus W$ (extending $L_*$ to negative integers via `lValueProdInt`, $L_*(-e) = L_*(e)^{-1}$). Applying that identity *twice* on a positive-definite space $J$ of rank $2k$—once with $+k$ and once with $-k$—and equating the two sides by Fubini's theorem cancels $L_*(k)$ identically, proving that the equal-rank ratio is a nonzero constant independent of $\nu$ (`exists_equalRankThetaRatio_eq_const`).

### Right panel: The four-corner theta square (`OneSquare.lean`)

Now apply the doubling trick of {{concept:seesaw-duality|seesaw dual reductive pairs}}: set $W = U_1 \oplus U_2$, and pair $W$ against $A$ (rank $|n| + 2t$) at the top and $A'$ (rank $|n|$) at the bottom. A form $f_1$ on $U_1$ can travel to $U_2$ along two routes:
- **Top route ($\Theta_i(x)$):** lift from $U_1$ to $A$, then take the transposed lift from $A$ to $U_2$ using the two column blocks of a matrix $x$ modulo $\mathfrak{m}_i$.
- **Bottom route ($\Theta'_i(y)$):** take the transposed lift from $U_1$ to $A'$, then lift from $A'$ to $U_2$ using the two row blocks of a matrix $y$ modulo $\mathfrak{m}_i$.

Because the theta kernel on $(U_1 \oplus U_2, A)$ splits as a product of the edge kernels on $(U_1, A)$ and $(U_2, A)$, integrating `exchange_identity` against forms $f_1$ on $U_1$ and $f_2$ on $U_2$ yields the {{concept:theta-square-law|theta square law}} (`one_square_identity_of_isEdgeFamily`): both routes transform by $\chi_i(\det g)$ under invertible block matrices, and for all {{term:siegel-weil-exchange|matched functions}} $(\Psi, \Psi')$ modulo $\mathfrak{m}_i$,
$$\sum_{y} \Psi'(y)\,\Theta'_i(y) = \frac{\kappa}{c_i} \sum_{x} \Psi(x)\,\Theta_i(x), \qquad \|c_i\|^2 = C \cdot L_*(t) \neq 0.$$
Here $\kappa \in \mathbb{C}$ is independent of the index $i$, the character $\nu_i$, the modulus $\mathfrak{m}_i$, and the forms.

**Critical mathematical honesty note on $\kappa$:** `one_square_identity_of_isEdgeFamily` does **NOT** prove $\kappa \neq 0$! As upstream `GUIDE.md` §2 states explicitly, $\kappa$ is independent of the character, modulus, and forms, is never computed, and is *not* shown nonzero in `OneSquare.lean`. That is completely harmless here: non-vanishing of the composite chain scalar is established later in `ChainNorms.lean` and `NormOfChain.lean` (`chainDeterminant_pos`) from the injectivity of the first theta lifts in the convergent range of the {{concept:rallis-inner-product|Rallis inner product formula}}.

### Where this analogy breaks

Drawing `OneSquare.lean` as a commuting diamond of arrows from $U_1$ to $U_2$ makes it look like $\Theta'_i = (\kappa / c_i)\,\Theta_i$ as individual maps with a known nonzero ratio. Both impressions break in the real proof. First, individual point-matrix lifts never commute on their own—commutation holds *only* after summing over matched functions $(\Psi, \Psi')$ over the finite residue ring $\mathcal{O}_E / \mathfrak{m}_i$, where left- and right-$\mathrm{GL}$ equivariance by $\chi_i \circ \det$ ties the blocks together (`SquareLaw`). Second, passing from polynomial Fock vectors to the abstract Schwartz vector $w$ delivered by `exchange_identity` via moment-Lipschitz density (`exists_eq_mul_of_forall_polyVector`) gives a single twist-independent limit $\kappa$ without certifying $\kappa \neq 0$.

## Level 2 — Undergrad Bridge

### 1. Complementary-rank exchange (`TheoremForPartners.lean`, 27 decls)

1. **Adjoint coordinates and flipping $U$ (`adjointCoords`, `adjointCoords_finEquiv_apply`, `adjointDatum`, `IsSchwartzVector.adjointDatum`, `neg_mem_flipSpace_Gamma_iff`, `AreMatched.right_eq_pow_mul_apply_smul`):**
   Passing from $(U, W)$ to $(W, U^-)$ (where $U^- = \mathrm{flipSpace}\, U$ negates the signs $u \mapsto -u$) acts on finite adelic matrices by conjugate transpose times the integral unit `normNegOne` of norm $-1$. `exists_localDatum_adjointDatum_eq_smul` and `exists_mul_thetaSmall_adjointDatum_eq_mul_integral` convert `thetaSmall` on $(W, U^-)$ into the integral of `pairTheta` on $(U, W)$ up to a complex scalar $d$ of modulus $1$.
2. **From standard data to matched functions (`thetaLarge_eq_smul_of_areMatched`, `exists_rowColExchangeIdentity_of_even_rank`, `exchange_of_matches`):**
   On standard determinant data (`stdRowDatum σ χ z`, `stdColDatum χ z`), removing hyperbolic planes from $W_0 = H \oplus W$ (`RemovingPlanes.lean`) proves `exists_rowColExchangeIdentity_of_even_rank`. Expanding any locally constant matched pair $(\Phi, \Psi)$ in translates of standard data (`thetaLarge_eq_smul_of_areMatched`) and flipping $U^-$ back to $U$ yields `exchange_of_matches`:
   $$C \|c\|^2 = \mathrm{lValueProdInt}\ \mathfrak{c}\ (|r| - |n|/2) \cdot \mathrm{equalRankThetaRatio}(H \oplus W, \mathfrak{c}, z),$$
   where `lValueProdInt` extends `lValueProd` to $\mathbb{Z}$ via $L_*(-e) = L_*(e)^{-1}$.
3. **Why the equal-rank theta ratio is a nonzero constant (`integral_thetaOnRealData_eq_zero`, `thetaLargeVanishes_of_equalRankThetaRatio_eq_zero`, `exists_equalRankThetaRatio_ne_zero`, `eq_mul_of_integral_pairTheta`, `exists_thetaLarge_eq_mul_quotMeasure`, `exists_quotMeasure_mul_eq_mul_integral_pairTheta`, `exists_equalRankThetaRatio_sq_eq`, `exists_equalRankThetaRatio_const_ne_zero`, `exists_equalRankThetaRatio_add_plane`, `equalRankThetaRatio_eq_of_signs_eq`, `exists_equalRankThetaRatio_eq_const`):**
   - For positive-definite $J$ of rank $2k$: applying `exchange_of_matches` to $(J, J, 0)$ gives $B |c_1|^2 = L_*(k)\, R(\mathfrak{c})$ and $\mathrm{thetaLarge}(J, J) = c_1 \cdot \mathrm{vol}([U(0)])$ (`exists_thetaLarge_eq_mul_quotMeasure`); applying it to $(J, 0, J)$ gives $C |c_2|^2 = L_*(-k)\, R(\mathfrak{c})$ and $\mathrm{vol}([U(0)])\xi = c_2 \int \mathrm{pairTheta}$ (`exists_quotMeasure_mul_eq_mul_integral_pairTheta`).
   - By Fubini's theorem on the compact quotient $[U(J)] \times [U(J)]$ (`eq_mul_of_integral_pairTheta`), $\mathrm{vol}\cdot\xi = c_2 \cdot c_1 \cdot \mathrm{vol}$, so $|c_1 c_2|^2 = |\xi|^2$ is independent of $\mathfrak{c}$. Multiplying the two norm equations cancels $L_*(k) L_*(-k) = 1$, forcing $R(\mathfrak{c})^2 = \mathrm{const}$ (`exists_equalRankThetaRatio_sq_eq`).
   - Non-vanishing for $k \le \mu$ (`exists_equalRankThetaRatio_ne_zero`, proved by contrapositive via `thetaLargeVanishes_of_equalRankThetaRatio_eq_zero` and `VanishingDescends.lean`) plus hyperbolic-plane induction (`exists_equalRankThetaRatio_add_plane`) proves `exists_equalRankThetaRatio_eq_const`: for $\operatorname{rk} W = 2k + 2q$ and $k \le \mu$, some Schwartz vector $z$ on $H \oplus W$ has `equalRankThetaRatio` equal to a single constant $C_0 \neq 0$ across all tame characters of weight $\mu$.
4. **Mod-$\mathfrak{m}$ matching and arbitrary splitting characters (`spaceChar_mul_spaceChar_eq_iff`, `exists_areMatched_modDatum_eq`, `exchange_theorem`, `exchange_identity`):**
   Lifting matched functions over $\mathcal{O}_E / \mathfrak{m}$ to local completions (`exists_areMatched_modDatum_eq`) and plugging in the constant-$C_0$ vector $z$ yields `exchange_theorem` (with characters $\gamma, 1$ vs. $\gamma, \gamma$), and `spaceChar_mul_spaceChar_eq_iff` generalizes to arbitrary splitting characters $\alpha, \alpha'$ with $\alpha.\mathrm{spaceChar}\,|a| \cdot \alpha'.\mathrm{spaceChar}\,|b| = \nu.\mathrm{baseChange}^{-1}$ in **`New.Exchange.exchange_identity`**.

### 2. The commuting theta square (`OneSquare.lean`, 32 decls)

1. **Equivariance and support of matched functions (`pairWeil_modDatum_single_mul_left`, `pairWeil_modDatum_single_mul_right`, `hasLeftInv_transpose_of_areMatched`, `hasLeftInv_of_areMatched`, `IsTameFor.exists_inv_rankTwist_inv`):**
   Matched $(\Psi, \Psi')$ over $\mathcal{O}_E / \mathfrak{m}$ force $\Psi$ to live on onto matrices (`HasLeftInv xᵀ`) and $\Psi'$ to live on left-invertible matrices (`HasLeftInv y`), ensuring convergence of the theta integrals on both sides.
2. **Integrability, Fock-moment Lipschitz control, and polynomial density (`exists_integrable_kernel_bound`, `exists_eq_mul_of_forall_polyVector`, `upIntegrand`, `downIntegrand`, `exists_upIntegrand_bound`, `integrable_upIntegrand`, `exists_norm_integral_upIntegrand_sub_le`, `integrable_downIntegrand`):**
   Rapidly decreasing cusp forms $f_1 \in U_1.\mathrm{rapidFunctions}$, $f_2 \in U_2.\mathrm{rapidFunctions}$ absorb the polynomial height growth of the theta kernel on $U_1 \oplus U_2$, making `upIntegrand` integrable and Lipschitz in `fockMoment N w`.
3. **Kernel splitting and passing to arbitrary Schwartz vectors (`modKernel_single_sum_left`, `modKernel_single_sum_right`, `integral_upIntegrand`, `integral_downIntegrand`, `integral_sum_upIntegrand_eq_mul`, `exists_integral_upIntegrand_eq_mul_integral_lift`):**
   On Fock polynomials $P_1 \cdot P_2$, the kernel on $U_1 \oplus U_2$ splits into the product of the two edge kernels (`integral_upIntegrand`, `integral_downIntegrand`). Applying `exists_eq_mul_of_forall_polyVector` extends that proportionality to the Schwartz vector $w$ returned by `exchange_identity`, yielding the twist-independent scalar $\kappa \in \mathbb{C}$ (possibly $0$).
4. **Equivariance + matched sum $\Rightarrow$ `SquareLaw` (`pairTransposedLift_comp_pairLift_mul`, `pairLift_comp_pairTransposedLift_mul`, `IsSquare`, `exists_sum_pairLift_comp_pairTransposedLift_eq_smul`, `equivariant_and_exists_sum_eq_of_areMatched`, `one_square_identity_of_isEdgeFamily`):**
   Integrating `integral_sum_upIntegrand_eq_mul` over $[U_1] \times [U_2]$ against $(f_1, \bar{z})$ yields $\kappa \sum \Psi(x) \langle z, \Theta_i(x) f_1 \rangle = c_i \sum \Psi'(y) \langle z, \Theta'_i(y) f_1 \rangle$. Dividing by $c_i \neq 0$ (`ne_zero_of_norm_sq_eq`) proves `exists_sum_pairLift_comp_pairTransposedLift_eq_smul`, joins it with $\mathrm{GL}$-equivariance in `equivariant_and_exists_sum_eq_of_areMatched`, and packages the two-family case into `SquareLaw (χ i) σ (κ / c)` in **`New.Exchange.one_square_identity_of_isEdgeFamily`**.

## Level 3 — Formal Lean Contract

The extracted skeletons `skeleton/05_determinant/theorem_for_partners.lean` (27 declarations, **3 stub truncations**) and `skeleton/05_determinant/one_square.lean` (32 declarations, **5 stub truncations**) mirror `src/New/Determinant/Exchange/TheoremForPartners.lean` and `src/New/Determinant/Exchange/OneSquare.lean` at pinned upstream commit `41c2716658c42ba08eb0648994a2efe83082a95c`.

### Critical Honesty Disclosure: All 8 `let`-Type Stub Truncations (`theorem_for_partners.lean` [3] & `one_square.lean` [5])

Because `rh-extract` splits declaration signatures from proof bodies at the first depth-0 `:=` token, every declaration that introduces a `let ... :=` binding *inside its return type* truncates immediately at that `let` binding in the extracted skeleton (replaced by `:= by sorry`). In Stage 05 Exchange, this affects **3 lemmas in `theorem_for_partners.lean`** (`exchange_of_matches` L138, `exists_quotMeasure_mul_eq_mul_integral_pairTheta` L248, `exchange_theorem` L368 — whereas `exchange_identity` at L389 extracts completely untruncated) and **5 declarations in `one_square.lean`** (`exists_integrable_kernel_bound` L73, `exists_norm_integral_upIntegrand_sub_le` L198, and **all three headline square theorems**: `exists_sum_pairLift_comp_pairTransposedLift_eq_smul` L449, `equivariant_and_exists_sum_eq_of_areMatched` L505, and `one_square_identity_of_isEdgeFamily` L544).

For complete mathematical accuracy, all **8 exact untruncated statements** from pinned upstream commit `41c2716658c4` are quoted verbatim below:

#### A. Three truncated lemmas in `TheoremForPartners.lean`

```lean
-- 1. Upstream: src/New/Determinant/Exchange/TheoremForPartners.lean:325-342
-- (truncated at `let γ : AutChar E :=` in skeleton/05_determinant/theorem_for_partners.lean:138)
lemma exchange_of_matches (W : Space E n s) (U : Space E b u) (μ : InfinitePlace E → ℤ)
    (V : Space E r t) (σ : b ⊕ r ≃ n)
    {z : PairFock E (hyperbolicSigns Unit ⊕ᵥ s) (hyperbolicSigns Unit ⊕ᵥ s)} {y : PairFock E u s}
    (hz : IsSchwartzVector z) (hy : IsSchwartzVector y)
    (h_signs : negCount s = negCount t + negCount u) (h_even : 2 ∣ card n) :
    ∃ (C : ℝ) (v : PairFock E s t), IsSchwartzVector v ∧ ∀ 𝔠 : SplitTameChar E μ,
      let γ : AutChar E := rankTwist 𝔠.character⁻¹ n
      ∃ c : ℂ,
        -- (a) the modulus of the constant
        C * ‖c‖ ^ 2 =
          lValueProdInt 𝔠 (card r - card n / 2) * equalRankThetaRatio (hyperbolicSum Unit W) 𝔠 z ∧
        -- (b) the identity of theta integrals, for all matched functions
        ∀ (Φ : Matrix r n (LocProd 𝔠.ramifiedPlaces) → ℂ)
          (Ψ : Matrix n b (LocProd 𝔠.ramifiedPlaces) → ℂ),
          IsLocallyConstant Ψ → AreMatched SemilocalField.O σ 𝔠.semilocalChar Φ Ψ → ∀ g : G E s,
          thetaIntegral W V γ 1 (localDatum W V v Φ 𝔠.readsAt) g =
            c * ∫ h : G E u, pairTheta U W γ γ (localDatum U W y Ψ 𝔠.readsAt) (h, g)
              ∂U.quotMeasure

-- 2. Upstream: src/New/Determinant/Exchange/TheoremForPartners.lean:646-657
-- (truncated at `let γ : AutChar E :=` in skeleton/05_determinant/theorem_for_partners.lean:248)
lemma exists_quotMeasure_mul_eq_mul_integral_pairTheta (μ : InfinitePlace E → ℤ) {k : ℕ}
    (hm : card m = 2 * k) (J : Space E m 1)
    {v : PairFock E (Sum.elim (hyperbolicSigns Unit) (1 : m → ℤˣ))
      (Sum.elim (hyperbolicSigns Unit) (1 : m → ℤˣ))}
    {y : PairFock E (1 : m → ℤˣ) 1} (hv : IsSchwartzVector v) (hy : IsSchwartzVector y) :
    ∃ (C : ℝ) (ξ : ℂ), ∀ 𝔠 : SplitTameChar E μ,
      let γ : AutChar E := rankTwist 𝔠.character⁻¹ m
      ∃ c : ℂ,
        C * ‖c‖ ^ 2 = lValueProdInt 𝔠 (-k) * equalRankThetaRatio (hyperbolicSum Unit J) 𝔠 v ∧
        ∀ g : G E (1 : m → ℤˣ), (stdSpace E Empty).quotMeasure.real Set.univ * ξ =
          c * ∫ h : G E (1 : m → ℤˣ), pairTheta J J γ γ
            (localDatum J J y (stdColDatum 𝔠.semilocalChar 1) 𝔠.readsAt) (h, g) ∂J.quotMeasure

-- 3. Upstream: src/New/Determinant/Exchange/TheoremForPartners.lean:985-1003
-- (truncated at `let γ : AutChar E :=` in skeleton/05_determinant/theorem_for_partners.lean:368)
lemma exchange_theorem (σ : b ⊕ a ≃ n) (h_signs : negCount s = negCount t + negCount t') {e : ℕ}
    (h_rank : card a = card b + 2 * e) (h_few_neg : 2 * negCount s + 2 ≤ card n)
    {μ : NumberField.InfinitePlace E → ℤ}
    (hμ : ∀ τ : NumberField.InfinitePlace E, card n ≤ 2 * μ τ + 2 * negCount s)
    {v : Fock.fockSpace (realSigns E (homSigns t' s))} (hv : IsSchwartzVector v) :
    ∃ (C : ℝ) (x : Fock.fockSpace (realSigns E (homSigns s t))),
      IsSchwartzVector x ∧
      ∀ (𝔪 : Modulus E) (ν : AutChar E) (χ : MulChar 𝔪.ResidueRing ℂ), IsTameFor 𝔪 ν μ χ →
        let γ : AutChar E := rankTwist ν⁻¹ n
        ∃ c ≠ (0 : ℂ),
          -- (a) the size of the constant
          ‖c‖ ^ 2 = C * lValueProd 𝔪.places ν e ∧
          -- (b) the exchange of the two theta integrals on matched functions modulo `𝔪`
          ∀ (Φ : Matrix a n 𝔪.ResidueRing → ℂ) (Ψ : Matrix n b 𝔪.ResidueRing → ℂ),
            AreMatched ⊤ σ χ Φ Ψ →
            ∀ g : G E s,
            thetaIntegral W V γ 1 (modDatum W V 𝔪 Φ x) g =
              c *
                ∫ h : G E t', pairTheta V' W γ γ (modDatum V' W 𝔪 Ψ v) (h, g) ∂V'.quotMeasure
```

#### B. Five truncated declarations in `OneSquare.lean` (including all three headline square theorems)

```lean
-- 4. Upstream: src/New/Determinant/Exchange/OneSquare.lean:156-167
-- (truncated at `let F : ... :=` in skeleton/05_determinant/one_square.lean:73)
lemma exists_integrable_kernel_bound {A : Space E n' s'} {U₁ : Space E n₁ s₁}
    {U₂ : Space E n₂ s₂} {φ₁ : G E s₁ → ℂ} {φ₂ : G E s₂ → ℂ} (hφ₁ : φ₁ ∈ U₁.rapidFunctions)
    (hφ₂ : φ₂ ∈ U₂.rapidFunctions) (ν₀ : AutChar E)
    {K : fockSpace s → G E (s₁ ⊕ᵥ s₂) → G E s' → ℂ}
    (hK_le : ∃ (B N : ℕ) (C : ℝ), ∀ (v : fockSpace s) (g : G E (s₁ ⊕ᵥ s₂)) (z : G E s'),
      fockMoment N v ≠ ⊤ → ‖K v g z‖ ≤ C * (fockMoment N v).toReal * (U₁.sum U₂).height g ^ B)
    (hK_cont : ∀ v : fockSpace s, Continuous ↿(K v)) :
    ∃ (N : ℕ) (C : ℝ), ∀ v : fockSpace s, fockMoment N v ≠ ⊤ →
      let F : (G E s₁ × G E s₂) × G E s' → ℂ := fun ((g₁, g₂), z) ↦
        φ₁ g₁ * φ₂ g₂ * (ν₀.val g₂.det * K v (G.sum (g₁, g₂)) z)
      Integrable F ((U₁.quotMeasure.prod U₂.quotMeasure).prod A.quotMeasure) ∧
      ∀ q : (G E s₁ × G E s₂) × G E s', ‖F q‖ ≤ C * (fockMoment N v).toReal

-- 5. Upstream: src/New/Determinant/Exchange/OneSquare.lean:350-359
-- (truncated at `let μA : ... :=` in skeleton/05_determinant/one_square.lean:198)
lemma exists_norm_integral_upIntegrand_sub_le (h𝔪 : Nonempty 𝔪.places)
    (hf₁ : f₁ ∈ U₁.rapidFunctions) (hf₂ : f₂ ∈ U₂.rapidFunctions)
    {x : Matrix n' (n₁ ⊕ n₂) 𝔪.ResidueRing} (hx : HasLeftInv xᵀ) :
    let μA : Measure ((G E s₁ × G E s₂) × G E s') :=
      (U₁.quotMeasure.prod U₂.quotMeasure).prod A.quotMeasure
    ∃ (N : ℕ) (D : ℝ), ∀ v v' : PairFock E (s₁ ⊕ᵥ s₂) s',
      fockMoment N v ≠ ⊤ → fockMoment N v' ≠ ⊤ → fockMoment N (v' - v) ≠ ⊤ →
      ‖∫ q : (G E s₁ × G E s₂) × G E s', upIntegrand A U₁ U₂ ν₀ γ b f₁ f₂ x v q ∂μA -
          ∫ q : (G E s₁ × G E s₂) × G E s', upIntegrand A U₁ U₂ ν₀ γ b f₁ f₂ x v' q ∂μA‖ ≤
        D * (fockMoment N (v' - v)).toReal

-- 6. Upstream: src/New/Determinant/Exchange/OneSquare.lean:747-771
-- (truncated at `let Θ ... :=` in skeleton/05_determinant/one_square.lean:449)
theorem exists_sum_pairLift_comp_pairTransposedLift_eq_smul
    (A' : Space E n s) (A : Space E n' s') (U₁ : Space E n₁ s₁) (U₂ : Space E n₂ s₂)
    {I : Type*} {e : GInf E s' → ℂ} {e' : GInf E s → ℂ} {e₁ : GInf E s₁ → ℂ} {e₂ : GInf E s₂ → ℂ}
    {a₁ a₂ b₁ b₂ a₁' a₂' b₁' b₂' : I → AutChar E}
    {P₁ : MvPolynomial (InfinitePlace E × n' × n₁) ℂ}
    {P₂ : MvPolynomial (InfinitePlace E × n' × n₂) ℂ}
    {P₁' : MvPolynomial (InfinitePlace E × n₁ × n) ℂ}
    {P₂' : MvPolynomial (InfinitePlace E × n₂ × n) ℂ} (σ : n ⊕ n' ≃ n₁ ⊕ n₂) {t : ℕ}
    {μ : InfinitePlace E → ℤ} {𝔪 : I → Modulus E} {ν : I → AutChar E}
    {χ : ∀ i : I, MulChar (𝔪 i).ResidueRing ℂ}
    (h_square : IsSquare e e' e₁ e₂ a₁ a₂ b₁ b₂ a₁' a₂' b₁' b₂' P₁ P₂ P₁' P₂' t μ 𝔪 ν χ) :
    let Θ (i : I) (x : Matrix n' (n₁ ⊕ n₂) (𝔪 i).ResidueRing) : U₁.Form e₁ →ₗ[ℂ] U₂.Form e₂ :=
      pairTransposedLift U₂ A (a₂ i) (b₂ i) P₂ (Pi.single x.toCols₂ 1) e e₂ ∘ₗ
        pairLift U₁ A (a₁ i) (b₁ i) P₁ (Pi.single x.toCols₁ 1) e₁ e
    let Θ' (i : I) (y : Matrix (n₁ ⊕ n₂) n (𝔪 i).ResidueRing) : U₁.Form e₁ →ₗ[ℂ] U₂.Form e₂ :=
      pairLift A' U₂ (b₂' i) (a₂' i) P₂' (Pi.single y.toRows₂ 1) e' e₂ ∘ₗ
        pairTransposedLift A' U₁ (b₁' i) (a₁' i) P₁' (Pi.single y.toRows₁ 1) e₁ e'
    ∃ C : ℝ, 0 < C ∧ ∃ κ : ℂ, ∀ i : I, ∃ c : ℂ,
      ‖c‖ ^ 2 = C * lValueProd (𝔪 i).places (ν i) t ∧
      C * lValueProd (𝔪 i).places (ν i) t ≠ 0 ∧
      ∀ (Ψ : Matrix n' (n₁ ⊕ n₂) (𝔪 i).ResidueRing → ℂ)
        (Ψ' : Matrix (n₁ ⊕ n₂) n (𝔪 i).ResidueRing → ℂ),
        AreMatched ⊤ σ (χ i) Ψ Ψ' →
          ∑ y : Matrix (n₁ ⊕ n₂) n (𝔪 i).ResidueRing, Ψ' y • Θ' i y =
            (κ / c) • ∑ x : Matrix n' (n₁ ⊕ n₂) (𝔪 i).ResidueRing, Ψ x • Θ i x

-- 7. Upstream: src/New/Determinant/Exchange/OneSquare.lean:896-929
-- (truncated at `let Θ ... :=` in skeleton/05_determinant/one_square.lean:505)
theorem equivariant_and_exists_sum_eq_of_areMatched {E n n' n₁ n₂ : Type} [Field E] [CMType E]
    [WeilData E] [Fintype n] [DecidableEq n] [Fintype n'] [DecidableEq n']
    [Fintype n₁] [DecidableEq n₁] [Fintype n₂] [DecidableEq n₂]
    {s : n → ℤˣ} {s' : n' → ℤˣ} {s₁ : n₁ → ℤˣ} {s₂ : n₂ → ℤˣ}
    (A' : Space E n s) (A : Space E n' s') (U₁ : Space E n₁ s₁) (U₂ : Space E n₂ s₂) {I : Type*}
    {e : GInf E s' → ℂ} {e' : GInf E s → ℂ} {e₁ : GInf E s₁ → ℂ} {e₂ : GInf E s₂ → ℂ}
    {a₁ a₂ b₁ b₂ a₁' a₂' b₁' b₂' : I → AutChar E}
    {P₁ : MvPolynomial (InfinitePlace E × n' × n₁) ℂ}
    {P₂ : MvPolynomial (InfinitePlace E × n' × n₂) ℂ}
    {P₁' : MvPolynomial (InfinitePlace E × n₁ × n) ℂ}
    {P₂' : MvPolynomial (InfinitePlace E × n₂ × n) ℂ} (σ : n ⊕ n' ≃ n₁ ⊕ n₂) {t : ℕ}
    {μ : InfinitePlace E → ℤ} {𝔪 : I → Modulus E} {ν : I → AutChar E}
    {χ : ∀ i : I, MulChar (𝔪 i).ResidueRing ℂ}
    (h_square : IsSquare e e' e₁ e₂ a₁ a₂ b₁ b₂ a₁' a₂' b₁' b₂' P₁ P₂ P₁' P₂' t μ 𝔪 ν χ) :
    let Θ (i : I) (x : Matrix n' (n₁ ⊕ n₂) (𝔪 i).ResidueRing) : U₁.Form e₁ →ₗ[ℂ] U₂.Form e₂ :=
      pairTransposedLift U₂ A (a₂ i) (b₂ i) P₂ (Pi.single x.toCols₂ 1) e e₂ ∘ₗ
        pairLift U₁ A (a₁ i) (b₁ i) P₁ (Pi.single x.toCols₁ 1) e₁ e
    let Θ' (i : I) (y : Matrix (n₁ ⊕ n₂) n (𝔪 i).ResidueRing) : U₁.Form e₁ →ₗ[ℂ] U₂.Form e₂ :=
      pairLift A' U₂ (b₂' i) (a₂' i) P₂' (Pi.single y.toRows₂ 1) e' e₂ ∘ₗ
        pairTransposedLift A' U₁ (b₁' i) (a₁' i) P₁' (Pi.single y.toRows₁ 1) e₁ e'
    -- (a)
    (∀ (i : I) (g : GL n' (𝔪 i).ResidueRing) (x : Matrix n' (n₁ ⊕ n₂) (𝔪 i).ResidueRing),
      Θ i (g.val * x) = χ i g.val.det • Θ i x) ∧
    (∀ (i : I) (g : GL n (𝔪 i).ResidueRing) (y : Matrix (n₁ ⊕ n₂) n (𝔪 i).ResidueRing),
      Θ' i (y * g.val) = χ i g.val.det • Θ' i y) ∧
    -- (b)
    ∃ C : ℝ, 0 < C ∧ ∃ κ : ℂ, ∀ i : I, ∃ c : ℂ,
      ‖c‖ ^ 2 = C * lValueProd (𝔪 i).places (ν i) t ∧
      C * lValueProd (𝔪 i).places (ν i) t ≠ 0 ∧
      ∀ (Ψ : Matrix n' (n₁ ⊕ n₂) (𝔪 i).ResidueRing → ℂ)
        (Ψ' : Matrix (n₁ ⊕ n₂) n (𝔪 i).ResidueRing → ℂ),
        AreMatched ⊤ σ (χ i) Ψ Ψ' →
          ∑ y : Matrix (n₁ ⊕ n₂) n (𝔪 i).ResidueRing, Ψ' y • Θ' i y =
            (κ / c) • ∑ x : Matrix n' (n₁ ⊕ n₂) (𝔪 i).ResidueRing, Ψ x • Θ i x

-- 8. Upstream: src/New/Determinant/Exchange/OneSquare.lean:972-1003
-- (truncated at `let Θ ... :=` in skeleton/05_determinant/one_square.lean:544)
lemma one_square_identity_of_isEdgeFamily (A' : Space E n s) (A : Space E n' s')
    (U₁ : Space E n₁ s₁) (U₂ : Space E n₂ s₂) {I : Type*}
    {k₁ : Bool → GInf E s₁ → ℂ} {k₂ : Bool → GInf E s₂ → ℂ} {d : Bool}
    {k : Bool → GInf E s' → ℂ} {Φ₁ : MvPolynomial (InfinitePlace E × n' × n₁) ℂ}
    {Φ₂ : MvPolynomial (InfinitePlace E × n' × n₂) ℂ} {k' : Bool → GInf E s → ℂ}
    {Φ₁' : MvPolynomial (InfinitePlace E × n₁ × n) ℂ}
    {Φ₂' : MvPolynomial (InfinitePlace E × n₂ × n) ℂ}
    {a b : I → ∀ (m : Type) [Fintype m], AutChar E} {ν : I → AutChar E} {𝔪 : I → Modulus E}
    {χ : ∀ i : I, MulChar (𝔪 i).ResidueRing ℂ}
    (h₂ : IsEdgeFamily b k₂ k d Φ₂) (h₁ : IsEdgeFamily a k₁ k (!d) Φ₁)
    (h₂' : IsEdgeFamily a k' k₂ (!d) Φ₂') (h₁' : IsEdgeFamily b k' k₁ d Φ₁') (σ : n ⊕ n' ≃ n₁ ⊕ n₂)
    (h_signs : negCount (s₁ ⊕ᵥ s₂) = negCount s' + negCount s) (t : ℕ)
    (h_rank : card n' = card n + 2 * t) (h_few_neg : 2 * negCount (s₁ ⊕ᵥ s₂) + 2 ≤ card (n₁ ⊕ n₂))
    {μ : InfinitePlace E → ℤ}
    (hμ : ∀ τ : InfinitePlace E, card (n₁ ⊕ n₂) ≤ 2 * μ τ + 2 * negCount (s₁ ⊕ᵥ s₂))
    (h_tame : ∀ i : I, IsTameFor (𝔪 i) (ν i) μ (χ i))
    (h_chars : ∀ (i : I) {p q : Type} [Fintype p] [Fintype q],
      card p + card q = card n₁ + card n₂ → a i p * b i q = rankTwist (ν i)⁻¹ (n₁ ⊕ n₂)) :
    let Θ (i : I) (x : Matrix n' (n₁ ⊕ n₂) (𝔪 i).ResidueRing) :
        U₁.Form (k₁ !d) →ₗ[ℂ] U₂.Form (k₂ !d) :=
      modTransposedLift U₂ A (b i) Φ₂ (Pi.single x.toCols₂ 1) (k !d) (k₂ !d) ∘ₗ
        modLift U₁ A (a i) Φ₁ (Pi.single x.toCols₁ 1) (k₁ !d) (k !d)
    let Θ' (i : I) (y : Matrix (n₁ ⊕ n₂) n (𝔪 i).ResidueRing) :
        U₁.Form (k₁ !d) →ₗ[ℂ] U₂.Form (k₂ !d) :=
      modLift A' U₂ (a i) Φ₂' (Pi.single y.toRows₂ 1) (k' !d) (k₂ !d) ∘ₗ
        modTransposedLift A' U₁ (b i) Φ₁' (Pi.single y.toRows₁ 1) (k₁ !d) (k' !d)
    ∃ (C : ℝ) (κ : ℂ), ∀ i : I, ∃ c : ℂ,
      c ≠ 0 ∧
      ‖c‖ ^ 2 = C * lValueProd (𝔪 i).places (ν i) t ∧
      ∀ (f : U₁.Form (k₁ !d)) (l : U₂.Form (k₂ !d) →ₗ[ℂ] ℂ),
        SquareLaw (χ i) σ (κ / c) (fun x : Matrix n' (n₁ ⊕ n₂) (𝔪 i).ResidueRing ↦ l (Θ i x f))
          (fun y : Matrix (n₁ ⊕ n₂) n (𝔪 i).ResidueRing ↦ l (Θ' i y f))
```

### Untruncated Flagship Signature (`exchange_identity`)

From `skeleton/05_determinant/theorem_for_partners.lean:389-407` (upstream `TheoremForPartners.lean:1051-1113`):

```lean
theorem New.Exchange.exchange_identity (σ : b ⊕ a ≃ n) (h_signs : negCount s = negCount t + negCount t')
    {e : ℕ} (h_rank : card a = card b + 2 * e) (h_few_neg : 2 * negCount s + 2 ≤ card n)
    {μ : NumberField.InfinitePlace E → ℤ}
    (hμ : ∀ τ : NumberField.InfinitePlace E, card n ≤ 2 * μ τ + 2 * negCount s)
    {v : PairFock E t' s} (hv : IsSchwartzVector v) :
    ∃ C : ℝ, 0 < C ∧ ∃ x : PairFock E s t, IsSchwartzVector x ∧
      ∀ (𝔪 : Modulus E) (ν : AutChar E) (χ : MulChar 𝔪.ResidueRing ℂ), IsTameFor 𝔪 ν μ χ →
      ∃ c : ℂ,
        ‖c‖ ^ 2 = C * lValueProd 𝔪.places ν e ∧
        C * lValueProd 𝔪.places ν e ≠ 0 ∧
        ∀ α α' : AutChar E,
          α.spaceChar (card a) * α'.spaceChar (card b) = ν.baseChange⁻¹ →
          ∀ (Φ : Matrix a n 𝔪.ResidueRing → ℂ) (Ψ : Matrix n b 𝔪.ResidueRing → ℂ),
            AreMatched ⊤ σ χ Φ Ψ →
            ∀ g : G E s,
              thetaIntegral W V (rankTwist ν⁻¹ n) α (modDatum W V 𝔪 Φ x) g =
                c * ∫ h : G E t',
                  pairTheta V' W α' (rankTwist ν⁻¹ n) (modDatum V' W 𝔪 Ψ v) (h, g)
                  ∂V'.quotMeasure
```

- **Verification status (`audit/verification-status.json`):** Local Lean compilation and kernel replay remain **`build_status: unverified-locally`** at pinned commit `41c2716658c42ba08eb0648994a2efe83082a95c` (`41c2716658c4`). Static token scanning has **`scan_scope: entry`**, covering only `Challenge.lean`, `src/Main.lean`, and `src/Statements.lean` (`0` forbidden tokens in solution files, `5` expected `sorry` tokens in `Challenge.lean`). Neither `TheoremForPartners.lean` nor `OneSquare.lean` has been locally compiled, kernel-checked, or statically scanned here.

<!-- rh:lean-node:05_determinant/theorem_for_partners -->

<!-- rh:lean-node:05_determinant/one_square -->
