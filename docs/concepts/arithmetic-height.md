---
slug: arithmetic-height
term: arithmetic height
aliases:
  - product formula
  - p-adic valuation
inline_glimpse: "A logarithmic measure combining ordinary archimedean size with prime-by-prime p-adic divisibility, locked together by the product formula so a nonzero algebraic quantity cannot be simultaneously small at every place."
visual_ref: assets/stage01-twisted-lattice-product-formula.svg
where_analogy_breaks: "Thinking of arithmetic height as the bit-length of a reduced fraction $a/b$ captures the cancellation in the rational product formula $|q| \\prod_p |q|_p = 1$, but misses why Stage 01 indexes families $y_\\iota$ over all prime isomorphisms $\\iota : \\mathbb{C} \\xrightarrow{\\sim} \\overline{\\mathbb{Q}}_l$: in `New.HeightLe`, a single uniform nonzero multiplier $H \\in \\mathbb{C}^\\times$ bounds archimedean conjugates while $y_\\iota$ varies with the prime twist $\\xi_\\iota$, and uniform $2$-adic boundedness forces $H$ to be algebraic so the product formula applies to its field norm $N_{\\mathbb{Q}(H)/\\mathbb{Q}}(H)$."
sources:
  - title: "Heights in Diophantine Geometry"
    citation: "E. Bombieri and W. Gubler, Cambridge University Press (2006), Chapter 1, §§1.4–1.5."
    kind: book
  - title: "Local Fields"
    citation: "J.-P. Serre, Springer Graduate Texts in Mathematics 67 (1979)."
    kind: book
  - title: "Pentes des fibrés vectoriels adéliques sur un corps global"
    citation: "É. Gaudron, Rendiconti del Seminario Matematico della Università di Padova 116 (2006), pp. 131–243."
    kind: paper
lean_anchors:
  - 01_reduction/twisted_lattice.lean#New.TwistedLattice.heightLe_pow_mul_det_gram
  - 01_reduction/twisted_lattice.lean#New.TwistedLattice.covolume_add_index_nonneg
  - 01_reduction/twisted_lattice.lean#New.PrimePadicIso.integers
child_concepts:
  - prime-number
  - complex-plane
  - gram-determinant
  - twisted-lattice
---

# Arithmetic Height

## Level 1 — Abitur / First-Principles

Take any nonzero fraction written in lowest terms, say $q = \frac{12}{25} = 2^2 \cdot 3^1 \cdot 5^{-2}$. Besides measuring its ordinary size $|q| = \frac{12}{25}$, you can inspect $q$ through the lens of each {{concept:prime-number|indivisible whole number $p \in \{2, 3, 5, 7, \dots\}$}} by counting how many powers of $p$ divide it. That exponent—positive for factors in the numerator, negative for factors in the denominator, and $0$ for primes not dividing either—is the **$p$-adic valuation** $v_p(q)$. Turning the exponent into a multiplicative size $|q|_p = p^{-v_p(q)}$ makes numbers highly divisible by $p$ count as "$p$-adically small": here $|q|_2 = \frac{1}{4}$, $|q|_3 = \frac{1}{3}$, $|q|_5 = 25$, and $|q|_p = 1$ for every other prime.

Multiply all of these measurements together—the ordinary absolute value times every prime absolute value:
$$|q| \cdot \prod_{p} |q|_p = \frac{12}{25} \cdot \frac{1}{4} \cdot \frac{1}{3} \cdot 25 \cdot 1 \cdot 1 \cdots = 1.$$
Every prime factor in the numerator enlarges $|q|$ by $p$ while shrinking $|q|_p$ by $1/p$, and every prime factor in the denominator does the reverse. Taking logarithms turns this **product formula** into a zero-sum conservation law:
$$\log |q| + \sum_{p} \log |q|_p = 0.$$
A nonzero rational number therefore cannot be tiny in every sense at once; any deficit in ordinary size must be paid for by prime denominators somewhere else. **Arithmetic height** is the logarithmic ledger that tracks these upper bounds across the ordinary size and all {{term:prime-number|prime numbers}} simultaneously, so that bounds add cleanly when quantities are multiplied.

In Stage 01, the quantities being multiplied live in {{concept:complex-plane|the two-dimensional coordinate plane of numbers $a + bi$}} and come from {{concept:gram-determinant|determinants of pairwise inner products measuring squared parallelotope volume}} inside a {{concept:twisted-lattice|family of twisted linear embeddings with prime-by-prime integrality rules}}. By extending the prime valuation rings to all of $\mathbb{C}$, the proof balances the ordinary size of a single untwisted {{term:gram-determinant}} against prime-by-prime valuations computed using a separate twist at each prime, turning the product formula into the master non-negativity inequality.

### Limits of the intuition

Viewing arithmetic height as the logarithmic size of a single reduced fraction accurately explains why prime factors cancel in the product formula and why height bounds add under multiplication. The analogy falls short in Stage 01 for two reasons. First, the input to a height bound in the proof is not a single rational number $q$ but a family $y_\iota$ of {{term:complex-plane|complex numbers}} that can change from one prime $l$ (and one embedding $\iota$ of $\mathbb{C}$ into the $l$-adic algebraic closure) to another, matching the choice of a different twist at each prime. Second, working over all of $\mathbb{C}$ includes transcendental numbers, where no product formula holds; the formal height predicate overcomes this by introducing a single uniform multiplier $H \neq 0$ whose boundedness at the prime $2$ forces $H$ to be algebraic over $\mathbb{Q}$, allowing the classical product formula to apply to its rational field norm.

## Level 2 — Undergrad Bridge

For a rational number $q \in \mathbb{Q}^\times$, Ostrowski's theorem classifies all nontrivial absolute values on $\mathbb{Q}$ up to equivalence as the archimedean absolute value $|\cdot|_\infty$ and the $p$-adic absolute values $|q|_p = p^{-v_p(q)}$, normalized so they satisfy the product formula $\log |q|_\infty + \sum_p \log |q|_p = 0$. The absolute logarithmic Weil height $h(q) = \log\max(|a|, |b|)$ (for $q = a/b$ in lowest terms) equals $\sum_v \log^+|q|_v$ over all places $v \in \{\infty\} \cup \{p \text{ prime}\}$; Bombieri & Gubler (2006, §§1.4–1.5) and Serre (1979) extend this framework to algebraic numbers via places of number fields.

To support prime-dependent twists without fixing a number field in advance, `New.Reduction.Lattices.Heights` indexes prime places by `PrimePadicIso`: pairs $\iota = (l, \iota_{\mathrm{iso}})$ consisting of a prime $l$ and a ring isomorphism $\iota_{\mathrm{iso}} : \mathbb{C} \xrightarrow{\sim} \overline{\mathbb{Q}}_l$. Pulling back the $l$-adic valuation on $\mathbb{C}_l$ along $\iota$ defines the valuation subring
$$\mathcal{O}_\iota = \{x \in \mathbb{C} : \|\iota(x)\|_l \le 1\} \subseteq \mathbb{C},$$
which plays the role of local integers at $\iota \mid l$. A family $y : \mathrm{PrimePadicIso} \to \mathbb{C}$ has **height at most $c$** (`HeightLe c y`) if there exist a single nonzero multiplier $H \in \mathbb{C}^\times$, an archimedean cap $\alpha \in \mathbb{R}$, and non-negative exponents $h_l \ge 0$ such that:
1. $|\sigma(H)| \le e^\alpha$ for all field automorphisms $\sigma \in \operatorname{Aut}(\mathbb{C})$;
2. $\|\iota(H \cdot y_\iota)\|_l \le l^{h_l}$ for every $\iota \mid l$;
3. $\alpha + \sum_{l \in T} h_l \log l \le c$ for every finite set $T$ of primes.

This definition enjoys a clean calculus: multiplying two families adds their height bounds (`HeightLe.mul`, by multiplying multipliers $H \cdot H'$ and adding $\alpha + \alpha'$ and $h + h'$), raising to the $n$-th power scales height by $n$ (`HeightLe.pow`), and multiplying $y_\iota$ by a rational power of an integer coprime to $l$ preserves height (`HeightLe.rpow_mul`).

The key global rigidity lemma is `HeightLe.nonneg_of_one`: if the constant family $1$ satisfies `HeightLe c 1`, then $0 \le c$. Its proof extracts the uniform multiplier $H \neq 0$; because $\|\iota(H)\|_2 \le 2^{h_2}$ holds uniformly over all embeddings $\iota : \mathbb{C} \xrightarrow{\sim} \overline{\mathbb{Q}}_2$, $H$ must be algebraic over $\mathbb{Q}$. Taking its field norm $q = N_{\mathbb{Q}(H)/\mathbb{Q}}(H) \in \mathbb{Q}^\times$ of degree $n = [\mathbb{Q}(H) : \mathbb{Q}]$ multiplies $n$ conjugates at every place, giving $\log|q| \le n\alpha$ and $\log|q|_l \le n h_l \log l$. Summing via the rational product formula yields $0 = \log|q| + \sum_l \log|q|_l \le n c$, hence $0 \le c$.

As an immediate corollary (`HeightLe.neg_le_of_norm_map_le`), if a nonzero complex number $z$ satisfies `HeightLe c (fun _ => z)` and $|\sigma(z)| \le e^{c'}$ for all $\sigma \in \operatorname{Aut}(\mathbb{C})$, then applying `nonneg_of_one` to $z^{-1} \cdot z = 1$ gives $0 \le c' + c$. In `TwistedLattice.lean`, `heightLe_pow_mul_det_gram` establishes `HeightLe (2 * δ)` for the twisted family $x_\iota = |\gamma_\iota|^{2n'} \operatorname{Gram}(\xi_\iota)$ directly from the index matrices at $\iota$ and $\bar{\iota}$ (taking $H = 1$), after which multiplying by the heights of $|\gamma(\xi_\iota)|^{-2n'}$ and $Z(1)/Z(\xi_\iota)$ collapses the family to the constant $\operatorname{Gram}(1)$ and produces `covolume_add_index_nonneg`.

## Level 3 — Formal Lean Contract

In `skeleton/01_reduction/twisted_lattice.lean` (extracted from upstream `src/New/Reduction/Lattices/TwistedLattice.lean`, building on `New.HeightLe` and `New.log_product_formula` in `src/New/Reduction/Lattices/Heights.lean`), the valuation ring, twisted Gram-determinant height lemma, and final product-formula inequality have the following signatures:

- `01_reduction/twisted_lattice.lean#New.PrimePadicIso.integers` (upstream `src/New/Reduction/Lattices/TwistedLattice.lean:61-62`):
  ```lean
  abbrev New.PrimePadicIso.integers : ValuationSubring ℂ
  ```
- `01_reduction/twisted_lattice.lean#New.TwistedLattice.heightLe_pow_mul_det_gram` (upstream `src/New/Reduction/Lattices/TwistedLattice.lean:421-440`):
  ```lean
  lemma New.TwistedLattice.heightLe_pow_mul_det_gram {n n' : ℕ} {y : Fin n → V} {z : Fin n' → V} {δ : ℝ}
      (hδ : L.IndexLe y δ) {θ : PrimePadicIso → T}
      (hθ : ∀ ι : PrimePadicIso, L.IsAllowed ι.prime (θ ι)) {γ : PrimePadicIso → ℂ}
      (hz : ∀ (ι : PrimePadicIso) (i : Fin n'), γ ι • L.lift (θ ι) (z i) ∈ L.lattice (θ ι) ι)
      (hz' : ∀ (ι : PrimePadicIso) (i : Fin n'), γ ι • L.lift (θ ι) (z i) ∈ L.lattice (θ ι) ι.conj) :
      HeightLe (2 * δ) fun ι : PrimePadicIso ↦
        ((‖γ ι‖ : ℂ) ^ 2) ^ n' * (gram ℂ (L.lift (θ ι) ∘ Sum.elim y z)).det
  ```
- `01_reduction/twisted_lattice.lean#New.TwistedLattice.covolume_add_index_nonneg` (upstream `src/New/Reduction/Lattices/TwistedLattice.lean:444-485`):
  ```lean
  theorem New.TwistedLattice.covolume_add_index_nonneg {n n' : ℕ} {y : Fin n → V} {z : Fin n' → V}
      (h_inj : ∀ ξ : T, Function.Injective (L.lift ξ))
      (h_indep : LinearIndependent ℂ (Sum.elim y z)) (h_card : finrank ℂ V = n + n')
      {θ : PrimePadicIso → T} (hθ : ∀ ι : PrimePadicIso, L.IsAllowed ι.prime (θ ι))
      {c δ b_volume b_short b_denom : ℝ} (hc : L.CovolumeLe y c) (hδ : L.IndexLe y δ)
      {γ : T → ℂ} (hγ_ne : ∀ ξ : T, γ ξ ≠ 0)
      (hz_int : ∀ (i : Fin n') (ι : PrimePadicIso) (ξ : T), L.IsAllowed ι.prime ξ →
        γ ξ • L.lift ξ (z i) ∈ L.lattice ξ ι)
      (hz_short : ∀ (i : Fin n') (σ : ℂ ≃+* ℂ), L.normSqAt (z i) σ ≤ exp b_short)
      (hγ : HeightLe b_denom fun ι : PrimePadicIso ↦ ((‖γ (θ ι)‖ : ℂ) ^ 2)⁻¹)
      (hZ : HeightLe b_volume fun ι : PrimePadicIso ↦ L.volume 1 / L.volume (θ ι)) :
      0 ≤ c + 2 * δ + n' * (b_short + b_denom) + b_volume
  ```

Here `New.HeightLe` (`src/New/Reduction/Lattices/Heights.lean:45-52`) carries the uniform nonzero multiplier `H : ℂ`, archimedean bound `α : ℝ`, and prime exponents `h : ℕ → ℝ`, while `New.log_product_formula` (`Heights.lean:57-72`) supplies the underlying rational conservation law used by `HeightLe.nonneg_of_one` (`Heights.lean:195-225`) and `HeightLe.neg_le_of_norm_map_le` (`Heights.lean:229-234`). See the page's live verification banner (`audit/verification-status.json`) for the currently pinned upstream snapshot and local Lean build status (`build_status: unverified-locally`).
