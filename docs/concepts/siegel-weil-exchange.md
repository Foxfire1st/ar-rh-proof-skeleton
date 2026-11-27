---
slug: siegel-weil-exchange
term: Siegel–Weil exchange
aliases: ["Siegel-Weil exchange", "exchange identity", "matched function", "equal-rank theta ratio", "complementary rank"]
inline_glimpse: "Comparing theta integrals to U(W) from two complementary-rank spaces V and V' on matched ramified matrix functions modulo m—agreeing up to a nonzero scalar c with |c|^2 = C * L_*(e)."
visual_ref: assets/stage05-exchange-and-one-square.svg
where_analogy_breaks: "Picturing Siegel–Weil exchange as a direct change of variables between two integrals hides the asymmetric existence statement: for a given real Schwartz vector v on (V', W), the theorem constructs a matching Schwartz vector x on (W, V) once and for all before the modulus m and character nu are chosen, and it evaluates |c|^2 = C * L_*(e) only up to an uncomputed positive real constant C independent of (m, nu, chi)."
sources:
  - title: "Sur la formule de Siegel dans la théorie des groupes classiques"
    citation: "A. Weil, Acta Math. 113 (1965), 1–87."
    kind: paper
  - title: "A regularized Siegel–Weil formula: the first term identity"
    citation: "S. S. Kudla & S. Rallis, Ann. of Math. 140 (1994), 1–80."
    kind: paper
  - title: "The regularized Siegel–Weil formula (the second term identity) and the Rallis inner product formula"
    citation: "W. T. Gan, Y. Qiu & S. Takeda, Invent. Math. 198 (2014), 739–831."
    kind: paper
lean_anchors:
  - 05_determinant/theorem_for_partners.lean#New.Exchange.exchange_identity
  - 05_determinant/theorem_for_partners.lean#New.Exchange.exchange_theorem
  - 05_determinant/theorem_for_partners.lean#New.Exchange.exchange_of_matches
  - 05_determinant/theorem_for_partners.lean#New.Exchange.exists_equalRankThetaRatio_eq_const
child_concepts:
  - complex-plane
  - theta-correspondence
  - seesaw-duality
  - hecke-l-value-ratio
  - theta-square-law
---

# Siegel–Weil Exchange

## Level 1 — First-Principles

Suppose a single Hermitian space $W$ sits in the middle between two partner spaces $V$ and $V'$ whose ranks add up to the rank of $W$—a pair of **complementary rank** partners with $\operatorname{rk}(W) = \operatorname{rk}(V) + \operatorname{rk}(V')$ and $\operatorname{rk}(V) = \operatorname{rk}(V') + 2e$. In {{concept:theta-correspondence|the unitary dual-pair machinery that pairs two Hermitian spaces via a theta integral}}, integrating a {{term:theta-correspondence|theta kernel}} over the larger partner group $U(V)$ produces a function on $U(W)$, and integrating a companion kernel over the smaller partner group $U(V')$ also produces a function on $U(W)$. **Siegel–Weil exchange** proves that these two integrals—one over a bigger group and one over a smaller group—are proportional as functions on $U(W)$ whenever their finite-prime data modulo an ideal $\mathfrak{m}$ are paired as **matched functions**.

To make that matching concrete, split the coordinates of $W$ into two blocks of sizes $|b| = \operatorname{rk}(V')$ and $|a| = \operatorname{rk}(V)$. Take an invertible matrix $k \in \mathrm{GL}_n(\mathcal{O}_E / \mathfrak{m})$: the first $|b|$ columns of $k$ form a tall block $y$, and the last $|a|$ rows of $k^{-1}$ form a wide block $x$. Notice that $x \cdot y = 0$ identically because $k^{-1} k = I$. Two finite-ring matrix functions $\Phi(x)$ and $\Psi(y)$ are **matched functions** for a residue character $\chi$ when $\Phi$ is supported on those last-row blocks of $k^{-1}$, $\Psi$ is supported on those first-column blocks of $k$, and $\Phi(x) = \chi(\det k)\,\Psi(y)$ for every invertible $k$.

The **exchange identity** (`New.Exchange.exchange_identity`) then delivers three pieces at once:
1. **Universal real vector chosen up front:** Given a real Schwartz test vector $v$ for $(V', W)$, there exist a constant $C > 0$ and a real Schwartz vector $x$ for $(W, V)$ chosen *before* any modulus $\mathfrak{m}$ or tame character $\nu$ of weight $\mu$ is picked.
2. **Exact proportionality of theta integrals:** For every tame character $\nu$ of weight $\mu$ ramified at $\mathfrak{m}$ with residue character $\chi$, and every pair of **matched functions** $(\Phi, \Psi)$ modulo $\mathfrak{m}$, there is a nonzero scalar $c \in \mathbb{C}$ (in {{concept:complex-plane|the complex plane}}) such that the theta integral over $[U(V)]$ at $(x, \Phi)$ equals $c$ times the theta integral over $[U(V')]$ at $(v, \Psi)$ pointwise on $U(W)$.
3. **Explicit modulus controlled by $L$-values:** While the phase of $c$ and the base constant $C > 0$ are not computed explicitly, its squared norm obeys
   $$\|c\|^2 = C \cdot L_*(e) \neq 0, \qquad L_*(e) = \Bigl(\prod_{w \in S} q_w^{e^2}\Bigr)\prod_{z=1}^{e} \bigl|L^S(z, \nu)\bigr|^2,$$
   where $S$ is the set of places dividing $\mathfrak{m}$, $q_w$ is the residue cardinality at $w$, and $L^S(z, \nu)$ is the {{concept:hecke-l-value-ratio|partial Hecke L-value omitting the places in $S \cup \bar{S}$}}.

Why does the proof care? Because replacing a theta integral over a large space $V$ by a theta integral over its smaller **complementary rank** partner $V'$—at a known cost controlled purely by $L_*(e)$—is the analytic engine that lets Stage 05 commute lifts past transposed lifts in the {{concept:theta-square-law|commuting theta square}} (`OneSquare.lean`) and lets Stage 06 execute three successive exchanges inside `ThreeExchanges.lean`.

### Limits of the intuition

Calling **Siegel–Weil exchange** a "swap between two complementary integrals" makes it sound symmetric and explicit, but two critical subtleties sit beneath the surface. First, the proof does not compute the constant $C > 0$ or the phase of $c$; it controls $\|c\|^2 / L_*(e) = C$ across all tame characters $\nu$ of fixed weight $\mu$ by comparing against an **equal-rank theta ratio** $|I(v)(1)|^2 / L_*(\operatorname{rk} W_0 / 2)$ on $W_0 = H \oplus W$ and showing that ratio is a nonzero constant independent of $\nu$ (`exists_equalRankThetaRatio_eq_const`). Second, the exchange holds only in the convergent/regularized sign and weight window $2q + 2 \le |n| \le 2\mu + 2q$, where $q$ is the number of negative signs of $W$ (matching the sum of negative signs of $V$ and $V'$).

## Level 2 — Undergrad Bridge

In classical automorphic forms, Weil's 1965 Siegel–Weil formula identifies the theta integral of a single space with a special value of an Eisenstein series, and Kudla–Rallis (1994) and Gan–Qiu–Takeda (2014) extend this to regularized first- and second-term identities comparing complementary spaces in the same Witt tower. `src/New/Determinant/Exchange/TheoremForPartners.lean` proves a ramified, tame-character unitary version in four stages:

1. **Adjoint coordinate flip (`adjointCoords`, `exists_localDatum_adjointDatum_eq_smul`, `exists_mul_thetaSmall_adjointDatum_eq_mul_integral`):**
   Switching $(U, W)$ to $(W, U^-)$ (where $U^- = \mathrm{flipSpace}\, U$ negates the Hermitian signs) acts on finite adelic matrices by conjugate-transposition scaled by an integral unit of norm $-1$. On matched local functions $(\Phi, \Psi)$ with $\Psi$ supported on left-invertible matrices, this moves `thetaSmall` on $(W, U^-)$ into the integral of `pairTheta` on $(U, W)$ up to a modulus-one factor $d$.
2. **Descent from the top of the Witt tower (`exists_rowColExchangeIdentity_of_even_rank`, `thetaLarge_eq_smul_of_areMatched`, `exchange_of_matches`):**
   Starting from an even-rank space $W$ with $\operatorname{rk} W = |b| + |r|$ and $\mathrm{negCount}(W) = \mathrm{negCount}(V) + \mathrm{negCount}(U)$, `exists_rowColExchangeIdentity_of_even_rank` descends from standard determinant data (`stdRowDatum`, `stdColDatum`) on $W_0 = H \oplus W$ by removing hyperbolic planes (`RemovingPlanes.lean`), and `thetaLarge_eq_smul_of_areMatched` upgrades standard data to all locally constant matched pairs $(\Phi, \Psi)$. The resulting constant satisfies
   $$C \|c\|^2 = L_*(\operatorname{rk} V - \operatorname{rk} W / 2) \cdot \mathrm{equalRankThetaRatio}(H \oplus W, \mathfrak{c}, z),$$
   where `lValueProdInt` extends $L_*(e)$ to $\mathbb{Z}$ via $L_*(-e) = L_*(e)^{-1}$.
3. **Constancy and non-vanishing of the equal-rank theta ratio (`exists_equalRankThetaRatio_sq_eq`, `exists_equalRankThetaRatio_const_ne_zero`, `exists_equalRankThetaRatio_add_plane`, `exists_equalRankThetaRatio_eq_const`):**
   Applying `exchange_of_matches` twice to a positive-definite space $J$ of rank $2k$—once for $(J, J, 0)$ (`exists_thetaLarge_eq_mul_quotMeasure`, picking up $L_*(k)$) and once for $(J, 0, J)$ (`exists_quotMeasure_mul_eq_mul_integral_pairTheta`, picking up $L_*(-k) = L_*(k)^{-1}$)—and combining by Fubini's theorem (`eq_mul_of_integral_pairTheta`) cancels $L_*(k)$ completely! Thus the square of `equalRankThetaRatio` is constant across all tame characters $\mathfrak{c}$ of weight $\mu$. Non-vanishing for $k \le \mu$ (`exists_equalRankThetaRatio_ne_zero`) plus hyperbolic-plane induction (`exists_equalRankThetaRatio_add_plane`, `equalRankThetaRatio_eq_of_signs_eq`) yields a Schwartz vector $z$ on $H \oplus W$ whose **equal-rank theta ratio** is a single constant $C_0 \neq 0$ for every tame character of weight $\mu$.
4. **Passage to residue rings and general splitting characters (`exists_areMatched_modDatum_eq`, `exchange_theorem`, `exchange_identity`):**
   Lifting matched functions modulo $\mathfrak{m}$ (`AreMatched ⊤ σ χ Φ Ψ`) to matched locally constant functions on completions (`exists_areMatched_modDatum_eq`), plugging in the constant-$C_0$ vector $z$ (`exchange_theorem`), and finally allowing arbitrary splitting characters $\alpha, \alpha'$ satisfying $\alpha.\mathrm{spaceChar}\,|a| \cdot \alpha'.\mathrm{spaceChar}\,|b| = \nu.\mathrm{baseChange}^{-1}$ (`spaceChar_mul_spaceChar_eq_iff`) produces the flagship theorem **`New.Exchange.exchange_identity`**.

## Level 3 — Formal Lean Contract

Stage 05 extracts the complementary-rank exchange machinery into `skeleton/05_determinant/theorem_for_partners.lean` (27 declarations; note that 3 intermediate lemmas — `exchange_of_matches`, `exists_quotMeasure_mul_eq_mul_integral_pairTheta`, and `exchange_theorem` — are truncated at their `let γ :=` bindings in the skeleton, whereas `exchange_identity` and `exists_equalRankThetaRatio_eq_const` extract untruncated; full upstream statements are documented in [Siegel–Weil Exchange and the Commuting Theta Square](../stages/05_determinant/exchange-and-one-square.md)):

- `05_determinant/theorem_for_partners.lean#New.Exchange.exchange_identity` (upstream `src/New/Determinant/Exchange/TheoremForPartners.lean:1051-1113`):
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
- `05_determinant/theorem_for_partners.lean#New.Exchange.exists_equalRankThetaRatio_eq_const` (upstream `src/New/Determinant/Exchange/TheoremForPartners.lean:815-852`):
  ```lean
  lemma New.Exchange.exists_equalRankThetaRatio_eq_const (W : Space E n s) (μ : InfinitePlace E → ℤ) (k : ℕ)
      (h_rank : card n = 2 * k + 2 * negCount s) (hμ : ∀ w : InfinitePlace E, k ≤ μ w) :
      ∃ v : PairFock E (Sum.elim (hyperbolicSigns Unit) s) (Sum.elim (hyperbolicSigns Unit) s),
        IsSchwartzVector v ∧ ∃ C ≠ (0 : ℝ),
          ∀ 𝔠 : SplitTameChar E μ, equalRankThetaRatio (hyperbolicSum Unit W) 𝔠 v = C
  ```
- Also anchored: `05_determinant/theorem_for_partners.lean#New.Exchange.exchange_of_matches` (upstream `TheoremForPartners.lean:325-342`) and `05_determinant/theorem_for_partners.lean#New.Exchange.exchange_theorem` (upstream `TheoremForPartners.lean:985-1003`).

See the page's live `<VerificationBanner />` (`audit/verification-status.json`) for the pinned upstream commit and local Lean build status (`build_status: unverified-locally`).
