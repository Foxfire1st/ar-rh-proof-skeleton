# Target Dimension, Multiplicity Equality, and One Good Translate

This page explains the top two landmarks of Stage 04 — `src/New/Spanning/Multiplicities/EqualMultiplicities.lean` (`04_spanning/equal_multiplicities`, 17 declarations, 0 stub truncations) and `src/New/Spanning/SecondMoment/SpanningTheorem.lean` (`04_spanning/spanning_theorem`, 21 declarations, 0 stub truncations) — together with the linear-algebraic spreading and greedy-covering engine in `src/New/Spanning/SecondMoment/FewTranslates.lean` and its consumption by Theorem B (`New.Data.enough_forms`) in [Stage 01](../01_reduction/main-inequality.md). Return to the [Stage 04 Overview](./overview.md), or continue to [Relative-Trace Unfolding & Second Moments](./unfolding-and-second-moments.md) and [Kloosterman Sieve & Remaining Terms](./main-term-sieve-and-remaining-terms.md).

## Level 1 — First-Principles

Before Stage 04 can prove that a few translates of the Poincaré map fill almost all of the target space `D.targetForms`, it faces two immediate obstacles:

1. **How big is the target space?** The target forms live on the indefinite unitary space $Y_0$. Because $Y_0$ is indefinite, there is no simple finite-sum formula for counting $d_T = D.\mathrm{targetDim} = \dim_{\mathbb{C}}(D.\mathrm{targetForms})$ directly.
2. **How do you turn an average overlap bound into spanning?** Even if you know the average overlap of translates $e_0 s P$ across the finite group $\bar{G}$, you still need to rule out a conspiracy in which every translate lands inside the same proper subspace $F \subsetneq D.\mathrm{targetForms}$.

`EqualMultiplicities.lean` solves the first obstacle, and `SpanningTheorem.lean` solves the second:

### Step 1: Counting $Y_0$ by transferring to a definite twin space (`EqualMultiplicities.lean`)

Imagine two finite-dimensional spaces $V$ and $V'$ carrying matching families of commuting symmetry operators. If every symmetry operator has the exact same trace on $V$ and $V'$, then both spaces must have the exact same dimension (just plug in the identity operator $T = 1$) and identical eigenspace dimensions for every joint eigensystem.

Stage 04 applies this {{concept:trace-comparison|matching of traces across spaces that share all finite primes and differ only in real signature}} between the indefinite space $Y_0$ and the definite companion spaces $W_1, \dots, W_m$. Why does the trace match cleanly? Because target forms are cut out by a {{concept:central-idempotent|orthogonal projection selecting finite-field polynomial labels including at least one primitive polynomial}} $e_0 = e_{p_0} \otimes e_{p_1}$. At a split finite place where the polynomial label $c_x$ is primitive, the local test function is cuspidal, which eliminates all non-elliptic terms in the trace formula. As a result, every spherical {{concept:hecke-operator|averaging operator built from finite matrix cosets away from the level}} has the exact same trace, characteristic polynomial, and determinant on the good forms of $Y_0$ and on the good forms of $W_j$.

That single transfer pays two separate dividends:
- **For Stage 04 (Theorem B):** Taking $T = 1$ equates $d_T = \dim(D.\mathrm{goodForms}\ W_m)$. Because $W_m$ is *definite* and the principal congruence level acts freely on $W_m$, all principal-level forms on $W_m$ have exact dimension $\#\bar{G} \cdot \mathrm{deg} \cdot \mathrm{mass}_r$, immediately giving the target-dimension ceiling $d_T \le \#\bar{G} \cdot \mathrm{deg} \cdot \mathrm{mass}_r$.
- **For Stage 05 (Theorem C):** Realizing the Rallis inner-product series of a {{concept:theta-correspondence|unitary dual-pair lift between automorphic forms}} as a spherical {{term:hecke-operator}} proves `det_rallisOnGood_eq`—the Rallis operator has the exact same determinant on the good forms of $Y_0$ and $W_j$, feeding directly into `05_determinant/norm_of_chain`.

### Step 2: Combining four inputs into one good translate (`SpanningTheorem.lean`)

With $d_T \le \#\bar{G}\cdot \mathrm{deg}\cdot \mathrm{mass}_r$ in hand, `SpanningTheorem.lean` combines four ingredients measured in {{concept:hilbert-schmidt-norm|root-sum-of-squares Frobenius operator norm}}:

1. **Rarity of bad polynomial labels (`bad_types_rare`):** Each prime-factor {{term:central-idempotent|good idempotent}} $e_{p_i}$ rejects only a tiny {{term:central-idempotent|Plancherel share}} $\operatorname{Re}((1 - e_{p_i})(1)) \le b^{-8} / 2$.
2. **Good-forms near-isometry (`isNearIsometry_e₀_comp_scaledPoincare`):** Combining `bad_types_rare` with {{concept:bessel-orbital-integral|unfolded Petersson amplitude blocks}} from `Moments.lean` shows that $e_0 P$ preserves the squared energy of every source subspace $U$ up to $5 b^{-4} D_X$: $\|e_0 P \mathrm{pr}_U\|_{\mathrm{HS}}^2 \ge \dim U - 5 b^{-4} D_X$.
3. **Whole-group second moment (`sum_hilbertSchmidtNorm_scaledPoincare_sq_le`):** Summing squared overlaps across all $s \in \bar{G}$ gives $\sum_{s \in \bar{G}} \|P^* s P\|_{\mathrm{HS}}^2 \le (1 + 9 b^{-4}) \mathcal{D}_{\mathrm{main}}$, where $\mathcal{D}_{\mathrm{main}} = D_X^2 / (\mathrm{deg}\cdot \mathrm{mass}_r)$.
4. **Cancellation of $\mathrm{deg}\cdot \mathrm{mass}_r$:** Multiplying the target-dimension ceiling by the second-moment bound cancels $\mathrm{deg}\cdot \mathrm{mass}_r$ identically:
   $$d_T \sum_{s \in \bar{G}} \|P^* s P\|_{\mathrm{HS}}^2 \le \#\bar{G}\,(1 + 9 b^{-4})\, D_X^2.$$

By Cauchy–Schwarz, the left-hand side can never fall significantly below $\#\bar{G}\, D_X^2$ (the {{term:hilbert-schmidt-norm|Welch bound}} for a {{concept:tight-frame|balanced overcomplete system of subspaces}}). Because the actual product is within $9 b^{-4}$ of that universal floor, no proper subspace $F \subsetneq D.\mathrm{targetForms}$ can trap every translate $e_0 s P$: if $F$ occupies fraction $\alpha = \dim F / d_T$ of the target space, some translate $s \in \bar{G}$ has preimage of $F$ of dimension at most $\frac{1 + \alpha + 3\varepsilon_0}{2} D_X$ (`New.Data.isSpread_goodPoincare`, with $\varepsilon_0 = (8 b^2)^{-1}$).

![Four-input funnel of Stage 04 combining trace comparison, good idempotent near-isometry, and the whole-group second moment through Welch-style spreading and Wolsey greedy covering into Theorem B](assets/stage04-second-moment-spreading-funnel.svg)

### Step 3: Handoff to Stage 01 (`FewTranslates.lean` $\to$ `ShortAndEnough.lean`)

Stage 04 finishes at the one-translate {{term:hilbert-schmidt-norm|spreading property}} `New.Data.isSpread_goodPoincare`. In [Stage 01](../01_reduction/main-inequality.md) (`01_reduction/short_and_enough.lean::New.Data.enough_forms`), Wolsey's {{term:hilbert-schmidt-norm|greedy covering}} lemma (`FewTranslates.lean::New.IsSpread.exists_few_le_finrank_iSup_range`) iteratively adds one good translate at a time: while more than a share $16\varepsilon_0 = 2/b^2$ of $D.\mathrm{targetForms}$ remains uncovered, each chosen translate multiplies the missing dimension by at most $1 - \frac{2 D_X}{5 d_T} \le \exp(-2 D_X / (5 d_T))$. Restricting from all source forms to {{concept:weyl-gauge|source forms whose central averages at primes dividing the level $\pi_N$ vanish}} (`D.ramifiedSourceForms`, codimension fraction $\le f/(N - 1) \le b^{-8}$) costs at most an extra $2/b^2$ share across all chosen translates, leaving a finite set $t \subseteq \bar{G}$ with $\#t \cdot \dim(\mathrm{src}) \le 5 d_T \log(1 + 2 b^2)$ whose images span at least $(1 - 4/b^2) d_T$ dimensions.

### Where this analogy breaks

Comparing `EqualMultiplicities.lean` to equating two finite matrices by their traces and `SpanningTheorem.lean` to scattering subspaces near the Welch bound is structurally faithful, but omits two load-bearing hypotheses. First, `trace_heckeOnGood_eq` requires both the indefinite and definite signatures to satisfy `(toHCParam s (D.paramW v)).HasGap D.gap` at every infinite place—without that discrete-series gap, the trace comparison would fail—and requires a primitive polynomial label at a split place in `D.goodTupleFamilies` to force cuspidality. Second, `New.Data.goodPoincare s` carries the scalar `#Ḡ` (matching `ShortAndEnough.lean`'s integer scaling) whereas `D.scaledPoincare` carries `(vol J · ν)^{-1/2}` (making $P^* P \approx 1$); `comap_scaledPoincare_eq_comap_goodPoincare` bridges the two only because nonzero scalars preserve preimage dimensions.

## Level 2 — Undergrad Bridge

### 1. Trace comparison and Rallis determinants (`EqualMultiplicities.lean`, 17 declarations)

Let $V = \mathrm{Space}\ D.E\ D.\mathrm{RowIndex}\ s$ and $V' = \mathrm{Space}\ D.E\ D.\mathrm{RowIndex}\ s'$ be two Hermitian spaces sharing the finite group $G_f$, principal congruence level $J_M = D.\mathrm{chosenLevel}$, and doubled parameter `D.paramW` satisfying `HasGap D.gap` for both sign tuples $s, s'$.

1. **Primitive witness and label decomposition (`exists_isPrimitivePolynomial`, `trace_heckeOnGood_eq_sum`):**
   Every $c \in D.\mathrm{goodTupleFamilies}$ has a primitive degree-$r$ polynomial $c_x$ over $\mathbb{F}_{w_x}$ at some chosen place $x$ above $p_0$ or $p_1$. Expanding $e_0 = \sum_{c \in D.\mathrm{goodTupleFamilies}} e_c$ gives
   $$\operatorname{tr}_{\mathbb{C}}\bigl(D.\mathrm{heckeOnGood}\ V\ T\bigr) = \sum_{c \in D.\mathrm{goodTupleFamilies}} \operatorname{tr}_{\mathbb{C}}\bigl(T_V \cdot \rho_V(e_c)\bigr).$$
2. **Cuspidal comparison (`trace_mul_tupleIdem_eq` $\Rightarrow$ `trace_heckeOnGood_eq`):**
   At the split place $x$ where $c_x$ is primitive, the local matrix coefficient for $e_{c_x}$ is supercuspidal, so `New.trace_eq_of_isCuspidalAt` equates each summand between $V$ and $V'$, yielding $\operatorname{tr}(D.\mathrm{heckeOnGood}\ V\ T) = \operatorname{tr}(D.\mathrm{heckeOnGood}\ V'\ T)$ for every spherical Hecke operator $T \in D.\mathrm{heckeAlgebra}$ away from $M$.
3. **Characteristic polynomials, determinants, and eigenspace dimensions:**
   - Applying `trace_heckeOnGood_eq` to all powers $T^n$ gives equal characteristic polynomials via Newton's identities (`charpoly_heckeOnGood_eq`) and equal determinants (`det_heckeOnGood_eq`).
   - Selecting an indicator operator $T \in D.\mathrm{heckeAlgebra}$ equal to $1$ on `eigenPart V q` and $0$ on all other eigensystems (`trace_heckeOnGood_eq_finrank`) proves **`finrank_goodForms_inf_eigenPart_eq`**: every joint eigensystem $q$ occurs with the exact same multiplicity on `goodForms V` and `goodForms V'`.
   - Setting $T = 1$ with $V = Y_0$ and $V' = W_j$ ($1 \le j \le m$) gives **`targetDim_eq_finrank_goodForms_spaceW`**: $D.\mathrm{targetDim} = \operatorname{finrank}_{\mathbb{C}}(D.\mathrm{goodForms}(D.\mathrm{spaceW}\ j))$.
4. **Rallis operator transfer for Stage 05 (`exists_heckeAlgebra_inner_eq_inner_translate` $\to$ `det_rallisOnGood_eq`):**
   Every off-level translate $\gamma \in D.\mathrm{offLevelGroup}$ averages over $K$-cosets to a single spherical Hecke operator $p \in D.\mathrm{heckeAlgebra}$ independent of $W$ (`exists_heckeAlgebra_inner_eq_inner_translate`). Hence every term `D.rallisTerm ϑ j W z y i` of Rallis' inner-product formula is a matrix coefficient of one $p_i \in D.\mathrm{heckeAlgebra}$ (`exists_rallisTerm_eq_inner`), and summing the series on both $V$ and $V'$ produces a single operator $p \in D.\mathrm{heckeAlgebra}$ acting as `D.rallisOperator ϑ j` on both spaces (`exists_heckeAlgebra_eq_mul_inner_of_hasSum_rallisTerm`, `exists_hecke_eq_rallisOperator`). Restricting to good forms defines `D.rallisOnGood` and gives **`New.Data.det_rallisOnGood_eq`**:
   $$(D.\mathrm{rallisOnGood}\ V\ \vartheta\ j).\mathrm{det} = (D.\mathrm{rallisOnGood}\ V'\ \vartheta\ j).\mathrm{det}.$$

### 2. Target dimension and Welch-style spreading (`SpanningTheorem.lean`, 21 declarations)

1. **Free principal level on $W_m$ (`finrank_principalForms_spaceW` $\Rightarrow$ `targetDim_le_card_mul_besselDegree_mul_mass`):**
   On the definite space $W_m = D.\mathrm{spaceW}\ D.m$, `D.chosenLevel` has trivial class stabilizers (`classAut = ⊥`) and index $\#\bar{G}$ inside the maximal compact level (whose class sum is `mass D.E D.r`). Each local weight space has dimension equal to the compact formal degree $\prod_v (toHCParam\ 1\ (D.\mathrm{paramW}\ v)).\mathrm{formalDegree} = D.\mathrm{filling.besselDegree}$ via Weyl's dimension polynomial. Therefore
   $$\dim_{\mathbb{C}}\bigl(D.\mathrm{principalForms}\ W_m\bigr) = \#\bar{G}\cdot \mathrm{deg}\cdot \mathrm{mass}_r,$$
   and combining with `targetDim_eq_finrank_goodForms_spaceW` yields
   $$D.\mathrm{targetDim} = \dim_{\mathbb{C}}\bigl(D.\mathrm{goodForms}\ W_m\bigr) \le \#\bar{G}\cdot \mathrm{deg}\cdot \mathrm{mass}_r.$$
2. **Scaling, unfolding, and rare bad types (`scaledPoincare`, `hasAmplitudeBlocks_scaledPoincare`, `bad_types_rare`):**
   Scaling the Poincaré map by `D.poincareScale` $= (\operatorname{vol}(J)\cdot \nu)^{-1/2} > 0$ puts it into `HasAmplitudeBlocks` (`hasAmplitudeBlocks_scaledPoincare`). Defining `D.goodIdempotentAbove i` ($i \in \{0, 1\}$) above each level prime $p_0, p_1$ proves `bad_types_rare`: each $e_{p_i}$ is a central idempotent, $e_{p_0} \otimes e_{p_1}$ acts as `D.e₀`, and $\operatorname{Re}((1 - e_{p_i})(1)) \le b^{-8}/2$.
3. **Feeding `Moments.lean` into `FewTranslates.lean`:**
   - `Moments.lean::isNearIsometry_piTensorElem_comp` gives **`isNearIsometry_e₀_comp_scaledPoincare`**: `IsNearIsometry (e₀ ∘ₗ P) (5 * b⁻⁴)`.
   - `Moments.lean::sum_hilbertSchmidtNorm_sq_le` gives **`sum_hilbertSchmidtNorm_scaledPoincare_sq_le`**:
     $$\sum_{s \in \bar{G}} \|P^* s P\|_{\mathrm{HS}}^2 \le \Bigl(1 + \frac{9}{b^4}\Bigr)\,\frac{D_X^2}{\mathrm{deg}\cdot \mathrm{mass}_r}.$$
   - Multiplying by `targetDim_le_card_mul_besselDegree_mul_mass` cancels $\mathrm{deg}\cdot \mathrm{mass}_r$ and gives **`targetDim_mul_sum_hilbertSchmidtNorm_sq_le`**:
     $$d_T \sum_{s \in \bar{G}} \|P^* s P\|_{\mathrm{HS}}^2 \le \#\bar{G}\,\Bigl(1 + \frac{9}{b^4}\Bigr)\, D_X^2.$$
   - Since $5 b^{-4} \le 9 b^{-4} \le \varepsilon_0 := (8 b^2)^{-1}$, both hypotheses of `FewTranslates.lean::isSpread_of_second_moment` hold with error $\varepsilon_0$. For any target subspace $F \subseteq D.\mathrm{targetForms}$ of share $\alpha = \dim F / d_T$, Cauchy–Schwarz on $S_P = \sum_s \rho(s) P P^* \rho(s)^{-1}$ and AM–GM ($\sqrt{\alpha(1+\varepsilon_0)} \le \frac{\alpha + 1 + \varepsilon_0}{2}$) yield some $s \in \bar{G}$ with $\|\mathrm{pr}_F \rho(s) P\|_{\mathrm{HS}}^2 \le \frac{\alpha + 1 + \varepsilon_0}{2} D_X$; near-isometry on the preimage $W' = (e_0 s P)^{-1}(F)$ forces
     $$\dim W' \le \Bigl(\frac{1 + \alpha}{2} + \frac{3}{2}\varepsilon_0\Bigr) D_X.$$
   - Transferring across nonzero scalars and `D.sourceFormCoords` (`comap_scaledPoincare_eq_comap_goodPoincare`) proves **`New.Data.isSpread_goodPoincare`**: `IsSpread D.goodPoincare (3 / 2 * (8 * D.b ^ 2)⁻¹)`.

### 3. Exact handoff to Stage 01 (`ShortAndEnough.lean::enough_forms`)

In `src/New/Reduction/ShortAndEnough.lean:349–392`, Stage 01 invokes `D.isSpread_goodPoincare.exists_few_le_finrank_iSup_range` (from `FewTranslates.lean:483–524`) with $\varepsilon = (8 b^2)^{-1}$ (so $16\varepsilon = 2/b^2$ and $\frac{5}{2}(1 + \log(b^2/2)) \le 5 \log(1 + 2 b^2)$):
- First, greedy covering picks $t \subseteq \bar{G}$ with $\#t \cdot \dim(\mathrm{SourceForm}) \le 5 d_T \log(1 + 2 b^2)$ whose `goodPoincare s` images span at least $(1 - 2/b^2) d_T$ dimensions.
- Second, restricting from `D.SourceForm` to `D.ramifiedSourceForms` (via `finrank_biSup_range_le`, using codimension fraction $\le f/(N-1) \le b^{-8}$ from Stage 03's `short_columns.lean`) loses at most $\#t \cdot b^{-8}\dim(\mathrm{SourceForm}) \le (2/b^2) d_T$ dimensions, yielding **`New.Data.enough_forms`**: $0 < d_T$ and $\dim\bigl(\bigsqcup_{s \in t} \operatorname{range}(D.\mathrm{normalizedPoincare}\ s)\bigr) \ge (1 - 4/b^2) d_T$.

## Level 3 — Formal Lean Contract

The extracted skeletons `skeleton/04_spanning/equal_multiplicities.lean` (17 declarations, 0 truncations) and `skeleton/04_spanning/spanning_theorem.lean` (21 declarations, 0 truncations) mirror `src/New/Spanning/Multiplicities/EqualMultiplicities.lean` and `src/New/Spanning/SecondMoment/SpanningTheorem.lean` at pinned upstream commit `41c2716658c42ba08eb0648994a2efe83082a95c`.

### Headline Theorem Signatures

From `skeleton/04_spanning/equal_multiplicities.lean` (upstream `EqualMultiplicities.lean:273–415`):

```lean
theorem New.Data.trace_heckeOnGood_eq [hs : ∀ v : D.Φ, (toHCParam s (D.paramW v)).HasGap D.gap]
    [hs' : ∀ v : D.Φ, (toHCParam s' (D.paramW v)).HasGap D.gap] (T : D.heckeAlgebra) :
    trace ℂ (D.goodForms V) (D.heckeOnGood V T) =
      trace ℂ (D.goodForms V') (D.heckeOnGood V' T)

theorem New.Data.det_rallisOnGood_eq [hs : ∀ v : D.Φ, (toHCParam s (D.paramW v)).HasGap D.gap]
    [hs' : ∀ v : D.Φ, (toHCParam s' (D.paramW v)).HasGap D.gap] (ϑ : D.TameChar) (j : ℕ) :
    (D.rallisOnGood V ϑ j).det = (D.rallisOnGood V' ϑ j).det

theorem New.Data.finrank_goodForms_inf_eigenPart_eq
    [hs : ∀ v : D.Φ, (toHCParam s (D.paramW v)).HasGap D.gap]
    [hs' : ∀ v : D.Φ, (toHCParam s' (D.paramW v)).HasGap D.gap] (q : FinPlace D.E × ℕ → ℂ) :
    finrank ℂ (D.goodForms V ⊓ D.eigenPart V q : Submodule ℂ (D.principalForms V)) =
      finrank ℂ (D.goodForms V' ⊓ D.eigenPart V' q : Submodule ℂ (D.principalForms V'))

theorem New.Data.targetDim_eq_finrank_goodForms_spaceW (j : D.LiftIndex) :
    D.targetDim = finrank ℂ (D.goodForms (D.spaceW j))
```

From `skeleton/04_spanning/spanning_theorem.lean` (upstream `SpanningTheorem.lean:91–359`):

```lean
theorem New.Data.targetDim_le_card_mul_besselDegree_mul_mass :
    D.targetDim ≤ Nat.card D.chosenGL * D.filling.besselDegree * mass D.E D.r

theorem New.Data.bad_types_rare :
    (∀ i : Fin 2, IsCentralProj (D.goodIdempotentAbove i)) ∧
    D.levelRepresentation.asAlgebraHom (piTensorElem D.goodIdempotentAbove) =
      (D.principalRep (D.spaceY 0)).asAlgebraHom D.e₀ ∧
    ∀ i : Fin 2, ((1 - D.goodIdempotentAbove i).coeff 1).re ≤ (D.b ^ 8)⁻¹ / 2

theorem New.Data.isNearIsometry_e₀_comp_scaledPoincare :
    IsNearIsometry ((D.principalRep (D.spaceY 0)).asAlgebraHom D.e₀ ∘ₗ D.scaledPoincare)
      (5 * (D.b ^ 4)⁻¹)

theorem New.Data.sum_hilbertSchmidtNorm_scaledPoincare_sq_le [Fintype D.chosenGL] :
    ∑ s : D.chosenGL,
        hilbertSchmidtNorm (D.scaledPoincare.adjoint ∘ₗ D.principalRep (D.spaceY 0) s ∘ₗ
          D.scaledPoincare) ^ 2 ≤
      (1 + 9 / D.b ^ 4) * D.filling.mainSecondMoment

theorem New.Data.isSpread_goodPoincare : IsSpread D.goodPoincare (3 / 2 * (8 * D.b ^ 2)⁻¹)
```

- **Verification status (`audit/verification-status.json`):** Local Lean compilation and kernel replay remain **`build_status: unverified-locally`** at pinned commit `41c2716658c42ba08eb0648994a2efe83082a95c` (`41c2716658c4`). Static token scanning has **`scan_scope: entry`**, covering only `Challenge.lean`, `src/Main.lean`, and `src/Statements.lean` (`0` forbidden tokens in solution files, `5` expected `sorry` tokens in `Challenge.lean`). Neither `EqualMultiplicities.lean` nor `SpanningTheorem.lean` has been locally compiled, kernel-checked, or statically scanned here.

<!-- rh:lean-node:04_spanning/equal_multiplicities -->

<!-- rh:lean-node:04_spanning/spanning_theorem -->
