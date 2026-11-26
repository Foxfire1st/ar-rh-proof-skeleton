---
slug: complex-plane
term: complex plane
aliases:
  - complex number
inline_glimpse: "The two-dimensional coordinate plane of numbers s = σ + it, where the horizontal coordinate σ = Re(s) sets decay rate and the vertical coordinate t = Im(s) sets oscillation frequency."
visual_ref: assets/complex-plane-critical-strip.svg
where_analogy_breaks: "Viewing the complex plane purely as $\\mathbb{R}^2$ with arrows explains addition and length, but hides complex multiplication as rotation-scaling and misses the extreme rigidity of complex-differentiable functions compared to smooth real maps on $\\mathbb{R}^2$."
sources:
  - title: "Complex Analysis"
    citation: "L. V. Ahlfors, 3rd ed., McGraw-Hill (1979), Chapters 1–2."
    kind: book
  - title: "Complex Analysis"
    citation: "E. M. Stein and R. Shakarchi, Princeton Lectures in Analysis II, Princeton University Press (2003), Chapter 1."
    kind: book
  - title: "Mathlib4: Complex"
    citation: "Mathlib.Data.Complex.Basic (pinned mathlib rev 31ce06eaadf4a200c0e1407c5f10953cf606b8f8)."
    kind: mathlib
lean_anchors: []
child_concepts:
  - infinite-series
  - analytic-continuation
  - critical-strip
  - critical-line
---

# Complex Plane

## Level 1 — Abitur / First-Principles

Every point on the familiar number line is fixed by one coordinate. Extend that line into a flat two-dimensional grid by adding a vertical axis measured in multiples of the imaginary unit $i$, defined by $i^2 = -1$. A point $s = \sigma + it$ (with real numbers $\sigma$ and $t$) is a **complex number**, and the full two-dimensional grid of all such points is the **complex plane**. Here $\sigma = \operatorname{Re}(s)$ is the horizontal coordinate (real part) and $t = \operatorname{Im}(s)$ is the vertical coordinate (imaginary part).

Why does a question about whole numbers move into two dimensions? In $n^{-s} = n^{-\sigma} \cdot n^{-it} = n^{-\sigma} e^{-it \ln n}$, the two coordinates play cleanly separated roles: the horizontal coordinate $\sigma$ controls how fast the term shrinks, while the vertical coordinate $t$ spins the term around the unit circle at frequency $\ln n$. When we add these rotating arrows inside {{concept:infinite-series|an unending sum of terms weighted by $n^{-s}$}} and extend the resulting function across the plane by {{concept:analytic-continuation|the unique smooth extension of a complex function beyond its initial domain}}, the zeros that control prime fluctuations sit inside {{concept:critical-strip|the vertical band between horizontal coordinates $0$ and $1$}}, and the Riemann Hypothesis predicts they all align on {{concept:critical-line|the vertical midline at horizontal coordinate $1/2$}}.

### Limits of the intuition

Thinking of the complex plane as a flat vector grid $\mathbb{R}^2$ accurately represents vector addition and distance $|s| = \sqrt{\sigma^2 + t^2}$. The analogy breaks once calculus begins: a smooth map from $\mathbb{R}^2$ to $\mathbb{R}^2$ can be bent locally without changing values far away, whereas a complex-differentiable function must preserve angles locally (the Cauchy–Riemann equations), locking its values across the entire connected domain.

## Level 2 — Undergrad Bridge

Algebraically, $\mathbb{C} \cong \mathbb{R}[x]/(x^2 + 1)$ is an algebraically closed complete normed field with norm $|s| = \sqrt{\sigma^2 + t^2}$ and conjugation $\overline{\sigma + it} = \sigma - it$. Multiplication by $r e^{i\theta}$ scales lengths by $r$ and rotates counterclockwise by $\theta$.

A function $f : U \to \mathbb{C}$ on an open set $U \subseteq \mathbb{C}$ is holomorphic (complex-differentiable) at $s_0 \in U$ if the limit
$$f'(s_0) = \lim_{h \to 0} \frac{f(s_0 + h) - f(s_0)}{h}$$
exists as $h \to 0$ along every direction in $\mathbb{C}$. This single requirement forces $f$ to be infinitely differentiable and locally equal to its Taylor power series (Cauchy's integral formula). In analytic number theory, the complex plane partitions into three vertical zones relative to a Dirichlet $L$-function:
- $\operatorname{Re}(s) > 1$: the half-plane of absolute convergence and Euler product representation, free of zeros;
- $0 < \operatorname{Re}(s) < 1$: the open strip where all nontrivial zeros lie, symmetric under $s \mapsto 1 - s$;
- $\operatorname{Re}(s) \le 0$: the left half-plane determined from $\operatorname{Re}(s) \ge 1$ by the functional equation, containing only predictable zeros at non-positive integers for $\zeta(s)$ and **primitive** $\chi$ (whereas an **imprimitive** character $\chi$ also introduces extra Euler-factor zeros on $\operatorname{Re}(s) = 0$).

## Level 3 — Formal Lean Contract

In Mathlib4 (`Mathlib.Data.Complex.Basic`), `ℂ` (`Complex`) is a structure with two real fields `re : ℝ` and `im : ℝ`. Every Stage 00 statement in `src/Statements.lean` and root `Challenge.lean` (lines `7–20`, starting with `riemannHypothesis` at lines `7–8`) quantifies over `s : ℂ` and expresses horizontal location with the projection `s.re` (such as `0 < s.re`, `s.re < 1`, `1 / 2 < s.re`, and the conclusion `s.re = 1 / 2`); upstream's `comparator/FormalConjecturesChallenge.lean` wrapper is archived on `alpoge@c900e67c2` pending re-integration on condensed `main`. See the page's live verification banner (`audit/verification-status.json`) for the currently pinned upstream snapshot and local Lean build status (`build_status: unverified-locally`).
