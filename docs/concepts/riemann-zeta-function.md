---
slug: riemann-zeta-function
term: Riemann zeta function
aliases:
  - zeta function
inline_glimpse: "The meromorphic function ζ(s) extending ∑ n⁻ˢ to all of ℂ ∖ {1}, linking prime numbers via its Euler product to zeros in the complex plane."
visual_ref: assets/prime-wave-zeta-zeros.svg
where_analogy_breaks: "Writing $\\zeta(s) = 1 + 2^{-s} + 3^{-s} + \\dots$ is valid only when $\\operatorname{Re}(s) > 1$; substituting $s = 1/2 + 14.13i$ directly into the sum produces a divergent series rather than $0$, so the equation $\\zeta(s) = 0$ on the critical line applies strictly to the analytic continuation."
sources:
  - title: "Ueber die Anzahl der Primzahlen unter einer gegebenen Grösse"
    citation: "B. Riemann, Monatsberichte der Königlichen Preussischen Akademie der Wissenschaften zu Berlin (November 1859), pp. 671–680."
    kind: paper
  - title: "The Theory of the Riemann Zeta-Function"
    citation: "E. C. Titchmarsh, 2nd ed. (revised by D. R. Heath-Brown), Oxford University Press (1986), Chapters 1–2."
    kind: book
  - title: "Mathlib4: riemannZeta"
    citation: "Mathlib.NumberTheory.LSeries.RiemannZeta (`riemannZeta`, `RiemannHypothesis`, pinned mathlib rev 31ce06eaadf4a200c0e1407c5f10953cf606b8f8)."
    kind: mathlib
lean_anchors:
  - 00_statement/statements.lean#RiemannZetaZerosOnLine
  - 00_statement/main.lean#riemannZeta_eq_zero_re
child_concepts:
  - prime-number
  - infinite-series
  - complex-plane
  - analytic-continuation
  - trivial-zeros
  - critical-strip
  - critical-line
---

# Riemann Zeta Function

## Level 1 — Abitur / First-Principles

For any real number $s > 1$, add the reciprocals of all positive whole numbers raised to the power $s$:
$$\zeta(s) = 1 + \frac{1}{2^s} + \frac{1}{3^s} + \frac{1}{4^s} + \dots$$
For $s = 2$, Euler proved this sum equals $\pi^2 / 6$; for $s = 1$, it becomes the harmonic sum and diverges to infinity. Allowing $s$ to vary over {{concept:complex-plane|the two-dimensional grid of real and imaginary coordinates}}, this {{concept:infinite-series|unending sum of weighted reciprocals}} converges whenever $\operatorname{Re}(s) > 1$ and factors into an infinite product over all {{concept:prime-number|whole numbers above $1$ that have no smaller divisors}}.

In 1859, Bernhard Riemann extended this formula to the entire plane (except a single pole at $s = 1$) using {{concept:analytic-continuation|the unique smooth extension of a complex function beyond where its original sum converges}}. That globally extended function is the **Riemann zeta function**. It vanishes at the negative even integers $-2, -4, -6, \dots$, which are called {{concept:trivial-zeros|predictable negative-integer zeros forced by the symmetry equation}}. Every other zero lies inside {{concept:critical-strip|the vertical band $0 < \operatorname{Re}(s) < 1$}}, and the Riemann Hypothesis states that every single one sits on {{concept:critical-line|the vertical line $\operatorname{Re}(s) = 1/2$}}.

### Limits of the intuition

Thinking of $\zeta(s)$ as the sum $\sum_{n=1}^{\infty} n^{-s}$ works only to the right of $\operatorname{Re}(s) = 1$. Inside the band $0 < \operatorname{Re}(s) < 1$ and to the left of $0$, the partial sums of $n^{-s}$ oscillate wildly or blow up. Computing $\zeta(s)$ there requires an alternative representation—such as the alternating Dirichlet eta formula $(1 - 2^{1-s})^{-1}\sum_{n=1}^{\infty} (-1)^{n-1} n^{-s}$ for $\operatorname{Re}(s) > 0$ except where $2^{1-s} = 1$ (the isolated points $s = 1 + \frac{2\pi i k}{\ln 2}$ on $\operatorname{Re}(s) = 1$), or Riemann's Mellin-integral representation valid on all of $\mathbb{C} \setminus \{1\}$.

## Level 2 — Undergrad Bridge

For $\operatorname{Re}(s) > 1$, the zeta function admits both a Dirichlet series and an Euler product:
$$\zeta(s) = \sum_{n=1}^{\infty} n^{-s} = \prod_{p} \left(1 - p^{-s}\right)^{-1}.$$
Multiplying by the archimedean gamma factor produces the completed zeta function
$$\Lambda(s) = \pi^{-s/2} \Gamma(s/2) \zeta(s),$$
which continues meromorphically to $\mathbb{C}$ with simple poles only at $s = 0$ and $s = 1$, and satisfies the functional equation
$$\Lambda(s) = \Lambda(1 - s).$$
This symmetry pairs any zero $\rho$ in $0 < \operatorname{Re}(s) < 1$ with $1 - \rho$ (and conjugation pairs $\rho$ with $\overline{\rho}$). For $\operatorname{Re}(s) \ge 1$, the Euler product (and Hadamard–de la Vallée Poussin's boundary theorem on $\operatorname{Re}(s) = 1$) forces $\zeta(s) \neq 0$. For $\operatorname{Re}(s) \le 0$, $\Lambda(s) = \Lambda(1 - s) \neq 0$ (except for the pole at $s = 0$ where $\zeta(0) = -1/2$), so $\zeta(s)$ vanishes there precisely at the poles of $\Gamma(s/2)$ with $s \neq 0$, namely $s = -2, -4, -6, \dots$.

Because $\zeta(s)$ is also the Dirichlet $L$-function of the trivial character modulo $1$, proving nonvanishing of all Dirichlet $L$-functions for $\operatorname{Re}(s) > 1/2$ immediately forces $\zeta(s) \neq 0$ on $1/2 < \operatorname{Re}(s) < 1$, and functional-equation symmetry $s \mapsto 1 - s$ then rules out $0 < \operatorname{Re}(s) < 1/2$ as well.

## Level 3 — Formal Lean Contract

In Mathlib4 (`Mathlib.NumberTheory.LSeries.RiemannZeta`), `riemannZeta : ℂ → ℂ` is defined via the Mellin transform of the Jacobi theta function and equals `completedRiemannZeta s / Gammaℝ s` away from `s = 0` (with `riemannZeta 0 = -1 / 2`; `s ≠ 1` is explicitly excluded in the hypothesis so the statement stays independent of Mathlib's total-function junk value at the simple pole $s = 1$, even though Mathlib defines `riemannZeta 1` to be nonzero via `riemannZeta_one_ne_zero`, making the guard redundant but harmless).

Stage 00 anchors this function in:
- `00_statement/statements.lean#RiemannZetaZerosOnLine` (upstream `src/Statements.lean:7-8`):
  ```lean
  def RiemannZetaZerosOnLine : Prop :=
    ∀ s : ℂ, riemannZeta s = 0 → (¬∃ n : ℕ, s = -2 * ((n : ℂ) + 1)) → s ≠ 1 → s.re = 1 / 2
  ```
- `00_statement/main.lean#riemannZeta_eq_zero_re` (upstream `src/Main.lean:11-12`), which witnesses `RiemannZetaZerosOnLine` directly from `riemannHypothesis`.

See the page's live verification banner (`audit/verification-status.json`) for the currently pinned upstream snapshot and local Lean build status (`build_status: unverified-locally`; entry static scan reports `0` forbidden tokens in solution files and `5` expected `sorry`s in root `Challenge.lean`).
