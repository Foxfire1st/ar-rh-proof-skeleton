# The Five Entry Theorems (`Main.lean` & `Challenge.lean`)

This landmark page explains the five headline theorems exposed at the top of the proof (`src/Main.lean`), their frozen counterparts in `src/Challenge.lean`, and why proving zero-freeness strictly to the right of $\operatorname{Re}(s) = 1/2$ suffices to pin every nontrivial zero onto $\operatorname{Re}(s) = 1/2$. Return to the [Stage 00 Overview](./overview.md) for the full stage map.

## Level 1 — Abitur / First-Principles

Imagine a mirror running vertically down the exact center of a hallway. Suppose you know two facts: every stain in the hallway has a twin reflected across the center mirror, and the entire right half of the hallway is completely clean. Without ever inspecting the left half directly, you know the left half must also be clean—otherwise a stain on the left would cast a reflected stain onto the clean right half. The only place any stain can exist is directly on the center line itself.

That mirror logic drives the five entry theorems in `src/Main.lean`. Working across {{concept:complex-plane|the two-dimensional grid of real and imaginary coordinates}}, the core proof engine is built to establish one master fact first: for every {{concept:dirichlet-character|periodic multiplicative weight $\chi$ modulo $N$}}, its {{concept:dirichlet-l-function|twisted global function $L(\chi, s)$}} never equals $0$ anywhere strictly to the right of {{concept:critical-line|the vertical center line $\operatorname{Re}(s) = 1/2$}} (skipping the single pole at $s = 1$ when $\chi = 1$). Inside {{concept:critical-strip|the open vertical band $0 < \operatorname{Re}(s) < 1$}}, reducing any character to its underlying {{concept:primitive-character|minimal-period character}} and reflecting $s \mapsto 1 - s$ pairs any hypothetical zero in the left half ($0 < \operatorname{Re}(s) < 1/2$) with a zero in the forbidden right half ($1/2 < \operatorname{Re}(s) < 1$). Therefore every zero inside the {{term:critical-strip}} must sit on the {{term:critical-line}}. Specializing to modulus $N = 1$ recovers {{concept:riemann-zeta-function|the global function extending $\sum_{n=1}^{\infty} n^{-s}$}}, ruling out every zero off the center line other than {{concept:trivial-zeros|the predictable negative even integers $-2, -4, -6, \dots$}}.

In {{concept:formal-verification|computer-checked proof systems}}, proving five theorems means little unless an independent file locks down what those five names mean. Upstream separates the contract into two files:
- `src/Challenge.lean` imports only Mathlib, states the five theorems with placeholder `sorry` proofs, and never touches the submission.
- `src/Main.lean` imports the proof library and fills in all five proofs.

Comparator is designed to check that every theorem name in `Main` has the exact same type as in `Challenge`, without custom axioms or `sorry`.

![Unique smooth extension of zeta(s) past Re(s)=1 with overlapping disks and symmetry reflection across Re(s)=1/2](assets/analytic-continuation-bridge.svg)

### Where this analogy breaks

The mirror-hallway analogy makes reflection look like a self-reflection of a single function $L(\chi, s)$. When $\chi$ takes non-real complex values, the functional equation does not reflect $L(\chi, s)$ to itself—it reflects $L(\chi, s)$ to the *inverse* character's function $L(\chi^{-1}, 1 - s)$, and only after replacing $\chi$ with its primitive character $\chi^*$. That is why the master nonvanishing theorem cannot be stated for one character at a time: it must clear the right half-plane for *all* characters simultaneously so that $(\chi^*)^{-1}$ is covered as well.

## Level 2 — Undergrad Bridge

The five theorems in `src/Main.lean` form a tight dependency tree rooted at `LFunction_ne_zero_of_half_lt_re`:

1. **`LFunction_ne_zero_of_half_lt_re : DirichletLNonvanishingRightOfHalf`**
   Proved directly as `UpperTheory.FinalTheorems.everyCharacter` (the endpoint of Stages 01–20, treated as a typed black box in Stage 00). It asserts that for all $N \ge 1$, all $\chi \bmod N$, and all $s \in \mathbb{C}$ with $\operatorname{Re}(s) > 1/2$ and $(\chi \neq 1 \lor s \neq 1)$, we have $L(\chi, s) \neq 0$.

2. **`LFunction_eq_zero_re : DirichletLZerosOnLine`**
   Proved by `Claim.strip LFunction_ne_zero_of_half_lt_re` (`ClaimStrip.lean:89-103`). Given $0 < \operatorname{Re}(s) < 1$ and $L(\chi, s) = 0$, apply trichotomy between $\operatorname{Re}(s)$ and $1/2$:
   - If $\operatorname{Re}(s) > 1/2$: $s \neq 1$ (since $\operatorname{Re}(s) < 1$), contradicting `LFunction_ne_zero_of_half_lt_re` immediately.
   - If $\operatorname{Re}(s) = 1/2$: done.
   - If $\operatorname{Re}(s) < 1/2$: pass to the primitive character $\chi^*$ of conductor $q \mid N$ via `Claim.prim_zero` (valid because the deleted Euler factors $1 - \chi^*(p)p^{-s}$ never vanish for $\operatorname{Re}(s) > 0$), then reflect across $s \mapsto 1 - s$ via `Claim.prim_reflect` to obtain $L((\chi^*)^{-1}, 1 - s) = 0$. Since $\operatorname{Re}(1 - s) = 1 - \operatorname{Re}(s) > 1/2$ and $1 - s \neq 1$ (because $s \neq 0$), this contradicts `LFunction_ne_zero_of_half_lt_re` for $(\chi^*)^{-1}$.

3. **`LFunction_eq_zero_re_of_isPrimitive : PrimitiveDirichletLZerosOnLine`**
   A three-line specialization of `LFunction_eq_zero_re` that accepts an extra hypothesis `χ.IsPrimitive` and ignores it, exposing the exact predicate needed by `comparator/FormalConjecturesBridge.lean`.

4. **`riemannHypothesis : RiemannHypothesis`**
   Proved by `Claim.zeta LFunction_ne_zero_of_half_lt_re` (`ClaimStrip.lean:105-132`). Given $\zeta(s) = 0$, $s \neq 1$, and $s \notin \{-2(n+1) : n \in \mathbb{N}\}$:
   - $\operatorname{Re}(s) \ge 1$ is impossible by Mathlib's `riemannZeta_ne_zero_of_one_le_re`.
   - $\operatorname{Re}(s) \le 0$ is impossible because $\zeta(0) = -1/2 \neq 0$, classically the gamma factor $\Gamma_{\mathbb{R}}(s)$ has poles at non-positive even integers and never vanishes (under Mathlib's total-function convention `Gammaℝ_eq_zero_iff`, `Gammaℝ s = 0` precisely at those poles, which are excluded by `htriv` and $s \neq 0$), and the functional equation sends any remaining zero with $\operatorname{Re}(s) \le 0$ to $\operatorname{Re}(1 - s) \ge 1$.
   - On $0 < \operatorname{Re}(s) < 1$, Mathlib's `LFunction_modOne_eq` identifies $\zeta(s)$ with $L(1 \bmod 1, s)$, so `Claim.strip` yields $\operatorname{Re}(s) = 1/2$.

5. **`riemannZeta_eq_zero_re : RiemannZetaZerosOnLine`**
   Proved in one identifier (`riemannHypothesis`), because `RiemannZetaZerosOnLine` is definitionally equal to Mathlib's `RiemannHypothesis`.

## Level 3 — Formal Lean Contract

In `comparator.json`, `Challenge` is the frozen challenge module (`5` `sorry` placeholders at `src/Challenge.lean:19, 21, 23, 25, 27`) and `Main` is the solution module (`src/Main.lean:43-56`), checked against the three permitted axioms (`propext`, `Quot.sound`, `Classical.choice`) and external kernels (`leanchecker-paranoid` and `nanoda`). Note that in the skeleton files below, proof bodies are intentionally replaced by extraction stubs while keeping exact upstream line ranges and SHA-256 digests.

**Verification status (`audit/verification-status.json`):** Local Lean build verification is **`unverified-locally`** (pinned upstream commit `c900e67c2fcd1fdb041685820bbd2b867fbc5a4b`; entry static scan reports `0` forbidden tokens in `src/Main.lean` and `5` expected `sorry` tokens in `src/Challenge.lean`). See [Formal Statement Contracts](./statement-definitions.md) for the predicate definitions imported by `Main` and [Independent Statement Equivalence](./formal-conjectures.md) for the external `formal-conjectures` Comparator target.

<!-- rh:lean-node:00_statement/main -->

<!-- rh:lean-node:00_statement/challenge -->
