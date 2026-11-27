---
slug: anticyclotomic-selmer-twisting
term: anticyclotomic Selmer twisting
aliases: ["divisible crystalline Selmer group", "twisting theorem", "Selmer datum", "Stickelberger system", "auxiliary twist modulus", "twist error"]
inline_glimpse: "Choosing one finite-order anticyclotomic m-fold twist tau on auxiliary places Q so that all powers xi = tau^j (1 <= j <= m) simultaneously keep ramification, divisible crystalline Selmer lengths of finitely many main summands, and all ramified Hecke L-value ratio valuations within twistError l D = (4f + 3)r v_l(chosenGroupOrder) + 2^(-l)."
visual_ref: assets/stage07-main-summand-twisting-pipeline.svg
where_analogy_breaks: "Thinking of anticyclotomic Selmer twisting as killing every Selmer group completely to zero is slightly too strong: local inertia at the chosen level primes p_0, p_1 leaves an irreducible floor bounded by (4f + 3) r v_l(D.chosenGroupOrder) (where chosenGroupOrder is the product over chosen places of |GL_{r*t}(k_w)|, not |GL_r|), plus an exponentially tiny tail 2^(-l), which is still tiny enough to be negligible when summed over all primes l."
sources:
  - title: "Diophantine stability"
    citation: "B. Mazur & K. Rubin, Amer. J. Math. 140 (2018), 571–616."
    kind: paper
  - title: "Anticyclotomic twists of Selmer groups"
    citation: "D. Kriz & A. Nordentoft, arXiv:2305.00297 (2023)."
    kind: reference
  - title: "Non-vanishing modulo p of Hecke L-values"
    citation: "H. Hida, in Geometric Aspects of Dwork Theory, Vol. II, de Gruyter (2004), 735–784."
    kind: paper
lean_anchors:
  - 07_congruences/twisting_theorem.lean#New.Selmer.IsSmallTwist.isAuxTwist
  - 07_congruences/twisting_theorem.lean#New.Selmer.SelmerDatum.TwistedLengthLe.hasDivisibleSelmerLengthLe
  - 07_congruences/twisting_theorem.lean#New.Selmer.exists_selmerData_isTorsionLevel
  - 07_congruences/twisting_theorem.lean#New.Selmer.exists_mFoldTwist_hasDivisibleSelmerLengthLe
  - 07_congruences/fit_twists_exist.lean#New.Selmer.exists_mFoldTwist_selmerLengthLe_inv_ratio_le
child_concepts:
  - galois-main-summand
  - regular-twist-family
  - hecke-l-value-ratio
  - carrier-form
  - arithmetic-height
---

# Anticyclotomic Selmer Twisting

## Level 1 — First-Principles

For an untwisted theta lift, congruences between lifted forms and non-lifted forms can be large because the Galois cohomology group governing extensions between a {{concept:galois-main-summand|fixed Galois main summand}} $R$ and the trivial character might be arbitrarily long at some prime $\ell$. Yet in {{concept:regular-twist-family|a regular twist family}}, Stage 07 gets to *choose* a fresh {{term:regular-twist-family|m-fold twist}} $\tau$ separately for each prime $\ell$ and embedding $\iota : \mathbb{C} \simeq \overline{\mathbb{Q}}_\ell$, as long as its first $m$ powers $\xi = \tau^j$ ($1 \le j \le m$) simultaneously control *both* the Galois extensions and all {{concept:hecke-l-value-ratio|ramified partial Hecke L-value ratios}}.

**Anticyclotomic Selmer twisting** (`New.Selmer.exists_mFoldTwist_hasDivisibleSelmerLengthLe`) is the horizontal twisting engine that finds such a $\tau$. Given finitely many {{term:galois-main-summand|main summands}} $R_i$ (both the rank-$r$ descent-pair summands and the rank-$1$ main-pair summands), Stage 07 packages two families of arithmetic data at the bad places $S_0$ dividing $\ell p_0 p_1 N$:

1. **A finite list of Selmer data (`SelmerDatum`):** Each **Selmer datum** fixes an integral lattice $T_i$ inside $R_i$ over a large coefficient ring $O$, unramified outside $\ell p_0 p_1$, equipped with crystalline local conditions above $\ell$ and a tame inertia generator $g_0$ at a chosen place $w_0 \nmid \ell$ acting by a scalar (`exists_selmerData_isTorsionLevel`).
2. **A finite list of Stickelberger systems (`StickelbergerSystem`):** Using Stage 06's {{concept:carrier-form|three-exchange class-sum machinery}} (imported via `Selmer/ClassSums.lean`), each ramified critical pair $p = (\nu, z)$ is interpolated by an integral twist measure whose evaluations at tame twists produce a **Stickelberger system** of group-ring elements encoding the $\ell$-adic valuations of $p.\mathrm{ratio}(\xi, \pm 1)$.

Feeding both lists into the general horizontal **twisting theorem** (`Selmer/GeneralTheorem.lean`) chooses an **auxiliary twist modulus** $Q$—a finite set of prime-norm places disjoint from $\bar Q$ and from $S_0$—and an $m$-fold anticyclotomic character $\tau$ ramified on $Q$ so that every power $\xi = \tau^j$ ($1 \le j \le m$, with Galois avatar $\psi = \xi.\mathrm{invAvatar}$) satisfies three simultaneous bounds controlled by a single explicit **twist error**:

$$e'_\ell \;=\; \mathrm{twistError}(\ell, D) \;=\; (4f + 3)\,r\,v_\ell(D.\mathrm{chosenGroupOrder}) + 2^{-\ell}.$$

Specifically:
- **Small auxiliary ramification (`IsAuxTwist`):** $\psi$ is trivial above $\ell$, its **auxiliary twist modulus** $Q$ avoids all bad places, and $\sum_{w \in Q} v_\ell(\psi(\gamma_w) - 1) \le e'_\ell$.
- **Short divisible crystalline Selmer groups (`HasDivisibleSelmerLengthLe`):** For every $R_i$, there is an inertial element $\gamma$ away from $\ell$ acting by a scalar $\zeta$ ($\zeta - 1$ a unit) such that over every large coefficient ring $O$, the **divisible crystalline Selmer group** of $\psi R_i$—cocycles valued in $\mathrm{Frac}(O)/O$, unramified outside $\ell p_0 p_1 Q \bar Q$, crystalline above $\ell$, and vanishing at $\gamma$—has normalized length at most $e'_\ell$.
- **Small $L$-value ratio valuations:** For every ramified pair $p$ and sign $s = \pm 1$, $v_\ell(\iota_\ell(p.\mathrm{ratio}(\xi, s))) \le e'_\ell$, so its inverse has $\ell$-adic norm at most $\ell^{e'_\ell}$, directly feeding `HasSmallRatios` in {{concept:arithmetic-height|global arithmetic height}} $\le x_1$.

### Limits of the intuition

Calling this "killing the Selmer group by twisting" overstates what is proved and obscures where the **twist error** comes from. The constant $c = (4f + 3) r v_\ell(D.\mathrm{chosenGroupOrder})$ is *not* zero at primes $\ell$ dividing $D.\mathrm{chosenGroupOrder}$—the product over chosen places above $p_0, p_1$ of $|\mathrm{GL}_{r\cdot t}(k_w)|$ (note the rank $r \cdot t$, not merely $r$). Those finitely many primes contribute a nonzero floor $c$, plus $2^{-\ell}$ from the general twisting approximation, yet $\sum_\ell e'_\ell \log \ell \le D.x^{999} \le D.x_1$ stays overwhelmingly negligible inside Stage 07's final budget.

## Level 2 — Undergrad Bridge

`src/New/Congruences/Selmer/TwistingTheorem.lean` (11 declarations, 1 `let`-type stub truncation) bridges `Selmer/GeneralTheorem.lean` and `Selmer/LValueSystems.lean` (which imports `06_one_lift/three_exchanges` via `Selmer/ClassSums.lean`) to `FitTwistsExist.lean`:

1. **Packaging main summands (`exists_selmerData_isTorsionLevel`):**
   Picks a chosen place $w_0 \in \mathrm{placesAboveM}\ D \setminus \mathrm{placesDividing}\ D.E\ \ell$ and its tame inertia generator $g_0$. Over a sufficiently large `O : CoeffRing ℓ`, each `R i` satisfying `IsMainSummandLike` receives a `SelmerDatum` `T i` unramified outside $\ell p_0 p_1$ with crystalline local conditions (`crysRelations`), normalized error term `O.normLength (T i).β ≤ c` where `c = (4 * D.f + 3) * D.r * padicValNat ℓ D.chosenGroupOrder`, and `IsTorsionLevel T (c + 2)` (so the residue field of $O$ has strictly more than $2\,\mathrm{rank}(T_i)\,\mathrm{len}(O/\ell^{c+2}) + 1$ elements).
2. **Transferring Selmer length bounds (`mapRel_crysRelations_subset`, `divisibleSelmerLength_le_divisibleLength`, `SelmerDatum.apply_γ_eq_one`, `SelmerDatum.TwistedLengthLe.hasDivisibleSelmerLengthLe`):**
   Because $\psi$ is trivial on the decomposition groups above $\ell$ and on $T.\gamma$, crystalline relations push forward cleanly under extension of scalars $O \le O'$ and twisting by $\psi$, turning `T.TwistedLengthLe Q ψ b` into `HasDivisibleSelmerLengthLe ψ R Q T.γ b`.
3. **Simultaneous $m$-fold twisting (`IsSmallTwist.isAuxTwist`, `four_mul_ellVal_levelPrime_sub_one_add_le_twistError`, `exists_mFoldTwist_hasDivisibleSelmerLengthLe`):**
   Applying `exists_anticyclotomic_twist` with tolerance $\varepsilon = 2^{-\ell}$ produces $\tau : D.\mathrm{MFoldTwist}$ whose first $m$ powers $\xi = \tau^j$ satisfy `IsAuxTwist`, `HasDivisibleSelmerLengthLe`, and `ellVal (ιℓ (p.ratio ξ s)) ≤ twistError ℓ D` simultaneously for all $1 \le j \le m$, all $i \in I$, and all ramified pairs $p$.
4. **Two-family wrapper (`FitTwistsExist.lean::exists_mFoldTwist_selmerLengthLe_inv_ratio_le`):**
   Takes the disjoint union $A \oplus B$ of rank-$r$ main summands $R : A \to \mathrm{GalRep}\ \ell\ D.E\ D.\mathrm{RowIndex}$ and rank-$1$ main summands $R_1 : B \to \mathrm{GalRep}\ \ell\ D.E\ \mathrm{Unit}$, invokes `exists_mFoldTwist_hasDivisibleSelmerLengthLe`, and converts `ellVal ≤ twistError` via `norm_inv_le_exp_of_ellVal_le` into `‖ι.ιℓ (p.ratio ϑ s)⁻¹‖ ≤ exp (twistError ℓ D * log ℓ)`.

## Level 3 — Formal Lean Contract

Five verified anchors span `skeleton/07_congruences/twisting_theorem.lean` (11 declarations, **1 `let`-type stub truncation**) and `skeleton/07_congruences/fit_twists_exist.lean` (**1 `let`-type stub truncation**):

- `07_congruences/twisting_theorem.lean#New.Selmer.IsSmallTwist.isAuxTwist` (upstream `TwistingTheorem.lean:118-145`).
- `07_congruences/twisting_theorem.lean#New.Selmer.SelmerDatum.TwistedLengthLe.hasDivisibleSelmerLengthLe` (upstream `TwistingTheorem.lean:189-240`).
- `07_congruences/twisting_theorem.lean#New.Selmer.exists_selmerData_isTorsionLevel` (upstream `TwistingTheorem.lean:294-345`).
- `07_congruences/twisting_theorem.lean#New.Selmer.exists_mFoldTwist_hasDivisibleSelmerLengthLe` (skeleton decl L172, truncated at `let ξ : D.Twist :=` on L177; complete conclusion from upstream `TwistingTheorem.lean:377-386`):
  ```lean
  ∃ τ : D.MFoldTwist, ∀ j : D.LiftIndex,
    let ξ : D.Twist := τ.powTwist j
    IsAuxTwist (badPlaces D ℓ) ξ.Q ξ.invAvatar (twistError ℓ D) ∧
    (∀ i : I, ∃ γ : AbsGal D,
      HasDivisibleSelmerLengthLe ξ.invAvatar (R i) ξ.Q γ (twistError ℓ D)) ∧
    ∀ (p : D.CriticalPair) (s : ℤˣ), p.IsRamified →
      ellVal (ιℓ (p.ratio ξ s)) ≤ twistError ℓ D
  ```
- `07_congruences/fit_twists_exist.lean#New.Selmer.exists_mFoldTwist_selmerLengthLe_inv_ratio_le` (skeleton decl L183, truncated at `let ϑ : D.Twist :=` on L189; complete conclusion from upstream `FitTwistsExist.lean:387-395`, quoted verbatim in [Ribet Denominators and Regular Twists](../stages/07_congruences/ribet-denominators-and-regular-twists.md)).

See the page's live `<VerificationBanner />` (`audit/verification-status.json`) for the pinned upstream commit and local Lean build status (`build_status: unverified-locally`).
