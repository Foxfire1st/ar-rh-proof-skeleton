# Vanishing Periods of False Lifts and Negative Heights of Twisted L-Values

This page explains the two analytic engines backing Stage 06: `src/New/OneLift/FalseLifts/PeriodsOfFalseLifts.lean` (`06_one_lift/periods_of_false_lifts`, 13 declarations, 1 `let`-type stub truncation) — which kills complementary-block theta periods of false lifts by climbing a hyperbolic-plane descent tower — and `src/New/OneLift/HeckeValues/TwistedValueHeights.lean` (`06_one_lift/twisted_value_heights`, 24 declarations, 2 `let`-type stub truncations) — which uses four-block rank-one character theta lifts and Stage 02's sharpness theorem to establish the negative arithmetic height of inverse twisted Hecke $L$-value ratios. Return to the [Stage 06 Overview](./overview.md), or view [Three Exchanges, the Carrier Form, and Divided-Lattice Gluing](./three-exchanges-and-gluing-maps.md) and [The Lift Comparison Operator and Single-Lift Volume Height](./lift-volume-height.md).

## Level 1 — First-Principles

Two obstacles stand between the gluing machinery of `GluingMaps.lean` and a negative single-lift height in `LiftVolumeHeight.lean`. First, how do we ensure that {{concept:divided-lattice-gluing|glued forms built across the divided lattice}} and test forms do not collide with **false lifts**—forms that mimic true lifts in congruence level and Hecke eigenvalue yet sit orthogonal to every lift? Second, how do we prove that the inverse {{concept:carrier-form|Hecke value product}} ratio $\Gamma_j(1)/\Gamma_j(\theta(\iota)^j)$ actually has a *negative* {{concept:arithmetic-height|global arithmetic height}}?

Stage 06 solves both problems in sequence—and uses the first solution directly inside the second.

![Two-panel diagram showing Left: the descent tower adding hyperbolic planes W -> W + H -> ... -> W + H^N under 2r + 2N + 3 < n killing complementary periods of false lifts; Right: the four-block RankOneIndex p.z = 12 + 4 + 4 + (2z - 20) seesaw and 4-factor height ledger summing to 6 denominatorBudget - D.directionAmount](assets/stage06-false-lifts-and-rank-one-blocks.svg)

### Part 1: Climbing the descent tower for false lifts (`PeriodsOfFalseLifts.lean`)

On a definite space $I_n$ paired against $W$ (rank $r < n$), Stage 06 rules out interference from a false lift $\Xi \in \mathrm{falseLiftsAt}$ by proving that every {{concept:false-lift-period|complementary-block theta period of a false lift}} vanishes. By definition, $\Xi$ is killed simultaneously by three operators:
1. **{{term:false-lift-period|Hyperplane sums}} at chosen places (`HyperplaneSumsAnnihilate`):** character-weighted sums $\sum \chi_v(\det s)^{-1} s$ over invertible matrices $s = 1 + u\lambda_v$ (which automatically annihilate forms in the image of the {{concept:central-idempotent|good idempotent}} $e_0$, and in rank one annihilate the mean idempotent `labelIdem Unit (X - 1)`, `hyperplaneSum_apply_eq_zero_of_rank_one`);
2. **Theta descent to $W$ (`O.normalizedDescent Ξ = 0`):** vanishing pairing against the {{concept:theta-correspondence|theta kernel}} back down to $U(W)$;
3. **{{term:false-lift-period|Small-root Hecke vanishing}} (`heckePolyOnForms`):** annihilation by the {{concept:hecke-operator|split-place Hecke polynomial}} $H_w(X_*)$ evaluated at `liftHeckeRoot w` of modulus $q^{1 - n + r/2}$ ($q$ the residue cardinality at a split place $w$ off the level).

Assuming nontrivial residue characters `O.tamePart` at every twist place and the **strict rank inequality**
$$2r + 2N + 3 < n,$$
Stage 06 climbs a {{term:false-lift-period|hyperbolic-plane descent tower}} from `BottomOfTower` (where hyperplane sums upgrade one vanishing harmonic descent `lift_polyVector_blockDatum_eq_zero` to all descents on $W$) up through $N$ steps of `HyperbolicPlaneStep` to $W \oplus \mathbb{H}^N$ (`descentsVanish_hyperbolicSum`): at every rung, small-root Hecke vanishing kills any bounded nonzero descent.

Once all descents to $W \oplus \mathbb{H}^N$ vanish, taking a Fourier coefficient kills the complementary block integral over $U(I_l)$ (`integral_conj_pairTheta_mul_algFormCoeff_eq_zero`) for any split $I_n = I_N \oplus I_l$, collapsing to vanishing discrete class sums (`stdClassSum_gramForm_eq_zero`, `stdClassSum_gramForm_kernelVector_eq_zero`). That single theorem feeds both consumers:
- **`stdClassSum_period_eq_zero`** certifies that four-block point-form class sums are orthogonal to false lifts in `TwistedValueHeights.lean`;
- **`DividedSplitting.stdClassSum_inner_productForm_eq_zero`** certifies that $V^\circ$-slices of false lifts vanish in `GluingMaps.lean`.

### Part 2: Rank-one lifts and negative $L$-value heights (`TwistedValueHeights.lean`)

For each {{concept:hecke-l-value-ratio|main pair}} $p = (\nu, z)$ of lift $j$, Stage 06 expresses $(p.\mathrm{ratio}(\vartheta))^{-1}$ as a conductor-norm power times $\|\varphi_1\|^2 / \|\varphi_\vartheta\|^2$, where $\varphi_\vartheta$ is the normalized {{concept:character-theta-lift|character theta lift}} of $\nu\vartheta$ from $U(1)$ to $U(2z)$ (powered by the {{concept:rallis-inner-product|Rallis inner product formula}}). To bound this ratio from above across all {{concept:prime-number|prime}} embeddings $\iota$ of {{concept:regular-twist-family|a regular twist family}} $\theta$, `TwistedValueHeights.lean` splits `RankOneIndex p.z` into four blocks of sizes $12 + 4 + 4 + (2z - 20)$:

- **Block 1 (`Fin 12`):** carries a degree-`rowLength` monomial on a `modKernel`, where [Stage 02's sharpness theorem](../02_derivatives/sharpness-bound-attained.md) (`02_derivatives/bound_attained`) guarantees that some Taylor derivative avoids excess $\ell$-adic divisibility beyond `localPrimeSumTotal l`.
- **Block 2 (`Fin 4`):** carries the twist $\vartheta$ via a ramified flat pair $q_2$, contributing its {{term:character-theta-lift|integral algebraic part}} and hence $\rho = q_2.\mathrm{ratio}(\vartheta)$.
- **Blocks 3 & 4 (`Fin 4` and `Fin (2z - 20)`):** carry untwisted flat pairs $q_3$ and $q$; summing over Block 4 via `trivialLiftClassSum` enforces {{term:false-lift-period|false-lift orthogonality}} (`isTestForm_trivialLiftClassSum_pointForm`, since $2\cdot 1 + 2\cdot 20 + 3 = 45 < 2z$).

Factoring the pairing against `X.kernelForm g` (`inner_pointForm_kernelForm` $\Rightarrow$ `exists_isTestForm_inner_kernelForm_eq`), selecting untwisted characters by {{term:character-theta-lift|multiplicative pigeonhole}} (`exists_mul_eq_norm_map_le_norm_map_mul`) and $U(1)$ character orthogonality (`exists_norm_le_norm_twisted_classSum`), and plugging in Stage 02's `bound_attained` produces a {{term:character-theta-lift|rank-one test form}} $\Xi$ whose $\ell$-adic pairing with $\varphi_\vartheta$ is bounded *below* by $\|\iota_p(c_0 \cdot q_2.\mathrm{ratio}\,\vartheta)\|\,\ell^{-\mathrm{localPrimeSumTotal}\,\ell}$ (`exists_isTestForm_le_norm_inner_trivialLift`).

Projecting $\Xi$ and its conjugate-place partner $\Xi'$ onto $\mathbb{C}\cdot \varphi_\vartheta$ yields the **exact 4-factor height ledger** in `New.Data.heightLe_inv_ratio`:
1. **Factor 1 — Untwisted lift norm $\|\varphi_1\|^2$ (`AlgebraicPartSize.lean::heightLe_norm_trivialLift_sq`):** contributes $\mathrm{denominatorBudget} - \sum_\tau \mathrm{jetSize}(\mathrm{weight}\,\tau)$. **This is where the minus sign on `jetSize` enters.**
2. **Factor 2 — Main-pair projection $\langle \mathrm{pr}\,\Xi, \mathrm{pr}\,\Xi'\rangle$ (`IsRegularFamily`):** contributes $\mathrm{denominatorBudget}$.
3. **Factor 3 — Auxiliary flat-pair inverse square $\rho^{-2}$ (`HasSmallRatios` on ramified $q_2$):** contributes $2 x_1 \le 2\,\mathrm{denominatorBudget}$.
4. **Factor 4 — Conjugate-paired test-form denominator $\rho^2 / (\langle \Xi, \varphi_\vartheta\rangle\langle \varphi_\vartheta, \Xi'\rangle)$ (`exists_isTestForm_le_norm_inner_trivialLift`):** contributes $2\,\mathrm{denominatorBudget} + \sum_\tau \mathrm{primeSum}(\mathrm{weight}\,\tau, \tau)$. Notice carefully: Stage 02's `BoundAttained` enters with a **plus sign**; it caps the *upward* $p$-adic valuation penalty below Factor 1's *downward* archimedean credit—never claim `BoundAttained` produces a negative height by itself!

Summing the four factors cancels $D.\mathrm{directionAmount} = \sum_\tau (\mathrm{jetSize}(\mathrm{weight}\,\tau) - \mathrm{primeSum}(\mathrm{weight}\,\tau, \tau))$ cleanly to prove **`heightLe_inv_ratio`**:
$$\mathrm{HeightLe}\bigl(6\,\mathrm{denominatorBudget} - D.\mathrm{directionAmount}\bigr) \quad\text{on}\quad \bigl(p.\mathrm{ratio}(\theta(\iota)^j)\bigr)^{-1}.$$

Finally, factoring $\Gamma_j(\vartheta)/\Gamma_j(1)$ over the three exchange pairs (`heckeValueProd_eq_mul_prod_ratio_sq`, `exists_heckeValueProd_eq_mul_prod_ratio`) splits the squared ratios into **exact pair counts**:
- **`#A = k / 2` main pairs** ($2\cdot \#A = D.k$), running over $7(m/2) < z \le 7(m/2) + k/2$ on `i = none` with sign $s = +1$, squared to yield **$k$ copies** of $6\,\mathrm{denominatorBudget} - D.\mathrm{directionAmount}$;
- **`#B = 5 * D.m` ramified pairs** ($m/2$ at `some (-1)` with sign $-1$, $m$ at `some 1` with sign $+1$, and $7(m/2)$ at `none` with sign $+1$), squared to yield **$10m$ copies** of $x_1$.

Adding these heights yields **`heightLe_inv_heckeValueProd_div`**:
$$\mathrm{HeightLe}\Bigl(10 m x_1 + k\bigl(6\,\mathrm{denominatorBudget} - D.\mathrm{directionAmount}\bigr)\Bigr) \quad\text{on}\quad \frac{\Gamma_j(1)}{\Gamma_j(\theta(\iota)^j)}.$$

### Where this analogy breaks

Thinking of the four-block seesaw as a simple product of four independent $L$-values hides two subtle arithmetic constraints. First, all four blocks share the *same* $U(1)$ variable $g$ and must multiply to the exact character `charV` of the main pair `X`, which is why Blocks 3 and 4 run over the finite character group `tameChars` and require multiplicative pigeonhole (`exists_mul_eq_norm_map_le_norm_map_mul`) plus a geometric-mean archimedean bound (`exists_root_prod_integralAlgebraicPart`) to pick characters without losing $\ell$-adic size. Second, the `12`-dimensional first block is not chosen arbitrarily: `Fin 12` is large enough for the Stage 02 theta-jet sharpness theorem while leaving $20 = 12 + 4 + 4$ small enough that $2\cdot 1 + 2\cdot 20 + 3 = 45 < 2z$ (since $z > m \ge 10000$) satisfies `PeriodsOfFalseLifts`'s strict inequality!

## Level 2 — Undergrad Bridge

### 1. Hyperbolic-plane descent tower (`PeriodsOfFalseLifts.lean`, 13 decls, 1 truncation)

1. **Bottom of the tower (`lift_polyVector_blockDatum_eq_zero`):** From `O.FitsParams l₀` and `O.normalizedDescent Ξ = 0`, real convolution spreads vanishing from `highestVector` to every Fock polynomial `polyVector P` against `O.blockDatum`. `BottomOfTower.lean::descentsVanish_of_hyperplaneSumsAnnihilate` then upgrades `O.blockDatum` to all finite data vacuum off `O.places.conjClosure`.
2. **Tower induction (`descentsVanish_hyperbolicSum`):** Under `h_ramified : ∀ i, O.tamePart (O.twistPlace i) ≠ 1`, `h_hyperplane`, `h_theta`, `h_hecke` at `O.liftHeckeRoot w` (modulus $q^{1 - n + r/2}$), and strict rank margin `2 * card r + 2 * card N + 3 < card n`, induction via `HyperbolicPlaneStep.lean` proves `DescentsVanish (hyperbolicSum N O.W) (stdSpace E n)` for every vector $\zeta$.
3. **From vanishing descents to class sums (`integral_conj_pairTheta_mul_algFormCoeff_eq_zero`, `stdClassSum_gramForm_eq_zero`, `stdClassSum_gramForm_kernelVector_eq_zero`):** Given $\varepsilon : N \oplus l \simeq n$ and $\gamma \in \mathrm{supportedOn}\ n\ O.\mathrm{places}$, Fourier-coefficient unfolding kills the $U(I_l)$ quotient integral of $\overline{\mathrm{pairTheta}}\cdot \mathrm{algFormCoeff}$. Equivariant compact integration turns this into `stdClassSum K f = 0` for every stabilizing level $K$.
4. **Rank-one and divided-splitting specializations (`hyperplaneSum_apply_eq_zero_of_rank_one`, `stdClassSum_period_eq_zero`, `DividedSplitting.stdClassSum_inner_productForm_eq_zero`):** In rank $r = 1$, `labelIdem Unit (X - 1)` is the uniform mean over $\mathrm{GL}_1(k_w)$, killed by any nontrivial hyperplane sum (`hyperplaneSum_mul_labelIdem_X_sub_one`), yielding `stdClassSum_period_eq_zero` for point forms (`TwistedValueHeights`). For divided splittings, `DividedSplitting.algFormRep_conj_block_eq_self` shows `principalLevel l ϖ` fixes `Ξ` around `L.point`, yielding `DividedSplitting.stdClassSum_inner_productForm_eq_zero` (`GluingMaps`).

### 2. Rank-one test forms and exact height accounting (`TwistedValueHeights.lean`, 24 decls, 2 truncations)

1. **Four-block test form construction (`trivialLiftClassSum`, `inner_pointForm_kernelForm`, `isTestForm_trivialLiftClassSum_pointForm`, `exists_isTestForm_inner_kernelForm_eq`):**
   On `RankOneIndex p.z` $= \mathrm{Fin}\,12 \oplus (\mathrm{Fin}\,4 \oplus \mathrm{Fin}\,4) \oplus \mathrm{Fin}(2z - 20)$, `testColumn` puts $x_0$ on Block 1 and standard basis vectors on Blocks 2–4 (injective by `testColumn_injective`), while `twistRow p.z` reads Block 2 at twist places. Summing `pointForm` over Block 4 gives an integral test form orthogonal to false lifts (`isTestForm_trivialLiftClassSum_pointForm`, calling `stdClassSum_period_eq_zero` with $45 < 2z$). Summing over Blocks 2 and 3 yields `exists_isTestForm_inner_kernelForm_eq`:
   $$\langle \Xi, X.\mathrm{kernelForm}\,g\rangle \cdot N^3 \cdot \mathrm{vacuumScale}^{2z - 12} = \frac{\prod_\tau (X.\mathrm{rowLength}\,\tau)!}{\alpha!}\,q.\mathrm{iap}(1)\,q_3.\mathrm{iap}(1)\,q_2.\mathrm{iap}(\vartheta)\,\mathrm{modKernel}_{\mathrm{Fin}\,12}.$$
2. **Pairing lower bound (`exists_norm_le_norm_twisted_classSum`, `exists_mul_eq_norm_map_le_norm_map_mul`, `exists_root_prod_integralAlgebraicPart`, `exists_isTestForm_le_norm_inner_trivialLift`):**
   Combining the `tameChars` geometric mean $L$ ($\|\sigma L\| \le \exp(x^{34})^2$), multiplicative pigeonhole, $U(1)$ character orthogonality, and `02_derivatives/bound_attained` gives $c_0 \neq 0$ ($\|\sigma c_0\| \le \exp(f m b / k + 3 x^{34}) \le \exp(\mathrm{denominatorBudget})$) and a test form $\Xi$ with $\|\iota_p(c_0 \cdot q_2.\mathrm{ratio}\,\vartheta)\|\,\ell^{-D.\mathrm{localPrimeSumTotal}\,\ell} \le \|\iota_p(\langle \Xi, \varphi_\vartheta\rangle)\|$.
3. **Exact 4-factor height sum (`heightLe_inv_ratio`):**
   Using $\langle \mathrm{pr}\,\Xi, \mathrm{pr}\,\Xi'\rangle = \langle \Xi, \varphi\rangle\langle \varphi, \Xi'\rangle / \|\varphi\|^2$ and $(p.\mathrm{ratio}(\vartheta))^{-1} = \mathrm{Nm}(Q)^{2z-1}\cdot \|\varphi_1\|^2 / \|\varphi_\vartheta\|^2$:
   $$\begin{aligned}
   \mathrm{Height}\bigl(\|\varphi_1\|^2\bigr) &\le \mathrm{denominatorBudget} - \sum_\tau D.\mathrm{jetSize}(D.\mathrm{weight}\,\tau) && (\texttt{heightLe\_norm\_trivialLift\_sq}) \\
   \mathrm{Height}\bigl(\langle \mathrm{pr}\,\Xi, \mathrm{pr}\,\Xi'\rangle\bigr) &\le \mathrm{denominatorBudget} && (\texttt{IsRegularFamily}\text{ mainPair}) \\
   \mathrm{Height}\bigl(\rho^{-2}\bigr) &\le 2 x_1 \le 2\,\mathrm{denominatorBudget} && (\texttt{HasSmallRatios}\text{ on }q_2) \\
   \mathrm{Height}\Bigl(\frac{\rho^2}{\langle \Xi, \varphi\rangle\langle \varphi, \Xi'\rangle}\Bigr) &\le 2\,\mathrm{denominatorBudget} + \sum_\tau D.\mathrm{primeSum}(D.\mathrm{weight}\,\tau, \tau) && (\text{conjugate-paired }\texttt{exists\_isTestForm\_le\_norm\_inner\_trivialLift}) \\
   \hline
   \mathrm{Height}\bigl((p.\mathrm{ratio}(\theta(\iota)^j))^{-1}\bigr) &\le 6\,\mathrm{denominatorBudget} - D.\mathrm{directionAmount}.
   \end{aligned}$$
4. **Exact pair-count aggregation (`heckeValueProd_eq_mul_prod_ratio_sq`, `exists_heckeValueProd_eq_mul_prod_ratio`, `heightLe_inv_heckeValueProd_div`):**
   With $A = (7(m/2),\, 7(m/2) + k/2]$ (`2 * #A = D.k`, all `IsMainPair j`, sign $+1$) and $B$ covering $(0, m/2]$ at `some (-1)`, $(0, m]$ at `some 1`, and $(0, 7(m/2)]$ at `none` (`#B = 7(m/2) + m + m/2 = 5 * D.m`, all `IsRamified`), squaring gives $k$ main-pair factors and $10m$ ramified factors (while $\mathrm{Nm}(Q)^{-e}$ adds $0$ height), proving
   $$\mathrm{HeightLe}\bigl(10 m x_1 + k(6\,\mathrm{denominatorBudget} - D.\mathrm{directionAmount})\bigr).$$

## Level 3 — Formal Lean Contract

`skeleton/06_one_lift/periods_of_false_lifts.lean` (13 declarations, 1 `let`-type stub truncation) and `skeleton/06_one_lift/twisted_value_heights.lean` (24 declarations, 2 `let`-type stub truncations) mirror `src/New/OneLift/FalseLifts/PeriodsOfFalseLifts.lean` and `src/New/OneLift/HeckeValues/TwistedValueHeights.lean` at pinned upstream commit `41c2716658c42ba08eb0648994a2efe83082a95c`.

### Stub-Truncation Disclosures (3 Truncations Total Across These Two Skeletons)

Because `rh-extract` splits signatures from proof bodies at the first depth-0 `:=` token, **3 declarations** across these two files truncate at a `let ... :=` binding inside their return type. Their exact untruncated signatures from the pinned upstream Lean files are quoted verbatim below:

1. **`New.Rows.stdClassSum_gramForm_eq_zero`** (`skeleton/06_one_lift/periods_of_false_lifts.lean` decl L91, truncated at L108; upstream `src/New/OneLift/FalseLifts/PeriodsOfFalseLifts.lean:326–345`):
   ```lean
   -- Verbatim from upstream src/New/OneLift/FalseLifts/PeriodsOfFalseLifts.lean:326-345
   theorem stdClassSum_gramForm_eq_zero (hO_fits : O.FitsParams l₀)
       (h_twist : ∀ i : O.TwistIndex, O.tamePart (O.twistPlace i) ≠ 1) {Ξ : O.FormV}
       (h_reflect : O.HyperplaneSumsAnnihilate Ξ) (hθ : O.normalizedDescent Ξ = 0)
       {w : FinPlace E} {hw : star w ≠ w} {hw_off : w ∉ O.places.conjClosure}
       (h_hecke : O.heckePolyOnForms hw hw_off (O.liftHeckeRoot w) Ξ = 0)
       (h_rank : 2 * card r + 2 * card N + 3 < card n) (ε : N ⊕ l ≃ n) {κ : InfinitePlace E → ℕ → ℕ}
       (C : (WeylIndex l κ → ℂ) →ₗ⋆[ℂ] WeylIndex n O.columnCount → ℂ)
       (hC : ∀ (g : GInf E (1 : n → ℤˣ)) (y : GInf E (1 : l → ℤˣ)) (ζ : WeylIndex l κ → ℂ),
         (∀ τ : InfinitePlace E, (g τ : GL n ℂ) = reindexGL ε (blockGL (1, y τ))) →
           conjWeylRep O.columnCount O.detExp g (C ζ) = C (conjWeylRep κ 0 y ζ))
       (ϑ : Gf E l → WeylIndex l κ → ℂ)
       (h_kernels : ∀ j : WeylIndex l κ, ∃ (c : ℂ) (g : G E O.signs) (b : AutChar E)
         (φ : 𝒮 (O.W.hom (stdSpace E l))),
         projOn (O.W.hom (stdSpace E l)) O.places.conjClosureᶜ φ = φ ∧
           ∀ (u : GInf E (1 : l → ℤˣ)) (h : Gf E l), conjWeylRep κ 0 u⁻¹ (ϑ h) j =
             c * conj (pairTheta O.W (stdSpace E l) O.charW b φ (g, u, h)))
       (h₁ : Gf E N) {x : Gf E n} (hx : x ∈ supportedOn n O.places) :
       let f (h : Gf E l) : ℂ := gramForm (C (ϑ h)) (Ξ (U.block ε (h₁, h) * x))
       (∀ δ ∈ (stdSpace E l).Γ, ∀ h : Gf E l, f (δ.2 * h) = f h) ∧
         ∀ (K : Subgroup (Gf E l)) [IsLevel K], K ≤ rightStabilizer f → stdClassSum K f = 0
   ```

2. **`New.Data.inner_pointForm_kernelForm`** (`skeleton/06_one_lift/twisted_value_heights.lean` decl L152, truncated at L170; upstream `src/New/OneLift/HeckeValues/TwistedValueHeights.lean:353–380`):
   ```lean
   -- Verbatim from upstream src/New/OneLift/HeckeValues/TwistedValueHeights.lean:353-380
   theorem inner_pointForm_kernelForm (hq : q.HasEmptyRow (Fin (2 * p.z - 20)))
       (hq₂ : q₂.HasEmptyRow (Fin 4)) (hq₃ : q₃.HasEmptyRow (Fin 4)) {j : ℕ}
       {α : NumberField.InfinitePlace D.E × Fin 12 × Unit →₀ ℕ} {χ : AutChar D.E}
       (hp : p.IsMainPair j) (ϑ : D.Twist)
       (x₀ : Matrix (Fin 12) Unit ((lValueData hp.hasLiftRank ϑ).levelModulus 1 false).ResidueRing)
       {c : WeylIndex (RankOneIndex p.z) (lValueData hp.hasLiftRank ϑ).descentPair.columnCount → ℤ}
       {a : ℂ}
       (h_pol : (lValueData hp.hasLiftRank ϑ).descentPair.fockPoly (star ((↑) ∘ c)) =
         a • renameRowsCols (Function.Embedding.trans .inl .inl : Fin 12 ↪ RankOneIndex p.z) id
           (monomial α 1))
       (h_char : χ * ((lValueData hq₂.hasLiftRank ϑ).descentPair.charV *
           (lValueData hq₃.hasLiftRank .one).descentPair.charV) *
             (lValueData hq.hasLiftRank .one).descentPair.charV =
         (lValueData hp.hasLiftRank ϑ).descentPair.charV)
       (g : Gf D.E Unit) (h₁ : Gf D.E (Fin (2 * p.z - 20))) (h₂ h₃ : Gf D.E (Fin 4))
       (i₁ : WeylIndex (Fin (2 * p.z - 20)) (lValueData hq.hasLiftRank .one).descentPair.columnCount)
       (i₂ : WeylIndex (Fin 4) (lValueData hq₂.hasLiftRank ϑ).descentPair.columnCount)
       (i₃ : WeylIndex (Fin 4) (lValueData hq₃.hasLiftRank .one).descentPair.columnCount) :
       let X : LValueData D.E (RankOneIndex p.z) := lValueData hp.hasLiftRank ϑ
       let u : D.TameChar := TameChar.one
       innerSLFlip ℂ (X.kernelForm g)
           (pointForm (U.blockSumOne (U.blockSumOne (1, U.blockSumOne (h₂, h₃)), h₁))
             (testColumn_injective hq hp ϑ x₀) (twistRow p.z) ((↑) ∘ c)) =
         a * (modKernel (stdSpace D.E Unit) (stdSpace D.E (Fin 12)) 1 χ (X.levelModulus 1 false)
             (Pi.single x₀ 1) (monomial α 1) (1, g) (1, 1) *
           (lValueData hq₂.hasLiftRank ϑ).kernelForm g h₂ i₂ *
           (lValueData hq₃.hasLiftRank u).kernelForm g h₃ i₃ *
           (lValueData hq.hasLiftRank u).kernelForm g h₁ i₁)
   ```

3. **`New.Data.exists_isTestForm_inner_kernelForm_eq`** (`skeleton/06_one_lift/twisted_value_heights.lean` decl L216, truncated at L228; upstream `src/New/OneLift/HeckeValues/TwistedValueHeights.lean:575–601`):
   ```lean
   -- Verbatim from upstream src/New/OneLift/HeckeValues/TwistedValueHeights.lean:575-601
   theorem exists_isTestForm_inner_kernelForm_eq {l : ℕ} [Fact l.Prime]
       (hq : q.HasEmptyRow (Fin (2 * p.z - 20))) (hq₂ : q₂.HasEmptyRow (Fin 4))
       (hq₃ : q₃.HasEmptyRow (Fin 4)) {j : ℕ}
       {α : NumberField.InfinitePlace D.E × Fin 12 × Unit →₀ ℕ} {χ : AutChar D.E} [PadicPlace l]
       (hp : p.IsMainPair j) (ϑ : D.Twist) (hl : l.Coprime ϑ.normQ)
       (x₀ : Matrix (Fin 12) Unit ((lValueData hp.hasLiftRank ϑ).levelModulus 1 false).ResidueRing)
       (hα : ∀ τ : NumberField.InfinitePlace D.E,
         ∑ i : Fin 12, α (τ, i, ()) = (lValueData hp.hasLiftRank ϑ).rowLength τ)
       (h_char : χ * ((lValueData hq₂.hasLiftRank ϑ).descentPair.charV *
           (lValueData hq₃.hasLiftRank .one).descentPair.charV) *
             (lValueData hq.hasLiftRank .one).descentPair.charV =
         (lValueData hp.hasLiftRank ϑ).descentPair.charV) :
       let X : LValueData D.E (RankOneIndex p.z) := lValueData hp.hasLiftRank ϑ
       let u : D.TameChar := TameChar.one
       ∃ Ξ : X.descentPair.FormV,
         (∀ g : Gf D.E Unit,
           inner ℂ Ξ (X.kernelForm g) *
               (stdClassSum X.chosenLevel 1 ^ 3 * P.vacuumScale D.towerExp ^ (2 * p.z - 20 + 8)) =
             (∏ τ : NumberField.InfinitePlace D.E, (X.rowLength τ)! : ℕ) /
               α.prod (fun _ (k : ℕ) ↦ (k ! : ℂ)) * q.integralAlgebraicPart hq.hasLiftRank u *
               q₃.integralAlgebraicPart hq₃.hasLiftRank u *
               q₂.integralAlgebraicPart hq₂.hasLiftRank ϑ *
               modKernel (stdSpace D.E Unit) (stdSpace D.E (Fin 12)) 1 χ (X.levelModulus 1 false)
                 (Pi.single x₀ 1) (monomial α 1) (1, g) (1, 1)) ∧
         X.descentPair.IsTestForm l Ξ
   ```

All 12 remaining declarations in `periods_of_false_lifts.lean` and all 22 remaining declarations in `twisted_value_heights.lean` (including `heightLe_inv_ratio`, `exists_heckeValueProd_eq_mul_prod_ratio`, and `heightLe_inv_heckeValueProd_div`) extract completely untruncated.

- **Verification status (`audit/verification-status.json`):** Local Lean compilation and kernel replay remain **`build_status: unverified-locally`** at pinned commit `41c2716658c42ba08eb0648994a2efe83082a95c` (`41c2716658c4`). Static token scanning has **`scan_scope: entry`**, covering only `Challenge.lean`, `src/Main.lean`, and `src/Statements.lean`. Neither `PeriodsOfFalseLifts.lean` nor `TwistedValueHeights.lean` has been locally compiled, kernel-checked, or statically scanned here.

<!-- rh:lean-node:06_one_lift/periods_of_false_lifts -->

<!-- rh:lean-node:06_one_lift/twisted_value_heights -->
