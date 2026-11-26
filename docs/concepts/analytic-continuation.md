---
slug: analytic-continuation
term: analytic continuation
aliases:
  - meromorphic continuation
inline_glimpse: "The unique extension of a complex-differentiable function from a smaller region to a larger domain, locked in place by the identity theorem."
visual_ref: assets/analytic-continuation-bridge.svg
where_analogy_breaks: "Comparing analytic continuation to drawing a smooth curve through data points suggests many possible continuations; for complex-differentiable functions, the extension to a connected domain is rigid and unique, though it can hit isolated poles or impassable natural boundaries."
sources:
  - title: "Complex Analysis"
    citation: "L. V. Ahlfors, 3rd ed., McGraw-Hill (1979), Chapter 8: Global Analytic Functions."
    kind: book
  - title: "Complex Analysis"
    citation: "E. M. Stein and R. Shakarchi, Princeton Lectures in Analysis II, Princeton University Press (2003), Chapters 2 and 6."
    kind: book
  - title: "Mathlib4: DirichletContinuation"
    citation: "Mathlib.NumberTheory.LSeries.DirichletContinuation (`riemannZeta`, `DirichletCharacter.LFunction`, pinned mathlib rev 31ce06eaadf4a200c0e1407c5f10953cf606b8f8)."
    kind: mathlib
lean_anchors: []
child_concepts:
  - complex-plane
  - infinite-series
  - riemann-zeta-function
  - trivial-zeros
---

# Analytic Continuation

## Level 1 — Abitur / First-Principles

Suppose a formula works only on part of {{concept:complex-plane|the two-dimensional grid of real and imaginary coordinates}}. For example, the geometric {{concept:infinite-series|unending sum}} $1 + z + z^2 + z^3 + \dots$ converges only inside the unit circle $|z| < 1$, where it equals $\frac{1}{1 - z}$. Yet the right-hand formula $\frac{1}{1 - z}$ makes sense at every point except $z = 1$, and agrees with the sum everywhere inside the circle.

Because complex-differentiable functions are completely determined by their values on any tiny open patch, there is at most one complex-differentiable way to extend a function onto a larger connected region (allowing isolated blow-up points called poles). That unique extension is **analytic continuation** (or **meromorphic continuation** when poles are permitted). For {{concept:riemann-zeta-function|the master function defined initially by $\sum_{n=1}^{\infty} n^{-s}$ for $\operatorname{Re}(s) > 1$}}, analytic continuation extends the function across the barrier $\operatorname{Re}(s) = 1$ to the whole plane except a single pole at $s = 1$, revealing both the unknown zeros inside the middle band and {{concept:trivial-zeros|the predictable zeros at negative even integers}}.

### Limits of the intuition

Imagining analytic continuation as smoothly extending a real graph is misleading in two opposite ways. On one hand, a smooth real function on $(1, \infty)$ can be extended to $\mathbb{R}$ in infinitely many smooth ways, whereas a complex-analytic extension is unique whenever it exists. On the other hand, not every function can be continued everywhere: some series (such as $\sum z^{2^n}$) hit a dense wall of singularities called a natural boundary beyond which no continuation exists at all.

## Level 2 — Undergrad Bridge

The uniqueness of analytic continuation rests on the **Identity Theorem**: if two holomorphic functions $f, g$ on a connected open set $\Omega \subseteq \mathbb{C}$ agree on a subset having an accumulation point in $\Omega$, then $f \equiv g$ on all of $\Omega$. In particular, once $\zeta(s)$ is defined on $\{\operatorname{Re}(s) > 1\}$ by $\sum n^{-s}$, any meromorphic function on $\mathbb{C}$ that matches $\sum n^{-s}$ on $\{\operatorname{Re}(s) > 1\}$ is the unique meromorphic continuation of $\zeta(s)$.

Riemann constructed this continuation via the Jacobi theta function $\theta(t) = \sum_{n \in \mathbb{Z}} e^{-\pi n^2 t}$ ($t > 0$) and its modular transformation $\theta(1/t) = t^{1/2} \theta(t)$. Splitting the Mellin integral at $t = 1$ gives
$$\pi^{-s/2}\Gamma(s/2)\zeta(s) = \frac{1}{s(s-1)} + \int_1^{\infty} \left(t^{s/2 - 1} + t^{(1-s)/2 - 1}\right)\frac{\theta(t) - 1}{2}\,dt.$$
Because $\theta(t) - 1$ decays exponentially as $t \to \infty$, the integral converges for every $s \in \mathbb{C}$ and defines an entire function. The rational term $\frac{1}{s(s-1)}$ supplies the only poles (simple poles at $s = 0$ and $s = 1$), and the integrand is manifestly invariant under $s \mapsto 1 - s$. The exact same mechanism (using twisted theta series) continues every nontrivial primitive Dirichlet $L$-function to an entire function on $\mathbb{C}$ (excluding the trivial character modulo $1$, which equals $\zeta(s)$ with its simple pole at $s = 1$).

## Level 3 — Formal Lean Contract

Mathlib4 implements this continuation in `Mathlib.NumberTheory.LSeries.DirichletContinuation` and `Mathlib.NumberTheory.LSeries.RiemannZeta` using Mellin transforms of theta functions (`completedHurwitzZetaEven`, `completedHurwitzZetaOdd`, `completedRiemannZeta`, and `DirichletCharacter.completedLFunction`). Upstream `src/Statements.lean:3` uses `public import Mathlib` (transitively bringing in `Mathlib.NumberTheory.LSeries.DirichletContinuation`), guaranteeing that `riemannZeta` and `DirichletCharacter.LFunction` denote these globally continued meromorphic functions rather than the raw `LSeries` sum. (Upstream's parallel `formal-conjectures` statement module, `comparator/FormalConjecturesStatement.lean`, is archived on `alpoge@c900e67c2` pending re-integration on condensed `main`.) See the page's live verification banner (`audit/verification-status.json`) for the currently pinned upstream snapshot and local Lean build status (`build_status: unverified-locally`).
