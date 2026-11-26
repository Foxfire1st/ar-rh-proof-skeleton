---
slug: critical-line
term: critical line
aliases: []
inline_glimpse: "The vertical line Re(s) = 1/2 bisecting the critical strip—the axis of mirror symmetry s ↦ 1 − s where the Riemann Hypothesis asserts all nontrivial zeros lie."
visual_ref: assets/complex-plane-critical-strip.svg
where_analogy_breaks: "Thinking of the critical line as a narrow target wire where zeros happen to balance under symmetry explains why $1/2$ is the symmetry axis, not why off-line mirrored pairs $\\rho$ and $1 - \\rho$ cannot exist; ruling out off-line pairs requires positivity and arithmetic structure far beyond symmetry alone."
sources:
  - title: "Ueber die Anzahl der Primzahlen unter einer gegebenen Grösse"
    citation: "B. Riemann, Monatsberichte der Königlichen Preussischen Akademie der Wissenschaften zu Berlin (November 1859), pp. 671–680."
    kind: paper
  - title: "Problems of the Millennium: The Riemann Hypothesis"
    citation: "E. Bombieri, Clay Mathematics Institute (2000)."
    kind: reference
  - title: "Mathlib4: RiemannHypothesis"
    citation: "Mathlib.NumberTheory.LSeries.RiemannZeta (`RiemannHypothesis`, pinned mathlib rev 31ce06eaadf4a200c0e1407c5f10953cf606b8f8)."
    kind: mathlib
lean_anchors:
  - 00_statement/statements.lean#RiemannZetaZerosOnLine
  - 00_statement/main.lean#riemannHypothesis
child_concepts:
  - complex-plane
  - critical-strip
  - riemann-zeta-function
  - dirichlet-l-function
---

# Critical Line

## Level 1 — Abitur / First-Principles

Inside {{concept:complex-plane|the two-dimensional grid of real and imaginary coordinates}}, {{concept:critical-strip|the vertical band $0 < \operatorname{Re}(s) < 1$}} is symmetric left-to-right around its center. The vertical line running down that exact center—all points $s = \frac{1}{2} + it$ with real coordinate $\frac{1}{2}$ and arbitrary vertical coordinate $t$—is the **critical line**.

The reflection $s \mapsto 1 - s$ flips points across the critical line while keeping every point on the critical line fixed in real part ($\operatorname{Re}(1 - (\frac{1}{2} + it)) = \frac{1}{2}$). Both the Riemann Hypothesis for {{concept:riemann-zeta-function|the global function built from $\sum n^{-s}$}} and the Generalized Riemann Hypothesis for every {{concept:dirichlet-l-function|twisted periodic sum $L(\chi, s)$}} assert that no nontrivial zero ever leaves this midline. In terms of primes, every zero at horizontal coordinate $\sigma$ contributes an error wave of size $x^{\sigma}$ to the prime count up to $x$; forcing every zero onto $\sigma = 1/2$ means prime fluctuations stay as small as random square-root cancellation allows ($O(x^{1/2} \log^2 x)$).

### Limits of the intuition

Seeing the critical line as the balance axis of the symmetry $s \mapsto 1 - s$ explains why $1/2$ is the only possible single line for all zeros. Symmetry alone, however, allows off-line zeros to occur in mirrored quartets $\rho, \overline{\rho}, 1 - \rho, 1 - \overline{\rho}$—and there exist artificial {{concept:infinite-series|sums weighted by $n^{-s}$}} satisfying functional equations that do have off-line zeros. Forcing all zeros onto $\operatorname{Re}(s) = 1/2$ requires the arithmetic Euler product and deep positivity mechanisms, not just reflection symmetry.

## Level 2 — Undergrad Bridge

Writing $s = \frac{1}{2} + it$ turns the completed Riemann zeta function $\xi(s) = \frac{1}{2} s(s-1)\pi^{-s/2}\Gamma(s/2)\zeta(s)$ into a real-valued, even entire function $\Xi(t) = \xi(\frac{1}{2} + it)$ of the real variable $t$. Zeros of $\zeta(s)$ on the critical line correspond precisely to real zeros of $\Xi(t)$. Hardy (1914) proved that infinitely many zeros lie on $\operatorname{Re}(s) = 1/2$; Selberg (1942) proved a positive proportion do; and the Riemann Hypothesis asserts that *all* nontrivial zeros do.

For prime counting, the Riemann–von Mangoldt explicit formula writes
$$\psi(x) = x - \sum_{\rho} \frac{x^{\rho}}{\rho} - \ln(2\pi) - \frac{1}{2}\ln(1 - x^{-2}).$$
Because $|x^{\rho}| = x^{\operatorname{Re}(\rho)}$, if $\Theta = \sup\{\operatorname{Re}(\rho)\}$ over nontrivial zeros $\rho$, then $\psi(x) = x + O(x^{\Theta} \log^2 x)$ and conversely $\psi(x) - x = \Omega(x^{\Theta - \varepsilon})$ for every $\varepsilon > 0$. Because reflection pairs $\rho$ with $1 - \rho$, $\Theta$ can never be smaller than $1/2$; thus $\Theta = 1/2$—all nontrivial zeros on the critical line—is the sharpest possible bound.

## Level 3 — Formal Lean Contract

In Stage 00, the critical-line condition appears directly as the conclusion `s.re = 1 / 2` in three of the four statement predicates in `src/Statements.lean` (`RiemannZetaZerosOnLine`, `DirichletLZerosOnLine`, and `PrimitiveDirichletLZerosOnLine`) as well as in the headline Mathlib statement `RiemannHypothesis`, while the fourth predicate (`DirichletLNonvanishingRightOfHalf`, `src/Statements.lean:10-12`) encodes the equivalent right-half-plane condition by taking `1 / 2 < s.re` as a hypothesis and concluding `DirichletCharacter.LFunction χ s ≠ 0`:
- `00_statement/statements.lean#RiemannZetaZerosOnLine` (upstream `src/Statements.lean:7-8`):
  ```lean
  def RiemannZetaZerosOnLine : Prop :=
    ∀ s : ℂ, riemannZeta s = 0 → (¬∃ n : ℕ, s = -2 * ((n : ℂ) + 1)) → s ≠ 1 → s.re = 1 / 2
  ```
- `00_statement/main.lean#riemannHypothesis` (upstream `src/Main.lean:8-9`):
  ```lean
  theorem riemannHypothesis : RiemannHypothesis := New.zeta_zeros_on_line New.grh
  ```
Here `RiemannHypothesis` is Mathlib's built-in definition (`Mathlib.NumberTheory.LSeries.RiemannZeta`), which is definitionally identical to `RiemannZetaZerosOnLine`, allowing `src/Main.lean:11-12` to prove `riemannZeta_eq_zero_re` in one word (`riemannHypothesis`). See the page's live verification banner (`audit/verification-status.json`) for the currently pinned upstream snapshot and local Lean build status (`build_status: unverified-locally`).
