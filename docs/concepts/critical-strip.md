---
slug: critical-strip
term: critical strip
aliases: []
inline_glimpse: "The open vertical band 0 < Re(s) < 1 in the complex plane that contains every nontrivial zero of ζ(s) and of every Dirichlet L-function."
visual_ref: assets/complex-plane-critical-strip.svg
where_analogy_breaks: "Visualizing the critical strip as a flat empty corridor between $0$ and $1$ hides its unbounded vertical height: zeros occur infinitely high along the imaginary axis with density $\\frac{T}{2\\pi} \\log \\frac{T}{2\\pi e}$ up to height $T$, and the function oscillates increasingly rapidly as $|\\operatorname{Im}(s)|$ grows."
sources:
  - title: "The Theory of the Riemann Zeta-Function"
    citation: "E. C. Titchmarsh, 2nd ed., Oxford University Press (1986), Chapters 2, 3, and 9."
    kind: book
  - title: "Analytic Number Theory"
    citation: "H. Iwaniec and E. Kowalski, AMS Colloquium Publications Vol. 53 (2004), Chapter 5, §§5.3–5.4."
    kind: book
lean_anchors:
  - 00_statement/statements.lean#DirichletLZerosOnLine
child_concepts:
  - complex-plane
  - riemann-zeta-function
  - trivial-zeros
  - critical-line
  - dirichlet-l-function
---

# Critical Strip

## Level 1 — Abitur / First-Principles

Picture {{concept:complex-plane|the two-dimensional grid of real and imaginary coordinates}} divided by two vertical fences: one at horizontal coordinate $\operatorname{Re}(s) = 0$ and one at $\operatorname{Re}(s) = 1$. Everything strictly to the right of $1$ is free of zeros because the product over primes never vanishes there. Everything on or to the left of $0$ is completely understood by mirror symmetry from the right half, leaving only {{concept:trivial-zeros|predictable non-positive integer zeros}} (plus, for non-primitive twists, extra zeros sitting right on the left fence $\operatorname{Re}(s) = 0$).

The open vertical band strictly between those two fences—$0 < \operatorname{Re}(s) < 1$—is the **critical strip**. Every mysterious zero of {{concept:riemann-zeta-function|the global function extending $\sum_{n=1}^{\infty} n^{-s}$}} and of every {{concept:dirichlet-l-function|twisted periodic sum $L(\chi, s)$}} must live inside this strip. Down the exact center of the strip runs {{concept:critical-line|the vertical midline $\operatorname{Re}(s) = 1/2$}}, which acts as the strip's mirror axis: if a zero lies inside the strip off the center, reflection across the midline produces a paired zero on the opposite side.

### Limits of the intuition

Drawing the critical strip as a small rectangle on paper captures the horizontal bounds $0 < \operatorname{Re}(s) < 1$, but obscures that the strip stretches to $+\infty$ and $-\infty$ vertically. As the height $|t|$ grows, the density of zeros rises logarithmically and the values of $\zeta(\sigma + it)$ fluctuate more violently; ruling out zeros across the entire infinite strip requires uniform analytic control at all heights, not finite numerical checking.

## Level 2 — Undergrad Bridge

The open critical strip is $S = \{s \in \mathbb{C} : 0 < \operatorname{Re}(s) < 1\}$. Three classical facts isolate all nontrivial zeros of $\zeta(s)$ and of every primitive $L(\chi, s)$ to $S$:
1. **Euler product ($\operatorname{Re}(s) > 1$):** Absolute convergence of $\prod_p (1 - \chi(p)p^{-s})^{-1}$ prevents vanishing for $\operatorname{Re}(s) > 1$.
2. **Boundary nonvanishing ($\operatorname{Re}(s) = 1$):** Hadamard and de la Vallée Poussin proved $\zeta(1 + it) \neq 0$ (and $L(\chi, 1 + it) \neq 0$ for all $t \in \mathbb{R}$, with $t \neq 0$ when $\chi = 1$): specifically, $\zeta(1 + it) \neq 0$ ($t \neq 0$) is equivalent to the Prime Number Theorem, $L(\chi, 1) \neq 0$ ($\chi \neq 1$) yields Dirichlet's theorem on primes in arithmetic progressions, and nonvanishing along the entire line $\operatorname{Re}(s) = 1$ yields the Prime Number Theorem in arithmetic progressions.
3. **Functional equation ($\operatorname{Re}(s) \le 0$):** Reflecting $s \mapsto 1 - s$ maps $\{\operatorname{Re}(s) \le 0\}$ onto $\{\operatorname{Re}(s) \ge 1\}$, showing that the completed function $\Lambda(\chi, s)$ has no zeros for $\operatorname{Re}(s) \le 0$.

Within $S$, the reflection $s \mapsto 1 - s$ maps the left half $0 < \operatorname{Re}(s) < 1/2$ bijectively onto the right half $1/2 < \operatorname{Re}(s) < 1$. Because a zero $\rho \in S$ of $L(\chi, s)$ reflects to a zero $1 - \rho \in S$ of $L(\overline{\chi}, s)$ (after reducing $\chi$ to its primitive character), showing that *no* Dirichlet $L$-function vanishes on $1/2 < \operatorname{Re}(s) < 1$ automatically clears the entire strip $S$ except the midline $\operatorname{Re}(s) = 1/2$.

## Level 3 — Formal Lean Contract

Stage 00 encodes the critical-strip condition directly in `00_statement/statements.lean#DirichletLZerosOnLine` (upstream `src/Statements.lean:14-16`):

```lean
def DirichletLZerosOnLine : Prop :=
  ∀ (N : ℕ) [NeZero N] (χ : DirichletCharacter ℂ N) (s : ℂ),
    0 < s.re → s.re < 1 → DirichletCharacter.LFunction χ s = 0 → s.re = 1 / 2
```

Restricting `s` to `0 < s.re` and `s.re < 1` automatically excludes both the Euler-product nonvanishing region (`1 ≤ s.re`), the pole at `s = 1`, all trivial zeros (`s.re ≤ 0`), and any imprimitive Euler-factor zeros on `s.re = 0`. Upstream `Claim.strip` (`src/Development/AutomorphicArithmetic/CharacterBoxes/ClaimStrip.lean:89-103`) deduces `DirichletLZerosOnLine` from `DirichletLNonvanishingRightOfHalf` by trichotomy on `s.re` versus `1 / 2`. Upstream provenance is pinned at commit `c900e67c2fcd1fdb041685820bbd2b867fbc5a4b` (`build_status: unverified-locally`).
