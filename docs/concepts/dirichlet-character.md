---
slug: dirichlet-character
term: Dirichlet character
aliases: []
inline_glimpse: "A periodic multiplicative label χ : ℤ → ℂ modulo N that equals 0 when gcd(n, N) > 1, takes roots of unity when gcd(n, N) = 1, and satisfies χ(ab) = χ(a)χ(b)."
visual_ref: assets/dirichlet-remainder-wheel.svg
where_analogy_breaks: "Comparing a Dirichlet character to a simple alternating $+1, -1$ wave covers only real (quadratic) characters; in general, character values are complex roots of unity rotating around the unit circle, and values vanish on integers sharing a factor with the modulus $N$."
sources:
  - title: "Beweis des Satzes, dass jede unbegrenzte arithmetische Progression..."
    citation: "P. G. L. Dirichlet, Abhandlungen der Königlichen Preussischen Akademie der Wissenschaften (1837), pp. 45–81."
    kind: paper
  - title: "Introduction to Analytic Number Theory"
    citation: "T. M. Apostol, Springer Undergraduate Texts in Mathematics (1976), Chapter 6: Dirichlet Characters."
    kind: book
  - title: "Mathlib4: DirichletCharacter"
    citation: "Mathlib.NumberTheory.DirichletCharacter.Basic (`DirichletCharacter`, `DirichletCharacter.Odd`, `DirichletCharacter.Even`, pinned mathlib rev 31ce06eaadf4a200c0e1407c5f10953cf606b8f8)."
    kind: mathlib
lean_anchors:
  - 00_statement/statements.lean#DirichletLNonvanishingRightOfHalf
child_concepts:
  - complex-plane
  - dirichlet-l-function
  - primitive-character
---

# Dirichlet Character

## Level 1 — Abitur / First-Principles

Suppose you want to count primes that leave a specific remainder when divided by $4$—for instance, primes of the form $4k + 1$ ($5, 13, 17, 29, \dots$) versus $4k + 3$ ($3, 7, 11, 19, \dots$). To separate those two residue classes without breaking multiplication, assign each integer a periodic weight repeating every $4$ steps:
$$\chi(n) = \begin{cases} 0 & \text{if } n \text{ is even,} \\ +1 & \text{if } n \equiv 1 \pmod 4, \\ -1 & \text{if } n \equiv 3 \pmod 4. \end{cases}$$
Check what happens when you multiply two numbers: $(4a + 3)(4b + 3) \equiv 1 \pmod 4$, and $(-1)(-1) = +1$, so $\chi(mn) = \chi(m)\chi(n)$ holds for every pair of integers.

A rule $\chi$ that repeats with period $N$, maps every number sharing a common factor with $N$ to $0$, maps every number coprime to $N$ to a point on the unit circle of {{concept:complex-plane|the two-dimensional plane of real and imaginary coordinates}}, and obeys $\chi(mn) = \chi(m)\chi(n)$ is a **Dirichlet character** modulo $N$. Multiplying $n^{-s}$ by $\chi(n)$ builds {{concept:dirichlet-l-function|a twisted family of functions whose zeros govern primes in arithmetic progressions}}. Every character inherits its true minimal period from a unique underlying {{concept:primitive-character|minimal-period character that cannot be factored through a smaller modulus}}.

### Limits of the intuition

The modulo-$4$ example takes only real values $\{-1, 0, +1\}$, which makes a Dirichlet character look like a simple sign switch. For moduli such as $N = 5$, the unit group $(\mathbb{Z}/5\mathbb{Z})^{\times}$ is cyclic of order $4$, so characters take non-real values like $i$ and $-i$; their $L$-functions need not be symmetric under real conjugation alone and instead pair $\chi$ with its inverse $\chi^{-1} = \overline{\chi}$ in the functional equation.

## Level 2 — Undergrad Bridge

Algebraically, a Dirichlet character modulo $N \ge 1$ is a group homomorphism $\chi : (\mathbb{Z}/N\mathbb{Z})^{\times} \to \mathbb{C}^{\times}$, extended to a completely multiplicative arithmetic function $\chi : \mathbb{Z} \to \mathbb{C}$ by setting $\chi(n) = \chi(n \bmod N)$ when $\gcd(n, N) = 1$ and $\chi(n) = 0$ when $\gcd(n, N) > 1$. There are exactly $\varphi(N)$ Dirichlet characters modulo $N$, forming a finite abelian group under pointwise multiplication whose identity element is the principal (trivial) character $\chi_0$ (taking value $1$ on all units modulo $N$).

Two structural properties drive Stage 00:
1. **Orthogonality:** For $\gcd(a, N) = 1$, $\frac{1}{\varphi(N)} \sum_{\chi \bmod N} \overline{\chi(a)}\chi(n)$ equals $1$ if $n \equiv a \pmod N$ and $0$ otherwise. This filters arithmetic progressions $n \equiv a \pmod N$ into linear combinations of Dirichlet $L$-functions.
2. **Parity:** Since $(-1)^2 = 1$ in $(\mathbb{Z}/N\mathbb{Z})^{\times}$, every character satisfies $\chi(-1) \in \{+1, -1\}$. Characters with $\chi(-1) = 1$ are *even* and characters with $\chi(-1) = -1$ are *odd*. Parity determines which gamma factor ($\Gamma(s/2)$ versus $\Gamma((s+1)/2)$) enters the functional equation, and therefore which non-positive integers are trivial zeros.

## Level 3 — Formal Lean Contract

In Mathlib4 (`Mathlib.NumberTheory.DirichletCharacter.Basic`), `DirichletCharacter R N` is defined as `MulChar (ZMod N) R`—a multiplicative character from `ZMod N` to a commutative monoid with zero `R` (taken as `ℂ` in Stage 00), automatically sending non-units to `0`.

Stage 00 anchors Dirichlet characters in:
- `00_statement/statements.lean#DirichletLNonvanishingRightOfHalf` (upstream `src/Statements.lean:10-12`): quantifies over every modulus `N : ℕ` with `[NeZero N]` and every `χ : DirichletCharacter ℂ N` (likewise in `DirichletLZerosOnLine` at `src/Statements.lean:14-16` and `PrimitiveDirichletLZerosOnLine` at `src/Statements.lean:18-20`).

For the parallel `formal-conjectures` statement wrapper (`comparator/FormalConjecturesStatement.lean`, archived on upstream `alpoge@c900e67c2` pending re-integration on condensed `main`), `DirichletCharacter.instDecidablePredOddOfDecidableEq_formalConjecturesForMathlib` provided the exact named `DecidablePred` instance for `χ.Odd` (`χ (-1) = -1`) required by the `if χ.Odd then ... else ...` branch inside `GRH.trivialZeros`. See the page's live verification banner (`audit/verification-status.json`) for the currently pinned upstream snapshot and local Lean build status (`build_status: unverified-locally`).
