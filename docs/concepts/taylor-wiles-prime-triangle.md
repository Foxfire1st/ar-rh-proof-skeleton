---
slug: taylor-wiles-prime-triangle
term: Taylor–Wiles prime triangle
aliases: ["Taylor-Wiles prime triangle", "prime triangle", "even Tate-twist class", "Kummer reciprocity map", "weak-lift root chain", "tied auxiliary prime"]
inline_glimpse: "An upper-triangular family of auxiliary primes Q_j (norm 1 mod l^nu) ordered by Tate-twist exponent e in (0, t') whose Frobenius and tame-inertia Kummer reciprocity laws eliminate every even Tate-twist cohomology class algebraically—bypassing Vandiver's conjecture—while preserving weak-lift root chains."
visual_ref: assets/stage07-ribet-triangle-to-regular-family.svg
where_analogy_breaks: "Thinking of a Taylor–Wiles prime triangle as proving that the even Tate-twist cohomology groups (and Vandiver's conjecture) are small is completely backwards: Stage 07 never bounds the order of the even classes at all—instead, adding tied auxiliary primes in an upper-triangular order detects and clears even classes through polynomial tie relations whose valuations depend only on cyclotomic separation (halfGap^22 * h), never on the size of the even class group."
sources:
  - title: "Ring-theoretic properties of certain Hecke algebras"
    citation: "R. Taylor & A. Wiles, Ann. of Math. 141 (1995), 553–572."
    kind: paper
  - title: "Extensions cycliques T-ramifiées"
    citation: "G. Gras & A. Munnier, Publ. Math. Fac. Sci. Besançon (1998), 1–17."
    kind: paper
  - title: "Cutting towers of number fields"
    citation: "F. Hajir, C. Maire & R. Ramakrishna, Ann. Inst. Fourier 72 (2022), 2059–2095."
    kind: paper
lean_anchors:
  - 07_congruences/denominator_bound.lean#New.Selmer.exists_cyclotomicChar_pow_sub_one_dvd
  - 07_congruences/denominator_bound.lean#New.Selmer.exists_eventually_hasRootChain_mostFrobeniiAt
  - 07_congruences/denominator_bound.lean#New.Selmer.exists_taylorWilesPlaces_hasRootChain
  - 07_congruences/denominator_bound.lean#New.Selmer.IsWeakLift
child_concepts:
  - galois-main-summand
  - prime-number
  - hecke-operator
  - arithmetic-height
---

# Taylor–Wiles Prime Triangle

## Level 1 — First-Principles

When Stage 07 uses Ribet's method to bound congruences between lifted forms and non-lifted forms, the Galois representation of a lift mixes its {{concept:galois-main-summand|fixed main summand}} with the {{term:galois-main-summand|inverse-cyclotomic string}} $\sum_{e=0}^{t'-1} \chi_{\mathrm{cyc}}^{-e}$. Bounding extensions between two different powers of $\chi_{\mathrm{cyc}}^{-1}$ leads directly to cyclotomic Galois cohomology classes over the CM field $E$ in Tate twists $\mathcal{O}/\ell^\nu(-e)$ ($0 < e < t'$).

Under complex conjugation, those cohomology classes split into two opposite parities:
- **Odd classes (`shaOdd`):** Controlled unconditionally by the Mazur–Wiles / Greither main conjecture (`Lib/Cyclotomic/OddSha.lean::exists_length_shaOdd_le`), bounded by `oddClassBound D` $\le 2^{4(N_{\mathrm{cyc}} + t + 3)^{27}} h_E$.
- **Even Tate-twist classes (`evenClasses`):** Living on the totally real subfield $E^+$, where bounding their orders unconditionally is equivalent to **Vandiver's conjecture**—a famously open problem!

How does the proof clear congruence denominators without ever solving Vandiver's conjecture? It builds a **Taylor–Wiles prime triangle** (`New.Selmer.exists_taylorWilesPlaces_hasRootChain`): a finite linearly ordered family of **tied auxiliary primes** $Q_j$ ($j \in J$) of norm $q_j \equiv 1 \pmod{\ell^\nu}$, ordered monotonically by Tate-twist exponent $0 < e(j) < t'$, chosen via Chebotarev density (`exists_eventually_hasRootChain_mostFrobeniiAt`) to satisfy two joint properties:

1. **Upper-triangular elimination of even classes (`IsPrimeTriangle`):** Ordering the **tied auxiliary primes** by exponent $e(j)$ creates a triangular system under the **Kummer reciprocity map** over $E^+$: any **even Tate-twist class** of exponent $e(j)$ that vanishes on all earlier tame generators $\tau_i$ ($i < j$) and all earlier Frobenius elements $\varphi_i$ of the same exponent has its value at $\tau_j$ killed by a tiny explicit power $\ell^{h_2 h}$ (where $h \le v_\ell(D.\mathrm{chosenGroupOrder})$ separates the powers $u^a - 1$ of a cyclotomic generator `exists_cyclotomicChar_pow_sub_one_dvd`, and $h = 0$ whenever $\ell > h_2^2$). The size of `evenClasses` completely drops out of the denominator bound!
2. **Preservation of weak-lift root chains (`HasRootChain`):** For every system $x$ that is {{concept:ribet-clearing-operator|a system whose normalized Galois characteristic polynomials all have root $1$}} (`IsWeakLift`) congruent modulo $\ell$ to the target lift $x_0$, the rescaled {{concept:hecke-operator|Hecke polynomial}} at every auxiliary {{concept:prime-number|prime}} $Q_j$ factors as a **weak-lift root chain**
   $$\prod_{i=0}^{t'-1}\bigl(X - q_j^i\bigr) \cdot e_j(X)$$
   with an integral polynomial $e_j$ satisfying $e_j(1) = 1$ independent of $x$. This ties each tame inertia generator $\tau_j$ to $\varphi_j$ via the relation $\tau_j - 1 - X_j(\varphi_j - 1)H_j(\varphi_j)$ inside Ribet's trace ideal without disturbing any true lift.

### Limits of the intuition

Do not read the **Taylor–Wiles prime triangle** as a trick that secretly proves **even Tate-twist classes** are small. It never bounds the cardinality of `evenClasses` at all. Instead, it trades an unknown global class-group order for a finite family of auxiliary places $Q_j$ whose Chebotarev choice costs only a {{concept:arithmetic-height|valuation penalty}} $h_2^{22} h$ coming from cyclotomic character separation (`exists_cyclotomicChar_pow_sub_one_dvd`), which vanishes identically for all primes $\ell > h_2^2 = D.r^{16}$.

## Level 2 — Undergrad Bridge

In `src/New/Congruences/FirstOrder/DenominatorBound.lean` (37 declarations, 0 stub truncations), three lemmas construct the **Taylor–Wiles prime triangle** before passing it to Ribet's element:

1. **Cyclotomic separation (`exists_isUnit_cyclotomicChar_pow_sub_one`, `exists_cyclotomicChar_pow_sub_one_dvd_pow_padicValNat`, `exists_cyclotomicChar_pow_sub_one_dvd`):**
   Finds a topological generator $\gamma = g_1$ of $\operatorname{Gal}(\overline{\mathbb{Q}}/E)$ modulo $\ker(\chi_{\mathrm{cyc}})$ and an integer $h \le v_\ell(D.\mathrm{chosenGroupOrder})$ (with $h = 0$ whenever $\ell > D.\mathrm{halfGap}^2$) such that $(\chi_{\mathrm{cyc}}(g_1)^a - 1) \mid \ell^h$ in $\mathbb{Z}_\ell$ for all $0 < a < D.t$.
2. **Dense Frobenius test points (`exists_eventually_hasRootChain_mostFrobeniiAt`):**
   For any $\nu$ and $g \in \operatorname{Gal}(\overline{\mathbb{Q}}/E)$, picks $\delta \in \ker(\chi_{\mathrm{cyc}})$ so that along `mostFrobeniiAt D.E (δ ^ ℓ ^ ν * g)`, places $y.\mathrm{place}$ avoid `badPlaces D ℓ` and base level, keep $\|\det(1 - \vartheta^{-1}(\varphi) R(\varphi))\| \ge 1$, and force every **weak lift** (`IsWeakLift O x.val`) whose characteristic polynomial modulo $\ell$ factors as $S \cdot \prod_{i < t'}(X - \chi_{\mathrm{cyc}}^{-i}(\varphi))$ (with $\|S(1)\| \ge 1$) to have a rescaled Hecke polynomial with **weak-lift root chain** `HasRootChain` times a divisor of an integral $e$ with $e(1) = 1$.
3. **Assembling the triangle (`exists_taylorWilesPlaces_hasRootChain`):**
   Produces a finite linear order $J$ and monotone map `exponent : J →o {e : Fin O.rankDiff // 0 < e}` such that for every $\nu > 0$, there exist `Q : TaylorWilesPlaces D.E J (ℓ ^ ν)` (so $(Q.\mathrm{place}\ j).q \equiv 1 \pmod{\ell^\nu}$) satisfying:
   - **(a)** `IsPrimeTriangle ν Q.place tame frob g₁⁻¹ O.rankDiff exponent (D.halfGap * h) (D.halfGap ^ 22 * h)`;
   - **(b)** Every $Q.\mathrm{place}\ j$ avoids `badPlaces D ℓ` and is off the base level `B.IsOffLevel ![]`;
   - **(c)** One tuple of integral polynomials `e : J → EllAdicIntegers[X]` with `(e j).eval 1 = 1` witnesses `HasRootChain` uniformly for *every* weak lift $x$ congruent to $x_0$ modulo $\ell$.

## Level 3 — Formal Lean Contract

All four anchors extract completely untruncated in `skeleton/07_congruences/denominator_bound.lean` (37 declarations, 0 stub truncations):

- `07_congruences/denominator_bound.lean#New.Selmer.exists_cyclotomicChar_pow_sub_one_dvd` (upstream `DenominatorBound.lean:124-146`).
- `07_congruences/denominator_bound.lean#New.Selmer.IsWeakLift` (upstream `DenominatorBound.lean:148-153`).
- `07_congruences/denominator_bound.lean#New.Selmer.exists_eventually_hasRootChain_mostFrobeniiAt` (upstream `DenominatorBound.lean:209-281`).
- `07_congruences/denominator_bound.lean#New.Selmer.exists_taylorWilesPlaces_hasRootChain` (upstream `DenominatorBound.lean:283-391`):
  ```lean
  lemma New.Selmer.exists_taylorWilesPlaces_hasRootChain ... :
      ∃ (J : Type) (_ : Fintype J) (_ : LinearOrder J)
        (exponent : J →o {e : Fin O.rankDiff // 0 < (e : ℕ)}),
        ∀ ν > (0 : ℕ), ∃ Q : TaylorWilesPlaces D.E J (ℓ ^ ν),
        IsPrimeTriangle ν Q.place (fun j : J ↦ (Q.frobData j).isPlaceOver.tameGenerator)
          (fun j : J ↦ (Q.frobData j).frob) g₁⁻¹ O.rankDiff exponent (D.halfGap * h)
          (D.halfGap ^ 22 * h) ∧
        (∀ j : J, Q.place j ∉ badPlaces D ℓ) ∧
        (∀ j : J, B.IsOffLevel ![] (Q.place j)) ∧
        ∃ e : J → EllAdicIntegers[X], (∀ j : J, (e j).eval 1 = 1) ∧
          ∀ (j : J) (x : B.Eigensystem ![]),
          (∀ (g : Gal(Ω/D.E)) (k : ℕ), ‖(normalisedRep O x.val.galRep g).val.charpoly.coeff k -
            (normalisedRep O x₀.galRep g).val.charpoly.coeff k‖ < 1) →
          IsWeakLift O x.val →
          HasRootChain (x.val.heckePolyAt (Q.place j)) ((e j).map (algebraMap EllAdicIntegers ℂ))
            (Q.place j).q (ιℓ.symm (normalizingAvatar O (Q.frobData j).frob).val)⁻¹ O.rankDiff
  ```

See the page's live `<VerificationBanner />` (`audit/verification-status.json`) for the pinned upstream commit and local Lean build status (`build_status: unverified-locally`).
