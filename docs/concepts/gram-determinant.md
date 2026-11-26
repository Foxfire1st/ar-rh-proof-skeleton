---
slug: gram-determinant
term: Gram determinant
aliases:
  - covolume
  - Hadamard's inequality
inline_glimpse: "The determinant of the matrix of pairwise inner products of a family of vectors, measuring the squared volume of the parallelotope they span and controlled by Hadamard's and Fischer's inequalities."
visual_ref: assets/stage01-twisted-lattice-product-formula.svg
where_analogy_breaks: "Treating the Gram determinant merely as the squared Euclidean volume of a box in $\\mathbb{R}^n$ captures orthogonality, Hadamard's inequality, and block subadditivity, but hides the algebraic-conjugate twist required over $\\mathbb{C}$: to feed into the global product formula, `CovolumeLe` must bound the determinant simultaneously after applying every field automorphism $\\sigma \\in \\operatorname{Aut}(\\mathbb{C})$ to coordinates, using a rational inner-product matrix $G$ so $\\sigma$ commutes with the form up to $\\sigma'(z) = \\overline{\\sigma(\\bar{z})}$."
sources:
  - title: "Matrix Analysis"
    citation: "R. A. Horn and C. R. Johnson, 2nd ed., Cambridge University Press (2013), §7.2 and Theorems 7.8.1, 7.8.5."
    kind: book
  - title: "Heights in Diophantine Geometry"
    citation: "E. Bombieri and W. Gubler, Cambridge University Press (2006), Chapter 1."
    kind: book
lean_anchors:
  - 01_reduction/twisted_lattice.lean#New.TwistedLattice.CovolumeLe
  - 01_reduction/twisted_lattice.lean#New.TwistedLattice.covolumeLe_of_normSqAt_le
  - 01_reduction/twisted_lattice.lean#New.TwistedLattice.CovolumeLe.sumElim
child_concepts:
  - complex-plane
  - twisted-lattice
  - arithmetic-height
---

# Gram Determinant

## Level 1 — Abitur / First-Principles

Take two vectors $v_1, v_2$ in space. The parallelogram they span has area $\|v_1\| \|v_2\| \sin\theta$, so its squared area is
$$\|v_1\|^2 \|v_2\|^2 (1 - \cos^2\theta) = \|v_1\|^2 \|v_2\|^2 - \langle v_1, v_2 \rangle^2 = \det \begin{pmatrix} \langle v_1, v_1 \rangle & \langle v_1, v_2 \rangle \\ \langle v_2, v_1 \rangle & \langle v_2, v_2 \rangle \end{pmatrix}.$$
That $2 \times 2$ determinant generalizes immediately to any $n$ vectors $v_1, \dots, v_n$: arrange all pairwise inner products $\langle v_i, v_j \rangle$ into an $n \times n$ table and take its determinant. The resulting number is the **Gram determinant**. It equals the square of the $n$-dimensional volume—the **covolume**—of the box spanned by $v_1, \dots, v_n$, and it works even when $v_1, \dots, v_n$ sit inside a much larger ambient space without any chosen coordinate axes.

Two geometric facts follow directly from thinking of a box. First, tilting the edges of a box can only flatten it, never expand it: the squared volume never exceeds the product of the squared edge lengths,
$$\det(\langle v_i, v_j \rangle) \le \|v_1\|^2 \|v_2\|^2 \cdots \|v_n\|^2,$$
with equality precisely when the vectors meet at right angles. This bound is **Hadamard's inequality**; taking logarithms, if every edge has squared length at most $e^h$, the logarithmic covolume of $n$ vectors is at most $n h$. Second, if you join two families of vectors $y$ and $z$ into one larger box, its volume is at most the product of the two smaller volumes (Fischer's inequality), so logarithmic covolumes simply add when you adjoin extra vectors.

In the Stage 01 reduction, vectors live over {{concept:complex-plane|the two-dimensional plane of numbers with real and imaginary coordinates}} inside a {{concept:twisted-lattice|system of twisted linear embeddings equipped with prime-by-prime integrality rules}}. Bounding the Gram determinant of a completed basis supplies the archimedean side of {{concept:arithmetic-height|the global balance between ordinary magnitude and prime-by-prime divisibility}}: it gives an upper cap on how large the determinant can be, which must then be compatible with how divisible the determinant is at the primes.

### Limits of the intuition

Visualizing the Gram determinant as the squared volume of a real box faithfully explains why linear dependence gives zero, why orthogonal vectors maximize volume (Hadamard's inequality), and why gluing two families adds logarithmic covolumes. The picture breaks down over {{term:complex-plane|complex numbers}} once algebraic conjugates enter: to compare with prime-by-prime divisibility, we must bound not only the actual determinant $\det(\langle \Theta_1 y_i, \Theta_1 y_k \rangle)$ but also its image $\sigma(\det(\langle \Theta_1 y_i, \Theta_1 y_k \rangle))$ under every field automorphism $\sigma$ of $\mathbb{C}$, most of which are discontinuous and scramble ordinary Euclidean geometry. Fixing a rational inner-product matrix $G$ on coordinates restores control by expressing each conjugate determinant as an exterior-power pairing bounded by the covolumes at $\sigma$ and $\sigma'(z) = \overline{\sigma(\bar{z})}$.

## Level 2 — Undergrad Bridge

For vectors $v_1, \dots, v_n$ in a complex inner product space $H$, the Gram matrix $M = (\langle v_i, v_j \rangle)_{1 \le i,j \le n} \in M_{n \times n}(\mathbb{C})$ is Hermitian positive semidefinite: for any $c \in \mathbb{C}^n$, $c^* M c = \|\sum_i c_i v_i\|^2 \ge 0$. Consequently $\det M \ge 0$, with $\det M > 0$ if and only if $v_1, \dots, v_n$ are linearly independent. Equivalently, $\det M = \|v_1 \wedge \dots \wedge v_n\|^2$ in $\bigwedge^n H$.

Three determinantal theorems from Hermitian matrix analysis (Horn & Johnson 2013, §7.2 and Theorems 7.8.1, 7.8.5) govern Gram determinants:

1. **Hadamard's inequality**: for any positive semidefinite matrix $M$,
   $$\det M \le \prod_{i=1}^n M_{ii}.$$
   Applied to $M_{ij} = \langle v_i, v_j \rangle$, if $\|v_i\|^2 \le e^h$ for every $i$, then $\det M \le e^{nh}$.
2. **Fischer's inequality**: if a positive semidefinite matrix is partitioned into blocks $M = \begin{pmatrix} A & B \\ B^* & D \end{pmatrix}$ with square diagonal blocks $A$ and $D$, then
   $$\det M \le (\det A)(\det D).$$
   Thus adjoining two families $y$ and $z$ multiplies their Gram determinant bounds, so logarithmic covolume bounds add: $c(y \cup z) \le c(y) + c(z)$.
3. **Cauchy–Schwarz in the exterior power**: for any positive semidefinite form $G \in M_{I \times I}(\mathbb{C})$ and two families $v, w : S \to \mathbb{C}^I$, the mixed pairing matrix $(v_i^* G w_k)$ satisfies
   $$\bigl|\det(v_i^* G w_k)\bigr|^2 \le \det(v_i^* G v_k) \cdot \det(w_i^* G w_k),$$
   proved by applying positive semidefiniteness to the block Gram matrix of $v \cup w$.

In a twisted lattice $L$, the trivial twist space $H_1$ has coordinates $\operatorname{coord} : H_1 \to \mathbb{C}^{\mathrm{Coord}}$ in which the inner product is represented by a positive-definite *rational* matrix $G$. For $\sigma \in \operatorname{Aut}(\mathbb{C})$, applying $\sigma$ entrywise to coordinates defines $\operatorname{conjCoord}(\sigma, y) = \sigma \circ \operatorname{coord}(\Theta_1 y)$, and rationality of $G$ yields
$$\sigma\bigl(\langle \Theta_1 v, \Theta_1 w \rangle\bigr) = \operatorname{conjCoord}(\sigma', v)^* G \operatorname{conjCoord}(\sigma, w), \qquad \sigma'(z) = \overline{\sigma(\bar{z})}.$$
Defining a family $y$ to have **covolume at most $c$** (`CovolumeLe y c`) by requiring $|\det(\operatorname{formGram}(G, \operatorname{conjCoord}(\sigma) \circ y))| \le e^c$ for all $\sigma \in \operatorname{Aut}(\mathbb{C})$ therefore immediately implies via exterior-power Cauchy–Schwarz that $|\sigma(\operatorname{Gram}(1))| \le e^c$ for every $\sigma \in \operatorname{Aut}(\mathbb{C})$.

Finally, when $V$ has dimension $d$ and $w_1, \dots, w_d$ is any basis of $V$, functoriality of the Gram determinant (`New.det_gram_comp`) gives $\det(\langle \Theta_\xi w_i, \Theta_\xi w_k \rangle) = Z(\xi) \det(\langle w_i, w_k \rangle)$, where $Z(\xi) = \det(\Theta_\xi^* \Theta_\xi)$. This identity allows Stage 01 to relate the trivial-twist Gram determinant $\operatorname{Gram}(1)$ to any twisted Gram determinant $\operatorname{Gram}(\xi)$ purely through the basis-independent ratio $Z(1)/Z(\xi)$.

## Level 3 — Formal Lean Contract

In `skeleton/01_reduction/twisted_lattice.lean` (extracted from upstream `src/New/Reduction/Lattices/TwistedLattice.lean`, backed by Hermitian Gram lemmas in `src/Lib/General/LinearAlgebra/GramDeterminant.lean`), logarithmic covolume and its Hadamard and Fischer closure properties have the following signatures:

- `01_reduction/twisted_lattice.lean#New.TwistedLattice.CovolumeLe` (upstream `src/New/Reduction/Lattices/TwistedLattice.lean:262-266`):
  ```lean
  def New.TwistedLattice.CovolumeLe (y : S → V) (c : ℝ) : Prop :=
    ∀ σ : ℂ ≃+* ℂ, ‖(formGram (L.gramMatrix.map (↑)) (L.conjCoord σ ∘ y)).det‖ ≤ exp c
  ```
- `01_reduction/twisted_lattice.lean#New.TwistedLattice.covolumeLe_of_normSqAt_le` (upstream `src/New/Reduction/Lattices/TwistedLattice.lean:301-310`, Hadamard's inequality):
  ```lean
  lemma New.TwistedLattice.covolumeLe_of_normSqAt_le {n : ℕ} {z : Fin n → V} {h : ℝ}
      (hz : ∀ (i : Fin n) (σ : ℂ ≃+* ℂ), L.normSqAt (z i) σ ≤ exp h) :
      L.CovolumeLe z (n * h)
  ```
- `01_reduction/twisted_lattice.lean#New.TwistedLattice.CovolumeLe.sumElim` (upstream `src/New/Reduction/Lattices/TwistedLattice.lean:312-325`, Fischer's inequality):
  ```lean
  lemma New.TwistedLattice.CovolumeLe.sumElim {S' : Type} [Fintype S'] [DecidableEq S']
      {y : S → V} {z : S' → V} {c c' : ℝ} (hy : L.CovolumeLe y c) (hz : L.CovolumeLe z c') :
      L.CovolumeLe (Sum.elim y z) (c + c')
  ```

Together with `CovolumeLe.norm_map_det_gram_le` (`TwistedLattice.lean:284-299`), these three declarations supply the exact archimedean bound `‖σ (Gram 1)‖ ≤ exp (c + n' * b_short)` inside `covolume_add_index_nonneg`. See the page's live verification banner (`audit/verification-status.json`) for the currently pinned upstream snapshot and local Lean build status (`build_status: unverified-locally`).
