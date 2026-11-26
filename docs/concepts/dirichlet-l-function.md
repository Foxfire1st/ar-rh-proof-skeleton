---
slug: dirichlet-l-function
term: Dirichlet L-function
aliases:
  - Dirichlet L-series
inline_glimpse: "The meromorphic continuation of ∑ χ(n) n⁻ˢ associated to a Dirichlet character χ, generalizing ζ(s) to track primes across residue classes modulo N."
visual_ref: assets/dirichlet-remainder-wheel.svg
where_analogy_breaks: "Treating every Dirichlet L-function as an exact copy of $\\zeta(s)$ misses two crucial differences: for any non-principal character $\\chi \\neq 1$, $L(\\chi, s)$ has no pole at $s = 1$ (it is entire on $\\mathbb{C}$), and its functional equation pairs $\\chi$ with $\\chi^{-1}$ rather than with itself unless $\\chi$ is real."
sources:
  - title: "Multiplicative Number Theory"
    citation: "H. Davenport, 3rd ed., Springer Graduate Texts in Mathematics 74 (2000), Chapters 4, 9, and 12."
    kind: book
  - title: "Analytic Number Theory"
    citation: "H. Iwaniec and E. Kowalski, AMS Colloquium Publications Vol. 53 (2004), Chapter 5, §§5.1–5.9."
    kind: book
  - title: "Mathlib4: DirichletCharacter.LFunction"
    citation: "Mathlib.NumberTheory.LSeries.DirichletContinuation (`DirichletCharacter.LFunction`, `DirichletCharacter.completedLFunction`, pinned mathlib rev 31ce06eaadf4a200c0e1407c5f10953cf606b8f8)."
    kind: mathlib
lean_anchors:
  - 00_statement/statements.lean#DirichletLZerosOnLine
  - 00_statement/main.lean#LFunction_eq_zero_re
child_concepts:
  - infinite-series
  - riemann-zeta-function
  - analytic-continuation
  - dirichlet-character
  - primitive-character
  - critical-strip
  - critical-line
---

# Dirichlet L-function

## Level 1 — First-Principles

Take {{concept:riemann-zeta-function|the sum $\sum_{n=1}^{\infty} n^{-s}$ over all positive integers}} and multiply each term by {{concept:dirichlet-character|a periodic multiplicative weight $\chi(n)$ modulo $N$}}:
$$L(\chi, s) = \sum_{n=1}^{\infty} \frac{\chi(n)}{n^s} = \frac{\chi(1)}{1^s} + \frac{\chi(2)}{2^s} + \frac{\chi(3)}{3^s} + \dots$$
This weighted {{concept:infinite-series|unending sum}} converges whenever $\operatorname{Re}(s) > 1$ (and for any non-principal weight $\chi \neq 1$—one that is not equal to $1$ on all numbers coprime to $N$—its cancellations make the sum converge all the way down to $\operatorname{Re}(s) > 0$). Extending the function to the whole plane via {{concept:analytic-continuation|the unique complex-differentiable extension beyond the convergence boundary}} produces the **Dirichlet L-function** (or **Dirichlet L-series** in its sum form).

When $N = 1$, $\chi(n) = 1$ for every $n$, so $L(\chi, s)$ collapses back to {{term:riemann-zeta-function|the Riemann zeta function}}. For general moduli $N$, these functions act like tuned receivers isolating primes in each residue class $a \bmod N$. The Generalized Riemann Hypothesis predicts that for every character $\chi$, every zero inside {{concept:critical-strip|the open vertical band $0 < \operatorname{Re}(s) < 1$}} lies on {{concept:critical-line|the central line $\operatorname{Re}(s) = 1/2$}}. In fact, the upstream submission is structured to establish this broader claim for all characters—reducing each character to its underlying {{concept:primitive-character|minimal-period character}}—and derive the classical Riemann Hypothesis as the $N = 1$ special case.

### Limits of the intuition

Thinking of $L(\chi, s)$ as merely a "tinted copy" of $\zeta(s)$ blurs two vital analytic distinctions. First, $\zeta(s)$ has a pole at $s = 1$, whereas every non-principal $L(\chi, s)$ ($\chi \neq 1$) is holomorphic across the entire {{concept:complex-plane|two-dimensional grid of real and imaginary coordinates}}—even at $s = 1$, where its nonvanishing $L(\chi, 1) \neq 0$ is the heart of Dirichlet's theorem on primes in arithmetic progressions. Second, if $\chi$ is imprimitive, $L(\chi, s)$ differs from its primitive companion by finitely many Euler factors $(1 - \chi^*(p) p^{-s})$, which introduce extra zeros along the left edge $\operatorname{Re}(s) = 0$.

## Level 2 — Undergrad Bridge

Because $\chi$ is completely multiplicative, $L(\chi, s)$ factors as an Euler product for $\operatorname{Re}(s) > 1$:
$$L(\chi, s) = \prod_{p\text{ prime}} \left(1 - \chi(p) p^{-s}\right)^{-1}.$$
Taking the logarithmic derivative and summing over all characters modulo $N$ via orthogonality yields the prime-counting generator for any residue class $a \bmod N$ with $\gcd(a, N) = 1$:
$$\sum_{\substack{n=1 \\ n \equiv a \pmod N}}^{\infty} \frac{\Lambda(n)}{n^s} = -\frac{1}{\varphi(N)} \sum_{\chi \bmod N} \overline{\chi(a)} \frac{L'(\chi, s)}{L(\chi, s)}.$$
For a primitive character $\chi \bmod q$, the completed function $\Lambda(\chi, s) = (q/\pi)^{(s + \mathfrak{a})/2} \Gamma((s + \mathfrak{a})/2) L(\chi, s)$ satisfies the functional equation
$$\Lambda(\chi, s) = \varepsilon(\chi) \Lambda(\overline{\chi}, 1 - s), \qquad |\varepsilon(\chi)| = 1,$$
where $\varepsilon(\chi)$ is the root number built from the Gauss sum of $\chi$. Note the crucial twist: reflection sends a zero $s$ of $L(\chi, s)$ to a zero $1 - s$ of $L(\overline{\chi}, s)$ (not $L(\chi, s)$ itself when $\chi$ is non-real). That is why Stage 00's nonvanishing hypothesis `DirichletLNonvanishingRightOfHalf` quantifies over *all* characters $\chi$ simultaneously: ruling out zeros of $L(\chi^{-1}, 1 - s)$ for $\operatorname{Re}(1 - s) > 1/2$ rules out zeros of $L(\chi, s)$ for $\operatorname{Re}(s) < 1/2$.

## Level 3 — Formal Lean Contract

In Mathlib4 (`Mathlib.NumberTheory.LSeries.DirichletContinuation`), `DirichletCharacter.LFunction χ s` defines the meromorphic continuation of $\sum \chi(n) n^{-s}$ for every `χ : DirichletCharacter ℂ N`. For `N = 1`, Mathlib proves `LFunction_modOne_eq : LFunction (1 : DirichletCharacter ℂ 1) = riemannZeta`.

Stage 00 anchors Dirichlet $L$-functions in:
- `00_statement/statements.lean#DirichletLZerosOnLine` (upstream `src/Statements.lean:14-16`):
  ```lean
  def DirichletLZerosOnLine : Prop :=
    ∀ (N : ℕ) [NeZero N] (χ : DirichletCharacter ℂ N) (s : ℂ),
      0 < s.re → s.re < 1 → DirichletCharacter.LFunction χ s = 0 → s.re = 1 / 2
  ```
- `00_statement/main.lean#LFunction_eq_zero_re` (upstream `src/Main.lean:19-21`), proved via `New.zeros_on_line New.grh`.

See the page's live verification banner (`audit/verification-status.json`) for the currently pinned upstream snapshot and local Lean build status (`build_status: unverified-locally`).
