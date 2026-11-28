# Crystalline Lifting, Kummer Charts, and Local Dual Defect

This page explains the two local $p$-adic Hodge landmarks of Stage 07: `src/New/Congruences/Lifting/CupOrthogonalityBounds.lean` (`07_congruences/cup_orthogonality_bounds`, 25 declarations, **0 stub truncations**) and `src/New/Congruences/LocalLoss/CrystallineDefect.lean` (`07_congruences/crystalline_defect`, 18 declarations, **0 stub truncations**)—43 declarations in total, every single one extracted completely untruncated. Return to the [Stage 07 Overview](./overview.md), or continue to [Galois Main Summands and the Anticyclotomic Selmer Twisting Theorem](./main-summands-and-twisting-theorem.md) and [First-Order Ribet Denominator Bounds and Existence of Regular Twist Families](./ribet-denominators-and-regular-twists.md).

## Level 1 — First-Principles

When Ribet's method turns a congruence modulo $\ell^M$ between lifted and non-lifted modular forms into a Galois cocycle valued in a torsion module $(O/\ell^M O)^d$, that cocycle satisfies linear relations inherited from the Galois representations of the congruent forms, yet it does not automatically lift to a *crystalline* cocycle above the {{concept:prime-number|prime}} $\ell$. (Recall from `GUIDE.md` §2 that "lifting" in `New/Congruences/Lifting/` means this local $p$-adic Hodge lifting of cocycles, not theta lifting.)

When all Hodge–Tate weights of a Galois lattice stay strictly below $\ell - 1$, classical integral $p$-adic Hodge theory guarantees zero obstruction. Inside this proof, however, the {{concept:galois-main-summand|Hodge–Tate jump windows}} of a main summand reach out to weight $b = a/x$, vastly larger than small and medium primes $\ell$. By local Tate duality, the length by which Ribet's torsion cocycles can miss true crystalline cocycles is controlled by the {{concept:crystalline-dual-defect|crystalline dual defect}} `dualDefect`: the length of the crystalline cocycles $Z_f(L^\vee(1))$ of the Tate dual modulo those orthogonal under local cup products to crystalline cocycles of congruent lattices.

![Two-panel diagram showing Left: opposite Hodge–Tate jump windows FitsWindows and Behrend-sparse b-letters along a Frobenius orbit avoiding three-in-a-row short gaps; Right: the four prime regimes converging into defectBound D l and summing across primes to 4 times 10^6 e^180 f (m/k) b log b well below D.denominatorBudget](assets/stage07-kummer-chart-defect-regimes.svg)

### Part 1: Four prime regimes for local crystalline lifting (`CupOrthogonalityBounds.lean`)

`CupOrthogonalityBounds.lean` translates Galois lattices into {{concept:kummer-chart-cup-orthogonality|Kummer chart cup orthogonality}}. A {{term:kummer-chart-cup-orthogonality|Kummer chart}} $k$ is a power-series ring $W(\mathbb{F}_{p^f})\llbracket z \rrbracket \subset A_{\mathrm{inf}}$ adapted to an open subgroup $\Gamma \le G_p$, whose finite-field Teichmüller "letters" $i \in k.\mathrm{Letter}$ cycle under the $p$-th-power Frobenius map `letterFrob : i ↦ i^p` (paralleling Stage 03's {{concept:frobenius-cycle|cyclic Frobenius orbits}}). Packaging a lattice $L$, its crystalline cocycles $Z_f(L)$, and its letter-by-letter jump sets into a {{term:kummer-chart-cup-orthogonality|pairing side}} `KummerChart.universalSide` lets Kisin-module {{term:kummer-chart-cup-orthogonality|Frobenius safe-move walks}} (`SafeMoves`, `CutExponentBounds`, `FewLowJumps`) bound `dualDefect` or produce an explicit {{term:kummer-chart-cup-orthogonality|cup exponent}} $e$ (so $p^e Z_f(L) \subseteq \mathrm{cupOrthogonal}$) across four exact regimes:

1. **Generic large primes with no small negative jump (`dualDefect_eq_zero_and_dualDefect_le` part (a)):**
   Let $k$ be unramified, $p \ge 5$, $\eta \ge 1$, $b_i + W + \eta \le (p - 3)a$, $6(a + \eta + W) < b_i$, and `NoMultipleNear p (6(a + η + W)) (b i)` ($p$ divides no nonzero integer within $6(a + \eta + W)$ of $0$ or $\pm b_i$). With $U$ and $U'$ in opposite windows $[-\eta, \eta] \cup \mp \varepsilon_i [b_i, b_i + W]$, if $U$ and $U'$ avoid $[-a, 0)$ (or $U' \subseteq [-\eta, 0]$), then **`dualDefect = 0`** identically.
2. **Few low jumps (`dualDefect_eq_zero_and_dualDefect_le` part (b) / `dualDefect_le_of_few_low_weights`):**
   Under the same generic separation, if $L$ has at most $m$ jumps in $[-\eta, \eta]$ at each embedding, then
   $$\mathrm{dualDefect}\bigl(\mathrm{InClass}\ \top\ \bot\ U,\; L\bigr) \;\le\; 11 f\bigl(m(a + 1) + \mathrm{rank}(L)\cdot (W + 1)\bigr)\cdot \mathrm{len}(O/p).$$
3. **Two-size far windows—any pattern and Behrend-sparse `b`-letters (`hasCupExponent_any_pattern`, `hasCupExponent_sparse_thick`):**
   When $p$ is smaller than the far weight $b$, the far window sits at $b$ on `b`-letters (`IsBLetter i`) and at the smaller weight $G$ elsewhere:
   - *Any pattern (`hasCupExponent_any_pattern`):* For unramified $k$, $p \ge 7$, $V + 4\eta \le M$, $0 < G \le b < K p^2$, $2G < K p$, and $M \le K$, the cup exponent is at most $10^4\bigl(K + \lfloor\log_p(2(b + M))\rfloor\bigr)$.
   - *Behrend-sparse `b`-letters (`hasCupExponent_sparse_thick`):* When $G + V \le \eta$ (so non-`b` far windows merge into $[-\eta, \eta]$) and **no three `b`-letters follow one another along `letterFrob` with both gaps in $[1, J]$**—directly consuming Stage 01's Behrend progression-free construction of `D.bPlaces`—the cup exponent drops to the rank-independent bound $10^3(\delta + (J + 1)M)$ (holding both for unramified $k$ with $p \ge 7$, $10 p^J \eta \le b$ and for possibly ramified $k$ with constant sign $\varepsilon$ and $8 e \le p^{J+1}$).
4. **Tame inertial types at the level primes $p_0, p_1$ (`IsTameType`, `hasCupExponent_of_isTameType`):**
   At the two level primes $p_0, p_1$, inertia is no longer unramified: it is killed by $X^e - 1$ with $e = \mathrm{lcm}_{1 \le i \le r}(p^i - 1) \le p^{r^2}$ (`lcm_pow_sub_one_le`) and by a monic polynomial over $\mathbb{Z}_p$ of degree $\le N$. Descending along a {{term:kummer-chart-cup-orthogonality|tame inertial chart}} `TameChart` of ramification index $e$ (`TameChart.inClass_of_pow_sub_one`, `exists_tameChart_e_eq_lcm`, with $p > b + 6(W + \eta) + r^2(N + 1)^2 + 2$) yields cup exponent $3 r^2(N + 1)^2 + 150(\eta + W + 1)$ completely independent of $e$.

### Part 2: One uniform prime-by-prime bound (`CrystallineDefect.lean`)

`LocalLoss/CrystallineDefect.lean` plugs the admissible data $D$ into those four regimes. At each embedding $\sigma$ of $E$, the {{term:crystalline-dual-defect|far jump window}} starts at `farWindowStart D i σ` $= w_i(\sigma) - 2t$ (built from $b = D.\mathrm{natB}$ on `D.bPlaces` and $D.\mathrm{offWeight}$ elsewhere) with width $4t$ and low window $[-\eta, \eta]$ ($\eta = h_2^2 + 1$, $h_2 = D.\mathrm{halfGap} = D.r^8$), so `fitsWindows_and_image_neg_sub_one` places $U$ and its Tate-dual jumps $-u - 1$ into opposite windows.

For small primes $\ell \le k t$, `notMem_bPlaces_of_coefficientEmbedding_eq_pow` turns Stage 01's `D.sparse` into the exact three-in-a-row Frobenius exclusion (`J = ⌊log_ℓ(h₂³)⌋`), and `eventually_exists_lettersReadThick` packages the chart data for every large coefficient ring $O$. Grouping the rank-$r$ main summand $L_{\mathrm{none}}$ and the $t$ inverse-cyclotomic character summands $L_{\mathrm{some}\,c}$ into a {{term:crystalline-dual-defect|summand class}} `IsSummandClass` yields the headline local theorem **`New.Selmer.normLength_dualDefect_le_of_isSummandClass`**: at every prime $\ell$,

$$\mathrm{normLength}\Bigl(\mathrm{dualDefect}\bigl(\mathrm{InClass}\ F\ K\ U,\; (L_c)^\vee(1)\bigr)\Bigr) \;\le\; [G_\ell : \Gamma]\cdot \mathrm{rank}(L_c)\cdot \frac{\mathrm{defectBound}(D, \ell)}{s},$$

where the summand divisor $s \in \{1, t\}$ equals $s = 1$ for the main summand (`c = none`) and $s = D.t$ for each character summand (`c = some c'`), with exact {{term:crystalline-dual-defect|crystalline loss}} and `defectBound` formulas

$$\mathrm{crystallineLoss}(h, b, \ell) \;=\; v_\ell\!\Bigl((2b)!\cdot \Bigl(\prod_{k \in (b - 30 h^3,\; b + 30 h^3]} k\Bigr)^{h^3}\Bigr), \qquad \mathrm{defectBound}(D, \ell) \;=\; 10^6 \cdot \frac{m}{k}\bigl(b\,v_\ell(M) + \mathrm{crystallineLoss}(h_2, b, \ell)\bigr),$$

where $h_2 = D.\mathrm{halfGap} = D.r^8$ and the lower endpoint $b - 30 h^3$ uses truncated $\mathbb{N}$-subtraction ($\max(b - 30 h^3, 0)$). When `FitTwistsExist.lean` sums `localDefectBound D ℓ = 4 f · defectBound D ℓ` weighted by $\log\ell$ across all primes, the entire local $p$-adic Hodge loss stays within $4\cdot 10^6 e^{180} f (m/k) b \log b$—dwarfed by {{concept:regular-twist-family|the denominator budget}} $D.\mathrm{denominatorBudget} = f a \log^3 b / (m k)$ in {{concept:arithmetic-height|global arithmetic height}}.

### Where this analogy breaks

Thinking of `defectBound D ℓ` as a uniform smooth function of $\ell$ obscures its discrete prime-by-prime behavior. Above $\ell > 2b$ (and $\ell \nmid M$), the $(2b)!$ factor contributes zero; the only large primes that pay *any* nonzero `crystallineLoss` are those dividing an integer inside the narrow window $(b - 30 h_2^3, b + 30 h_2^3]$ (where `NoMultipleNear` fails). Moreover, splitting $s = 1$ for the main summand versus $s = D.t$ for the $t$ characters is vital: without dividing by $D.t$ on each character summand, summing over $c \in \mathrm{Fin}\,t$ would lose a factor of $t = 5r$ on every single prime.

## Level 2 — Undergrad Bridge

### 1. All 25 declarations of `CupOrthogonalityBounds.lean` (0 truncations)

| Group | Declarations (Count) | Mathematical Role |
| --- | --- | --- |
| Tame type & side setup | `IsTameType`, `letterJumps`, `universalSide`, `mem_letterJumps`, `fitsWindows_letterJumps`, `FitsWindows.neg_add_le_of_opposite`, `le_of_mem_jumpSet_tateDualSide`, `sum_card_algHom_le_finrank`, `length_quotient_mul_length_residueField`, `InClass.sum_jump_le_finrank`, `lcm_pow_sub_one_le`, `sum_jumpMultiplicity_le`, `pow_smul_mem_cupOrthogonal_of_isScaledCupCoboundary` (13) | Defines `IsTameType` ($X^e - 1$ with $e = \mathrm{lcm}_{1 \le i \le r}(p^i - 1) \le p^{r^2}$ plus monic $\mathbb{Z}_p$-poly of degree $\le N$) and `KummerChart.universalSide`; maps embeddings to letters (`∑ #g ≤ finrank O`) and transfers opposite-window jump bounds and `IsScaledCupCoboundary` into `cupOrthogonal`. |
| Generic primes & few low jumps | `crysCocycles_le_cupOrthogonal_of_no_small_neg_jump`, `dualDefect_le_of_few_low_weights`, `dualDefect_eq_zero_and_dualDefect_le` (3) | Regime 1 (`dualDefect = 0` when $U, U'$ avoid $[-a, 0)$ or $U' \subseteq [-\eta, 0]$) and Regime 2 (`dualDefect ≤ 11 f (m(a + 1) + rank L · (W + 1)) len(O/p)`). |
| Two-size & Behrend-sparse `b`-letters | `hasCupExponent_any_pattern`, `hasCupExponent_sparse_thick` (2) | Regime 3: general exponent $10^4(K + \lfloor\log_p(2(b+M))\rfloor)$ and Behrend-sparse exponent $10^3(\delta + (J+1)M)$ excluding 3 consecutive `b`-letters with gaps in $[1, J]$. |
| Tame inertial descent | `linearFrob_comp_subtype`, `TameChart.linearFrob_eq_one`, `TameChart.inClass_of_pow_sub_one`, `exists_tameChart_e_eq_lcm`, `TameChart.differentBound_le_one`, `noMultipleNear_of_add_lt`, `hasCupExponent_of_isTameType` (7) | Regime 4: restricts `InClass F K U` to `d.subgroup` on a `TameChart` with $d.e = \mathrm{lcm}_{1 \le i \le r}(p^i - 1)$ and `d.differentBound ≤ 1`, proving `HasCupExponent` $3 r^2(N + 1)^2 + 150(\eta + W + 1)$ for $p > b + 6(W + \eta) + r^2(N + 1)^2 + 2$. |

### 2. All 18 declarations of `CrystallineDefect.lean` (0 truncations)

1. **Windows & Behrend-sparse charts (8 decls):** `farWindowStart`, `fitsWindows_and_image_neg_sub_one`, `sum_count_neg_sub_one_le`, `smul_eq_pow_of_coefficientEmbedding_eq_pow`, `complexEmbeddingOf_eq_of_coefficientEmbedding_eq`, `pow_ordProj_pow_sub_one_eq_one`, `notMem_bPlaces_of_coefficientEmbedding_eq_pow`, `eventually_exists_lettersReadThick`.
2. **Cyclotomic splitting, jump floors & arithmetic comparisons (5 decls):** `mem_range_of_modEq_one` ($\ell \equiv 1 \pmod{D.\mathrm{cyclotomicConductor}} \Rightarrow j$ surjective, used at the level primes $p_0, p_1$), `IsMainSummandWeights.le_two_mul_natAbs` ($|u| \ge h_2/2$), `two_mul_tame_exponent_le_defectBound` (absorbs $2(3 r^2((r + t)r + 1)^2 + 150(h_2^2 + 4t + 2)) \le \mathrm{defectBound}(D, p_n) / s$ using just the $b\,v_\ell(M)$ term), `crystalline_bound_le_defectBound` (absorbs $10^5(\rho\lfloor 2\lambda/(kt)\rfloor m + \lfloor \rho m/(k s)\rfloor\lambda) \le \rho\cdot \mathrm{defectBound}(D, \ell)/s$ for $s \in \{1, t\}$), and `crystallineSizes_data`.
3. **Summand classes & main local theorem (5 decls):** `IsSummandClass`, `hasFewLowJumps_tateDual_of_isMainSummandWeights` ($\le \lfloor rm/k\rfloor$ dual jumps in $[-\eta, \eta]$), `subset_Icc_of_jump_eq_single` (character dual jump $-c - 1 \in [-\eta, 0]$), `normLength_dualDefect_le_defectBound_of_isTameType` (tame branch at $p_0, p_1$), and **`normLength_dualDefect_le_of_isSummandClass`** (unified prime-by-prime theorem).

## Level 3 — Formal Lean Contract

Both extracted skeletons `skeleton/07_congruences/cup_orthogonality_bounds.lean` (**25 declarations, 0 stub truncations**) and `skeleton/07_congruences/crystalline_defect.lean` (**18 declarations, 0 stub truncations**) mirror `src/New/Congruences/Lifting/CupOrthogonalityBounds.lean` and `src/New/Congruences/LocalLoss/CrystallineDefect.lean` at pinned upstream commit `41c2716658c42ba08eb0648994a2efe83082a95c`. All 43 declarations across both files extract completely untruncated.

### Headline Signatures

```lean
-- skeleton/07_congruences/cup_orthogonality_bounds.lean (upstream CupOrthogonalityBounds.lean:346-589)
lemma New.Lift.KummerChart.dualDefect_eq_zero_and_dualDefect_le ... :
    ((∀ (g : O →ₐ[ℤ_[p]] ℂ_[p]) (u : ℤ), u ∈ U g ∨ u ∈ U' g → u ∉ Ico (-a : ℤ) 0) ∨
        (∀ g : O →ₐ[ℤ_[p]] ℂ_[p], U' g ⊆ Icc (-η : ℤ) 0) →
      dualDefect (InClass ⊤ ⊥ U) L = 0) ∧
    ∀ m : ℕ, (∀ g : O →ₐ[ℤ_[p]] ℂ_[p], ∑ u ∈ Icc (-η : ℤ) η, jump L.ρ g u ≤ m) →
      dualDefect (InClass ⊤ ⊥ U) L ≤
        (11 * (k.f * (m * (a + 1) + finrank O L * (W + 1))) : ℕ) *
          length O (O ⧸ Ideal.span {(p : O)})

lemma New.Lift.KummerChart.hasCupExponent_sparse_thick ... :
    HasCupExponent (InClass ⊤ ⊥ U) L (10 ^ 3 * (δ + (J + 1) * M))

lemma New.Lift.hasCupExponent_of_isTameType ... :
    HasCupExponent (InClass F K U) L (3 * (r ^ 2 * (N + 1) ^ 2) + 150 * (η + W + 1))

-- skeleton/07_congruences/crystalline_defect.lean (upstream CrystallineDefect.lean:519-624)
lemma New.Selmer.normLength_dualDefect_le_of_isSummandClass {P : Parameters} {ℓ : ℕ} {Γ : Type}
    {D : Data P} [hp : ThePrime ℓ] [PadicIso ℓ] [Group Γ] [𝒥 : OverGp Γ ℓ] [TopologicalSpace Γ]
    {e : D.E →+* PadicAlgCl ℓ} {r t : ℕ} {J : (D.E →+* ℂ) → Multiset ℤ}
    (hJ : IsMainSummandWeights D r t J) (h_group : IsCompletionGroup e 𝒥.toGp) :
    ∀ᶠ O : CoeffRing ℓ in Filter.atTop,
      ∀ {L : Option (Fin t) → GalLattice O Γ} {F : Filter Γ} {K : Γ → Polynomial O → Prop}
        {U : (O →ₐ[ℤ_[ℓ]] ℂ_[ℓ]) → Finset ℤ},
      IsSummandClass D r (fun g : Gp ℓ ↦ J (complexEmbeddingOf e g)) L F K U →
      ∀ c : Option (Fin t), InClass F K U (L c) →
        O.normLength (dualDefect (InClass F K U) (L c).tateDual) ≤
          𝒥.toGp.range.index *
            ↑(finrank O (L c) * (defectBound D ℓ / (if c = none then 1 else D.t : ℕ)))
```

- **Verification status (`audit/verification-status.json`):** Local Lean compilation and kernel replay remain **`build_status: unverified-locally`** at pinned commit `41c2716658c42ba08eb0648994a2efe83082a95c` (`41c2716658c4`). Static token scanning has **`scan_scope: entry`**, covering only `Challenge.lean`, `src/Main.lean`, and `src/Statements.lean`. Neither `CupOrthogonalityBounds.lean` nor `CrystallineDefect.lean` has been locally compiled, kernel-checked, or statically scanned here.

<!-- rh:lean-node:07_congruences/cup_orthogonality_bounds -->

<!-- rh:lean-node:07_congruences/crystalline_defect -->
