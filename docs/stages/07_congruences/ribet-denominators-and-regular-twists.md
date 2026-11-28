# First-Order Ribet Denominator Bounds and Existence of Regular Twist Families

This page explains the two culminating landmarks of Stage 07—and of the entire 8-pillar proof: `src/New/Congruences/FirstOrder/DenominatorBound.lean` (`07_congruences/denominator_bound`, 37 declarations, **0 stub truncations**) — which constructs Taylor–Wiles prime triangles bypassing Vandiver's conjecture and assembles diagonal Ribet clearing operators — and `src/New/Congruences/FitTwistsExist.lean` (`07_congruences/fit_twists_exist`, 19 declarations, **1 `let`-type stub truncation** documented verbatim below, while **`New.exists_isRegularFamily`** extracts completely untruncated) — which sums all prime-by-prime budgets inside `D.denominatorBudget` and closes the final black box of [Stage 05 — Regular Twist Families and the Assembly of Theorem C](../05_determinant/regular-twists-and-theorem-c.md). Return to the [Stage 07 Overview](./overview.md), or revisit [Crystalline Lifting, Kummer Charts, and Local Dual Defect](./crystalline-lifting-and-local-defect.md) and [Galois Main Summands and the Anticyclotomic Selmer Twisting Theorem](./main-summands-and-twisting-theorem.md).

## Level 1 — First-Principles

We now hold every local and global ingredient at a {{concept:prime-number|prime}} $\ell$: uniform {{concept:crystalline-dual-defect|crystalline dual defect}} bounds from `CrystallineDefect.lean`, finitely many twist-independent {{concept:galois-main-summand|Galois main summands}} from `RowsFromEigensystems.lean`, and an $m$-fold twist $\tau$ from {{concept:anticyclotomic-selmer-twisting|anticyclotomic Selmer twisting}} (`TwistingTheorem.lean`) whose powers $\vartheta = \tau^j$ make all twisted Selmer groups and ramified {{concept:hecke-l-value-ratio|partial Hecke L-value ratios}} bounded by `twistError ℓ D`.

Two decisive steps remain to prove **`New.exists_isRegularFamily`**:
1. Convert those Selmer and local-defect bounds into an explicit linear operator $\Lambda$ clearing the $\ell$-adic denominators of orthogonal projection onto the theta-lift subspace $Z$ (`DenominatorBound.lean`)—without ever falling into Vandiver's conjecture.
2. Apply that operator to both the rank-$r$ good lifts and the rank-$1$ main-pair lines, sum every prime's budget weighted by $\log\ell$ inside `D.denominatorBudget`, and assemble the full {{concept:regular-twist-family|regular twist family}} $\theta$ (`FitTwistsExist.lean`).

![Culminating assembly diagram of Stage 07 from upper-triangular auxiliary primes eliminating even cyclotomic classes through the trace-ideal polynomial family, diagonal denominator-clearing operator, and prime-weighted budget ledger into New.exists_isRegularFamily plugging into Stage 05 LongForms.lean](assets/stage07-ribet-triangle-to-regular-family.svg)

### Part 1: Bypassing Vandiver's conjecture and clearing denominators (`DenominatorBound.lean`)

When Ribet's method analyzes congruences between the {{term:galois-main-summand|inverse-cyclotomic string}} $\sum_{e=0}^{t'-1} \chi_{\mathrm{cyc}}^{-e}$ and itself, cohomology classes appear in the Tate twists $(\mathcal{O}/\ell^\nu)(-e)$ ($0 < e < t'$) over the CM field $E$. Under complex conjugation, those classes split sharply:
- **Odd classes (`shaOdd`):** Unconditionally bounded by `Lib/Cyclotomic/OddSha.lean::exists_length_shaOdd_le` (following Mazur–Wiles 1984 and Greither 1992), giving the integer {{term:crystalline-dual-defect|odd class bound}} $\mathrm{oddClassBound}(D) \le 2^{4(N_{\mathrm{cyc}} + t + 3)^{27}} h_E$.
- **Even cyclotomic classes (`evenClasses`):** Living over the totally real subfield $E^+$. Vandiver's conjecture asserts $\ell \nmid h_{E^+}$ (vanishing of the even class group at $\ell$), so even cyclotomic classes live directly in Vandiver's territory—and Stage 07 **never bounds the order of `evenClasses`** at all.

Instead, `exists_taylorWilesPlaces_hasRootChain` builds an upper-triangular {{concept:taylor-wiles-prime-triangle|Taylor–Wiles prime triangle}} (`IsPrimeTriangle`) of auxiliary places $Q_j$ of norm $q_j \equiv 1 \pmod{\ell^\nu}$, ordered monotonically by exponent $0 < e(j) < t'$:
- Using a cyclotomic topological generator $g_1$ separating powers $u^a - 1 \mid \ell^h$ (`exists_cyclotomicChar_pow_sub_one_dvd`, with $h \le v_\ell(D.\mathrm{chosenGroupOrder})$ and $h = 0$ for $\ell > h_2^2$), the {{term:taylor-wiles-prime-triangle|Kummer reciprocity map}} over $E^+$ eliminates every {{term:taylor-wiles-prime-triangle|even Tate-twist class}} algebraically in triangular order with total separation loss $h_2^{22} h$, completely independent of the cardinality of `evenClasses`.
- Simultaneously, Chebotarev density (`exists_eventually_hasRootChain_mostFrobeniiAt`) preserves {{term:taylor-wiles-prime-triangle|weak-lift root chains}} $\prod_{i < t'}(X - q_j^i)\cdot e_j(X)$ ($e_j$ integral, $e_j(1) = 1$) at every place $Q_j$ for every {{concept:ribet-clearing-operator|weak lift}} (`IsWeakLift`) congruent modulo $\ell$ to the target lift $x_0$.

Feeding that prime triangle into the trace ideal produces a {{term:ribet-clearing-operator|common Ribet element}} (`exists_isCommonRibetElement_ellVal_le`) whose leading coefficient $d$ at $x_0$ satisfies the {{term:ribet-clearing-operator|lift valuation bound}}
$$v_\ell(N^2 d) \;\le\; \mathrm{liftValuationBound}(\ell, O, \mathrm{ram}, \mathrm{sel}) \;=\; \mathrm{sel} + 2\,O.\mathrm{rankDiff}\cdot\mathrm{ram} + (\mathrm{card}\,r)\cdot \mathrm{localDefectBound}(D, \ell) + \mathrm{denominatorError}(D, \ell),$$
where $\mathrm{localDefectBound}(D, \ell) = 4f\cdot \mathrm{defectBound}(D, \ell)$ and
$$\mathrm{denominatorError}(D, \ell) \;=\; t\,v_\ell\bigl(\mathrm{oddClassBound}(D)\bigr) + \bigl(8 f t + 2 + (4t + 1) h_2^{22} + h_2\bigr)\,v_\ell(N), \qquad N = D.\mathrm{chosenGroupOrder}.$$

Now {{concept:false-lift-period|false-lift orthogonality}} closes the linear-algebra step (`typeProjection_sub_starProjection_mem_of_isOrthogonalToFalseLifts`): for any $\iota_\ell$-integral form $y$ orthogonal to false lifts, $y - \mathrm{pr}_Z y$ has zero component on every weak lift outside $Z$. Assembling the Hecke-diagonal clearing operators across all {{term:ribet-clearing-operator|lift generators}} (`IsLiftGenerator`) spanning $Z$ builds a {{term:ribet-clearing-operator|Ribet clearing operator}} $\Lambda_0$ on $Z$ with all eigenvalue valuations $\le \mathrm{liftValuationBound}$ (`exists_operator_ellVal_le_liftValuationBound`). Rescaling $\Lambda = N\cdot \Lambda_0$ (`exists_operator_det_clearingBound`) guarantees pairing integrality against forms integral at *all* places above $\ell$—adding one $+v_\ell(N)$ term to form the exact {{term:ribet-clearing-operator|clearing bound}}
$$\mathrm{clearingBound}(D, \ell) \;=\; \mathrm{localDefectBound}(D, \ell) + (1 + 2t)\,e'_\ell + \mathrm{denominatorError}(D, \ell) + v_\ell(N)$$
and proving **`New.Selmer.denominatorLe_clearingBound`** (`DenominatorLe` with exponent $\beta = \mathrm{clearingBound}(D, \ell)\log\ell$).

### Part 2: Prime-weighted budget ledger and `New.exists_isRegularFamily` (`FitTwistsExist.lean`)

`FitTwistsExist.lean` stitches all of Stage 07 together in three final steps:

1. **Join rank-$r$ and rank-$1$ families (`exists_isMainSummand_mainPair`, `range_goodLift_le_span_hasLiftHeckePolynomial`, `exists_mFoldTwist_selmerLengthLe_inv_ratio_le`):**
   Combining the finitely many rank-$r$ main summands of `D.goodForms (D.spaceW D.m)` (from `RowsFromEigensystems.lean`) with the finitely many rank-$1$ main summands of main pairs (built from the trivial rank-$1$ representation via `isRepOfType_one`), `exists_mFoldTwist_selmerLengthLe_inv_ratio_le` chooses one $m$-fold twist $\tau$ controlling both families and all ramified $L$-value ratios simultaneously. Applying `denominatorLe_clearingBound` clears both `range (D.goodLift ϑ j)` (`denominatorLe_range_goodLift`) and every rank-$1$ main-pair line (`denominatorLe_span_trivialLift`), proving `exists_mFoldTwist_hasDenominatorsLe` at $(\ell, \iota)$ with per-prime exponent `clearingBound D ℓ * log ℓ + v_ℓ(|StdClasses_{U(1)}(D.chosenLevel)|) * log ℓ`.
2. **Sum every error budget weighted by $\log\ell$ (`sum_clearingBound_add_padicValNat_le`):**
   Over any finite set $T$ of primes $\ell$, every term in the prime-weighted sum is bounded explicitly:
   - Local crystalline defect (`sum_localDefectBound_mul_log_le`): $\le 4\cdot 10^6 e^{180} f (m/k) b \log b$;
   - Odd classes (`isNegligible_padicValNat_oddClassBound`): $\le D.x^{2000}$;
   - Twist error (`isNegligible_twistError`): $\le D.x^{999}$ (which simultaneously upgrades to $\le D.x_1$ for `HasSmallRatios` in {{concept:arithmetic-height|global arithmetic height}});
   - Denominator error (`isNegligible_denominatorError`): $\le D.x^{4000}$;
   - $U(1)$ class number (`isNegligible_padicValNat_card_stdClasses`): $\le D.x^{30}$.
   Because $a = x b$ supplies an extra factor of $x$ vastly exceeding $m^2 e^{180}$ and all polynomial powers of $x$, **`sum_clearingBound_add_padicValNat_le`** proves
   $$\sum_{\ell \in T} \Bigl(\mathrm{clearingBound}(D, \ell) + v_\ell\bigl(\bigl|\mathrm{StdClasses}_{U(1)}(D.\mathrm{chosenLevel})\bigr|\bigr)\Bigr)\log\ell \;\le\; D.\mathrm{denominatorBudget} \;=\; \frac{f a \log^3 b}{m k}.$$
3. **Sealing all 8 pillars (`New.exists_isRegularFamily`):**
   Selecting $\theta(\kappa) : D.\mathrm{MFoldTwist}$ independently for each `κ : PrimePadicIso` from `exists_mFoldTwist_hasDenominatorsLe` yields **`New.exists_isRegularFamily`**: `∃ θ : PrimePadicIso → D.MFoldTwist, D.IsRegularFamily θ`. Plugging $\theta$ directly into [Stage 05 — Regular Twist Families and the Assembly of Theorem C](../05_determinant/regular-twists-and-theorem-c.md) (`05_determinant/long_forms`, paired with [Stage 06 — Single-Lift Height](../06_one_lift/lift-volume-height.md)) discharges Theorem C (`New.Data.integral_forms_long`), which meets Theorems A and B in [Stage 01 — The Main Cancellation Inequality](../01_reduction/main-inequality.md) (`New.star_inequality`) to establish `New.grh` and all five headline theorems of [Stage 00 — The Five Entry Theorems](../00_statement/main-theorems.md).

### Where this analogy breaks

Presenting `FitTwistsExist.lean` as "sum five budgets and call `Classical.choose`" compresses the real miracle of `DenominatorBound.lean`: why the clearing operator $\Lambda$ does not depend on the unknown size of `evenClasses` or on the arbitrary number of auxiliary Taylor–Wiles primes $|J|$. In `exists_taylorWilesPlaces_hasRootChain`, the number of primes $|J|$ and test points depends only on the finite module dimensions at a fixed $\ell$, yet inside `RibetElement.lean`, the valuation bound `h₂²² * h` depends *only* on the cyclotomic separation $h \le v_\ell(N)$ and $h_2 = r^8$, remaining uniformly polynomial in $x$ across all primes $\ell$.

## Level 2 — Undergrad Bridge

### 1. Walkthrough of all 37 declarations in `DenominatorBound.lean` (0 truncations)

| Section | Declarations (Count) | Exact Mechanism |
| --- | --- | --- |
| Cyclotomic separation & lift predicates | `exists_isUnit_cyclotomicChar_pow_sub_one`, `exists_cyclotomicChar_pow_sub_one_dvd_pow_padicValNat`, `exists_cyclotomicChar_pow_sub_one_dvd`, `IsWeakLift`, `IsNonzeroIn`, `IsLiftWithTwistedLattice`, `liftValuationBound`, `invAvatar_eq_one_of_mem_inertiaAbove` (8) | Finds cyclotomic generator $g_1$ with $u^a - 1 \mid \ell^h$ ($h \le v_\ell(N)$, $h = 0$ for $\ell > h_2^2$), defines `IsWeakLift` and `liftValuationBound`, and checks `ϑ.invAvatar` kills inertia above $M$. |
| Taylor–Wiles prime triangle & non-weak-lift elimination | `exists_eventually_hasRootChain_mostFrobeniiAt`, `exists_taylorWilesPlaces_hasRootChain`, `norm_charpoly_coeff_le_one_of_continuous`, `exists_det_one_sub_ne_zero`, `exists_prod_sub_det_one_sub_eq_zero`, `exists_dvd_prime_pow_succ`, `normalizingAvatar_tameGenerator` (7) | Builds `TaylorWilesPlaces` satisfying `IsPrimeTriangle` (loss $h_2^{22} h$) and uniform `HasRootChain` for all weak lifts congruent to $x_0$, and constructs finite determinant products killing every non-weak lift. |
| Common Ribet element & diagonal base-level operator | `exists_isCommonRibetElement_ellVal_le`, `exists_ellVal_le_and_diagOp_mem_ellIntegralForms` (2) | Produces `IsCommonRibetElement` with $v_\ell(N^2 d) \le \mathrm{liftValuationBound}$, lifting to a base-level diagonal operator `B.diagOp s` keeping `N² • B.diagOp s y` $\ell$-integral whenever $l$ and $y$ agree outside non-weak lifts. |
| False-lift orthogonality & lift-generator span | `exists_heckePolyOnForms_eq_zero`, `typeProjection_sub_starProjection_mem`, `IsLiftGenerator`, `IsLiftGenerator.mono`, `span_mem_invtSubmodule_heckePolyOnForms`, `IsLiftGenerator.typeProjection_eq_zero_or_mem_range`, `IsLiftGenerator.exists_isMainSummandLike`, `span_isLiftGenerator_mem_invtSubmodule_heckePolyOnForms`, `span_isLiftGenerator_mem_invtSubmodule_typeProjection`, `span_isLiftGenerator_mem_invtSubmodule`, `typeProjection_sub_starProjection_mem_of_isOrthogonalToFalseLifts`, `mem_integralForms_ofIso_iff`, `IsLiftGenerator.exists_isMainSummandLike_hasTwistedLattice`, `diagOp_toSmoothForm_typeProjection`, `diagOp_typeProjection_eq_zero_of_mem_span` (15) | Proves the span of `IsLiftGenerator`s is invariant under `heckePolyOnForms` and `typeProjection O c`, uses `IsOrthogonalToFalseLifts` to show `typeProjection O c (y - pr_Z y)` misses all weak lifts outside $Z$, and matches `integralForms` with `BaseLevel.ellIntegralForms`. |
| Operator assembly & `DenominatorLe` | `exists_linearMap_eq_smul_mem_integralForms`, `exists_operator_ellVal_le_liftValuationBound`, `exp_le_norm_det_smul_of_ellVal_le`, `exists_operator_det_clearingBound`, `denominatorLe_clearingBound` (5) | Builds one clearing map per generator, assembles $\Lambda_0$ on $Z$, multiplies by $N = D.\mathrm{chosenGroupOrder}$ to clear pairings against forms integral at all places above $\ell$, and proves `DenominatorLe ι.ιℓ O E Z (clearingBound D ℓ * log ℓ)`. |

### 2. Walkthrough of all 19 declarations in `FitTwistsExist.lean` (1 truncation)

1. **Prime-weighted budget sums & projection-clearing bridge (11 decls):** `DenominatorLe.clearsProjection` (bridging `DenominatorLe` to `D.ClearsProjection`), `sum_padicValNat_mul_log_le`, `sum_half_pow_mul_log_le` ($\sum 2^{-l}\log l \le 2$), `prod_Ioc_natB_le_pow`, `sum_defectBound_mul_log_le`, `sum_localDefectBound_mul_log_le` ($\le 4\cdot 10^6 e^{180} f (m/k) b \log b$), `isNegligible_padicValNat_oddClassBound` ($x^{2000}$), `isNegligible_twistError` ($x^{999}$), `isNegligible_denominatorError` ($x^{4000}$), `isNegligible_padicValNat_card_stdClasses` ($x^{30}$), and **`sum_clearingBound_add_padicValNat_le`** ($\le D.\mathrm{denominatorBudget}$).
2. **Main summands & lift splitting (3 decls):** `isRepOfType_one`, `exists_isMainSummand_mainPair`, `range_goodLift_le_span_hasLiftHeckePolynomial` (proves `(D.goodLift ϑ j).range` is spanned by forms in admissible `typeProjection`s lifting good eigensystems on $W_m$).
3. **Two-family twisting, denominator clearing & final theorem (5 decls):** `exists_mFoldTwist_selmerLengthLe_inv_ratio_le` (1 `let`-type stub truncation), `denominatorLe_range_goodLift`, `denominatorLe_span_trivialLift`, `exists_mFoldTwist_hasDenominatorsLe`, and **`New.exists_isRegularFamily`** (untruncated, closing all 8 pillars).

## Level 3 — Formal Lean Contract

`skeleton/07_congruences/denominator_bound.lean` (**37 declarations, 0 stub truncations**) and `skeleton/07_congruences/fit_twists_exist.lean` (**19 declarations, 1 `let`-type stub truncation**) mirror `src/New/Congruences/FirstOrder/DenominatorBound.lean` and `src/New/Congruences/FitTwistsExist.lean` at pinned upstream commit `41c2716658c42ba08eb0648994a2efe83082a95c`. All 37 declarations in `denominator_bound.lean` and **`New.exists_isRegularFamily`** in `fit_twists_exist.lean` extract completely untruncated.

### Stub-Truncation Disclosure (1 Truncation in `fit_twists_exist.lean`; 0 in `denominator_bound.lean`)

In `skeleton/07_congruences/fit_twists_exist.lean`, **1 declaration** — `New.Selmer.exists_mFoldTwist_selmerLengthLe_inv_ratio_le` (decl starts **L183**, truncated at `let ϑ : D.Twist :=` on **L189**) — is cut at its `let ... :=` return-type binding by `rh-extract`'s depth-0 `:=` splitter. Its exact untruncated signature from pinned upstream `src/New/Congruences/FitTwistsExist.lean:382–395` is:

```lean
-- Verbatim from upstream src/New/Congruences/FitTwistsExist.lean:382-395
lemma New.Selmer.exists_mFoldTwist_selmerLengthLe_inv_ratio_le {P : Parameters} (D : Data P) {ℓ : ℕ}
    [ThePrime ℓ] (ι : PadicIso ℓ) {A B : Type} [Finite A] [Finite B]
    {R : A → GalRep ℓ D.E D.RowIndex} {R₁ : B → GalRep ℓ D.E Unit}
    (hR : ∀ a : A, ∃ (t : ℕ) (J : (D.E →+* ℂ) → Multiset ℤ), IsMainSummandLike D t J (R a))
    (hR₁ : ∀ b : B, ∃ (t : ℕ) (J : (D.E →+* ℂ) → Multiset ℤ), IsMainSummandLike D t J (R₁ b)) :
    ∃ τ : D.MFoldTwist, ∀ j : D.LiftIndex,
      let ϑ : D.Twist := τ.powTwist j
      IsAuxTwist (badPlaces D ℓ) ϑ.Q ϑ.invAvatar (twistError ℓ D) ∧
      (∀ a : A, ∃ γ : AbsGal D,
        HasDivisibleSelmerLengthLe ϑ.invAvatar (R a) ϑ.Q γ (twistError ℓ D)) ∧
      (∀ b : B, ∃ γ : AbsGal D,
        HasDivisibleSelmerLengthLe ϑ.invAvatar (R₁ b) ϑ.Q γ (twistError ℓ D)) ∧
      ∀ (p : D.CriticalPair) (s : ℤˣ), p.IsRamified →
        ‖ι.ιℓ (p.ratio ϑ s)⁻¹‖ ≤ exp (twistError ℓ D * log ℓ)
```

All 18 remaining declarations in `fit_twists_exist.lean` (including `sum_clearingBound_add_padicValNat_le`, `exists_mFoldTwist_hasDenominatorsLe`, and `New.exists_isRegularFamily`) and all 37 declarations in `denominator_bound.lean` extract completely untruncated.

### Headline Untruncated Signatures

```lean
-- skeleton/07_congruences/denominator_bound.lean (upstream DenominatorBound.lean:1269-1293)
theorem New.Selmer.denominatorLe_clearingBound {P : Parameters} {D : Data P} {ℓ : ℕ} [ThePrime ℓ]
    (ι : PadicIso ℓ) (ϑ : D.Twist)
    (hϑ : IsAuxTwist (badPlaces D ℓ) ϑ.Q ϑ.invAvatar (twistError ℓ D))
    {r n : Type} [Fintype r] [DecidableEq r] [Fintype n] [DecidableEq n] {O : DescentPair D.E r n}
    {E : MonoidAlgebra ℂ (O.ChosenGL r)} {Z : Submodule ℂ O.FormV}
    (h_span : Z ≤ span ℂ {y : O.FormV | IsLiftGenerator ϑ O E Z (twistError ℓ D) y}) :
    DenominatorLe ι.ιℓ O E Z (clearingBound D ℓ * log ℓ)

-- skeleton/07_congruences/fit_twists_exist.lean (upstream FitTwistsExist.lean:230-260, 520-543)
lemma New.Selmer.sum_clearingBound_add_padicValNat_le {P : Parameters} (D : Data P) (T : Finset ℕ)
    (hT : ∀ l ∈ T, l.Prime) :
    ∑ l ∈ T, (clearingBound D l * log l +
      padicValNat l (Nat.card (StdClasses (n := Unit) D.chosenLevel)) * log l) ≤
        D.denominatorBudget

lemma New.exists_isRegularFamily {P : Parameters} (D : Data P) :
    ∃ θ : PrimePadicIso → D.MFoldTwist, D.IsRegularFamily θ
```

- **Handoff closing all 8 pillars:** `New.exists_isRegularFamily` (`fit_twists_exist.lean:254–259`, upstream `FitTwistsExist.lean:520–543`) is consumed directly by `05_determinant/long_forms` (`New.Data.exists_hasSmallRatios_heightLe_liftDeterminant_div` in [Regular Twist Families and the Assembly of Theorem C](../05_determinant/regular-twists-and-theorem-c.md)), discharging Theorem C (`New.Data.integral_forms_long`) into [Stage 01 `main-inequality`](../01_reduction/main-inequality.md) (`New.star_inequality`) and completing the entire proof of GRH and RH in [Stage 00 `main-theorems`](../00_statement/main-theorems.md).
- **Verification status (`audit/verification-status.json`):** Local Lean compilation and kernel replay remain **`build_status: unverified-locally`** at pinned commit `41c2716658c42ba08eb0648994a2efe83082a95c` (`41c2716658c4`). Static token scanning has **`scan_scope: entry`**, covering only `Challenge.lean`, `src/Main.lean`, and `src/Statements.lean`. Neither `DenominatorBound.lean` nor `FitTwistsExist.lean` has been locally compiled, kernel-checked, or statically scanned here.

<!-- rh:lean-node:07_congruences/denominator_bound -->

<!-- rh:lean-node:07_congruences/fit_twists_exist -->
