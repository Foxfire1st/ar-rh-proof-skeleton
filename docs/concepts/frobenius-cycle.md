---
slug: frobenius-cycle
term: Frobenius cycle
aliases: ["Stepanov's count", "edge condition", "defect threshold"]
inline_glimpse: "A cyclic ring of $N$ Frobenius-twisted embeddings $W \\to O$ alternating between standard and contragredient actions of $\\mathrm{GL}_k(W)$, whose Stepanov zero counts and Levi-invariant averaging yield low-dimension edge conditions modulo $w^c$."
visual_ref: assets/stage03-four-case-divisibility-and-hecke-colength.svg
where_analogy_breaks: "Viewing a Frobenius cycle as an ordinary Galois cycle of embeddings $\\sigma, \\sigma^p, \\dots, \\sigma^{p^{N-1}}$ into characteristic $p$ captures why $P$-th powers rotate the letters, but misses three layers of Stage 03: the cycle lives in mixed characteristic ($\\operatorname{char} O = 0$, $p \\in \\mathfrak{m}_O$, $w^2 = p \\in O$) with strictly alternating even (standard) and odd (inverse-transpose) $\\mathrm{GL}_k(W)$-actions (`isEven_succ`); `New.stepanov_count` does not directly count points on a curve over $\\mathbb{F}_q$, instead constraining minimal-valuation-plus-weight terms of $\\mathrm{SL}_2(W)$-translates (`defect_nonpos_of_extreme_term`) and feeding through Frobenius-amplified Pieri-cone volumes (`DefectCount.lean`) and Levi-invariant averaging (`AveragingMap.lean`) into `exists_isEdgeCondition`; and the $(1+\\theta)$ dimension bound holds only in the large-degree regime `IsLargeDegree` where $n^4 \\le q$ and $(N k \\log q / \\theta)^5 P^{3r} \\le n$."
sources:
  - title: "Counting points on curves over finite fields (d'après S. A. Stepanov)"
    citation: "E. Bombieri, Séminaire Bourbaki 430 (1973)."
    kind: paper
  - title: "Automorphic vector bundles on the stack of G-zips"
    citation: "N. Imai & J.-S. Koskivirta, preprint (2021)."
    kind: paper
lean_anchors:
  - 03_upper_bound/elementary_divisor_theorem.lean#New.FrobeniusCycle.IsLargeDegree
  - 03_upper_bound/elementary_divisor_theorem.lean#New.FrobeniusCycle.IsEdgeCondition
  - 03_upper_bound/elementary_divisor_theorem.lean#New.FrobeniusCycle.exists_isEdgeCondition
  - 03_upper_bound/elementary_divisor_theorem.lean#New.FrobeniusCycle.IsLargeDegree.mul_defectThreshold_le_mul_degree
child_concepts:
  - prime-number
  - formal-group-law
  - hecke-operator
  - elementary-divisor
  - arithmetic-height
---

# Frobenius Cycle

## Level 1 — First-Principles

Picture $N$ copies of a $k$-variable polynomial space arranged in a closed circle, with positions—called *letters*—indexed by $s \in \mathbb{Z}/N$. Fix a {{concept:prime-number|indivisible whole number $p \in \{2, 3, 5, 7, \dots\}$}}, a prime power $P = p^{\mathrm{frobExp}} \ge 5$, and a local ring $W$ whose finite residue field has $q = P^N$ elements. A **Frobenius cycle** equips this circle with $N$ ring maps $\iota_s : W \to O$ into a characteristic-zero valuation domain $O$ (containing a square root $w$ of $p$ with $w^2 = p$) such that stepping to the next letter raises every element to the $P$-th power modulo $p$—a cyclic {{concept:formal-group-law|prime-power raising relation linking adjacent embeddings modulo $p$}}:
$$\iota_{s+1}(x) \equiv \iota_s(x)^P \pmod{p}.$$
Equally important, the letters strictly alternate between *even* and *odd* parities: an invertible matrix $g \in \mathrm{GL}_k(W)$ acts on even letters directly via $\iota_s(g)$ and on odd letters by the contragredient inverse transpose $\iota_s(g^{-\mathsf{T}})$.

Why build this alternating ring of embeddings? To prove that an {{concept:hecke-operator|averaging operator summing form values over neighboring sublattices}} has small {{concept:elementary-divisor|prime-power invariant factors and determinant-divisibility index of a sublattice}}, Stage 03 needs a set of **edge conditions** (`New.FrobeniusCycle.IsEdgeCondition`)—linear congruences modulo a high power $w^c$ imposed on the vector of polynomial coefficients sitting on each lattice edge. Those **edge conditions** must satisfy three competing demands simultaneously:

1. **Inclusion of global forms:** Every form coming from the halfway lattices of all edges out of a vertex (`allEdgesForms`) automatically obeys the **edge conditions** ($M \le C$).
2. **An averaging map matching the edge pairing:** An explicit matrix `Av` pushes arbitrary edge vectors into $C$ while acting, against every dual-edge form in $M$, as scalar multiplication by $p^{N c_0}$ modulo $w^c$—the engine behind the modulo-$w^c$ near inverse.
3. **Tight residue-field dimension:** Whenever $w^{c-1} h \in C$, the reduction of $h$ modulo the maximal ideal lands in a small residue-field subspace $S$ whose dimension satisfies $[\mathrm{GL}_k(W) : P_{r, k-r}]\cdot \dim S \le (1 + \theta)\cdot \#\text{Exponents}$.

Proving that such **edge conditions** exist (`exists_isEdgeCondition`) combines two arithmetic mechanisms. First comes **Stepanov's count**: if a nonzero polynomial $\Phi(X)$ over a ring $R$ vanishes to order at least $E$ at the image of every residue class of $W$, then its degree must be at least $E\cdot q$. Applying **Stepanov's count** to an auxiliary polynomial built from $\mathrm{SL}_2(W)$-translates around the **Frobenius cycle** forces any extreme monomial (minimizing $w$-adic order plus a directional weight, in the spirit of {{concept:arithmetic-height|balancing local valuations across places}}) to have non-positive *defect*. Second, Stage 03 sets a quantitative **defect threshold** $c^* = 3 k c$ tied to the congruence precision $c = 5 N c_0$. Requiring all block-diagonal Levi translates to have their coefficients above $3 c^*$ divisible by $w^c$ defines $C$. When the polynomial degree $n$ is large relative to $c^*$ and $P^{2r}$ (`IsLargeDegree`, yielding `mul_defectThreshold_le_mul_degree`), the **defect threshold** becomes a negligible sliver of $n$, forcing $\dim S$ to match the inverse parahoric index up to $1 + \theta$.

### Limits of the intuition

Thinking of a **Frobenius cycle** as an ordinary Galois cycle of characteristic-$p$ embeddings $\sigma, \sigma^p, \dots, \sigma^{p^{N-1}}$ explains why $P$-th powers rotate the letters around $\mathbb{Z}/N$, but breaks down in three concrete ways. First, the target ring $O$ has characteristic $0$ with $p \in \mathfrak{m}_O$ and $w^2 = p \in O$, and the letters strictly alternate between standard and inverse-transpose actions of $\mathrm{GL}_k(W)$ (`isEven_succ`), so the defect weights carry alternating signs around the cycle. Second, **Stepanov's count** is not used here to count rational points on an algebraic curve; it bounds the order of vanishing of a translated auxiliary binary form at the $q + 1$ points of $\mathbb{P}^1(\mathbb{F}_q)$, which feeds through upstream helper modules (`RootLemma.lean`, `AveragingMap.lean`, and `DefectCount.lean`) rather than directly proving `exists_isEdgeCondition` in one step. Third, the $(1 + \theta)$ dimension estimate holds only inside the asymptotic `IsLargeDegree` window requiring both $n^4 \le q$ and $(N k \log q / \theta)^5 P^{3r} \le n$.

## Level 2 — Undergrad Bridge

Let $W$ be a local ring with finite residue field of cardinality $q = P^N$, where $p$ is prime, $P = p^{\mathrm{frobExp}} \ge 5$, and $N \ge 2$ is even. A **Frobenius cycle** `New.FrobeniusCycle N W O` consists of:
- $N$ ring homomorphisms $\iota_s : W \to O$ ($s \in \mathbb{Z}/N$) satisfying $p \mid \iota_{s+1}(x) - \iota_s(x)^P$ for all $x \in W$,
- an alternating parity function $\operatorname{isEven} : \mathbb{Z}/N \to \mathrm{Bool}$ with $\operatorname{isEven}(s + 1) = \neg\operatorname{isEven}(s)$,
- a square root $w = \operatorname{sqrtPrime} \in O$ with $w^2 = p$.

Matrices $g \in \mathrm{GL}_k(W)$ act on `Exponents N k n` (monomials homogeneous of degree $n$ in each of the $N$ letters, with $k$ variables per letter) through the twisted tensor representation $\rho(g) = T.\operatorname{letterRep}\ n\ g$, acting via $\iota_s(g)$ on even letters and $\iota_s(g^{-\mathsf{T}})$ on odd letters. Set $c_0 = T.\operatorname{multinomialExponent}\ k = k^2 \log_p q$, congruence precision $c = T.\operatorname{congruencePrecision}\ k = 5 N c_0$, and **defect threshold** $c^* = \operatorname{defectThreshold}\ k\ c = 3 k c$.

1. **Large-degree regime and defect-threshold inequality (`IsLargeDegree`, `mul_defectThreshold_le_mul_degree`):** For block size $r$ and exceptional proportion $\theta$, `T.IsLargeDegree k n r θ` requires
   $$0 < r \le \frac{k}{2}, \qquad 0 < \theta \le 1, \qquad n^4 \le q, \qquad \left(\frac{N k \log q}{\theta}\right)^5 P^{3r} \le n.$$
   Because $c_0 \le \frac{3}{2} k^2 \log q$ (`multinomialExponent_le_mul_log`) and $c \le \frac{15}{2} k^2 N \log q$, this regime immediately implies
   $$8 N k\, c^*\, P^{2r} \le \theta n \qquad \bigl(c^* = \operatorname{defectThreshold}(k, c) = 3 k c\bigr),$$
   ensuring that the defect slack divided by $n$ is absorbed into $\theta$.

2. **From Stepanov's count to edge conditions (`New.stepanov_count`, `IsEdgeCondition`, `exists_isEdgeCondition`):** Notice an important provenance boundary: `New.stepanov_count` itself lives in the upstream helper file `src/New/UpperBound/ElementaryDivisors/RootLemma.lean` (outside the five extracted Stage 03 skeleton files) and feeds into `exists_isEdgeCondition` via two intermediate upstream modules:
   - **Root lemma (`RootLemma.lean`):** For any ring map $\iota : W \to R$ and nonzero $\Phi \in R[X]$, if $X^E \mid \Phi(X + \iota(x))$ for every $x \in W$, then $E \cdot |\operatorname{ResidueField}(W)| \le \deg \Phi$ (`New.stepanov_count`, following Bombieri 1973). Specializing to an auxiliary polynomial tracking $\mathrm{SL}_2(W)$-translates across the cyclic letters yields `defect_nonpos_of_extreme_term`: any extreme term (least $w$-adic order plus weight) has non-positive defect along every root (analogous to weight inequalities in the G-zip framework of Imai & Koskivirta 2021).
   - **Levi-invariant averaging (`AveragingMap.lean`):** Substituting generic matrices across the $N$ letters and applying a Levi-conjugation-invariant linear form produces a matrix $\mathrm{Av}$ whose outputs have no Levi translate with defect above $3 c^*$ modulo $w^c$, while preserving the edge pairing against $M = T.\operatorname{allEdgesForms}\ k\ n\ r$ up to $p^{N c_0} \pmod{w^c}$ (`exists_average_mem_defectConditions_and_dvd_sub`).
   - **Defect dimension count (`DefectCount.lean`):** Over the characteristic-$p$ residue field $k_O$, packing forms into $q^j$-th powers (`powerPacking`) and applying the root-lemma defect bound along flags (cosets of the lower Iwahori subgroup) shifts leading exponents into the Pieri cone of non-positive simple-root defects. Letting $j \to \infty$ compares the count with the cone volume inside the degree slab, proving
     $$[\mathrm{GL}_k(W) : \operatorname{parahoric}(W, \operatorname{twoBlock}_{k, r})]\cdot \dim_{k_O} S \le (1 + \theta)\cdot |\operatorname{Exponents}(N, k, n)|.$$

   Assembling these three results in `exists_isEdgeCondition` produces $(C, S, \mathrm{Av})$ satisfying all five fields of `T.IsEdgeCondition r c θ b C S Av`: `allEdgesForms_le` ($M \le C$), `average_mem` ($\mathrm{Av}\, h \in C$), `pow_dvd_pairing_average` ($w^c \mid h' \cdot (b * \mathrm{Av}\, h) - p^{N c_0} h' \cdot (b * h)$), `residue_mem` ($w^{c-1} h \in C \implies \bar{h} \in S$), and `index_mul_finrank_le`.

## Level 3 — Formal Lean Contract

Stage 03 extracts the large-degree predicate, defect-threshold bound, edge-condition structure, and existence theorem into `skeleton/03_upper_bound/elementary_divisor_theorem.lean` (from upstream `src/New/UpperBound/ElementaryDivisors/ElementaryDivisorTheorem.lean`), while `New.stepanov_count` resides in upstream helper `src/New/UpperBound/ElementaryDivisors/RootLemma.lean` (outside the 5 extracted skeletons, flowing via `AveragingMap.lean` and `DefectCount.lean` into `exists_isEdgeCondition`):

- `03_upper_bound/elementary_divisor_theorem.lean#New.FrobeniusCycle.IsLargeDegree` (upstream `src/New/UpperBound/ElementaryDivisors/ElementaryDivisorTheorem.lean:385-399`):
  ```lean
  structure New.FrobeniusCycle.IsLargeDegree (k n r : ℕ) (θ : ℝ) : Prop where
    block_pos : 0 < r
    two_mul_block_le : 2 * r ≤ k
    proportion_pos : 0 < θ
    proportion_le_one : θ ≤ 1
    degree_pow_le : n ^ 4 ≤ T.q
    le_degree : (N * k * Real.log T.q / θ) ^ 5 * T.P ^ (3 * r) ≤ n
  ```
- `03_upper_bound/elementary_divisor_theorem.lean#New.FrobeniusCycle.IsLargeDegree.mul_defectThreshold_le_mul_degree` (upstream `src/New/UpperBound/ElementaryDivisors/ElementaryDivisorTheorem.lean:577-610`):
  ```lean
  lemma New.FrobeniusCycle.IsLargeDegree.mul_defectThreshold_le_mul_degree (h_large : T.IsLargeDegree k n r θ) :
      8 * N * k * defectThreshold k (T.congruencePrecision k) * T.P ^ (2 * r) ≤ θ * n
  ```
- `03_upper_bound/elementary_divisor_theorem.lean#New.FrobeniusCycle.IsEdgeCondition` (upstream `src/New/UpperBound/ElementaryDivisors/ElementaryDivisorTheorem.lean:618-643`):
  ```lean
  structure New.FrobeniusCycle.IsEdgeCondition {N k n : ℕ} {W O : Type} [NeZero N] [CommRing W] [IsLocalRing W]
      [CommRing O] [IsDomain O] [ValuationRing O] (T : FrobeniusCycle N W O) (r c : ℕ) (θ : ℝ)
      (b : Exponents N k n → O)
      (C : Submodule O (Exponents N k n → O))
      (S : Submodule (ResidueField O) (Exponents N k n → ResidueField O))
      (Av : Matrix (Exponents N k n) (Exponents N k n) O) : Prop where
    allEdgesForms_le : T.allEdgesForms k n r ≤ C
    average_mem : ∀ h : Exponents N k n → O, Av *ᵥ h ∈ C
    pow_dvd_pairing_average : ∀ h : Exponents N k n → O, ∀ h' ∈ T.allEdgesForms k n r,
      T.sqrtPrime ^ c ∣ h' ⬝ᵥ (b * Av *ᵥ h) - (T.p : O) ^ (N * T.multinomialExponent k) * h' ⬝ᵥ
        (b * h)
    residue_mem : ∀ h : Exponents N k n → O,
      T.sqrtPrime ^ (c - 1) • h ∈ C → (fun β : Exponents N k n ↦ residue O (h β)) ∈ S
    index_mul_finrank_le : ((parahoric W (twoBlock k r)).index * finrank (ResidueField O) S : ℝ) ≤
      (1 + θ) * Fintype.card (Exponents N k n)
  ```
- `03_upper_bound/elementary_divisor_theorem.lean#New.FrobeniusCycle.exists_isEdgeCondition` (upstream `src/New/UpperBound/ElementaryDivisors/ElementaryDivisorTheorem.lean:675-727`):
  ```lean
  theorem New.FrobeniusCycle.exists_isEdgeCondition (hp : ¬IsUnit (T.p : O)) {r : ℕ} {θ : ℝ}
      (h_large : T.IsLargeDegree k n r θ) {b b' : Exponents N k n → O} (hb : T.IsEdgePairing b b') :
      ∃ (C : Submodule O (Exponents N k n → O))
        (S : Submodule (ResidueField O) (Exponents N k n → ResidueField O))
        (Av : Matrix (Exponents N k n) (Exponents N k n) O),
        T.IsEdgeCondition r (T.congruencePrecision k) θ b C S Av
  ```
- Upstream helper citation (`src/New/UpperBound/ElementaryDivisors/RootLemma.lean:65-67`, not extracted into `skeleton/03_upper_bound/`):
  ```lean
  lemma New.stepanov_count {R W : Type} [CommRing R] [CommRing W] [IsLocalRing W] (ι : W →+* R)
      {Φ : R[X]} (E : ℕ) (hΦ : Φ ≠ 0) (h_zero : ∀ x : W, X ^ E ∣ taylor (ι x) Φ) :
      E * Nat.card (ResidueField W) ≤ Φ.natDegree
  ```

`exists_isEdgeCondition` feeds directly into `IsEdgeCondition.pow_dvd_mul_sub_smul_one` and `IsEdgeCondition.exists_finset_smul_top_le_range_sup_span`, completing the Frobenius-cycle input to `New.FrobeniusCycle.elementary_divisors`. See the page's live verification banner (`audit/verification-status.json`) for the currently pinned upstream snapshot and local Lean build status (`build_status: unverified-locally`).
