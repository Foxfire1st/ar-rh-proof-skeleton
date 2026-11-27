---
slug: regular-twist-family
term: regular twist family
aliases: ["regular family of twists", "m-fold twist", "small ratios", "denominator budget", "projection clearing"]
inline_glimpse: "A prime-by-prime family of m-fold anticyclotomic twists theta(iota) with modulus coprime to l, inverse L-value ratios of height at most x_1, and lift/projection denominators summing within f*a*(log b)^3 / (m*k)."
visual_ref: assets/stage05-theorem-c-blueprint.svg
where_analogy_breaks: "Picturing a regular twist family as picking one good character twist that works at all primes simultaneously is false: each prime embedding iota : C -> Qbar_l gets its own m-fold twist theta(iota) whose modulus Q(theta(iota)) is coprime to l, and the global arithmetic height bound stitches those per-prime choices together only because the clearing matrix in TwistedLattice is twist-independent while the denominator budget sums across all primes l."
sources:
  - title: "p-adic Automorphic Forms on Shimura Varieties"
    citation: "H. Hida, Springer Monographs in Mathematics, Springer (2004)."
    kind: book
  - title: "Class fields of abelian extensions of Q"
    citation: "B. Mazur & A. Wiles, Invent. Math. 76 (1984), 179–330."
    kind: paper
  - title: "On modular representations of Gal(Q̄/Q) arising from modular forms"
    citation: "K. Ribet, Invent. Math. 100 (1990), 431–476."
    kind: paper
lean_anchors:
  - 05_determinant/fit_twists.lean#New.Data.IsRegularFamily
  - 05_determinant/fit_twists.lean#New.Data.HasSmallRatios
  - 05_determinant/fit_twists.lean#New.Data.IsDenominatorBound
  - 05_determinant/fit_twists.lean#New.Data.ClearsProjection
  - 05_determinant/fit_twists.lean#New.Data.denominatorBudget
child_concepts:
  - prime-number
  - twisted-lattice
  - arithmetic-height
  - hecke-l-value-ratio
  - rallis-inner-product
---

# Regular Twist Family

## Level 1 — First-Principles

To prove Theorem C's lower bound inside Stage 01's {{concept:twisted-lattice|twisted-lattice height formalism}}, the proof must compare the untwisted chain volume $Z(1)$ against a twisted chain volume $Z(\xi_\iota)$ at every {{concept:prime-number|prime}} $\ell$ and embedding $\iota : \mathbb{C} \simeq \overline{\mathbb{Q}}_\ell$. If we used the trivial twist $\xi_\iota = 1$, the ratio $Z(1)/Z(1) = 1$ would give zero lower bound. If we picked an arbitrary twist $\xi_\iota$, two things could go wrong: the twist's modulus could collide with $\ell$, its auxiliary $L$-value ratios could blow up, or congruences between lifted forms and non-lifted forms could create huge $\ell$-adic denominators when projecting onto the space of lifts.

A **regular twist family** (`New.Data.IsRegularFamily`) is the exact contract specifying what makes a family of twists $\theta(\iota)$ usable across all seven downstream consumers in Stages 05, 06, and 07. Each $\theta(\iota)$ is an **m-fold twist** (`D.MFoldTwist`)—an anticyclotomic character of $U(1)$ of weight $0$, tamely ramified on a set of places $Q$ prime to $MN$ and disjoint from its conjugate, whose first $m$ powers $\theta(\iota)^1, \dots, \theta(\iota)^m$ remain nontrivial at every place of $Q$. To be regular, the family $\theta$ must satisfy two joint requirements:

1. **Small ratios (`HasSmallRatios`):** For every prime $\ell$ and embedding $\iota$, the norm $\mathrm{Nm}(Q)$ of the twist modulus is coprime to $\ell$, and for every $1 \le j \le m$ and every {{concept:hecke-l-value-ratio|ramified critical pair}}, the inverse $L$-value ratios at $\theta(\iota)^j$ and $\theta(\iota)^{-j}$ have {{concept:arithmetic-height|global arithmetic height}} at most $x_1$.
2. **Denominators within budget (`IsDenominatorBound`):** For every lift index $1 \le j \le m$, there is a sequence of real numbers $\gamma(\ell)$ whose sum over every finite set of primes stays within the **denominator budget**
   $$\mathrm{denominatorBudget} = \frac{f a \log^3 b}{m k},$$
   such that at every $\iota \mid \ell$ the $j$-th power $\theta(\iota)^j$ satisfies two $\ell$-adic bounds (`HasDenominatorsLe`):
   - **Projection clearing (`ClearsProjection`):** Some linear operator $\Lambda$ on the space $Z$ of good lifts from $W_j$, with $\log|\iota(\det \Lambda)| \ge -(\dim Z)\, r\, \gamma(\ell)$, clears all $\ell$-adic denominators when projecting integral forms orthogonal to false lifts onto $Z$.
   - **Main-pair projection bound:** For every {{term:hecke-l-value-ratio|main pair}} $p$ for $j$, the inner product of projections of two test forms onto the line of the rank-one theta lift of $\nu \theta(\iota)^j$ has $\ell$-adic norm at most $e^{\gamma(\ell)}$.

Notice the clean architectural separation: Stage 05 (`FitTwists.lean`) *defines* this contract (note that the file is named `FitTwists.lean` while the Lean predicate is `IsRegularFamily`—the upstream reading guide notes *"fit for regular"*); Stage 06 (`06_one_lift`) *consumes* it to bound the height of each single-lift determinant ratio $Z_j(1)/Z_j(\theta(\iota)^j)$; and Stage 07 (`07_congruences`) *constructs* such a family (`exists_isRegularFamily`) via Galois representations, Ribet's method, and Selmer-group twisting.

### Limits of the intuition

Thinking of a **regular twist family** as a single "lucky character" misses the core geometric idea of `TwistedLattice`: no single finite-modulus twist can be coprime to *every* prime $\ell$ simultaneously, nor simultaneously avoid congruence primes everywhere. Instead, $\theta$ is a *function* $\iota \mapsto \theta(\iota)$ assigning a tailored **m-fold twist** to each prime-place isomorphism $\iota : \mathbb{C} \simeq \overline{\mathbb{Q}}_\ell$. What makes that per-prime freedom legitimate is that `HeightLe` sums $\ell$-adic bounds across primes while `TwistedLattice` pairs the single untwisted archimedean {{concept:gram-determinant|squared volume bound}} against $\theta(\iota)$ at each prime $\ell$.

## Level 2 — Undergrad Bridge

`src/New/Determinant/Twist/FitTwists.lean` is the 7-consumer interface sitting at the hub of Theorem C (consumed by `norm_of_chain`, `spanning_forms`, `06_one_lift/three_exchanges`, `06_one_lift/twisted_value_heights`, `07_congruences/rows_from_eigensystems`, `07_congruences/denominator_bound`, and `07_congruences/twisting_theorem`):

1. **Denominator budget (`D.denominatorBudget`):**
   Defined as $D.\mathrm{errorUnit} / (D.m \cdot D.k) = f a \log^3 b / (m k)$. Inside `LongForms.lean`, multiplying by $k \log b$ yields $k \log b \cdot \mathrm{denominatorBudget} = f a \log^4 b / m$, which matches the exact error share allotted to each of the $m$ single lifts in Stage 06 (`heightLe_liftDeterminant_ratio`).
2. **Projection clearing (`D.ClearsProjection ι ϑ j β`):**
   Let $Z = (D.\mathrm{goodLift}\ \vartheta\ j).\mathrm{range} \subseteq (D.\mathrm{descentPair}\ \vartheta\ j).\mathrm{FormV}$ and $\mathrm{pr}$ be orthogonal projection onto $Z$. `ClearsProjection` asserts the existence of $\Lambda \in \mathrm{End}_{\mathbb{C}}(Z)$ with $\log \|\iota(\det \Lambda)\| \ge -(\dim Z)\cdot r \cdot \beta$ such that $\Lambda(\mathrm{pr}\,y)$ is $\iota$-integral (`integralForms`) for every $\iota$-integral form $y$ orthogonal to the false lifts (`IsOrthogonalToFalseLifts`). In arithmetic geometry, the $\ell$-adic valuation of $\det \Lambda$ measures the congruence modulus between the theta-lift subspace $Z$ and its orthogonal complement (after removing false lifts killed by split-place Hecke polynomials), which Stage 07 bounds via first-order Ribet's method and anticyclotomic Selmer groups.
3. **Joint denominator bound (`HasDenominatorsLe`, `IsDenominatorBound`):**
   `HasDenominatorsLe ι ϑ j β` pairs `ClearsProjection ι ϑ j β` with the rank-one main-pair projection bound $\|\iota\langle \mathrm{pr}_L \Xi, \mathrm{pr}_L \Xi' \rangle\| \le \exp \beta$ for every main pair $p$ for $j$ and test forms $\Xi, \Xi'$. `IsDenominatorBound θ j γ` asserts $\sum_{\ell \in T} \gamma(\ell) \le D.\mathrm{denominatorBudget}$ for every finite set $T$ of primes and `HasDenominatorsLe ι.2 ((θ ι).powTwist j) j (γ ι.1)` for every `ι : PrimePadicIso`.
4. **Negligible error sequences (`IsNegligible C e`):**
   A helper predicate $\sum_{\ell \in T} e(\ell)\log \ell \le D.x^C$ used by Stage 07 to package small exceptional-prime losses whose total contribution is dwarfed by `denominatorBudget`.
5. **Regularity (`HasSmallRatios`, `IsRegularFamily`):**
   `HasSmallRatios θ` requires `Nat.Coprime ι.1 (θ ι).normQ` and `HeightLe D.x₁ (fun ι ↦ (p.ratio ((θ ι).powTwist j) s)⁻¹)` for all $1 \le j \le m$, all ramified pairs $p$, and $s \in \{\pm 1\}$. `IsRegularFamily θ` extends `HasSmallRatios θ` with `∀ j : D.LiftIndex, ∃ γ, D.IsDenominatorBound θ j γ`.

## Level 3 — Formal Lean Contract

Stage 05 extracts the entire regular-twist interface into `skeleton/05_determinant/fit_twists.lean` (17 declarations, 0 stub truncations):

- `05_determinant/fit_twists.lean#New.Data.denominatorBudget` (upstream `src/New/Determinant/Twist/FitTwists.lean:153-157`):
  ```lean
  noncomputable abbrev New.Data.denominatorBudget : ℝ
  ```
- `05_determinant/fit_twists.lean#New.Data.ClearsProjection` (upstream `FitTwists.lean:160-175`):
  ```lean
  def New.Data.ClearsProjection {ℓ : ℕ} [Fact ℓ.Prime] (ι : ℂ ≃+* PadicAlgCl ℓ) (ϑ : D.Twist) (j : ℕ)
      (β : ℝ) : Prop
  ```
- `05_determinant/fit_twists.lean#New.Data.IsDenominatorBound` (upstream `FitTwists.lean:198-207`):
  ```lean
  structure New.Data.IsDenominatorBound (θ : PrimePadicIso → D.MFoldTwist) (j : D.LiftIndex) (γ : ℕ → ℝ) :
      Prop where
    sum_le (T : Finset ℕ) (hT : ∀ ℓ ∈ T, ℓ.Prime) : ∑ ℓ ∈ T, γ ℓ ≤ D.denominatorBudget
    hasDenominatorsLe (ι : PrimePadicIso) : D.HasDenominatorsLe ι.2 ((θ ι).powTwist j) j (γ ι.1)
  ```
- `05_determinant/fit_twists.lean#New.Data.HasSmallRatios` (upstream `FitTwists.lean:216-226`):
  ```lean
  structure New.Data.HasSmallRatios (θ : PrimePadicIso → D.MFoldTwist) : Prop where
    coprime (ι : PrimePadicIso) : Nat.Coprime ι.1 (θ ι).normQ
    height_le (j : D.LiftIndex) (p : D.CriticalPair) (s : ℤˣ) (hp : p.IsRamified) :
      HeightLe (D.x₁) fun ι : PrimePadicIso ↦ (p.ratio ((θ ι).powTwist j) s)⁻¹
  ```
- `05_determinant/fit_twists.lean#New.Data.IsRegularFamily` (upstream `FitTwists.lean:228-233`):
  ```lean
  structure New.Data.IsRegularFamily (θ : PrimePadicIso → D.MFoldTwist) : Prop extends D.HasSmallRatios θ where
    exists_denominator_bound (j : D.LiftIndex) : ∃ γ : ℕ → ℝ, D.IsDenominatorBound θ j γ
  ```

See the page's live `<VerificationBanner />` (`audit/verification-status.json`) for the pinned upstream commit and local Lean build status (`build_status: unverified-locally`).
