---
slug: character-theta-lift
term: character theta lift
aliases: ["four-block seesaw", "integral algebraic part", "multiplicative pigeonhole", "rank-one test form"]
inline_glimpse: "Lifting a main-pair character nu * theta from U(1) to U(2z), where a four-block seesaw (12 + 4 + 4 + (2z - 20)) builds a rank-one test form whose p-adic pairing bound combines with the untwisted lift norm to yield negative height 6 * denominatorBudget - directionAmount on (p.ratio)^-1."
visual_ref: assets/stage06-false-lifts-and-rank-one-blocks.svg
where_analogy_breaks: "Thinking of the rank-one sharpness theorem from Stage 02 as directly producing a negative height reverses the sign ledger: the negative term -sum jetSize comes from the archimedean height of the untwisted lift norm ||phi_1||^2 (AlgebraicPartSize), whereas Stage 02 BoundAttained enters with a PLUS sign (+sum primeSum) to cap the upward p-adic valuation penalty below that archimedean credit so the net difference -(sum jetSize - sum primeSum) = -directionAmount stays negative."
sources:
  - title: "Theta liftings and Hecke L-functions"
    citation: "T. Yang, J. Reine Angew. Math. 485 (1997), 25–53."
    kind: paper
  - title: "Integrality of a ratio of Petersson norms and level-lowering congruences"
    citation: "K. Prasanna, Ann. of Math. 163 (2006), 901–941."
    kind: paper
  - title: "The Tate–Shafarevich group for elliptic curves with complex multiplication II"
    citation: "J. Coates, Z. Liang & R. Sujatha, J. Algebra 322 (2009), 2639–2668."
    kind: paper
lean_anchors:
  - 06_one_lift/twisted_value_heights.lean#New.Data.inner_pointForm_kernelForm
  - 06_one_lift/twisted_value_heights.lean#New.Data.isTestForm_trivialLiftClassSum_pointForm
  - 06_one_lift/twisted_value_heights.lean#New.Data.exists_isTestForm_inner_kernelForm_eq
  - 06_one_lift/twisted_value_heights.lean#New.Data.exists_isTestForm_le_norm_inner_trivialLift
  - 06_one_lift/twisted_value_heights.lean#New.Data.heightLe_inv_ratio
child_concepts:
  - rallis-inner-product
  - hecke-l-value-ratio
  - arithmetic-height
  - false-lift-period
  - regular-twist-family
---

# Character Theta Lift

## Level 1 — First-Principles

To make Theorem C beat the naive $2/3$ barrier, Stage 06 needs a *negative* upper bound on the {{concept:arithmetic-height|global arithmetic height}} of the inverse {{concept:hecke-l-value-ratio|Hecke L-value ratio}} $(p.\mathrm{ratio}(\theta(\iota)^j))^{-1}$ for every {{term:hecke-l-value-ratio|main pair}} $p = (\nu, z)$. How can a height bound be negative? By turning $p.\mathrm{ratio}(\vartheta)$ into a ratio of squared norms of **character theta lifts**: lifting the character $\nu \vartheta$ from the rank-one unitary group $U(1)$ to the definite unitary group $U(2z)$ via the {{concept:rallis-inner-product|Rallis inner product formula}}.

Up to an integer power of the conductor norm $\mathrm{Nm}(Q)$, the inverse ratio $(p.\mathrm{ratio}(\vartheta))^{-1}$ equals $\|\varphi_1\|^2 / \|\varphi_\vartheta\|^2$, where $\varphi_1$ is the normalized untwisted lift and $\varphi_\vartheta$ is the normalized twisted lift. For any two **rank-one test forms** $\Xi, \Xi'$ on $U(2z)$—meaning forms that are $\ell$-adically integral and satisfy {{concept:false-lift-period|false-lift orthogonality}}—projecting them onto the complex line $\mathbb{C}\cdot \varphi_\vartheta$ yields a 4-factor identity:
$$\frac{\|\varphi_1\|^2}{\|\varphi_\vartheta\|^2} = \underbrace{\|\varphi_1\|^2}_{\text{(1)}} \cdot \underbrace{\langle \mathrm{pr}\,\Xi, \mathrm{pr}\,\Xi'\rangle}_{\text{(2)}} \cdot \underbrace{\rho^{-2}}_{\text{(3)}} \cdot \underbrace{\frac{\rho^2}{\langle \Xi, \varphi_\vartheta\rangle\langle \varphi_\vartheta, \Xi'\rangle}}_{\text{(4)}},$$
where $\rho = q_2.\mathrm{ratio}(\vartheta)$ is an auxiliary flat-pair ratio on rank $4$.

To build a **rank-one test form** $\Xi$ whose pairing $\langle \Xi, \varphi_\vartheta\rangle$ is *not* overly divisible at $\ell$, Stage 06 uses a **four-block seesaw**, partitioning the $2z$ coordinates of `RankOneIndex p.z` into four pieces:
$$2z = 12 \;+\; 4 \;+\; 4 \;+\; (2z - 20).$$
- **Block 1 (`Fin 12`):** carries a monomial derivative of a {{concept:theta-correspondence|rank-one dual-pair theta kernel}}, where Stage 02's sharpness theorem (`02_derivatives/bound_attained`) guarantees that some Taylor coefficient avoids excess $\ell$-adic divisibility beyond `localPrimeSumTotal l`.
- **Block 2 (`Fin 4`):** carries the twist $\vartheta$ via an auxiliary flat pair $q_2$, contributing its **integral algebraic part** (and hence $\rho = q_2.\mathrm{ratio}(\vartheta)$).
- **Blocks 3 & 4 (`Fin 4` and `Fin (2z - 20)`):** carry untwisted flat pairs $q_3$ and $q$; averaging over the huge fourth block via `trivialLiftClassSum` enforces {{term:false-lift-period|false-lift orthogonality}} (`isTestForm_trivialLiftClassSum_pointForm`).

Choosing the untwisted characters by **multiplicative pigeonhole** (`exists_mul_eq_norm_map_le_norm_map_mul`) and character orthogonality on $U(1)$ bounds all four factors inside {{concept:regular-twist-family|a regular twist family}} $\theta$:
1. $\|\varphi_1\|^2$ contributes $\mathrm{denominatorBudget} - \sum_\tau \mathrm{jetSize}(\mathrm{weight}\,\tau)$ (this is where the minus sign enters!).
2. $\langle \mathrm{pr}\,\Xi, \mathrm{pr}\,\Xi'\rangle$ contributes at most $\mathrm{denominatorBudget}$ from `IsRegularFamily`'s main-pair projection control.
3. $\rho^{-2}$ contributes at most $2 x_1 \le 2\,\mathrm{denominatorBudget}$ from `HasSmallRatios`.
4. $\rho^2 / (\langle \Xi, \varphi_\vartheta\rangle\langle \varphi_\vartheta, \Xi'\rangle)$ contributes at most $2\,\mathrm{denominatorBudget} + \sum_\tau \mathrm{primeSum}(\mathrm{weight}\,\tau, \tau)$ from Stage 02's sharpness bound.

Adding the four heights yields `heightLe_inv_ratio`:
$$\mathrm{HeightLe}\bigl(6\,\mathrm{denominatorBudget} - D.\mathrm{directionAmount}\bigr) \quad\text{on}\quad \bigl(p.\mathrm{ratio}(\theta(\iota)^j)\bigr)^{-1},$$
because $D.\mathrm{directionAmount} = \sum_\tau (\mathrm{jetSize}(\mathrm{weight}\,\tau) - \mathrm{primeSum}(\mathrm{weight}\,\tau, \tau))$.

### Limits of the intuition

Treating Stage 02's sharpness theorem (`BoundAttained`) as the direct source of the negative height is a classic sign-ledger trap. `BoundAttained` bounds a *denominator* $\langle \Xi, \varphi_\vartheta\rangle$ from below at $\ell$, so its contribution $+\sum_\tau \mathrm{primeSum}$ is **positive** (an upward $p$-adic valuation penalty). The negative term $-\sum_\tau \mathrm{jetSize}$ comes strictly from Factor (1), `heightLe_norm_trivialLift_sq` (`AlgebraicPartSize.lean`), which compares the untwisted archimedean norm of $\varphi_1$ against a flat pair of the same rank; `BoundAttained` ensures the upward $p$-adic penalty stays strictly smaller than that downward archimedean credit.

## Level 2 — Undergrad Bridge

In Yang (1997), Prasanna (2006), and Coates–Liang–Sujatha (2009), special values of CM Hecke $L$-functions appear as Petersson norms of rank-one theta lifts, and $p$-adic divisibility is controlled by pairing against integral test forms. `src/New/OneLift/HeckeValues/TwistedValueHeights.lean` (24 declarations, 2 `let`-type stub truncations) builds the rank-one height bound in four steps:

1. **Four-block seesaw factorization (`inner_pointForm_kernelForm`, `isTestForm_trivialLiftClassSum_pointForm`, `exists_isTestForm_inner_kernelForm_eq`):**
   At a block-diagonal point $\mathrm{diag}(1, h_2, h_3, h_1)$ of $U(2z)(\mathbb{A}_f)$ with datum `testColumn` and twist row `twistRow p.z` in Block 2, the pairing of a point form against `X.kernelForm g` splits into a `Fin 12` `modKernel` times three flat-pair kernels (`inner_pointForm_kernelForm`). Summing over Block 4 against `trivialLift hq .one` yields an integral test form orthogonal to false lifts (`isTestForm_trivialLiftClassSum_pointForm`, invoking `stdClassSum_period_eq_zero` with $2\cdot 1 + 2\cdot 20 + 3 = 45 < 2z$). Summing over Blocks 2 and 3 yields `exists_isTestForm_inner_kernelForm_eq`: a test form $\Xi$ whose pairing against `X.kernelForm g` equals a multinomial factorial times the three **integral algebraic parts** ($q, q_3$ untwisted, $q_2$ twisted by $\vartheta$) times the `Fin 12` `modKernel`.
2. **Pigeonhole + $U(1)$ orthogonality + Stage 02 sharpness (`exists_mul_eq_norm_map_le_norm_map_mul`, `exists_norm_le_norm_twisted_classSum`, `exists_root_prod_integralAlgebraicPart`, `exists_isTestForm_le_norm_inner_trivialLift`):**
   Taking the geometric mean $L$ over `tameChars` of $q_3(\chi)\cdot q_1(\chi)$ (conjugate size $\le \exp(x^{34})^2$), applying **multiplicative pigeonhole** to find a character factorization, using $U(1)$ character orthogonality to isolate the class sum at $1$, and invoking `02_derivatives/bound_attained` on the `Fin 12` theta kernel produces a twist-independent $c_0 \neq 0$ ($\|\sigma c_0\| \le \exp(f m b / k + 3 x^{34}) \le \exp(\mathrm{denominatorBudget})$) and a test form $\Xi$ with
   $$\|\iota_p(c_0 \cdot q_2.\mathrm{ratio}\,\vartheta)\|\,\ell^{-D.\mathrm{localPrimeSumTotal}\,\ell} \le \|\iota_p(\langle \Xi, \varphi_\vartheta\rangle)\|.$$
3. **Conjugate pairing and 4-factor height sum (`heightLe_inv_ratio`):**
   Applying `exists_isTestForm_le_norm_inner_trivialLift` at both $\iota$ and $\iota \circ \mathrm{conj}$ yields $\Xi, \Xi'$ for Factor (4), contributing $2\,\mathrm{denominatorBudget} + \sum_\tau \mathrm{primeSum}(\mathrm{weight}\,\tau, \tau)$ via `D.sum_localPrimeSumTotal_mul_log_le`. Combining with `heightLe_norm_trivialLift_sq` ($\mathrm{denominatorBudget} - \sum_\tau \mathrm{jetSize}$), `IsRegularFamily` main-pair projection ($\mathrm{denominatorBudget}$), and `HasSmallRatios` squared ($2 x_1 \le 2\,\mathrm{denominatorBudget}$) proves `HeightLe (6 * D.denominatorBudget - D.directionAmount)` on `(p.ratio ((θ ι).powTwist j))⁻¹`.

## Level 3 — Formal Lean Contract

All five anchors live in `skeleton/06_one_lift/twisted_value_heights.lean` (24 declarations; 2 declarations—`inner_pointForm_kernelForm` and `exists_isTestForm_inner_kernelForm_eq`—are truncated at `let X : LValueData ... :=` in the skeleton, with full verbatim upstream signatures quoted in [Vanishing Periods of False Lifts and Negative Heights of Twisted L-Values](../stages/06_one_lift/false-lifts-and-twisted-l-values.md)):

- `06_one_lift/twisted_value_heights.lean#New.Data.inner_pointForm_kernelForm` (upstream `TwistedValueHeights.lean:353-380`) and `06_one_lift/twisted_value_heights.lean#New.Data.isTestForm_trivialLiftClassSum_pointForm` (upstream `TwistedValueHeights.lean:452-466`).
- `06_one_lift/twisted_value_heights.lean#New.Data.exists_isTestForm_inner_kernelForm_eq` (upstream `TwistedValueHeights.lean:575-601`).
- `06_one_lift/twisted_value_heights.lean#New.Data.exists_isTestForm_le_norm_inner_trivialLift` (upstream `TwistedValueHeights.lean:695-856`):
  ```lean
  theorem New.Data.exists_isTestForm_le_norm_inner_trivialLift {j : D.LiftIndex} {p q₂ : D.CriticalPair}
      (hp : p.IsMainPair j) (hq₂ : q₂.HasEmptyRow (Fin 4)) :
      ∃ c₀ : ℂ, c₀ ≠ 0 ∧
        (∀ σ : ℂ ≃+* ℂ, ‖σ c₀‖ ≤ exp (D.f * D.m / D.k * D.b + 3 * D.x ^ 34)) ∧
        ∀ (l : ℕ) [Fact l.Prime] [PadicPlace l] (ϑ : D.Twist), l.Coprime ϑ.normQ →
          ∃ Ξ : (lValueData hp.hasLiftRank ϑ).descentPair.FormV,
            (lValueData hp.hasLiftRank ϑ).descentPair.IsTestForm l Ξ ∧
            ‖ιₚ (c₀ * q₂.ratio ϑ)‖ * (l : ℝ) ^ (-D.localPrimeSumTotal l) ≤
              ‖ιₚ (inner ℂ Ξ (CriticalPair.trivialLift hp.hasLiftRank ϑ))‖
  ```
- `06_one_lift/twisted_value_heights.lean#New.Data.heightLe_inv_ratio` (upstream `TwistedValueHeights.lean:858-971`):
  ```lean
  theorem New.Data.heightLe_inv_ratio {θ : PrimePadicIso → D.MFoldTwist} (hθ : D.IsRegularFamily θ)
      (j : D.LiftIndex) (p : D.CriticalPair) (hp : p.IsMainPair j) :
      HeightLe (6 * D.denominatorBudget - D.directionAmount) fun ι : PrimePadicIso ↦
        (p.ratio ((θ ι).powTwist j))⁻¹
  ```

See the page's live `<VerificationBanner />` (`audit/verification-status.json`) for the pinned upstream commit and local Lean build status (`build_status: unverified-locally`).
