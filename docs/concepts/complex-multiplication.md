---
slug: complex-multiplication
term: complex multiplication
aliases: ["CM curve", "CM lattice"]
inline_glimpse: "Extra scaling-and-rotation symmetries of a two-dimensional lattice in ℂ preserved by the integers 𝓞_K of an imaginary quadratic field K = ℚ(√(-d)), pinning down every Galois-conjugate lattice area via the Chowla–Selberg formula."
visual_ref: assets/stage02-tight-frame-to-integral-vacuum.svg
where_analogy_breaks: "Picturing complex multiplication as a static square or hexagonal grid symmetric under $90^\\circ$ or $60^\\circ$ rotation captures closure under non-integer multiplications, but misses the arithmetic content needed in Stage 02: for $d \\equiv 7 \\pmod 8$, the extra symmetry ring $\\mathcal{O}_K$ acts on a non-square lattice $\\Omega_0 \\mathcal{O}_K$ whose period $\\Omega_0$ is transcendental, and the Chowla–Selberg formula ties the areas of all Galois-conjugate lattices $\\mathcal{E}^\\sigma$ simultaneously to $L'(1, \\chi_K)/L(1, \\chi_K)$ up to a single globally $p$-adic unit $u$."
sources:
  - title: "Advanced Topics in the Arithmetic of Elliptic Curves"
    citation: "J. H. Silverman, Springer Graduate Texts in Mathematics 151 (1994), Chapter II."
    kind: book
  - title: "On Epstein's zeta-function"
    citation: "A. Selberg and S. Chowla, Journal für die reine und angewandte Mathematik 227 (1967), pp. 86–110."
    kind: paper
  - title: "On the periods of abelian varieties"
    citation: "B. H. Gross, Inventiones Mathematicae 45 (1978), pp. 193–211."
    kind: paper
lean_anchors:
  - 02_derivatives/tight_frames.lean#New.Parameters.cmCurve
  - 02_derivatives/tight_frames.lean#New.Parameters.latticeModule_cmCurve
  - 02_derivatives/tight_frames.lean#New.Parameters.area_mul_norm_chowlaSelbergUnit_sq
child_concepts:
  - complex-plane
  - prime-number
  - quadratic-twist
  - elliptic-theta-function
  - arithmetic-height
---

# Complex Multiplication

## Level 1 — First-Principles

Draw a regular two-dimensional grid of points—a lattice $\Lambda$—inside {{concept:complex-plane|the two-dimensional plane of real and imaginary coordinates}}. Multiplying every grid point by any whole number $n \in \mathbb{Z}$ simply stretches the grid by $n$ (or flips it if $n < 0$) and always lands back on the grid. For a generic tilted grid, whole numbers are the *only* multipliers that preserve $\Lambda$. A **CM lattice** is a rare, highly structured grid that admits extra scaling-and-rotation symmetries: multiplying by certain non-real algebraic integers—such as $i$ for a square grid or $\tfrac{1 + \sqrt{-3}}{2}$ for a hexagonal grid—still sends every lattice point onto another lattice point. Folding the plane along the grid yields a doughnut-shaped surface $\mathcal{E} = \mathbb{C}/\Lambda$ called a **CM curve**, and these extra symmetries are known as **complex multiplication**.

In Stage 02, the proof fixes an odd {{concept:prime-number|indivisible whole number $d \in \{2, 3, 5, 7, \dots\}$}} with $d \equiv 7 \pmod 8$—the same prime chosen via {{concept:quadratic-twist|multiplying characters by the square-non-square indicator modulo $d$}} in Stage 01—and builds a canonical **CM lattice** $\Omega_0 \mathcal{O}_K$ from the integer ring $\mathcal{O}_K = \mathbb{Z}\bigl[\tfrac{1 + \sqrt{-d}}{2}\bigr]$ of $K = \mathbb{Q}(\sqrt{-d})$, scaled by a single period $\Omega_0 \in \mathbb{C}^\times$. Because $\mathcal{O}_K$ is closed under multiplication, multiplying any point of this **CM lattice** by an element of $\mathcal{O}_K$ stays inside the lattice. That closure property is what lets higher-dimensional vectors from a tower of fields over $K$ land squarely on the **CM curve** when projected along Hermitian pairings, enabling the proof to assemble multi-variable {{concept:elliptic-theta-function|quasi-periodic entire functions on a lattice that shift by controlled exponential factors}} from one-variable pieces.

The second structural consequence of **complex multiplication** is how tightly it controls geometry across all algebraic symmetries of {{term:complex-plane|the complex plane}}. Applying any field automorphism $\sigma$ that fixes $K$ sends the **CM curve** $\mathcal{E}$ to a conjugate **CM curve** $\mathcal{E}^\sigma$ whose period parallelogram could a priori have a completely different area. The Chowla–Selberg formula rules out any such drift: there is a single constant $u$ that has size $1$ at every prime place—so it adds zero denominator cost to {{concept:arithmetic-height|the global balance between ordinary magnitude and divisibility at every prime}}—such that $\operatorname{Area}(\mathcal{E}^\sigma) \cdot |\sigma(u)|^2$ equals the exact same explicit constant for *every* automorphism $\sigma$. That uniform area identity pins down the size of all Galois-conjugated derivatives and locks the archimedean scale of Stage 02 into place.

### Limits of the intuition

Visualizing **complex multiplication** through the $90^\circ$ rotation of a square grid or the $60^\circ$ rotation of a hexagonal grid accurately conveys why extra symmetries are rare and discrete. The geometric picture breaks down in two ways in Stage 02. First, for primes $d \equiv 7 \pmod 8$, the **CM lattice** $\Omega_0 \mathcal{O}_K$ is not generated by roots of unity—its extra symmetries combine stretching and rotation by algebraic integers like $\tfrac{1 + \sqrt{-d}}{2}$, and the period $\Omega_0$ itself is a transcendental {{term:complex-plane|complex number}} whose conjugates cannot be computed elementarily. Second, the real leverage of a **CM curve** in this proof is arithmetic rather than purely geometric: the Chowla–Selberg identity links every conjugate lattice area directly to $L'(1, \chi_K)/L(1, \chi_K)$, while $u$ remains a unit at every prime place.

## Level 2 — Undergrad Bridge

Let $\Lambda \subset \mathbb{C}$ be a discrete rank-$2$ lattice, defining a complex elliptic curve $\mathcal{E} = \mathbb{C}/\Lambda$. Its endomorphism ring is
$$\operatorname{End}(\mathcal{E}) = \{\alpha \in \mathbb{C} : \alpha \Lambda \subseteq \Lambda\}.$$
Generically $\operatorname{End}(\mathcal{E}) = \mathbb{Z}$. When $\operatorname{End}(\mathcal{E})$ strictly contains $\mathbb{Z}$, $\mathcal{E}$ has **complex multiplication** (Silverman 1994, Ch. II), and $\operatorname{End}(\mathcal{E})$ is an order in an imaginary quadratic field $K = \mathbb{Q}(\sqrt{-d})$.

In Stage 02, $d = P.d$ is a prime satisfying $d \equiv 7 \pmod 8$, so $-d \equiv 1 \pmod 8$ is the fundamental discriminant of $K = \mathbb{Q}(\sqrt{-d})$ (in which $2$ splits) with maximal order $\mathcal{O}_K = \mathbb{Z}\bigl[\tfrac{1 + \sqrt{-d}}{2}\bigr]$. The parameters choose a fixed CM lattice `cmCurve` with period $\Omega_0 = \mathrm{cmPeriod} \in \mathbb{C}^\times$ such that
$$\Lambda(\mathcal{E}) = \Omega_0 \mathcal{O}_K, \qquad \operatorname{Area}(\mathcal{E}) = |\Omega_0|^2 \frac{\sqrt{d}}{2}.$$
Over the cyclotomic CM tower $E = P.E(s) = K(\zeta_{q_1})$ with CM type $\Phi = \operatorname{InfinitePlace}(E)$, two structural consequences drive the Vacuum and FormalGroup arguments:

1. **Integral Hermitian projections land in the CM lattice.** For elements $x, y \in \mathfrak{a}_0 = E \cap \alpha \widehat{\mathcal{O}}_E$ (having integral finite images under the Weil-data idele $\alpha$), the CM-type Hermitian pairing
   $$\langle x, y \rangle = \sum_{\tau \in \Phi} \tau(x)\,\overline{\tau(y)}$$
   lies in $\mathcal{O}_K$ (`sum_mul_conj_mem_quadInt`). Because `cmCurve` is preserved by $\mathcal{O}_K$, multiplying any lattice point $a \in C.\Lambda$ by $\langle y, x \rangle$ stays inside $C.\Lambda$ (`mul_sum_mul_conj_mem_lattice`). This is why rational shifts along a tight frame land on lattice shifts of one-variable theta sections.

2. **Uniform Galois control via Chowla–Selberg.** Every automorphism $\sigma \in \operatorname{Gal}(\mathbb{C}/K)$ sends the CM lattice $\mathcal{E}$ to a Galois-conjugate CM lattice $\mathcal{E}^\sigma$ (`galConjugate σ`), still having CM by $\mathcal{O}_K$. By the Chowla–Selberg formula (Selberg & Chowla 1967; Gross 1978), there exists a single complex number $u = \mathrm{chowlaSelbergUnit}$ that is a $p$-adic unit at every prime place ($\|\iota_p(u)\|_p = 1$ for all $p$) and satisfies, uniformly for all $\sigma \in \operatorname{Gal}(\mathbb{C}/K)$,
   $$\operatorname{Area}(\mathcal{E}^\sigma) \cdot |\sigma(u)|^2 = \pi \exp(c_d) \cdot \frac{2\pi}{\sqrt{d}}, \qquad c_d = \gamma - \operatorname{Re}\frac{L'(1, \chi_K)}{L(1, \chi_K)}.$$
   Setting $\mathrm{periodFactor} = \mathrm{vacuumRoot}/u$, $\mathrm{areaScale} = \sqrt{2\pi/\sqrt{d}}$, and $\mathrm{jetScale} = \mathrm{periodFactor} \cdot \mathrm{areaScale}/\Omega_0$, dividing by $u$ changes no $p$-adic valuation anywhere while canceling the conjugate lattice area at every infinite place: whenever $\operatorname{Area}(\mathcal{E}^\sigma) = |\Omega|^2 \sqrt{d}/2$,
   $$\left|\frac{\sigma(\mathrm{periodFactor})}{\Omega} \cdot \sqrt{\frac{2\pi}{\sqrt{d}}}\right|^2 = \Omega_{\mathrm{big}}^{-1}, \qquad \Omega_{\mathrm{big}} = \exp(c_d)\,\frac{2\pi}{\operatorname{rd}(E)},$$
   which is `norm_gal_periodFactor_div_mul_areaScale_sq` in `integral_vacuum.lean`. Together with uniform archimedean bounds on torsion values of $\theta$ on `cmCurve` (`exists_norm_gal_torsion_value_le`), this gives simultaneously $p$-adic integrality and uniform Galois-conjugate bounds for the integral vacuum and its jet series.

## Level 3 — Formal Lean Contract

Stage 02 extracts the canonical CM curve, its period-scaled ideal lattice, and the Galois-uniform Chowla–Selberg area identity into `skeleton/02_derivatives/tight_frames.lean` from upstream `src/New/Derivatives/Vacuum/TightFrames.lean`:

- `02_derivatives/tight_frames.lean#New.Parameters.cmCurve` (upstream `src/New/Derivatives/Vacuum/TightFrames.lean:85-88`):
  ```lean
  def New.Parameters.cmCurve : CMLattice P.d
  ```
- `02_derivatives/tight_frames.lean#New.Parameters.latticeModule_cmCurve` (upstream `src/New/Derivatives/Vacuum/TightFrames.lean:96-100`):
  ```lean
  lemma New.Parameters.latticeModule_cmCurve :
      P.cmCurve.latticeModule = QuadField.scaledIdeal P.d (P.cmPeriod, 1)
  ```
- `02_derivatives/tight_frames.lean#New.Parameters.area_mul_norm_chowlaSelbergUnit_sq` (upstream `src/New/Derivatives/Vacuum/TightFrames.lean:110-116`):
  ```lean
  lemma New.Parameters.area_mul_norm_chowlaSelbergUnit_sq (σ : Gal(ℂ/P.K)) :
      (P.cmCurve.galConjugate σ).area * ‖σ P.chowlaSelbergUnit‖ ^ 2 =
        π * rexp (chowlaSelbergConst P.d) * (2 * π / √P.d)
  ```

Companion declarations in `tight_frames.lean` fix `New.Parameters.cmPeriod : ℂˣ` (`TightFrames.lean:90-93`), `New.Parameters.chowlaSelbergUnit : ℂ` (`TightFrames.lean:102-104`), its global $p$-adic unit property `New.Parameters.norm_iota_chowlaSelbergUnit` (`TightFrames.lean:106-108`), and the closure lemma `New.Parameters.mul_sum_mul_conj_mem_lattice` (`TightFrames.lean:665-672`). Downstream in `02_derivatives/integral_vacuum.lean`, `New.Parameters.norm_gal_periodFactor_div_mul_areaScale_sq` (`IntegralVacuum.lean:469-489`) converts `area_mul_norm_chowlaSelbergUnit_sq` into the exact conjugate jet-scale norm `(P.bigOmega s)⁻¹`. See the page's live verification banner (`audit/verification-status.json`) for the currently pinned upstream snapshot and local Lean build status (`build_status: unverified-locally`).
