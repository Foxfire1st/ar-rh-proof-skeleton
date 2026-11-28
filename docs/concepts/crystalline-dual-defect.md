---
slug: crystalline-dual-defect
term: crystalline dual defect
aliases: ["dual defect", "crystalline loss", "local defect bound", "summand class", "far jump window", "odd class bound"]
inline_glimpse: "The normalized length of crystalline cocycles of a summand's Tate dual modulo those cup-orthogonal to congruent lattices, uniformly bounded at every prime l by [G_l : Gamma] * rank(L_c) * defectBound(D, l) / s and summing across all primes well inside D.denominatorBudget."
visual_ref: assets/stage07-kummer-chart-defect-regimes.svg
where_analogy_breaks: "Treating crystalline dual defect as a small constant correction at each prime hides why its exact formula depends on (2b)! and a product over (b - 30 h_2^3, b + 30 h_2^3] raised to h_2^3: that factorial-and-interval product automatically vanishes at all primes l > 2b (outside a narrow band near b) while summing with log l across all primes to at most 4 * 10^6 * e^180 * f * (m/k) * b * log b, still dwarfed by D.denominatorBudget = f * a * (log b)^3 / (m * k)."
sources:
  - title: "Torsion p-adic Galois representations and a conjecture of Fontaine"
    citation: "T. Liu, Ann. Sci. Éc. Norm. Supér. 40 (2007), 633–674."
    kind: paper
  - title: "On the size of crystalline cohomology"
    citation: "S. Chang & F. Diamond, J. Inst. Math. Jussieu 10 (2011), 1–19."
    kind: paper
  - title: "Class fields of abelian extensions of Q"
    citation: "B. Mazur & A. Wiles, Invent. Math. 76 (1984), 179–330."
    kind: paper
lean_anchors:
  - 07_congruences/crystalline_defect.lean#New.Selmer.IsSummandClass
  - 07_congruences/crystalline_defect.lean#New.Selmer.farWindowStart
  - 07_congruences/crystalline_defect.lean#New.Selmer.hasFewLowJumps_tateDual_of_isMainSummandWeights
  - 07_congruences/crystalline_defect.lean#New.Selmer.normLength_dualDefect_le_defectBound_of_isTameType
  - 07_congruences/crystalline_defect.lean#New.Selmer.normLength_dualDefect_le_of_isSummandClass
  - 07_congruences/fit_twists_exist.lean#New.Selmer.sum_localDefectBound_mul_log_le
  - 07_congruences/fit_twists_exist.lean#New.Selmer.sum_clearingBound_add_padicValNat_le
  - 07_congruences/fit_twists_exist.lean#New.exists_isRegularFamily
child_concepts:
  - kummer-chart-cup-orthogonality
  - galois-main-summand
  - regular-twist-family
  - arithmetic-height
  - prime-number
---

# Crystalline Dual Defect

## Level 1 — First-Principles

When Ribet's method turns a congruence modulo $\ell^M$ into a torsion Galois cocycle in one of the summands of a theta lift (either the rank-$r$ {{concept:galois-main-summand|Galois main summand}} $L_{\mathrm{none}}$ or one of the $t$ inverse-cyclotomic character summands $L_{\mathrm{some}\,c}$), that torsion cocycle may miss the reductions of true crystalline cocycles above $\ell$. By local duality, the length by which it can miss—its **crystalline dual defect** (`dualDefect`)—is bounded by the normalized length of crystalline cocycles of the Tate dual $(L_c)^\vee(1)$ modulo those cup-orthogonal to the class of congruent lattices.

`LocalLoss/CrystallineDefect.lean` binds the four abstract prime regimes of {{concept:kummer-chart-cup-orthogonality|Kummer chart cup orthogonality}} to the actual weights and level primes of the admissible data $D$. At each complex embedding $\sigma$ of $E$, the **far jump window** begins at $S = \mathrm{farWindowStart}(D, i, \sigma) = w_i(\sigma) - 2t$ (anchored at $b = D.\mathrm{natB}$ on `D.bPlaces` and $D.\mathrm{offWeight}$ elsewhere) and has width $4t$, while low jumps sit in $[-\eta, \eta]$ with $\eta = h_2^2 + 1$ ($h_2 = D.\mathrm{halfGap} = D.r^8$). Packaging all summands $L_c$ into a **summand class** (`IsSummandClass`) lets Stage 07 prove one uniform prime-by-prime bound (`New.Selmer.normLength_dualDefect_le_of_isSummandClass`):

$$\mathrm{normLength}\Bigl(\mathrm{dualDefect}\bigl(\mathrm{InClass}\ F\ K\ U,\; L_c^\vee(1)\bigr)\Bigr) \;\le\; [G_\ell : \Gamma]\cdot \mathrm{rank}(L_c)\cdot \frac{\mathrm{defectBound}(D, \ell)}{s},$$

where the summand divisor $s \in \{1, D.t\}$ equals $1$ for the main summand and $D.t$ for each of the $t$ character summands (so summing across all $t$ characters contributes at most one more copy of `defectBound`!). Here the **crystalline loss** and `defectBound` are given explicitly by

$$\mathrm{crystallineLoss}(h, b, \ell) \;=\; v_\ell\!\Bigl((2b)!\cdot \Bigl(\prod_{k \in (b - 30 h^3,\; b + 30 h^3]} k\Bigr)^{h^3}\Bigr), \qquad \mathrm{defectBound}(D, \ell) \;=\; 10^6 \cdot \frac{m}{k}\bigl(b\,v_\ell(M) + \mathrm{crystallineLoss}(h_2, b, \ell)\bigr)$$

(using truncated $\mathbb{N}$-subtraction on $b - 30 h^3$). Multiplying by $4f$ gives the **local defect bound** $\mathrm{localDefectBound}(D, \ell) = 4f\cdot \mathrm{defectBound}(D, \ell)$.

Why does this single formula seal the entire proof? When `FitTwistsExist.lean` sums the $\log\ell$-weighted error terms over any finite set $T$ of {{concept:prime-number|primes}} (`sum_localDefectBound_mul_log_le`), $\sum_{\ell \in T} \mathrm{localDefectBound}(D, \ell)\log\ell \le 4\cdot 10^6 e^{180} f (m/k) b \log b$. Combining that with the negligible sums from the **odd class bound** ($\le x^{2000}$), {{concept:anticyclotomic-selmer-twisting|simultaneous $m$-fold Selmer twisting tolerance}} $e'_\ell$ ($\le x^{999}$), denominator error ($\le x^{4000}$), and `StdClasses` ($\le x^{30}$) fits strictly inside {{concept:regular-twist-family|the denominator budget}} $D.\mathrm{denominatorBudget} = f a \log^3 b / (m k)$ (`sum_clearingBound_add_padicValNat_le`)—because $a = xb$ carries an extra factor of $x \gg m^2 e^{180}$—proving **`New.exists_isRegularFamily`** and closing the final black box of the entire proof.

### Limits of the intuition

Notice why the **crystalline loss** has two distinct pieces inside $v_\ell(\cdots)$: $(2b)!$ pays for all small and medium primes $\ell \le 2b$ (where generic weight separation fails and the two-size/Behrend-sparse Kummer chart bounds apply), while $\bigl(\prod_{(b - 30h_2^3, b + 30h_2^3]} k\bigr)^{h_2^3}$ pays for the finitely many large primes $\ell > 2b$ that happen to divide an integer close to $b$ (violating `NoMultipleNear`). At every other large prime $\ell > 2b$ not dividing $M$, $\mathrm{defectBound}(D, \ell) = 0$ identically!

## Level 2 — Undergrad Bridge

`src/New/Congruences/LocalLoss/CrystallineDefect.lean` (18 declarations, 0 stub truncations) and `src/New/Congruences/FitTwistsExist.lean` (19 declarations, 1 `let`-type stub truncation) bridge local $p$-adic Hodge bounds into the global prime sum:

1. **Binding windows and Behrend sparsity (`farWindowStart`, `fitsWindows_and_image_neg_sub_one`, `notMem_bPlaces_of_coefficientEmbedding_eq_pow`, `eventually_exists_lettersReadThick`):**
   Verifies that `U` and its dual `-u - 1` sit in opposite windows `[-η, η] ∪ ∓ cmSign σ [S, S + 4t]` (`η = h₂² + 1`, `S = farWindowStart D i σ = w_i - 2t`), proves `notMem_bPlaces_of_coefficientEmbedding_eq_pow` (for $\ell \le kt$, no three `b`-places sit at Frobenius gaps $a, b \in [1, \lfloor\log_\ell(h_2^3)\rfloor]$), and builds the chart `LettersReadThick` over all large `O : CoeffRing ℓ`.
2. **Main-summand vs. character bounds (`hasFewLowJumps_tateDual_of_isMainSummandWeights`, `subset_Icc_of_jump_eq_single`, `two_mul_tame_exponent_le_defectBound`, `crystalline_bound_le_defectBound`):**
   - For the main summand ($s = 1$), its Tate dual has at most $\lfloor rm/k \rfloor$ jumps in $[-\eta, \eta]$, and `crystalline_bound_le_defectBound` compares `normLength_dualDefect_le_of_crystallineSizes` to $\rho \cdot \mathrm{defectBound}(D, \ell)$.
   - For each character summand ($s = D.t$), its single dual jump $-c - 1$ lies in $[-\eta, 0]$, yielding exact vanishing at generic primes and a factor $1/D.t$ in `defectBound / D.t`.
   - At the two level primes $p_0, p_1$, `two_mul_tame_exponent_le_defectBound` and `normLength_dualDefect_le_defectBound_of_isTameType` absorb the tame cup exponent $2(3r^2((r+t)r + 1)^2 + 150(h_2^2 + 4t + 2))$ cleanly into $10^6 (m/k) b v_\ell(M) / s$.
3. **Uniform prime-by-prime theorem (`normLength_dualDefect_le_of_isSummandClass`):**
   Combines the tame and crystalline branches across every prime $\ell$ and every summand `c : Option (Fin t)` of `IsSummandClass`.
4. **Global prime-weighted summation (`sum_defectBound_mul_log_le`, `sum_localDefectBound_mul_log_le`, `sum_clearingBound_add_padicValNat_le`, `New.exists_isRegularFamily`):**
   In `FitTwistsExist.lean`, `prod_Ioc_natB_le_pow` bounds $\prod_{(b - 30h_2^3, b + 30h_2^3]} k \le (b + 30h_2^3)^{60h_2^3}$, yielding $\sum_{\ell \in T} \mathrm{localDefectBound}(D, \ell)\log\ell \le 4\cdot 10^6 e^{180} f (m/k) b \log b$. Adding the four negligible budgets (`oddClassBound` $x^{2000}$, `twistError` $x^{999}$, `denominatorError` $x^{4000}$, `StdClasses` $x^{30}$) fits strictly inside `D.denominatorBudget` (`sum_clearingBound_add_padicValNat_le`), establishing `New.exists_isRegularFamily` via the {{concept:arithmetic-height|global height sum}}.

## Level 3 — Formal Lean Contract

All eight anchors extract completely untruncated across `skeleton/07_congruences/crystalline_defect.lean` (18 declarations, 0 stub truncations) and `skeleton/07_congruences/fit_twists_exist.lean`:

- `07_congruences/crystalline_defect.lean#New.Selmer.farWindowStart` (upstream `CrystallineDefect.lean:52-59`), `#New.Selmer.IsSummandClass` (`CrystallineDefect.lean:383-406`), `#New.Selmer.hasFewLowJumps_tateDual_of_isMainSummandWeights` (`CrystallineDefect.lean:420-452`), `#New.Selmer.normLength_dualDefect_le_defectBound_of_isTameType` (`CrystallineDefect.lean:467-517`), and `#New.Selmer.normLength_dualDefect_le_of_isSummandClass` (`CrystallineDefect.lean:519-624`):
  ```lean
  lemma New.Selmer.normLength_dualDefect_le_of_isSummandClass ... :
      ∀ᶠ O : CoeffRing ℓ in Filter.atTop,
        ∀ {L : Option (Fin t) → GalLattice O Γ} {F : Filter Γ} {K : Γ → Polynomial O → Prop}
          {U : (O →ₐ[ℤ_[ℓ]] ℂ_[ℓ]) → Finset ℤ},
        IsSummandClass D r (fun g : Gp ℓ ↦ J (complexEmbeddingOf e g)) L F K U →
        ∀ c : Option (Fin t), InClass F K U (L c) →
          O.normLength (dualDefect (InClass F K U) (L c).tateDual) ≤
            𝒥.toGp.range.index *
              ↑(finrank O (L c) * (defectBound D ℓ / (if c = none then 1 else D.t : ℕ)))
  ```
- `07_congruences/fit_twists_exist.lean#New.Selmer.sum_localDefectBound_mul_log_le` (upstream `FitTwistsExist.lean:136-155`), `#New.Selmer.sum_clearingBound_add_padicValNat_le` (`FitTwistsExist.lean:230-260`), and `#New.exists_isRegularFamily` (`FitTwistsExist.lean:520-543`, untruncated at `fit_twists_exist.lean:254-259`):
  ```lean
  lemma New.exists_isRegularFamily {P : Parameters} (D : Data P) :
      ∃ θ : PrimePadicIso → D.MFoldTwist, D.IsRegularFamily θ
  ```

See the page's live `<VerificationBanner />` (`audit/verification-status.json`) for the pinned upstream commit and local Lean build status (`build_status: unverified-locally`).
