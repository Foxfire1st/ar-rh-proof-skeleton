---
slug: hecke-operator
term: Hecke operator
aliases: ["scaled Hecke operator", "edge Hecke operator", "parahoric subgroup"]
inline_glimpse: "An averaging operator that sums values of an automorphic form over neighboring sublattices—indexed by cosets of a parahoric subgroup—which Stage 03 scales by powers of a prime to eliminate collision losses in the composite theta lift."
visual_ref: assets/stage03-four-case-divisibility-and-hecke-colength.svg
where_analogy_breaks: "Thinking of a Hecke operator as a simple graph adjacency average on vertices captures why summing over neighbors acts linearly and preserves integrality, but misses three layers: the neighbor steps are cosets of a two-block parahoric subgroup of $\\mathrm{GL}_k(W)$ carrying explicit basis-change matrices (`originBasisChange`, `terminusBasisChange`) and module transitions (`transition`) in `New.EdgeCoordinates`; at an exceptional non-split prime $p < x_1$, Stage 03 does not apply a raw Hecke operator but raises a product over a full mark $\\Lambda$ of `scaledHeckeOperator` (each rescaled by $p^{A_l}$) to the $m$-th power; and the resulting operator is not surjective on integral source forms, only surjective modulo $p^{j_p}$ up to an explicit colength budget $\\operatorname{loss}(p)\\cdot \\dim(\\text{SourceForm})$."
sources:
  - title: "Introduction to the Arithmetic Theory of Automorphic Functions"
    citation: "G. Shimura, Princeton Univ. Press (1971), ch. 3."
    kind: book
  - title: "Estimées pour les valuations p-adiques des valeurs propres des opérateurs de Hecke"
    citation: "V. Lafforgue, Bull. SMF 139 (2011)."
    kind: paper
  - title: "Reductive groups over local fields"
    citation: "J. Tits, Proc. Sympos. Pure Math. 33 (1979)."
    kind: paper
lean_anchors:
  - 03_upper_bound/local_divisibility.lean#New.Data.IsWeakChainDatum.gaugeLe_restrictedChain_of_isFullMark
  - 03_upper_bound/local_divisibility.lean#New.Data.IsMarked.isOntoUpToColength_prod_scaledHeckeOperator_pow
  - 03_upper_bound/elementary_divisor_theorem.lean#New.EdgeCoordinates
  - 03_upper_bound/elementary_divisor_theorem.lean#New.FrobeniusCycle.edgeHeckeOperator
child_concepts:
  - prime-number
  - theta-correspondence
  - weyl-gauge
  - elementary-divisor
  - frobenius-cycle
  - arithmetic-height
---

# Hecke Operator

## Level 1 — First-Principles

Imagine a function attached not to points on a line, but to lattices—regular grids of vectors in a higher-dimensional space. Given a {{concept:prime-number|indivisible whole number $p \in \{2, 3, 5, 7, \dots\}$}}, you can modify a lattice by shrinking a chosen subset of $r$ independent basis directions by a factor of $p$ while keeping the remaining $k - r$ directions unchanged. Each such modification produces a neighboring sublattice, and a **Hecke operator** averages the values of a function across all neighbors of a given lattice. Just as averaging a signal over adjacent nodes on a network smooths out local irregularities without destroying global symmetries, a **Hecke operator** acts linearly on automorphic forms while commuting with the underlying arithmetic structure.

How do we list every neighbor cleanly? If you change the basis of a rank-$k$ lattice over a local ring $W$ using an invertible matrix in $\mathrm{GL}_k(W)$, the new basis defines the same neighbor precisely when the change of basis preserves the distinguished $r$-dimensional subspace modulo the maximal ideal of $W$. Those stabilizing matrices—matrices whose off-diagonal block connecting the last $k - r$ coordinates into the first $r$ lies in the maximal ideal—form a **parahoric subgroup**. The cosets of this **parahoric subgroup** index every outgoing edge from a lattice to its neighbors. In Stage 03, a discrete group $\Gamma$ acts freely with finitely many orbits on pairs consisting of a lattice and an auxiliary coefficient module, so a finite set of orbit representatives $X$ together with the **parahoric subgroup** cosets turns the lattice graph into a concrete finite coordinate system (`New.EdgeCoordinates`). Summing a form's transported values over all incoming edges into each representative $x \in X$ yields the **edge Hecke operator**.

Why does Stage 03 need **Hecke operators** at all? In the upper-bound proof, Stage 03 passes source forms through an $m$-step {{concept:theta-correspondence|integral-kernel transform pairing automorphic forms between two dual symmetry groups}} and measures how strongly the output is divisible by $p$ using a {{concept:weyl-gauge|non-archimedean local size bound tracking divisibility at every adelic point}}, contributing to the global {{concept:arithmetic-height|balance between ordinary magnitude and prime-by-prime divisibility}}. At most primes, the chain of $m$ lifts delivers its full target divisibility directly. At an exceptional small non-split prime $p < x_1$, however, paired infinite places collide, threatening to erode that divisibility on every single lift. Stage 03 repairs those collisions by selecting a *full mark* $\Lambda$—one prime of the field $E$ above $p$ from each conjugate pair induced by the long-row $a$-places—and multiplying each local **Hecke operator** at $l \in \Lambda$ by an explicit prime power $p^{A_l}$, creating a **scaled Hecke operator**. Taking the $m$-th power $Y^m$ of their product $Y$ across $\Lambda$ achieves a crucial two-sided balance:

1. **Divisibility restoration:** Applying $Y^m$ before the $m$-step lift cancels the collision penalty and restores the output's divisibility exponent up to $2\,\operatorname{loss}(p)$ (`gaugeLe_restrictedChain_of_isFullMark`).
2. **Small loss of room:** Enlarging the image of $Y^m$ by $p^{j_p}$ times all integral source forms leaves only a tiny index—measured via {{concept:elementary-divisor|prime-power invariant factors and determinant-divisibility index of a sublattice}}—bounded by $\operatorname{loss}(p)\cdot \dim(\text{SourceForm})$ (`isOntoUpToColength_prod_scaledHeckeOperator_pow`), proved by reducing the **edge Hecke operator** to a {{concept:frobenius-cycle|cyclic system of prime-power twisted embeddings with alternating actions}}.

### Limits of the intuition

Picturing a **Hecke operator** as an ordinary graph adjacency average on vertices captures linearity and integrality, but breaks down in three concrete ways. First, each directed edge $e = (x, b)$ in `New.EdgeCoordinates` is not merely a pair of vertices: it carries a coset $b$ of the two-block **parahoric subgroup** of $\mathrm{GL}_k(W)$, adapted lattice basis changes (`originBasisChange`, `terminusBasisChange`) factoring the group element $\delta^{-1}$ through $\operatorname{diag}(p^{-1}, \dots, p^{-1}, 1, \dots, 1)$, and a coefficient-module matrix `transition e` in $\mathrm{GL}_H(O)$. Second, at an exceptional non-split prime $p < x_1$, Stage 03 never applies a bare **Hecke operator**: it weights each factor at $l \in \Lambda$ by $p^{A_l}$ into a **scaled Hecke operator** and takes the $m$-th power $Y^m$ of their product so one factor accompanies each of the $m$ lifts. Third, $Y^m$ is not surjective on the integral source forms $M$; the theorem controls only the enlarged submodule $Y^m(M) + p^{j_p} M$, whose colength exponent in $M$ is bounded by $\operatorname{loss}(p) \cdot \dim(\text{SourceForm})$ rather than zero.

## Level 2 — Undergrad Bridge

Let $W$ be a local ring with finite residue field of size $q = P^N$, and fix integers $0 < r \le k/2$. The two-block labelling `twoBlock k r : Fin k → ℕ` assigns $0$ to indices $b < r$ and $1$ to $b \ge r$. The **parahoric subgroup** (Tits 1979) attached to `twoBlock k r` is
$$\operatorname{parahoric}(W, \operatorname{twoBlock}_{k,r}) = \bigl\{g \in \mathrm{GL}_k(W) : g_{i,j} \in \mathfrak{m}_W \text{ whenever } i < r \le j\bigr\},$$
and its coset space $\operatorname{ParahoricCosets}(W, k, r) = \mathrm{GL}_k(W) / \operatorname{parahoric}(W, \operatorname{twoBlock}_{k,r})$ is finite, parametrizing the sublattices adjacent to a fixed rank-$k$ $W$-lattice (classically double cosets of $\operatorname{diag}(p^{-1},\dots,p^{-1},1,\dots,1)$ in Shimura 1971, ch. 3).

1. **Edge coordinates and the edge Hecke operator (`New.EdgeCoordinates`, `New.FrobeniusCycle.edgeHeckeOperator`):** Suppose a group $\Gamma$ acts freely with finitely many orbits (represented by $X$) on pairs consisting of a rank-$k$ $W$-lattice and a free $O$-module of basis $H$. An edge $e = (x, b) \in X \times \operatorname{ParahoricCosets}(W, k, r)$ into $x$ records:
   - its origin orbit representative $\operatorname{origin}(e) \in X$,
   - lattice basis changes $\operatorname{originBasisChange}(e), \operatorname{terminusBasisChange}(b) \in \mathrm{GL}_k(W)$ with $(\operatorname{terminusBasisChange}(b))^{-1}$ representing the coset $b$ (modelling $\delta^{-1} = \operatorname{originBasisChange}(e)\cdot \operatorname{diag}(p^{-1},\dots,p^{-1}, 1,\dots,1)\cdot \operatorname{terminusBasisChange}(b)$),
   - a transition matrix $\operatorname{transition}(e) \in \mathrm{GL}_H(O)$ transporting the coefficient module,
   - freeness (`eq_of_origin_eq`): an edge is uniquely determined by $\operatorname{origin}(e)$ and the parahoric coset of $\operatorname{originBasisChange}(e)^\mathsf{T}$.

   Given a Frobenius cycle $T$ with $N$ letters, degree-$n$ representation $\rho = T.\operatorname{letterRep}\ n$, diagonal depth-power matrix $D^2 = T.\operatorname{depthPower}\ k\ n\ r$ (entries $p^{\operatorname{depth}(\beta)}$), and a splitting $J : I \simeq \operatorname{Exponents}(N, k, n) \times H$, the **edge Hecke operator** acts on $F : X \to I \to O$ entrywise by
   $$\bigl(T.\operatorname{edgeHeckeOperator}\ n\ J\ \mathcal{E}\ F\bigr)(x, i) = \sum_{b \in \operatorname{ParahoricCosets}} \sum_{j \in I} \mathcal{E}.\operatorname{transition}(x, b)_{(J i)_2, (J j)_2}\cdot M(x, b)_{(J j)_1, (J i)_1}\cdot F\bigl(\mathcal{E}.\operatorname{origin}(x, b), j\bigr),$$
   where $M(x, b) = \rho(\mathcal{E}.\operatorname{originBasisChange}(x, b))\, D^2\, \rho(\mathcal{E}.\operatorname{terminusBasisChange}(b))$. Under the adjoint pairing $b_\beta$, this operator factors cleanly through edge restriction and dual-edge pairing (`edgeHeckeOperator_regroup`), opening the door to the elementary-divisor theorem.

2. **Scaled Hecke operators, valuation gain, and colength (`gaugeLe_restrictedChain_of_isFullMark`, `isOntoUpToColength_prod_scaledHeckeOperator_pow`):** Let $p < x_1$ be an exceptional non-split prime ($\chi_K(p) \neq 1$), $m$ the number of lifts, $f$ the number of infinite places of $E$, and $a$ the long-row length. A *marked set* $\Lambda$ is a set of primes of $E$ above $p$, disjoint from its complex conjugate, each induced by an $a$-place; it is a *full mark* (`D.IsFullMark Λ`) if it contains one prime from every conjugate pair induced by the $a$-places. For each $l \in \Lambda$, define
   $$A_l = D.\operatorname{heckeScalingExponent}(l) = a \cdot \#\{\text{$a$-places inducing } l\}, \qquad j_p = D.\operatorname{collisionExponent}(p) = \lceil 2 m f a / p \rceil,$$
   and let `hΛ.scaledHeckeOperator l` be the Hecke operator at $l$ of index $D.\operatorname{ceilLogLifts}(p)$ multiplied by $p^{A_l}$ (echoing the $p$-adic valuation scaling of Hecke operators in Lafforgue 2011). Setting $Y = \prod_{l \in \Lambda} (\text{scaledHeckeOperator}\ l)$:
   - **Valuation theorem:** For every weak chain datum $\Psi$ for a twist $\vartheta$ with $\gcd(p, \vartheta.\operatorname{normQ}) = 1$ and every integral source form $v \in D.\operatorname{integralSourceForms}$,
     $$\operatorname{GaugeLe}\ 1\ \bigl(p^{2 D.\operatorname{loss}(p) - D.\operatorname{gain}(p)}\bigr)\ \bigl(\operatorname{restrictedChain}\ \vartheta\ \Psi\ (Y^m v)\bigr),$$
     where $D.\operatorname{loss}(p) = f a (\log b)^2 / p$ (multiplied by $m$ if $p \equiv \pm 1 \pmod m$).
   - **Colength theorem:** Summing the per-dimension colength exponent $12 A_l / (m p) + j_p / (m^2 f)$ over $l \in \Lambda$ and multiplying by $m$ stays at most $D.\operatorname{loss}(p)$ (`mul_sum_heckeScalingExponent_le_loss`). Consequently, $Y^m$ preserves $M = D.\operatorname{integralSourceForms}$ and satisfies
     $$\operatorname{IsOntoUpToColength}\ p\ M\ j_p\ Y^m\ \bigl(D.\operatorname{loss}(p) \cdot \operatorname{finrank}_\mathbb{C} D.\operatorname{SourceForm}\bigr),$$
     meaning the index of $Y^m(M) + p^{j_p} M$ in $M$ divides $p^c$ for some $c \le D.\operatorname{loss}(p) \cdot \dim_\mathbb{C} D.\operatorname{SourceForm}$.

## Level 3 — Formal Lean Contract

Stage 03 extracts the local Hecke valuation and colength theorems into `skeleton/03_upper_bound/local_divisibility.lean` (from upstream `src/New/UpperBound/Divisibility/LocalDivisibility.lean`) and the coordinate edge Hecke operator into `skeleton/03_upper_bound/elementary_divisor_theorem.lean` (from upstream `src/New/UpperBound/ElementaryDivisors/ElementaryDivisorTheorem.lean`):

- `03_upper_bound/local_divisibility.lean#New.Data.IsWeakChainDatum.gaugeLe_restrictedChain_of_isFullMark` (upstream `src/New/UpperBound/Divisibility/LocalDivisibility.lean:246-285`):
  ```lean
  theorem New.Data.IsWeakChainDatum.gaugeLe_restrictedChain_of_isFullMark (hΨ : IsWeakChainDatum ϑ Ψ)
      (hQ : p.Coprime ϑ.normQ)
      {v : D.SourceForm} (hv : v ∈ D.integralSourceForms) (hΛ : D.IsFullMark Λ) :
      GaugeLe 1 (p ^ (2 * D.loss p - D.gain p))
        (restrictedChain ϑ Ψ (((Λ.attach.toList.map hΛ.scaledHeckeOperator).prod ^ D.m) v))
  ```
- `03_upper_bound/local_divisibility.lean#New.Data.IsMarked.isOntoUpToColength_prod_scaledHeckeOperator_pow` (upstream `src/New/UpperBound/Divisibility/LocalDivisibility.lean:328-357`):
  ```lean
  theorem New.Data.IsMarked.isOntoUpToColength_prod_scaledHeckeOperator_pow (hΛ : D.IsMarked Λ) :
      IsOntoUpToColength ↑p D.integralSourceForms (D.collisionExponent p)
        ((Λ.attach.toList.map hΛ.scaledHeckeOperator).prod ^ D.m)
          (D.loss p * finrank ℂ D.SourceForm)
  ```
- `03_upper_bound/elementary_divisor_theorem.lean#New.EdgeCoordinates` (upstream `src/New/UpperBound/ElementaryDivisors/ElementaryDivisorTheorem.lean:342-368`):
  ```lean
  structure New.EdgeCoordinates (W O : Type) [CommRing W] [IsLocalRing W] [CommRing O] (k r : ℕ)
      (X H : Type*) [Fintype H] [DecidableEq H] where
    origin : X × ParahoricCosets W k r → X
    originBasisChange : X × ParahoricCosets W k r → GL (Fin k) W
    terminusBasisChange : ParahoricCosets W k r → GL (Fin k) W
    transition : X × ParahoricCosets W k r → GL H O
    mk_terminusBasisChange_inv : ∀ b : ParahoricCosets W k r,
      QuotientGroup.mk (terminusBasisChange b)⁻¹ = b
    eq_of_origin_eq : ∀ e e' : X × ParahoricCosets W k r, origin e = origin e' →
      transposeGL (originBasisChange e) * (transposeGL (originBasisChange e'))⁻¹ ∈ parahoric W
        (twoBlock k r) →
        e = e'
  ```
- `03_upper_bound/elementary_divisor_theorem.lean#New.FrobeniusCycle.edgeHeckeOperator` (upstream `src/New/UpperBound/ElementaryDivisors/ElementaryDivisorTheorem.lean:742-754`):
  ```lean
  def New.FrobeniusCycle.edgeHeckeOperator (n : ℕ) (J : I ≃ Exponents N k n × H) (𝓔 : EdgeCoordinates W O k r X H) :
      End O (X → I → O)
  ```

Together, `gaugeLe_restrictedChain_of_isFullMark` and `isOntoUpToColength_prod_scaledHeckeOperator_pow` resolve the exceptional non-split prime case of `New.Data.exists_colengthDvd_gaugeLe_compositeLift`, with `New.EdgeCoordinates` and `New.FrobeniusCycle.edgeHeckeOperator` supplying the coordinate interface to the abstract elementary-divisor theorem. See the page's live verification banner (`audit/verification-status.json`) for the currently pinned upstream snapshot and local Lean build status (`build_status: unverified-locally`).
