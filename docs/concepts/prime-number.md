---
slug: prime-number
term: prime number
aliases: []
inline_glimpse: "A whole number greater than 1 whose only positive whole-number divisors are 1 and itself; every integer above 1 factors uniquely into a product of them."
visual_ref: assets/prime-wave-zeta-zeros.svg
where_analogy_breaks: "Calling prime numbers the 'atoms' of arithmetic explains unique factorization, but unlike chemical elements there are infinitely many primes and their spacing follows no fixed periodic table—their fluctuations encode deep analytic structure."
sources:
  - title: "An Introduction to the Theory of Numbers"
    citation: "G. H. Hardy and E. M. Wright, 6th ed., Oxford University Press (2008), Chapters 1, 2, and 22."
    kind: book
  - title: "Multiplicative Number Theory"
    citation: "H. Davenport, 3rd ed., Springer Graduate Texts in Mathematics 74 (2000), Chapters 1–7."
    kind: book
  - title: "Mathlib4: Nat.Prime"
    citation: "Mathlib.Data.Nat.Prime.Basic (pinned mathlib rev 31ce06eaadf4a200c0e1407c5f10953cf606b8f8)."
    kind: mathlib
lean_anchors: []
child_concepts:
  - infinite-series
  - riemann-zeta-function
---

# Prime Number

## Level 1 — First-Principles

Take any whole number greater than $1$, such as $12$ or $17$. You can split $12$ into smaller factors ($12 = 2 \times 2 \times 3$), whereas $17$ admits no split other than $1 \times 17$. A whole number greater than $1$ that cannot be broken into a product of two smaller positive whole numbers is a **prime number**. The sequence begins $2, 3, 5, 7, 11, 13, 17, 19, \dots$ and never ends: Euclid proved that any finite list of primes leaves out at least one prime dividing their product plus $1$.

Two facts make prime numbers central to the Riemann Hypothesis. First, every integer $n \ge 2$ factors into primes in a single way once you ignore the order of factors (the Fundamental Theorem of Arithmetic). Second, primes thin out as numbers grow, yet their exact count $\pi(x)$ up to a threshold $x$ oscillates irregularly around the smooth estimate $\operatorname{Li}(x) = \int_2^x \frac{dt}{\ln t}$. Because unique factorization turns multiplicative sums over all integers into products over primes, tracking $\pi(x)$ reduces to studying {{concept:infinite-series|sums with infinitely many terms added according to a rule}}—most directly {{concept:riemann-zeta-function|the master function built by summing $n^{-s}$ over all positive integers $n$}}.

### Limits of the intuition

Comparing prime numbers to chemical atoms captures unique factorization, since every integer builds cleanly from prime factors. The analogy fails on global distribution: chemistry has finitely many elements arranged in a regular periodic table, whereas primes are infinite in number and exhibit irregular local gaps whose statistical error term is governed by complex-analytic zeros rather than any simple repeating pattern.

## Level 2 — Undergrad Bridge

Formally, $p \in \mathbb{N}$ is prime if $p \ge 2$ and whenever $p \mid ab$ for integers $a, b$, either $p \mid a$ or $p \mid b$. Unique factorization implies the Euler product identity for $\operatorname{Re}(s) > 1$:
$$\sum_{n=1}^{\infty} \frac{1}{n^s} = \prod_{p\text{ prime}} \left(1 - p^{-s}\right)^{-1}.$$
Expanding each geometric series $(1 - p^{-s})^{-1} = 1 + p^{-s} + p^{-2s} + \dots$ and multiplying across all primes generates each term $n^{-s}$ once, because each positive integer $n$ has one prime-power factorization.

To study primes smoothly, analytic number theory replaces the step function $\pi(x)$ with the Chebyshev function $\psi(x) = \sum_{n \le x} \Lambda(n)$, where the von Mangoldt function $\Lambda(n)$ equals $\ln p$ if $n = p^k$ ($k \ge 1$) and $0$ otherwise. Taking the logarithmic derivative of the Euler product gives
$$-\frac{\zeta'(s)}{\zeta(s)} = \sum_{n=1}^{\infty} \frac{\Lambda(n)}{n^s} \qquad (\operatorname{Re}(s) > 1).$$
Perron's formula converts this identity into an explicit formula expressing $\psi(x) - x$ as a sum over the nontrivial zeros $\rho$ of $\zeta(s)$, with each zero contributing a wave of amplitude proportional to $x^{\operatorname{Re}(\rho)}$. The bound $|\psi(x) - x| = O(x^{1/2} \log^2 x)$ is therefore equivalent to every nontrivial zero satisfying $\operatorname{Re}(\rho) = 1/2$.

## Level 3 — Formal Lean Contract

In Mathlib4 (`Mathlib.Data.Nat.Prime.Basic`), primality for natural numbers is defined as `Nat.Prime p`, requiring `p ≠ 1` and irreducibility (`∀ a b, p = a * b → a = 1 ∨ b = 1`, packaged via `Irreducible p`). Stage 00 does not declare a custom prime predicate; upstream `src/Statements.lean` and root `Challenge.lean` (lines `7–20`, with `riemannHypothesis` at lines `7–8`) build directly on Mathlib's `riemannZeta` and `DirichletCharacter.LFunction`, which encode primes through Mathlib's Euler product and `LSeries` library (`Mathlib.NumberTheory.LSeries.EulerProduct`), while upstream's `GUIDE.md` and `INDEX.md` index how the prime-sum bounds and `Lib/Analytic/PrimeNumberTheorem` feed into `New.grh`. (Upstream's parallel `comparator/FormalConjecturesStatement.lean` wrapper is archived on `alpoge@c900e67c2` pending re-integration on condensed `main`.) See the page's live verification banner (`audit/verification-status.json`) for the currently pinned upstream snapshot and local Lean build status (`build_status: unverified-locally`, Mathlib rev `31ce06eaadf4a200c0e1407c5f10953cf606b8f8`).
