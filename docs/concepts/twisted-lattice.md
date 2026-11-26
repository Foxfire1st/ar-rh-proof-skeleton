---
slug: twisted-lattice
term: twisted lattice
aliases: []
inline_glimpse: "A family of linear lifts from one complex vector space into inner-product spaces—one per twist—equipped with prime-by-prime integrality lattices so each prime can pick its own allowed twist."
visual_ref: assets/stage01-twisted-lattice-product-formula.svg
where_analogy_breaks: "Picturing a twisted lattice as a single tilted integer grid inside $\\mathbb{R}^n$ captures the trade-off between box volume and divisibility, but misses the core mechanism: archimedean geometry lives solely in the trivial twist $\\Theta_1$ via a rational inner-product matrix, while integrality at each prime $l$ is enforced simultaneously across every twist allowed at $l$, allowing a separate twist $\\xi_\\iota$ to be chosen at every prime place $\\iota \\mid l$."
sources:
  - title: "Algebraic leaves of algebraic foliations over number fields"
    citation: "J.-B. Bost, Publications Mathématiques de l'IHÉS 93 (2001), pp. 161–221."
    kind: paper
  - title: "Linear forms in two logarithms and interpolation determinants"
    citation: "M. Laurent, Acta Arithmetica 66 (1994), pp. 181–199."
    kind: paper
  - title: "Pentes des fibrés vectoriels adéliques sur un corps global"
    citation: "É. Gaudron, Rendiconti del Seminario Matematico della Università di Padova 116 (2006), pp. 131–243."
    kind: paper
lean_anchors:
  - 01_reduction/twisted_lattice.lean#New.TwistedLattice
  - 01_reduction/twisted_lattice.lean#New.TwistedLattice.covolume_add_index_nonneg
child_concepts:
  - prime-number
  - complex-plane
  - gram-determinant
  - arithmetic-height
---

# Twisted Lattice

## Level 1 — First-Principles

In ordinary geometry, take a grid of whole-number points inside Euclidean space. If you pick $n$ linearly independent grid vectors, the box they span has volume at least $1$, because the determinant of an invertible whole-number matrix is a nonzero integer and therefore has size at least $1$. Even if your vectors carry denominators, a simple tension governs them: shrinking their geometric box forces larger prime denominators to appear somewhere, while limiting those denominators puts a firm floor under the box volume.

A **twisted lattice** generalizes this volume-versus-divisibility balance to a whole family of views of the same vector space $V$ over {{concept:complex-plane|the two-dimensional plane of real and imaginary coordinates}}. Instead of a single embedding of $V$, you have a family of linear maps $\Theta_\xi$, indexed by parameters $\xi$ called twists, with one distinguished "untwisted" map $\Theta_1$. Each twisted target space has its own notion of length and angle (an inner product), and for every {{concept:prime-number|indivisible whole number $l \in \{2, 3, 5, 7, \dots\}$}} a designated subset of twists is *allowed* at $l$, each carrying its own rule for which target vectors count as "integral" at $l$. A vector in $V$ is declared integral at $l$ if its image under $\Theta_\xi$ is integral for *every* twist $\xi$ allowed at $l$.

This separation of duties is what gives the structure its leverage. You measure the geometric size of a family of vectors—its {{concept:gram-determinant|squared parallelotope volume computed from the determinant of pairwise inner products}}—once and for all in the single trivial twist $\Theta_1$. At each prime $l$, however, you are free to choose *whichever* allowed twist $\xi_l$ makes the integrality bound sharpest. Comparing the trivial-twist {{term:gram-determinant|covolume}} against the prime-by-prime integrality index through {{concept:arithmetic-height|the global balance between ordinary magnitude and prime-by-prime divisibility}} yields a non-negativity inequality: geometric smallness and divisibility tightness cannot both beat the cost of switching twists.

### Limits of the intuition

Thinking of a twisted lattice as a fixed integer grid in $\mathbb{R}^n$ with slightly rescaled axes captures the core obstruction—a nonzero determinant cannot be simultaneously tiny in ordinary size and free of prime denominators. The picture breaks down in two ways. First, the vector space $V$ lives over {{term:complex-plane|complex numbers}} rather than reals, so "ordinary size" must bound not just the determinant itself but all of its algebraic conjugates under field automorphisms of $\mathbb{C}$. Second, there is no single global $\mathbb{Z}$-lattice inside $V$; instead, integrality is tested locally at every prime $l$ across all twists allowed at $l$, while the comparison theorem selects a different allowed twist $\xi_\iota$ at every prime place.

## Level 2 — Undergrad Bridge

Let $V$ be a finite-dimensional complex vector space and let $T$ be an index set of twists containing a distinguished trivial twist $1 \in T$. A twisted lattice $L$ on $V$ specifies:

1. For each twist $\xi \in T$, a complex inner product space $H_\xi$ and a $\mathbb{C}$-linear map $\Theta_\xi : V \to H_\xi$.
2. An allowance relation $\operatorname{IsAllowed}(l, \xi)$ specifying which twists $\xi \in T$ are permitted at each prime $l$.
3. For each pair $\iota = (l, \iota_{\mathrm{iso}})$ consisting of a prime $l$ and a ring isomorphism $\iota_{\mathrm{iso}} : \mathbb{C} \xrightarrow{\sim} \overline{\mathbb{Q}}_l$ (with valuation ring $\mathcal{O}_\iota = \{x \in \mathbb{C} : \|\iota(x)\|_l \le 1\}$ and conjugate $\bar{\iota}(z) = \iota(\bar{z})$), an $\mathcal{O}_\iota$-submodule $\Lambda_{\xi,\iota} \subseteq H_\xi$ such that whenever $\xi$ is allowed at $l$,
   $$\varphi \in \Lambda_{\xi,\bar{\iota}},\ \varphi' \in \Lambda_{\xi,\iota} \implies \|\iota(\langle \varphi, \varphi' \rangle)\|_l \le 1.$$
4. On the trivial twist space $H_1$, a finite coordinate map $\operatorname{coord} : H_1 \to \mathbb{C}^I$ and a positive-definite *rational* matrix $G \in M_{I \times I}(\mathbb{Q})$ satisfying $\langle \varphi, \varphi' \rangle = \operatorname{coord}(\varphi)^* G \operatorname{coord}(\varphi')$.

A vector $v \in V$ belongs to the integral submodule $V_{\mathcal{O}_\iota} \subseteq V$ if $\Theta_\xi(v) \in \Lambda_{\xi,\iota}$ for all $\xi$ allowed at $l$. For a finite family $y = (y_1, \dots, y_n)$ in $V$:

- **Index at most $\delta$**: there exist real numbers $\delta_l \ge 0$ with $\sum_{l \in U} \delta_l \log l \le \delta$ over all finite sets $U$ of primes such that for every $\iota \mid l$, some matrix $u \in M_{n \times n}(\mathcal{O}_\iota)$ satisfies $\|\iota(\det u)\|_l \ge l^{-\delta_l}$ and sends $y$ into $V_{\mathcal{O}_\iota}$ (crucially, $u$ is independent of the twist $\xi$).
- **Covolume at most $c$**: for every field automorphism $\sigma \in \operatorname{Aut}(\mathbb{C})$, the Gram determinant of the coordinate-conjugated family $\sigma(\operatorname{coord}(\Theta_1 y_i))$ under $G$ has absolute value at most $e^c$.
- **Twist volume**: once $V$ carries an inner product, $Z(\xi) = \det(\Theta_\xi^* \Theta_\xi)$ measures the squared volume distortion of $\Theta_\xi$; for any basis $w_1, \dots, w_d$ of $V$, $\operatorname{Gram}(\xi) = \det(\langle \Theta_\xi w_i, \Theta_\xi w_k \rangle) = Z(\xi) \det(\langle w_i, w_k \rangle)$, so the ratio $\operatorname{Gram}(1)/\operatorname{Gram}(\xi) = Z(1)/Z(\xi)$ is independent of the chosen basis.

Extending $y_1, \dots, y_n$ to a basis $y_1, \dots, y_n, z_1, \dots, z_{n'}$ of $V$ and selecting an allowed twist $\xi_\iota = \theta(\iota)$ at every $\iota$, we factor
$$\operatorname{Gram}(1) = \Bigl(|\gamma(\xi_\iota)|^{2n'} \operatorname{Gram}(\xi_\iota)\Bigr) \cdot \bigl(|\gamma(\xi_\iota)|^{-2}\bigr)^{n'} \cdot \frac{Z(1)}{Z(\xi_\iota)}.$$
The first factor has arithmetic height at most $2\delta$ (combining $u$ at $\iota$ and $u'$ at $\bar{\iota}$), while the remaining factors have heights bounded by $n' b_{\mathrm{denom}}$ and $b_{\mathrm{volume}}$. Meanwhile, Fischer's and Hadamard's inequalities bound $|\sigma(\operatorname{Gram}(1))| \le \exp(c + n' b_{\mathrm{short}})$ for every $\sigma \in \operatorname{Aut}(\mathbb{C})$. Applying the product formula (`HeightLe.neg_le_of_norm_map_le`) forces
$$0 \le c + 2\delta + n'(b_{\mathrm{short}} + b_{\mathrm{denom}}) + b_{\mathrm{volume}}.$$
In Arakelov geometry (Bost 2001, Gaudron 2006) and transcendence theory (Laurent 1994), this is the slope-inequality principle for evaluation/interpolation determinants, here enhanced by allowing the twist $\xi_\iota$ to vary with each prime place $\iota$.

## Level 3 — Formal Lean Contract

Stage 01 extracts the twisted-lattice interface and its main global inequality into `skeleton/01_reduction/twisted_lattice.lean` from upstream `src/New/Reduction/Lattices/TwistedLattice.lean`:

- `01_reduction/twisted_lattice.lean#New.TwistedLattice` (upstream `src/New/Reduction/Lattices/TwistedLattice.lean:109-138`):
  ```lean
  structure New.TwistedLattice (V T : Type) [AddCommGroup V] [Module ℂ V] [One T] where
    Space : T → Type
    [normedAddCommGroup (ξ : T) : NormedAddCommGroup (Space ξ)]
    [innerProductSpace (ξ : T) : InnerProductSpace ℂ (Space ξ)]
    lift (ξ : T) : V →ₗ[ℂ] Space ξ
    IsAllowed : ℕ → T → Prop
    lattice (ξ : T) (ι : PrimePadicIso) : Submodule ι.integers (Space ξ)
    norm_inner_le_one {ξ : T} (ι : PrimePadicIso) (hξ : IsAllowed ι.prime ξ) :
      ∀ φ ∈ lattice ξ ι.conj, ∀ φ' ∈ lattice ξ ι, ‖ι.iso ⟪φ, φ'⟫_ℂ‖ ≤ 1
    Coord : Type
    [fintypeCoord : Fintype Coord]
    coord : Space 1 →ₗ[ℂ] Coord → ℂ
    gramMatrix : Matrix Coord Coord ℚ
    posDef_gramMatrix : (gramMatrix.map (↑) : Matrix Coord Coord ℂ).PosDef
    inner_eq (φ φ' : Space 1) : ⟪φ, φ'⟫_ℂ = star (coord φ) ⬝ᵥ gramMatrix.map (↑) *ᵥ coord φ'
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

Notice that `New.TwistedLattice` itself assumes no inner product or finite-dimensionality on `V`; those typeclasses (`InnerProductSpace ℂ V`, `Module.Finite ℂ V`) enter only in the `Twist` section defining `L.volume ξ = (L.lift ξ).normDet ^ 2` and proving `covolume_add_index_nonneg`. See the page's live verification banner (`audit/verification-status.json`) for the currently pinned upstream snapshot and local Lean build status (`build_status: unverified-locally`).
