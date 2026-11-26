# Formal Statement Contracts (`Statements.lean`)

This landmark page examines the four `Prop` definitions in `src/Statements.lean` that serve as the shared contract between the proof library and the entry theorems. Return to the [Stage 00 Overview](./overview.md) or read [The Five Entry Theorems](./main-theorems.md) to see how `Main.lean` proves them.

## Level 1 — Abitur / First-Principles

Writing a mathematical conjecture in a computer language forces every silent assumption out into the open. When mathematicians say informally that "all zeros of $\zeta(s)$ have real part $1/2$," human readers automatically skip the pole at $s = 1$ and the negative even integers $-2, -4, -6, \dots$. A computer checker takes every word literally: if you forget to exclude even one point where a function behaves specially, the statement becomes false.

`src/Statements.lean` packages four carefully scoped propositions over {{concept:complex-plane|the two-dimensional grid of points $s = \sigma + it$}}:
1. **Whole-plane form for $\zeta(s)$ (`RiemannZetaZerosOnLine`):** For {{concept:riemann-zeta-function|the global function extending $\sum_{n=1}^{\infty} n^{-s}$}}, whenever $\zeta(s) = 0$, $s$ is not one of {{concept:trivial-zeros|the predictable negative even integers $-2, -4, -6, \dots$}}, and $s \neq 1$, the point $s$ lies on {{concept:critical-line|the vertical line $\operatorname{Re}(s) = 1/2$}}.
2. **Right-half-plane nonvanishing (`DirichletLNonvanishingRightOfHalf`):** For every modulus $N \ge 1$ and every {{concept:dirichlet-character|periodic multiplicative weight $\chi$ modulo $N$}}, {{concept:dirichlet-l-function|the twisted function $L(\chi, s)$}} is nonzero whenever $\operatorname{Re}(s) > 1/2$, provided we exclude the single pole case where $\chi = 1$ and $s = 1$ at the same time.
3. **Strip form for all characters (`DirichletLZerosOnLine`):** For every modulus $N \ge 1$ and every character $\chi$, any zero of $L(\chi, s)$ strictly inside {{concept:critical-strip|the open vertical band $0 < \operatorname{Re}(s) < 1$}} lies on the {{term:critical-line}}.
4. **Strip form for minimal-period characters (`PrimitiveDirichletLZerosOnLine`):** The exact same strip claim restricted to {{concept:primitive-character|characters whose pattern cannot be inherited from a strictly smaller dividing modulus}}, isolating the exact version needed to prove equivalence with independent external formulations.

Why does `RiemannZetaZerosOnLine` still require `s ≠ 1` if $\zeta(s)$ has a pole at $s = 1$ rather than a zero? Because in {{concept:formal-verification|dependent type theory}}, every function `ℂ → ℂ` must return *some* complex number at every input—there is no "undefined" value. Excluding `s ≠ 1` makes the theorem completely independent of whatever conventional junk value Mathlib assigns at the pole.

![Dirichlet characters as periodic remainder wheels modulo 4 and modulo 5 contrasting primitive and induced characters](assets/dirichlet-remainder-wheel.svg)

### Where this analogy breaks

Thinking of the extra hypotheses (`s ≠ 1`, `0 < s.re`, `s.re < 1`) as mere defensive boilerplate understates their mathematical role. For imprimitive characters, the strict inequality `0 < s.re` is *mathematically required* rather than a technicality: imprimitive $L$-functions genuinely possess infinitely many zeros on the line $\operatorname{Re}(s) = 0$ coming from deleted prime factors, so dropping `0 < s.re` without adding primitivity and excluding trivial zeros would turn `DirichletLZerosOnLine` into a false statement.

## Level 2 — Undergrad Bridge

Each hypothesis in the four predicates of `src/Statements.lean` matches a concrete analytic boundary condition:

- **`RiemannZetaZerosOnLine`**
  $$\forall s \in \mathbb{C},\quad \zeta(s) = 0 \;\land\; (\neg \exists n \in \mathbb{N},\; s = -2(n + 1)) \;\land\; s \neq 1 \;\Longrightarrow\; \operatorname{Re}(s) = \tfrac{1}{2}.$$
  Here `n : ℕ` starts at `0`, so `-2 * ((n : ℂ) + 1)` ranges over `{-2, -4, -6, ...}` and intentionally *excludes* `0` (where $\zeta(0) = -1/2 \neq 0$). The guard `s ≠ 1` shields the statement from the total-function convention at the simple pole $s = 1$ (even though Mathlib actually defines `riemannZeta 1` to be a nonzero constant, `riemannZeta_one_ne_zero`). This matches Mathlib's `RiemannHypothesis` word for word.

- **`DirichletLNonvanishingRightOfHalf`**
  $$\forall N \ge 1,\; \forall \chi \bmod N,\; \forall s \in \mathbb{C},\quad \operatorname{Re}(s) > \tfrac{1}{2} \;\land\; (\chi \neq 1 \lor s \neq 1) \;\Longrightarrow\; L(\chi, s) \neq 0.$$
  Why `(χ ≠ 1 ∨ s ≠ 1)` instead of `s ≠ 1`? When $\chi$ is non-principal ($\chi \neq 1$), $L(\chi, s)$ is entire—it has *no* pole at $s = 1$, and $L(\chi, 1) \neq 0$ is Dirichlet's theorem on primes in arithmetic progressions. Only when $\chi = 1$ (a principal character modulo $N$, whose $L$-function equals $\zeta(s)\prod_{p \mid N}(1 - p^{-s})$) does $L(\chi, s)$ inherit a pole at $s = 1$. Requiring `(χ ≠ 1 ∨ s ≠ 1)` preserves the genuine nonvanishing assertion $L(\chi, 1) \neq 0$ for all non-principal characters while excluding only the genuine pole pair $(\chi = 1, s = 1)$.

- **`DirichletLZerosOnLine`**
  $$\forall N \ge 1,\; \forall \chi \bmod N,\; \forall s \in \mathbb{C},\quad 0 < \operatorname{Re}(s) < 1 \;\land\; L(\chi, s) = 0 \;\Longrightarrow\; \operatorname{Re}(s) = \tfrac{1}{2}.$$
  Confining $s$ to the open strip $0 < \operatorname{Re}(s) < 1$ avoids three boundary phenomena simultaneously: the pole at $s = 1$ (for $\chi = 1$), the non-positive integer trivial zeros in $\operatorname{Re}(s) \le 0$, and the infinitely many Euler-factor zeros on $\operatorname{Re}(s) = 0$ that appear whenever $\chi$ is imprimitive. Because `NeZero N` excludes $N = 0$, every modulus $N \ge 1$ is covered cleanly.

- **`PrimitiveDirichletLZerosOnLine`**
  Restricts `DirichletLZerosOnLine` by inserting `χ.IsPrimitive`. Mathematically it is strictly equivalent to `DirichletLZerosOnLine` (since deleting primes $p \mid N$ multiplies $L(\chi^*, s)$ by factors $1 - \chi^*(p)p^{-s}$ that never vanish for $\operatorname{Re}(s) > 0$), and it serves as the handoff interface to the external `formal-conjectures` bridge (`comparator/FormalConjecturesBridge.lean`, archived on upstream's `alpoge` branch pending re-integration).

## Level 3 — Formal Lean Contract

`src/Statements.lean` (lines `1–20`) declares `module` (line 1), `public import Mathlib` (line 3)—importing zero files from the proof submission—and `@[expose] public section` (line 5) so every definition unfolds transparently for downstream proofs and for Comparator.

All four declarations are preserved verbatim (with `preserve_defs: true`) in `skeleton/00_statement/statements.lean` from upstream `src/Statements.lean:7-20` at pinned commit `41c2716658c42ba08eb0648994a2efe83082a95c` (`41c2716658c4`). In the condensed tree, `Challenge.lean` now lives at the repository root and directly executes `import Statements` at line 1 rather than duplicating the four definitions; nothing in the submission imports `Challenge.lean`, and `Statements` imports only `Mathlib`.

**Verification status (`audit/verification-status.json`):** Local Lean build verification is **`unverified-locally`** (pinned upstream commit `41c2716658c42ba08eb0648994a2efe83082a95c`; entry static scan reports `0` forbidden tokens in `src/Statements.lean`). Proceed to [Independent Statement Equivalence](./formal-conjectures.md) to see how `PrimitiveDirichletLZerosOnLine` bridges to the external `formal-conjectures` statement of GRH.

<!-- rh:lean-node:00_statement/statements -->
