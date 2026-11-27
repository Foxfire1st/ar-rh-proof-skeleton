# Regular Twist Families and the Assembly of Theorem C

This page explains the two files in `src/New/Determinant/Twist/`: `src/New/Determinant/Twist/FitTwists.lean` (`05_determinant/fit_twists`, 17 declarations, 0 stub truncations) — the 7-consumer contract defining regular families of $m$-fold twists — and `src/New/Determinant/Twist/LongForms.lean` (`05_determinant/long_forms`, 9 declarations, 0 stub truncations) — the culminating assembly of **Theorem C** (`New.Data.integral_forms_long`) feeding directly into [Stage 01's Main Cancellation Inequality](../01_reduction/main-inequality.md). Return to the [Stage 05 Overview](./overview.md), or revisit [Siegel–Weil Exchange and the Commuting Theta Square](./exchange-and-one-square.md) and [Chain Determinant Factorization and Two-Grid Auxiliary Forms](./chain-determinant-and-auxiliary-forms.md).

## Level 1 — First-Principles

Stage 05 ends by turning all the automorphic machinery of Theorem C into one crisp arithmetic inequality. Two files carry that final step: `FitTwists.lean` writes down the exact contract that a family of twists must satisfy so Stages 06 and 07 can speak to each other cleanly, and `LongForms.lean` gathers four inputs into Stage 01's {{concept:twisted-lattice|twisted-lattice height inequality}} to prove **Theorem C**.

![Four-input convergence blueprint showing how SpanningForms, NormOfChain, Stage 06 LiftVolumeHeight, and Stage 07 FitTwistsExist assemble inside TwistedLattice.covolume_add_index_nonneg to prove Theorem C (New.Data.integral_forms_long)](assets/stage05-theorem-c-blueprint.svg)

### Part 1: The 7-consumer contract of regular twist families (`FitTwists.lean`)

First, a helpful naming note from upstream `GUIDE.md` §2: the file is named `FitTwists.lean` (*"fit for regular"*), whereas the Lean structure it defines is called `New.Data.IsRegularFamily`.

Why does Theorem C need a *family* of twists rather than one twist? Inside Stage 01's {{term:twisted-lattice}}, each {{concept:prime-number|prime}} $\ell$ and field isomorphism $\iota : \mathbb{C} \simeq \overline{\mathbb{Q}}_\ell$ can use its own {{concept:regular-twist-family|m-fold twist}} $\theta(\iota)$ whose modulus $Q(\theta(\iota))$ is coprime to $\ell$. To make every piece of Stages 05, 06, and 07 click together, `FitTwists.lean` specifies what makes $\theta$ a {{term:regular-twist-family}}:

1. **Small $L$-value ratios (`HasSmallRatios`):** For every $\iota \mid \ell$, $\mathrm{Nm}(Q(\theta(\iota)))$ is coprime to $\ell$, and for every lift $1 \le j \le m$ and every {{concept:hecke-l-value-ratio|ramified critical pair}} $p = (\nu, z)$, the inverse {{term:hecke-l-value-ratio}} $(p.\mathrm{ratio}(\theta(\iota)^j, \pm 1))^{-1}$ has {{concept:arithmetic-height|global arithmetic height}} at most $x_1$. (Notice the subtle Euler-factor convention: in $p.\mathrm{ratio}(\vartheta, s) = |L^S(z, (\nu \vartheta^s)^{-1})| / |L^S(z, \nu^{-1})|$, the denominator omits the chosen places and their conjugates, whereas the numerator omits *both* the chosen places and the twist places $Q$ and their conjugates.)
2. **Denominators within budget (`IsDenominatorBound`):** For every lift $1 \le j \le m$, there are real numbers $\gamma(\ell)$ summing over any finite set of primes to at most the {{term:regular-twist-family|denominator budget}} $f a \log^3 b / (m k)$, such that at every $\iota \mid \ell$ the $j$-th power $\theta(\iota)^j$ satisfies both clauses of `HasDenominatorsLe`:
   - {{term:regular-twist-family|Projection clearing}} (`ClearsProjection`): an operator $\Lambda$ on the space of good lifts `D.goodLift` from $W_j$ with $\log|\iota(\det \Lambda)| \ge -(\dim Z)\,r\,\gamma(\ell)$ clears $\ell$-adic denominators when projecting integral forms orthogonal to false lifts onto the lift subspace.
   - {{term:hecke-l-value-ratio|Main pair}} projection control: for every main pair $p$ for $j$ (each automatically ramified, `IsMainPair.isRamified`), the inner product of projections of two test forms onto the line of the rank-one theta lift of $\nu \theta(\iota)^j$ has $\ell$-adic norm at most $e^{\gamma(\ell)}$.

Seven landmark files sit around this single interface: Stage 05's `norm_of_chain` and `spanning_forms` consume `HasSmallRatios`; Stage 06 (`06_one_lift/three_exchanges`, `06_one_lift/twisted_value_heights`, feeding `06_one_lift/lift_volume_height`) consumes `IsRegularFamily` to bound each single-lift volume height; and Stage 07 (`07_congruences/rows_from_eigensystems`, `07_congruences/denominator_bound`, `07_congruences/twisting_theorem`, feeding `07_congruences/fit_twists_exist`) constructs a {{term:regular-twist-family}} via Galois representations, Ribet's method, and Selmer-group twisting.

### Part 2: Four inputs converge into Theorem C (`LongForms.lean`)

`LongForms.lean` contains only **9 declarations**, yet it closes the entire lower-bound side of the proof in five clean steps:

1. **Wrap basis-completion forms (`IsAuxiliaryForm`, `exists_span_isAuxiliaryForm_eq_top`):** Applying the {{concept:central-idempotent|good projection}} `D.goodProjTranslate 1` to Stage 05's `exists_span_short_lifts_eq_top` spans `D.targetForms` by {{concept:auxiliary-spanning-form|auxiliary spanning forms}} whose untwisted lifts have squared norm at most $\exp(2 x_1^2 b^2)$ and whose twisted lifts stay integral after scaling by $\gamma(\vartheta)$ with $\|\gamma(\vartheta)\| = \mathrm{Nm}(Q)^{e/2} \prod_{i,j < m} \mathrm{turnRatio}_{i,j}(\vartheta)$.
2. **Bound the turn-ratio scalar and completion cost (`heightLe_inv_rpow_mul_prod_turnRatio`, `mul_completion_cost_le`):** Under {{term:regular-twist-family|small ratios}}, the inverse $m^2$-turn product has height $\le m^2 t x_1$. Squaring it for $|\gamma(\theta_\iota)|^{-2}$ gives height $\le 2 m^2 t x_1 \le b$ (`LongForms.lean:231–233`). Each missing basis vector therefore costs at most $2 x_1^2 b^2 + b$. When Theorem B gives $n \ge (1 - 4/b^2)d$, there are at most $n' = d - n \le 4d/b^2$ missing vectors, so $b^2$ cancels and the total {{term:auxiliary-spanning-form|completion cost}} is at most $d b$.
3. **Pull in Stage 07 and Stage 06 (`exists_hasSmallRatios_heightLe_liftDeterminant_div`):** Invoke Stage 07's `exists_isRegularFamily` (typed black box, unpacked in Stage 07) to obtain a regular family $\theta$, and feed it into Stage 06's `heightLe_liftDeterminant_ratio` (typed black box, unpacked in Stage 06) for each $1 \le j \le m$. Because $k \cdot \log b \cdot \mathrm{denominatorBudget} = f a \log^4 b / m$, each single {{concept:rallis-inner-product|lift determinant}} ratio $Z_j(1)/Z_j(\theta(\iota)^j)$ has height at most
   $$d \cdot \Bigl(\frac{f a \log^4 b}{m} - k \cdot D.\mathrm{directionAmount}\Bigr).$$
4. **Sum the $m$ single-lift heights (`exists_chainDeterminant_one_div_eq`, `heightLe_chainDeterminant_one_div`):** Inverting `NormOfChain`'s factorization expresses $Z(1)/Z(\theta(\iota))$ as the inverse quotient-ratio scalar (height $m^2 t x_1 \le b$, raised to power $d$) times the product of the $m$ single-lift ratios. Adding heights across $j = 1, \dots, m$ uses $m \cdot k \cdot D.\mathrm{directionAmount} = D.\mathrm{mainAmount}$ to give
   $$\mathrm{HeightLe}\Bigl(d \cdot \bigl(b + f a \log^4 b - D.\mathrm{mainAmount}\bigr)\Bigr) \quad\text{on}\quad \frac{Z(1)}{Z(\theta(\iota))}.$$
5. **Apply `TwistedLattice` $\Longrightarrow$ Theorem C (`integral_forms_long_with_completion_cost` $\Rightarrow$ `integral_forms_long`):** Completing any linearly independent family $y_1, \dots, y_n$ (of {{concept:gram-determinant|squared Gram-determinant volume bound}} $\le c$ and index $\le \delta$) to a full basis of `D.targetForms` with $n' = d - n$ auxiliary forms and invoking Stage 01's `TwistedLattice.covolume_add_index_nonneg` yields
   $$d \cdot \bigl(D.\mathrm{mainAmount} - f a \log^4 b - b\bigr) \le c + 2\delta + (d - n)\bigl(2 x_1^2 b^2 + b\bigr).$$
   Using `mul_completion_cost_le` to bound $(d - n)(2 x_1^2 b^2 + b) \le d b$ and absorbing the two $d b$ terms into $2 d f a \log^4 b$ gives **Theorem C (`New.Data.integral_forms_long`)**:
   $$d \cdot \bigl(D.\mathrm{mainAmount} - 3 f a \log^4 b\bigr) \le c + 2\delta.$$
   Inside [Stage 01 `main-inequality`](../01_reduction/main-inequality.md) (`New.star_inequality`), this exact lower bound meets Theorem A's upper {{term:gram-determinant|covolume}} bound and Theorem B's spanning count: $d \cdot D.\mathrm{mainAmount}$ cancels identically from both sides, forcing $m \cdot D.\mathrm{totalRemainder} \le f a \log^5 b$ and completing the road to GRH.

### Where this analogy breaks

Reading `LongForms.lean` as a 9-lemma algebraic wrap-up makes Theorem C look almost effortless—just adding $m$ numbers and invoking `TwistedLattice.covolume_add_index_nonneg`. That brevity is a deliberate architectural payoff, not an absence of depth: the entire minus sign $-D.\mathrm{mainAmount}$ that drives the contradiction in `star_inequality` comes from the negative height of $Z_j(1)/Z_j(\theta(\iota)^j)$ proved in Stage 06 (`06_one_lift`, breaking the naive $2/3$ barrier via three exchanges, gluing maps, and rank-one $L$-value heights), while the existence of $\theta$ with denominators inside `denominatorBudget` is the full 53-file Galois-representation and Selmer-group engine of Stage 07 (`07_congruences`).

## Level 2 — Undergrad Bridge

### 1. The regular-twist interface (`FitTwists.lean`, 17 decls, 0 truncations)

| Declaration | Role in the 7-consumer interface |
| --- | --- |
| `Twist.toAutChar_one`, `Twist.Q_one`, `MFoldTwist.toTwist_one`, `toAutChar_one_and_toTwist_one_and_Q_one` | Normalization lemmas certifying that the trivial twist `1` has character `1`, empty modulus `Q = ∅`, and underlies the trivial `m`-fold twist. |
| `CriticalPair.ratio p ϑ s` | Complex coercion of $\|L^{\mathrm{chosen} \cup Q}(z, (\nu \vartheta^s)^{-1})\| / \|L^{\mathrm{chosen}}(z, \nu^{-1})\|$ (default $s = 1$). Note the numerator omits both `chosen` and `ϑ.Q` (and conjugates), while the denominator omits `chosen` (and conjugates). |
| `RankOneIndex z` | Index type of cardinality $20 + (2z - 20) = 2z$ (for $z \ge 10$), split into blocks of ranks $12, 4, 4, 2z - 20$, used for rank-one theta lifts of main-pair characters in Stage 06. |
| `goodLift ϑ j` | Restriction of `D.singleLift ϑ j` to `D.goodForms (D.spaceW j) →ₗ[ℂ] (D.descentPair ϑ j).FormV` (takes `D.TameChar` directly). |
| `CriticalPair.IsRamified`, `CriticalPair.IsMainPair`, `IsMainPair.isRamified` | `IsRamified`: tame part nontrivial iff at a chosen place. `IsMainPair j`: `HasLiftRank (RankOneIndex p.z)`, $m < z \le r$, $1 \le j \le m$, tame part `D.chiMLocal ^ j`, weight $w_j + r - t \le c \le w_j$. Because `D.chiMLocal` has order $> 8m$ at chosen places, every main pair is ramified (`IsMainPair.isRamified`). |
| `denominatorBudget` | $D.\mathrm{errorUnit} / (D.m \cdot D.k) = f a \log^3 b / (m k)$, translating in `LongForms.lean` via $k \log b \cdot \mathrm{denominatorBudget} = f a \log^4 b / m$. |
| `ClearsProjection`, `HasDenominatorsLe`, `IsDenominatorBound`, `IsNegligible` | Encodes $\ell$-adic projection clearing ($\log|\iota(\det \Lambda)| \ge -(\dim Z) r \beta$) and main-pair rank-one test-form projection bounds ($\le e^\beta$), summing over all primes $\ell$ to $\le \mathrm{denominatorBudget}$, plus negligible prime sequences ($\sum e(\ell)\log \ell \le x^C$). |
| `HasSmallRatios`, `IsRegularFamily` | `HasSmallRatios θ`: `Nat.Coprime ι.1 (θ ι).normQ` and `HeightLe D.x₁` on $(p.\mathrm{ratio}((\theta\ \iota)^j, \pm 1))^{-1}$ for every ramified pair. `IsRegularFamily θ` extends `HasSmallRatios θ` with `∃ γ, D.IsDenominatorBound θ j γ` for every $1 \le j \le m$. |

### 2. Step-by-step walk through all 9 declarations of `LongForms.lean`

1. **`New.Data.IsAuxiliaryForm` (L47–54):** A target form $y \in D.\mathrm{targetForms}$ is auxiliary for $\gamma : D.\mathrm{MFoldTwist} \to \mathbb{C}$ if $\gamma(\vartheta)\cdot D.\Theta(\vartheta)(y) \in \mathrm{integralForms}\ \iota.\mathrm{place}$ whenever $\gcd(\iota.1, \mathrm{Nm}(Q(\vartheta))) = 1$, and $(D.\Theta(1)(y)).\mathrm{normSqAt}\ \sigma \le \exp(2 x_1^2 b^2)$ for all $\sigma \in \operatorname{Aut}(\mathbb{C})$.
2. **`New.Data.exists_span_isAuxiliaryForm_eq_top` (L63–105):** Applies `D.goodProjTranslate 1` (surjective onto `D.targetForms`) to `exists_span_short_lifts_eq_top`, setting $\gamma(\vartheta) = c(\vartheta)/c(1)$ (so $\gamma(1) = 1$, $\gamma(\vartheta) \neq 0$, $\|\gamma(\vartheta)\| = \mathrm{Nm}(Q)^{e/2}\prod_{i,j < m}\mathrm{turnRatio}_{i,j}(\vartheta)$) and using $x^{99} + x^{202} b^2 \le 2 x_1^2 b^2$ (`D.le_of_hasLogBound`).
3. **`New.Data.HasSmallRatios.heightLe_inv_rpow_mul_prod_turnRatio` (L112–119):** Proves `HeightLe (D.m * D.m * (D.t * D.x₁))` on $(\mathrm{Nm}(Q)^{e/2}\prod_{i,j < m}\mathrm{turnRatio}_{i,j}(\theta\ \iota))^{-1}$. Inside `integral_forms_long_with_completion_cost` (`LongForms.lean:231–235`), squaring this bound gives `HeightLe D.b` on $((‖γ (θ ι)‖)^2)^{-1}$ via `2 * (D.m * D.m * (D.t * D.x₁)) ≤ D.b` (`D.le_of_hasLogBound`).
4. **`New.Data.mul_completion_cost_le` (L124–134):** From $(1 - 4/b^2)d \le n$ and $d = n + n'$, derives $n' \le (4/b^2)d$ and multiplies by $2 x_1^2 b^2 + b$:
   $$n'\bigl(2 x_1^2 b^2 + b\bigr) \le d \cdot 4\bigl(2 x_1^2 + b^{-1}\bigr) \le d \cdot b,$$
   using `4 * (2 * D.x₁ ^ 2 + D.b⁻¹) ≤ D.b` (`D.le_of_hasLogBound`).
5. **`New.Data.exists_hasSmallRatios_heightLe_liftDeterminant_div` (L142–154):** Calls `exists_isRegularFamily D` (Stage 07) and `heightLe_liftDeterminant_ratio hθ j` (Stage 06), rewriting $k \log b \cdot \mathrm{denominatorBudget} = f a \log^4 b / m$ to produce `HeightLe (d * (f * a * logB^4 / m - k * directionAmount))` on `liftDeterminant 1 j / liftDeterminant (θ ι) j`.
6. **`New.Data.exists_chainDeterminant_one_div_eq` (L165–175):** Inverts `NormOfChain`'s `exists_chainDeterminant_pos_and_div_chainDeterminant_one_eq` to express `chainDeterminant 1 / chainDeterminant ϑ` as the $d$-th power of the inverse quotient-ratio scalar times $\prod_{j=1}^m \mathrm{liftDeterminant}\ 1\ j / \mathrm{liftDeterminant}\ \vartheta\ j$.
7. **`New.Data.heightLe_chainDeterminant_one_div` (L183–207):** Upgrades `heightLe_inv_rpow_mul_prod_quotientRatio` to `HeightLe D.b` via `D.m * D.m * (D.t * D.x₁) ≤ D.b`, raises to power $d$, and multiplies by the product of the $m$ single-lift heights (using $m \cdot k \cdot D.\mathrm{directionAmount} = D.\mathrm{mainAmount}$) to prove `HeightLe (d * (b + f * a * logB^4 - mainAmount))` on $Z(1)/Z(\theta(\iota))$.
8. **`New.Data.integral_forms_long_with_completion_cost` (L214–246):** Extends $y : \mathrm{Fin}\,n \to D.\mathrm{targetForms}$ to a basis with $n' = d - n$ auxiliary forms (`exists_extend_basis_of_span_eq_top`), invokes Stage 01's `TwistedLattice.covolume_add_index_nonneg` with `injective_lift_targetLattice`, the squared turn-ratio scalar bound (`LongForms.lean:231–235`), and `heightLe_chainDeterminant_one_div`, yielding
   $$d\bigl(D.\mathrm{mainAmount} - f a \log^4 b - b\bigr) \le c + 2\delta + (d - n)\bigl(2 x_1^2 b^2 + b\bigr).$$
9. **`New.Data.integral_forms_long` (L254–266 — Theorem C):** Applies `mul_completion_cost_le` to replace $(d - n)(2 x_1^2 b^2 + b)$ by $d b$, then uses `D.targetDim * D.b ≤ D.targetDim * (D.f * D.a * D.logB ^ 4)` to fold both $d b$ terms into $2 d f a \log^4 b$, giving $d \cdot (D.\mathrm{mainAmount} - 3 f a \log^4 b) \le c + 2\delta$.

## Level 3 — Formal Lean Contract

The extracted skeletons `skeleton/05_determinant/fit_twists.lean` (17 declarations, 0 stub truncations) and `skeleton/05_determinant/long_forms.lean` (9 declarations, 0 stub truncations) mirror `src/New/Determinant/Twist/FitTwists.lean` and `src/New/Determinant/Twist/LongForms.lean` at pinned upstream commit `41c2716658c42ba08eb0648994a2efe83082a95c`. All 26 declarations across both files extract completely untruncated.

### Headline Signatures

```lean
-- skeleton/05_determinant/fit_twists.lean (upstream FitTwists.lean:216-233)
structure New.Data.HasSmallRatios (θ : PrimePadicIso → D.MFoldTwist) : Prop where
  coprime (ι : PrimePadicIso) : Nat.Coprime ι.1 (θ ι).normQ
  height_le (j : D.LiftIndex) (p : D.CriticalPair) (s : ℤˣ) (hp : p.IsRamified) :
    HeightLe (D.x₁) fun ι : PrimePadicIso ↦ (p.ratio ((θ ι).powTwist j) s)⁻¹

structure New.Data.IsRegularFamily (θ : PrimePadicIso → D.MFoldTwist) : Prop extends D.HasSmallRatios θ where
  exists_denominator_bound (j : D.LiftIndex) : ∃ γ : ℕ → ℝ, D.IsDenominatorBound θ j γ

-- skeleton/05_determinant/long_forms.lean (upstream LongForms.lean:177-266)
lemma New.Data.heightLe_chainDeterminant_one_div {θ : PrimePadicIso → D.MFoldTwist} (hθ : D.HasSmallRatios θ)
    (h_single : ∀ j ∈ Icc 1 D.m,
      HeightLe (D.targetDim * (D.f * D.a * D.logB ^ 4 / D.m - D.k * D.directionAmount))
        fun ι : PrimePadicIso ↦ D.liftDeterminant 1 j / D.liftDeterminant (θ ι) j) :
    HeightLe (D.targetDim * (D.b + D.f * D.a * D.logB ^ 4 - D.mainAmount))
      fun ι : PrimePadicIso ↦ D.chainDeterminant (1 : D.MFoldTwist) / D.chainDeterminant (θ ι)

theorem New.Data.integral_forms_long_with_completion_cost {n : ℕ} {y : Fin n → D.targetForms} {δ c : ℝ}
    (hy : LinearIndependent ℂ y)
    (hδ : D.targetLattice.IndexLe y δ) (hc : D.targetLattice.CovolumeLe y c) :
    D.targetDim * (D.mainAmount - D.f * D.a * D.logB ^ 4 - D.b) ≤
      c + 2 * δ + ((D.targetDim : ℝ) - n) * (2 * D.x₁ ^ 2 * D.b ^ 2 + D.b)

theorem New.Data.integral_forms_long {n : ℕ} {y : Fin n → D.targetForms} {δ c : ℝ}
    (hy : LinearIndependent ℂ y) (hn : (1 - 4 / D.b ^ 2) * D.targetDim ≤ n)
    (hδ : D.targetLattice.IndexLe y δ) (hc : D.targetLattice.CovolumeLe y c) :
    D.targetDim * (D.mainAmount - 3 * (D.f * D.a * D.logB ^ 4)) ≤ c + 2 * δ
```

- **Handoff to Stage 01:** `New.Data.integral_forms_long` is consumed directly by `01_reduction/main_inequality` (`New.star_inequality` in [The Main Cancellation Inequality and Theorems A & B](../01_reduction/main-inequality.md)).
- **Verification status (`audit/verification-status.json`):** Local Lean compilation and kernel replay remain **`build_status: unverified-locally`** at pinned commit `41c2716658c42ba08eb0648994a2efe83082a95c` (`41c2716658c4`). Static token scanning has **`scan_scope: entry`**, covering only `Challenge.lean`, `src/Main.lean`, and `src/Statements.lean` (`0` forbidden tokens in solution files, `5` expected `sorry` tokens in `Challenge.lean`). Neither `FitTwists.lean` nor `LongForms.lean` has been locally compiled, kernel-checked, or statically scanned here.

<!-- rh:lean-node:05_determinant/fit_twists -->

<!-- rh:lean-node:05_determinant/long_forms -->
