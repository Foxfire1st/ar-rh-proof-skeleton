# Galois Main Summands and the Anticyclotomic Selmer Twisting Theorem

This page explains the two representation-theoretic and twisting landmarks of Stage 07: `src/New/Congruences/GaloisRepresentations/RowsFromEigensystems.lean` (`07_congruences/rows_from_eigensystems`, 15 declarations, **0 stub truncations**) — which extracts fixed, twist-independent Galois main summands from Hecke eigensystems of theta lifts — and `src/New/Congruences/Selmer/TwistingTheorem.lean` (`07_congruences/twisting_theorem`, 11 declarations, **1 `let`-type stub truncation** documented verbatim below) — which chooses one $m$-fold anticyclotomic twist $\tau$ simultaneously keeping auxiliary ramification, divisible crystalline Selmer lengths, and ramified Hecke $L$-value ratio valuations inside `twistError ℓ D`. Return to the [Stage 07 Overview](./overview.md), revisit [Crystalline Lifting, Kummer Charts, and Local Dual Defect](./crystalline-lifting-and-local-defect.md), or proceed to [First-Order Ribet Denominator Bounds and Existence of Regular Twist Families](./ribet-denominators-and-regular-twists.md).

## Level 1 — First-Principles

Before Stage 07 can clear congruence denominators by Ribet's method, it must solve two global problems at each {{concept:prime-number|prime}} $\ell$ and embedding $\iota : \mathbb{C} \simeq \overline{\mathbb{Q}}_\ell$:
1. Identify the *finitely many* twist-independent Galois representations whose Selmer groups govern all congruences of theta lifts across every twist $\vartheta$.
2. Find a single {{concept:regular-twist-family|m-fold twist}} $\tau$ whose first $m$ powers $\xi = \tau^j$ ($1 \le j \le m$) simultaneously make *all* of those Selmer groups short **and** keep *all* {{concept:hecke-l-value-ratio|ramified partial Hecke L-value ratios}} barely divisible by $\ell$.

![Pipeline diagram showing how good Hecke eigenforms on W_m (rank r) and rank-1 critical pairs produce finitely many twist-independent Galois blocks R_a and R_{1,b}, paired with Stage 06 integral twist measures from three_exchanges via ClassSums inside exists_mFoldTwist_hasDivisibleSelmerLengthLe to choose one m-fold twist tau bounded by twistError](assets/stage07-main-summand-twisting-pipeline.svg)

### Part 1: Extracting twist-independent Galois blocks (`RowsFromEigensystems.lean`)

Take any good Hecke eigenform on the rank-$r$ space $W_m$ (inside the image of the {{concept:central-idempotent|good finite-field idempotent}} $e_0$) with {{concept:hecke-operator|split-place Hecke eigenvalues}} $b(w, i)$, and consider its {{concept:theta-correspondence|theta lift}} $y$ on the rank-$(r + t)$ target space $V_j$ twisted by an arbitrary tame character $\vartheta$. Because the unramified theta correspondence multiplies the source Hecke polynomial by a $q$-Pochhammer factor $\prod_{e=0}^{t-1}(1 - q^e \alpha X)$, comparing linear terms (`apply_one_eq_of_heckePolyEval_eq`, `neg_coeff_eq_of_hasLiftHeckePolynomial`) and passing to Frobenius elements along `mostFrobenii D.E` proves the exact {{concept:galois-main-summand|lift trace decomposition}} (`eventually_trace_frob_eq_of_hasLiftHeckePolynomial`):

$$-\mathrm{normalizingAvatar}(V, \varphi)\,\iota_\ell\bigl(H_w.\mathrm{coeff}(n - 1)\bigr) \;=\; \psi_\vartheta(\varphi)\cdot \operatorname{tr}\bigl(T(\varphi)\bigr) \;+\; \sum_{e=0}^{t-1} \chi_{\mathrm{cyc}}^{-e}(\varphi).$$

Here $\psi_\vartheta$ is the $\ell$-adic Galois avatar of $\vartheta^{-1}$, $\sum_{e=0}^{t-1}\chi_{\mathrm{cyc}}^{-e}$ is the {{term:galois-main-summand|inverse-cyclotomic string}}, and $T$ is the {{term:galois-main-summand|Galois main summand}} (`IsMainSummand O b T`)—completely independent of $\vartheta$.

`RowsFromEigensystems.lean` also verifies that the Hodge–Tate jumps of both rank-$r$ descent pairs and rank-$1$ main pairs satisfy the exact separation constraints `IsMainSummandWeights` required by `CrystallineDefect.lean`:
- **Rank-$r$ descent pairs (`isMainSummandWeights_descentPair`):** For each $1 \le j \le m$, the $r$ jumps $u_q = \lfloor t/2\rfloor - \varepsilon(\lambda_q + w_j)$ split into $k - 1$ high jumps ($|\varepsilon u_q + w_j| \le t$) and exactly $m$ low jumps ($\Delta/4 \le |u_q| \le 2m\Delta$).
- **Rank-$1$ main pairs (`isMainSummandWeights_mainPair`):** For each main pair $p = (\nu, z)$ ($m < z \le r$, $w_j + r - t \le c \le w_j$), the single jump $z - 1 - \varepsilon c$ lies within $t$ of $-\varepsilon w_j$ and obeys `IsMainSummandWeights D 1 (2 * p.z - 1)`.

Attaching a conjugate self-dual Galois representation $R$ to each good eigensystem on $W_m$ (under an admissible {{term:galois-main-summand|lift setting}} `IsLiftSetting`), twisting by the algebraic character $\mu^{-1}(x/\bar x)|x|^a$ (`isMainSummandLike_twistByChar`), and verifying Frobenius trace limits (`exists_isMainSummand_of_isRepOfType`) proves **`New.Selmer.exists_isMainSummand_descentPair`**. Since `D.goodForms (D.spaceW D.m)` is finite-dimensional, only finitely many rank-$r$ main summands occur across all good lifts.

### Part 2: Simultaneous $m$-fold Selmer and $L$-value twisting (`TwistingTheorem.lean`)

Next, `Selmer/TwistingTheorem.lean` feeds finitely many {{term:galois-main-summand|main summands}} $R_i$ into {{concept:anticyclotomic-selmer-twisting|anticyclotomic Selmer twisting}} via two paired inputs:
1. **Selmer data (`exists_selmerData_isTorsionLevel`):** Choosing a chosen place $w_0 \nmid \ell$ above $M = p_0 p_1$ and its tame inertia generator $g_0$, every $R_i$ admits a {{term:anticyclotomic-selmer-twisting|Selmer datum}} $T_i$ over a large coefficient ring $O$, unramified outside $\ell p_0 p_1$, with crystalline local conditions above $\ell$, normalized error $\beta(T_i) \le c = (4f + 3) r v_\ell(D.\mathrm{chosenGroupOrder})$, and residue field large enough to satisfy `IsTorsionLevel T (c + 2)`.
2. **Stickelberger systems from Stage 06 (`Selmer/ClassSums.lean` + `Selmer/LValueSystems.lean`):** Importing [Stage 06 — Three Exchanges and Gluing](../06_one_lift/three-exchanges-and-gluing-maps.md) (`06_one_lift/three_exchanges`) via `Selmer/ClassSums.lean`, Stage 07 constructs integral twist measures (`IsTwistMeasure`) interpolating every ramified pair's $L$-value ratio $p.\mathrm{ratio}(\xi, \pm 1)$ into a {{term:anticyclotomic-selmer-twisting|Stickelberger system}} of group-ring elements.

Invoking the horizontal twisting theorem (`Selmer/GeneralTheorem.lean::exists_anticyclotomic_twist`) with tolerance $2^{-\ell}$ proves **`New.Selmer.exists_mFoldTwist_hasDivisibleSelmerLengthLe`**: there exists one $m$-fold twist $\tau : D.\mathrm{MFoldTwist}$ whose powers $\xi = \tau^j$ ($1 \le j \le m$) simultaneously satisfy:
- **Auxiliary ramification (`IsSmallTwist.isAuxTwist`):** $Q = \xi.Q$ has prime-norm places disjoint from $\bar Q$ and `badPlaces D ℓ` (places dividing $\ell p_0 p_1 N$), $\psi = \xi.\mathrm{invAvatar}$ is anticyclotomic and trivial above $\ell$, and $\sum_{w \in Q} v_\ell(\psi(\gamma_w) - 1) \le e'_\ell$;
- **Short {{term:anticyclotomic-selmer-twisting|divisible crystalline Selmer groups}} (`HasDivisibleSelmerLengthLe`):** Every $\psi R_i$ has a lattice with an inertial scalar element $\gamma$ whose crystalline Selmer group with coefficients in $\mathrm{Frac}(O)/O$ (unramified outside $\ell p_0 p_1 Q \bar Q$) has normalized length at most $e'_\ell$;
- **Small $L$-value ratio valuations:** For every ramified pair $p$ and sign $s = \pm 1$, $v_\ell(\iota_\ell(p.\mathrm{ratio}(\xi, s))) \le e'_\ell$ (so by `norm_inv_le_exp_of_ellVal_le`, its inverse has norm $\le \ell^{e'_\ell}$, contributing directly to {{concept:arithmetic-height|global arithmetic height}} $\le x_1$).

Here the explicit **twist error** is

$$e'_\ell \;=\; \mathrm{twistError}(\ell, D) \;=\; (4 D.f + 3)\,D.r\,v_\ell(D.\mathrm{chosenGroupOrder}) + 2^{-\ell}.$$

Notice an important detail in that formula: $D.\mathrm{chosenGroupOrder}$ is the product over the chosen places above $p_0, p_1$ of $|\mathrm{GL}_{r\cdot t}(k_w)|$ (rank $r \cdot t$, **not** $|\mathrm{GL}_r(k_w)|$). Because $D.\mathrm{chosenGroupOrder}$ is a fixed integer depending only on $D$, summing $e'_\ell \log\ell$ across all primes stays bounded by $D.x^{999}$ (`isNegligible_twistError` in `FitTwistsExist.lean`).

### Where this analogy breaks

Thinking of `RowsFromEigensystems.lean` and `TwistingTheorem.lean` as "take one Galois representation, twist it once, and shrink its Selmer group" omits two crucial constraints. First, a *single* $m$-fold twist $\tau$ must work simultaneously for *all* $m$ powers $\xi = \tau^1, \dots, \tau^m$, for *all* finitely many main summands $R_i$ (both rank $r$ and rank $1$), and for *all* ramified critical pairs $p$ with both signs $s = \pm 1$. Second, `SelmerDatum.TwistedLengthLe.hasDivisibleSelmerLengthLe` only works because the auxiliary twist character $\psi$ is trivial on every decomposition group above $\ell$ (`mapRel_crysRelations_subset`) and on the tame inertia generator $T.\gamma$ (`SelmerDatum.apply_γ_eq_one`), preserving crystalline conditions unchanged under twisting.

## Level 2 — Undergrad Bridge

### 1. All 15 declarations of `RowsFromEigensystems.lean` (0 truncations)

| Group | Declarations (Count) | Mathematical Role |
| --- | --- | --- |
| Jump-window checks | `cmSign_star`, `abs_descentPair_weight_add_liftWeight_le`, `le_natAbs_descentPair_jump_and_le`, `countP_low_weights_descentPair`, `isMainSummandWeights_descentPair`, `abs_mainPair_weight_add_liftWeight_le`, `isMainSummandWeights_mainPair` (7) | Verifies `IsMainSummandWeights D D.r D.t` for the $j$-th descent pair ($k - 1$ high jumps within $t$ of $-\varepsilon w_j$, exactly $m$ low jumps in $[\Delta/4, 2m\Delta]$) and `IsMainSummandWeights D 1 (2 * p.z - 1)` for every main pair $p$. |
| Hecke-polynomial & trace factorization | `apply_one_eq_of_heckePolyEval_eq`, `neg_coeff_eq_of_hasLiftHeckePolynomial`, `eventually_trace_frob_eq_of_hasLiftHeckePolynomial` (3) | Converts `HasLiftHeckePolynomial` into the linear-coefficient identity $-H_w.\mathrm{coeff}(n - 1) = q^t(\nu/\nu') b(w, 1) + (\sum_{i < t} q^i)\nu$ and then into the $\ell$-adic Frobenius trace identity along `mostFrobenii D.E`. |
| Main summand construction | `IsRepOfType.isMainSummandLike_twistByChar`, `exists_isMainSummand_of_isRepOfType`, `map_jumps_sub_eq_map_targetParameter`, `Data.isEigenform_of_hasHeckePolyAt`, `exists_isMainSummand_descentPair` (5) | Twists an attached `IsRepOfType` representation by $\mu^{-1}(x/\bar x)|x|^a$ ($r - t = 2a$), matches Harish-Chandra jumps to `targetParameter`, and constructs `IsMainSummand (D.descentPair 1 j) p T` for every good eigensystem on $W_m$. |

### 2. All 11 declarations of `TwistingTheorem.lean` (1 truncation)

1. **Valuation & crystalline pushforward (5 decls):** `norm_inv_le_exp_of_ellVal_le` ($v_\ell(\iota_\ell x) \le c \Rightarrow \|\iota_\ell x^{-1}\| \le \exp(c \log \ell)$), `mapRel_crysRelations_subset`, `IsSmallTwist.isAuxTwist`, `divisibleSelmerLength_le_divisibleLength`, `SelmerDatum.apply_γ_eq_one`.
2. **Selmer length transfer & error bounds (4 decls):** `SelmerDatum.TwistedLengthLe.hasDivisibleSelmerLengthLe`, `four_mul_ellVal_levelPrime_sub_one_add_le_twistError` ($4 v_\ell(p_i - 1) + 2^{-\ell} \le \mathrm{twistError}\ \ell\ D$), `invAvatar_pow`, `tamePart_ne_one_of_invAvatar_ne_one`.
3. **Packaging & horizontal twisting theorem (2 decls):** `exists_selmerData_isTorsionLevel` (builds `SelmerDatum` family over `O` with `IsTorsionLevel T (c + 2)`) and **`exists_mFoldTwist_hasDivisibleSelmerLengthLe`** (combining `Selmer/GeneralTheorem.lean` with `Selmer/LValueSystems.lean` / `Selmer/ClassSums.lean` imported from `06_one_lift/three_exchanges`).

## Level 3 — Formal Lean Contract

`skeleton/07_congruences/rows_from_eigensystems.lean` (**15 declarations, 0 stub truncations**) and `skeleton/07_congruences/twisting_theorem.lean` (**11 declarations, 1 `let`-type stub truncation**) mirror `src/New/Congruences/GaloisRepresentations/RowsFromEigensystems.lean` and `src/New/Congruences/Selmer/TwistingTheorem.lean` at pinned upstream commit `41c2716658c42ba08eb0648994a2efe83082a95c`. All 15 declarations in `rows_from_eigensystems.lean` extract completely untruncated.

### Stub-Truncation Disclosure (1 Truncation in `twisting_theorem.lean`; 0 in `rows_from_eigensystems.lean`)

In `skeleton/07_congruences/twisting_theorem.lean`, **1 declaration** — `New.Selmer.exists_mFoldTwist_hasDivisibleSelmerLengthLe` (decl starts **L172**, truncated at `let ξ : D.Twist :=` on **L177**) — is cut at its `let ... :=` return-type binding by `rh-extract`'s depth-0 `:=` splitter. Its exact untruncated signature from pinned upstream `src/New/Congruences/Selmer/TwistingTheorem.lean:373–386` is:

```lean
-- Verbatim from upstream src/New/Congruences/Selmer/TwistingTheorem.lean:373-386
theorem exists_mFoldTwist_hasDivisibleSelmerLengthLe {P : Parameters} (D : Data P) (ℓ : ℕ)
    [ThePrime ℓ] (ι : PadicIso ℓ) {I : Type} [Finite I] {r : I → Type} [∀ i : I, Fintype (r i)]
    [∀ i : I, DecidableEq (r i)] {R : ∀ i : I, GalRep ℓ D.E (r i)}
    (hR : ∀ i : I, ∃ (t : ℕ) (J : (D.E →+* ℂ) → Multiset ℤ), IsMainSummandLike D t J (R i)) :
    ∃ τ : D.MFoldTwist, ∀ j : D.LiftIndex,
      let ξ : D.Twist := τ.powTwist j
      -- The ramification is auxiliary and small.
      IsAuxTwist (badPlaces D ℓ) ξ.Q ξ.invAvatar (twistError ℓ D) ∧
      -- The Selmer groups of the twisted representations are short.
      (∀ i : I, ∃ γ : AbsGal D,
        HasDivisibleSelmerLengthLe ξ.invAvatar (R i) ξ.Q γ (twistError ℓ D)) ∧
      -- The ratios of `L`-values have small valuation.
      ∀ (p : D.CriticalPair) (s : ℤˣ), p.IsRamified →
        ellVal (ιℓ (p.ratio ξ s)) ≤ twistError ℓ D
```

All 10 remaining declarations in `twisting_theorem.lean` and all 15 declarations in `rows_from_eigensystems.lean` extract completely untruncated.

### Headline Untruncated Signatures (`rows_from_eigensystems.lean`)

```lean
-- skeleton/07_congruences/rows_from_eigensystems.lean (upstream RowsFromEigensystems.lean:476-561)
theorem New.Selmer.exists_isMainSummand_descentPair {ℓ : ℕ} [ThePrime ℓ] (ι : PadicIso ℓ)
    {p : FinPlace D.E × ℕ → ℂ}
    (h_ne_bot : D.goodForms (D.spaceW D.m) ⊓ D.eigenPart (D.spaceW D.m) p ≠ ⊥) (j : D.LiftIndex) :
    ∃ T : GalRep ℓ D.E D.RowIndex,
      IsMainSummand (D.descentPair TameChar.one j) (fun (w : FinPlace D.E) (i : ℕ) ↦ p (w, i))
        T
```

- **Verification status (`audit/verification-status.json`):** Local Lean compilation and kernel replay remain **`build_status: unverified-locally`** at pinned commit `41c2716658c42ba08eb0648994a2efe83082a95c` (`41c2716658c4`). Static token scanning has **`scan_scope: entry`**, covering only `Challenge.lean`, `src/Main.lean`, and `src/Statements.lean`. Neither `RowsFromEigensystems.lean` nor `TwistingTheorem.lean` has been locally compiled, kernel-checked, or statically scanned here.

<!-- rh:lean-node:07_congruences/rows_from_eigensystems -->

<!-- rh:lean-node:07_congruences/twisting_theorem -->
