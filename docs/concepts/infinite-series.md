---
slug: infinite-series
term: infinite series
aliases:
  - Dirichlet series
inline_glimpse: "A limit of partial sums formed by adding infinitely many terms; in number theory, weighting terms by n⁻ˢ produces a Dirichlet series that turns integer arithmetic into a smooth function."
visual_ref: assets/prime-wave-zeta-zeros.svg
where_analogy_breaks: "Treating an infinite series as an ordinary long addition breaks down whenever terms fail to shrink fast enough: reordering terms can change a conditionally convergent sum, and outside its half-plane of convergence the series expression diverges even when the underlying function still makes sense."
sources:
  - title: "Introduction to Analytic Number Theory"
    citation: "T. M. Apostol, Springer Undergraduate Texts in Mathematics (1976), Chapter 11: Dirichlet Series and Euler Products."
    kind: book
  - title: "Principles of Mathematical Analysis"
    citation: "W. Rudin, 3rd ed., McGraw-Hill (1976), Chapter 3: Numerical Sequences and Series."
    kind: book
  - title: "Mathlib4: LSeries"
    citation: "Mathlib.NumberTheory.LSeries.Basic (pinned mathlib rev 31ce06eaadf4a200c0e1407c5f10953cf606b8f8)."
    kind: mathlib
lean_anchors: []
child_concepts:
  - prime-number
  - complex-plane
  - riemann-zeta-function
  - analytic-continuation
---

# Infinite Series

## Level 1 — Abitur / First-Principles

Given a sequence of numbers $a_1, a_2, a_3, \dots$, form the running totals $S_N = a_1 + a_2 + \dots + a_N$. If those running totals settle closer and closer to a single finite value $S$ as $N$ grows without bound, the **infinite series** $\sum_{n=1}^{\infty} a_n$ converges to $S$. For example, $\frac{1}{2} + \frac{1}{4} + \frac{1}{8} + \dots$ converges to $1$, whereas the harmonic sum $1 + \frac{1}{2} + \frac{1}{3} + \dots$ grows past every bound and diverges.

Number theory uses a structured family of infinite sums called **Dirichlet series**, written $\sum_{n=1}^{\infty} a_n n^{-s}$, where $a_n$ carries arithmetic information and $s$ is a variable drawn from {{concept:complex-plane|the two-dimensional number plane whose points combine a horizontal real coordinate and a vertical imaginary coordinate}}. Setting $a_n = 1$ for every $n$ produces {{concept:riemann-zeta-function|the classic sum $\sum_{n=1}^{\infty} n^{-s}$ linked directly to prime factorization}} whenever the horizontal coordinate of $s$ exceeds $1$. Whenever $a_n$ respects multiplication ($a_{mn} = a_m a_n$), the series splits into a product over {{concept:prime-number|whole numbers greater than $1$ with no smaller factors}}. To inspect values of $s$ where the raw sum blows up, mathematicians extend the function beyond the convergence boundary by {{concept:analytic-continuation|uniquely extending a smooth complex function past the region where its original formula converges}}.

### Limits of the intuition

Thinking of an infinite series as ordinary addition with many terms is safe only when the sum of absolute values $\sum |a_n|$ is finite (absolute convergence). Without absolute convergence, rearranging terms can change the limit or destroy convergence altogether, and once the input $s$ crosses the convergence boundary (such as $\operatorname{Re}(s) \le 1$ for $\sum n^{-s}$), the series ceases to converge at all—so statements about zeros at $\operatorname{Re}(s) = 1/2$ refer to the extended function, never to the raw divergent sum.

## Level 2 — Undergrad Bridge

For a sequence $f : \mathbb{N} \to \mathbb{C}$, the associated Dirichlet series is
$$L(f, s) = \sum_{n=1}^{\infty} \frac{f(n)}{n^s}, \qquad n^{-s} = \exp(-s \ln n).$$
Because $|n^{-s}| = n^{-\operatorname{Re}(s)}$, the region of convergence is a half-plane rather than a disk: there exist abscissas of convergence $\sigma_c \le \sigma_a$ such that the series converges for $\operatorname{Re}(s) > \sigma_c$, diverges for $\operatorname{Re}(s) < \sigma_c$, and converges absolutely for $\operatorname{Re}(s) > \sigma_a$, with $\sigma_a - \sigma_c \le 1$. On every compact subset of $\{\operatorname{Re}(s) > \sigma_c\}$, convergence is uniform, so $L(f, s)$ is holomorphic there and can be differentiated term by term.

Two Dirichlet series $\sum f(n) n^{-s}$ and $\sum g(n) n^{-s}$ multiply via Dirichlet convolution:
$$\left(\sum_{n=1}^{\infty} \frac{f(n)}{n^s}\right)\left(\sum_{n=1}^{\infty} \frac{g(n)}{n^s}\right) = \sum_{n=1}^{\infty} \frac{(f * g)(n)}{n^s}, \qquad (f * g)(n) = \sum_{d \mid n} f(d) g(n/d).$$
For completely multiplicative coefficients ($f(mn) = f(m)f(n)$ and $f(1) = 1$) with $|f(n)| \le 1$, absolute convergence holds for $\operatorname{Re}(s) > 1$ and the series factors into an Euler product $\prod_p (1 - f(p)p^{-s})^{-1}$. No factor in this product vanishes for $\operatorname{Re}(s) > 1$, which immediately guarantees that $L(f, s)$ has no zeros in the half-plane $\operatorname{Re}(s) > 1$.

## Level 3 — Formal Lean Contract

In Mathlib4 (`Mathlib.NumberTheory.LSeries.Basic`), `LSeries f s` is defined as the unconditional sum `∑' n, LSeries.term f s n`, where `LSeries.term f s 0 = 0` and `LSeries.term f s n = f n / n ^ s` for `n ≠ 0`. In Lean, `tsum` (`∑'`) returns `0` whenever a series does not converge, so `LSeries` alone cannot define $\zeta(s)$ or $L(\chi, s)$ inside the strip $0 < \operatorname{Re}(s) < 1$. Consequently, the Stage 00 contracts in `src/Statements.lean` state all zero-location claims using the analytically continued functions `riemannZeta` and `DirichletCharacter.LFunction`, which agree with `LSeries` on `{s : ℂ | 1 < s.re}` and extend meromorphically to all of `ℂ`. Local Lean build verification is `unverified-locally` (pinned upstream commit `c900e67c2fcd1fdb041685820bbd2b867fbc5a4b`).
