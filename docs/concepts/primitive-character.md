---
slug: primitive-character
term: primitive character
aliases:
  - primitive Dirichlet character
inline_glimpse: "A Dirichlet character modulo q whose pattern does not come from any strictly smaller dividing modulus; its modulus q equals its minimal period (conductor)."
visual_ref: assets/dirichlet-remainder-wheel.svg
where_analogy_breaks: "Viewing an imprimitive character as a harmless redundant copy of its primitive parent breaks down on the line $\\operatorname{Re}(s) = 0$: deleting primes $p \\mid N$ multiplies $L(\\chi^*, s)$ by $(1 - \\chi^*(p)p^{-s})$, which creates infinitely many extra zeros on $\\operatorname{Re}(s) = 0$ and breaks the clean symmetric functional equation."
sources:
  - title: "Multiplicative Number Theory"
    citation: "H. Davenport, 3rd ed., Springer Graduate Texts in Mathematics 74 (2000), Chapter 5: Primitive Characters."
    kind: book
  - title: "Introduction to Analytic Number Theory"
    citation: "T. M. Apostol, Springer Undergraduate Texts in Mathematics (1976), Chapter 8: Gauss Sums and Quadratic Reciprocity."
    kind: book
  - title: "Mathlib4: DirichletCharacter.IsPrimitive"
    citation: "Mathlib.NumberTheory.DirichletCharacter.Basic (`DirichletCharacter.IsPrimitive`, `DirichletCharacter.conductor`, `DirichletCharacter.primitiveCharacter`, pinned mathlib rev 31ce06eaadf4a200c0e1407c5f10953cf606b8f8)."
    kind: mathlib
lean_anchors:
  - 00_statement/statements.lean#PrimitiveDirichletLZerosOnLine
  - 00_statement/main.lean#LFunction_eq_zero_re_of_isPrimitive
child_concepts:
  - dirichlet-character
  - dirichlet-l-function
  - trivial-zeros
---

# Primitive Character

## Level 1 — First-Principles

Take the modulo-$4$ pattern $\chi_4$ ($0, +1, 0, -1, \dots$) and pretend you only know it modulo $12$. For every number coprime to $12$, set $\chi_{12}(n) = \chi_4(n)$, and set $\chi_{12}(n) = 0$ whenever $n$ is divisible by $2$ or $3$. This gives a valid {{concept:dirichlet-character|periodic multiplicative label}} modulo $12$, yet its values on units are completely inherited from modulus $4$—the factor $3$ only punched extra zeros into the sequence at multiples of $3$.

A character modulo $q$ that is genuinely native to modulus $q$—meaning it cannot be inherited from any strictly smaller divisor $d \mid q$—is a **primitive character** (or **primitive Dirichlet character**). Its modulus $q$ is called its *conductor*. Every character $\chi \bmod N$ comes from a unique underlying primitive character $\chi^* \bmod q$ with $q \mid N$. When forming {{concept:dirichlet-l-function|the twisted sum $L(\chi, s)$}}, only primitive characters obey the clean mirror symmetry $s \mapsto 1 - s$ and have no extra zeros outside {{concept:trivial-zeros|the predictable non-positive integer zeros}} and the open vertical band $0 < \operatorname{Re}(s) < 1$.

### Limits of the intuition

Treating an imprimitive character as "essentially the same" as its primitive parent is safe inside the open strip $0 < \operatorname{Re}(s) < 1$, because the missing prime factors $(1 - \chi^*(p) p^{-s})$ never vanish when $\operatorname{Re}(s) > 0$. The analogy breaks right on the boundary $\operatorname{Re}(s) = 0$: whenever $\chi^*(p) \neq 0$, the factor $1 - \chi^*(p) p^{-s}$ vanishes at infinitely many purely imaginary points $s = i(\arg \chi^*(p) + 2\pi k)/\ln p$ on $\operatorname{Re}(s) = 0$, which are *not* negative integers and would falsify a naive "all non-integer zeros lie on $\operatorname{Re}(s) = 1/2$" statement if primitivity were omitted.

## Level 2 — Undergrad Bridge

Let $\chi$ be a Dirichlet character modulo $N$. A positive divisor $d \mid N$ *induces* $\chi$ if there exists a character $\psi \bmod d$ such that $\chi(n) = \psi(n)$ for all $n$ with $\gcd(n, N) = 1$. The smallest such divisor $q \mid N$ is the **conductor** of $\chi$, and the unique character $\chi^* \bmod q$ inducing $\chi$ is its associated primitive character. We call $\chi$ **primitive** if its conductor equals $N$.

For any $\chi \bmod N$ with primitive character $\chi^* \bmod q$, the two $L$-functions are linked for all $s \in \mathbb{C}$ (away from $s = 1$ if $\chi = 1$) by
$$L(\chi, s) = L(\chi^*, s) \prod_{p \mid N} \left(1 - \chi^*(p) p^{-s}\right).$$
This identity explains two central steps in Stage 00:
1. **Inside $0 < \operatorname{Re}(s) < 1$:** For every prime $p \mid N$, $|\chi^*(p) p^{-s}| \le p^{-\operatorname{Re}(s)} < 1$, so $1 - \chi^*(p) p^{-s} \neq 0$. Thus $L(\chi, s)$ and $L(\chi^*, s)$ have the exact same zeros inside the open critical strip. In the condensed upstream proof, `New.zeros_on_line` (`src/New/Main.lean:29-36`) deduces `DirichletLZerosOnLine` for all characters directly from `DirichletLNonvanishingRightOfHalf` via the functional-equation symmetry `New.gammaFactor_eq_zero_or_LFunction_one_sub_eq_zero`, and the primitive case follows immediately by specialization.
2. **Across all of $\mathbb{C}$:** Because imprimitive characters pick up zeros on $\operatorname{Re}(s) = 0$, `formal-conjectures` restricts `GRH.generalized_riemann_hypothesis` to `hχ : χ.IsPrimitive` so that `s ∉ Int.cast '' GRH.trivialZeros χ` captures all nontrivial zeros cleanly.

## Level 3 — Formal Lean Contract

In Mathlib4 (`Mathlib.NumberTheory.DirichletCharacter.Basic`), `χ.IsPrimitive` holds when `χ.conductor = N`. Stage 00 anchors primitive characters in:
- `00_statement/statements.lean#PrimitiveDirichletLZerosOnLine` (upstream `src/Statements.lean:18-20`):
  ```lean
  def PrimitiveDirichletLZerosOnLine : Prop :=
    ∀ (N : ℕ) [NeZero N] (χ : DirichletCharacter ℂ N), χ.IsPrimitive →
      ∀ s : ℂ, 0 < s.re → s.re < 1 → DirichletCharacter.LFunction χ s = 0 → s.re = 1 / 2
  ```
- `00_statement/main.lean#LFunction_eq_zero_re_of_isPrimitive` (upstream `src/Main.lean:23-26`), proved by the one-line lambda `fun N _ χ _ ↦ LFunction_eq_zero_re N χ`, which specializes `LFunction_eq_zero_re` and drops the unused `χ.IsPrimitive` hypothesis.

In `formal-conjectures` (archived on upstream `alpoge@c900e67c2` in `comparator/FormalConjecturesBridge.lean` pending re-integration on condensed `main`), `primitiveDirichletLZerosOnLine_iff` proved that `PrimitiveDirichletLZerosOnLine` is logically equivalent to `formal-conjectures`' whole-plane `GRH.generalized_riemann_hypothesis`. See the page's live verification banner (`audit/verification-status.json`) for the currently pinned upstream snapshot and local Lean build status (`build_status: unverified-locally`).
