# Independent Statement Equivalence (`FormalConjecturesStatement` & `FormalConjecturesChallenge`)

This landmark page explains why Stage 00 includes a second, independent Comparator setup drawn from Google DeepMind's `formal-conjectures` repository, and how it rules out the classic formalization risk of proving a subtly weakened statement. Return to the [Stage 00 Overview](./overview.md) or review [Formal Statement Contracts](./statement-definitions.md) for the submission's own predicates.

## Level 1 — Abitur / First-Principles

In {{concept:formal-verification|machine-checked mathematics}}, a proof kernel guarantees that every step follows validly from the definitions, yet the kernel alone cannot judge whether the author wrote down the *right* theorem. Suppose someone defines a custom predicate, accidentally adds an extra restriction, and proves it with zero errors. The kernel reports success, even though the famous conjecture was never proved.

To close that gap, the community maintainers added a second independent target drawn from an outside benchmark (`formal-conjectures`, written before this proof existed). That external benchmark states two theorems in {{concept:complex-plane|the two-dimensional grid of complex inputs}}:
- **The Riemann Hypothesis (`RiemannHypothesis.riemannHypothesis`):** using Mathlib's built-in definition for {{concept:riemann-zeta-function|the global function extending $\sum_{n=1}^{\infty} n^{-s}$}}.
- **The Generalized Riemann Hypothesis (`GRH.generalized_riemann_hypothesis`):** stated across the *entire* plane rather than just inside {{concept:critical-strip|the open vertical band $0 < \operatorname{Re}(s) < 1$}}. For any {{concept:primitive-character|minimal-period multiplicative weight $\chi$}} (a {{concept:dirichlet-character|periodic multiplicative label modulo $q$}} that comes from no smaller modulus), whenever {{concept:dirichlet-l-function|its twisted function $L(\chi, s)$}} equals $0$ and $s$ is not in the explicit set `GRH.trivialZeros χ` of {{concept:trivial-zeros|predictable non-positive integer zeros}}, the point $s$ must sit on {{concept:critical-line|the central vertical line $\operatorname{Re}(s) = 1/2$}}.

Notice the subtle difference between the two formulations:
- The submission's `PrimitiveDirichletLZerosOnLine` talks only about zeros inside the {{term:critical-strip}} ($0 < \operatorname{Re}(s) < 1$).
- The external `GRH.generalized_riemann_hypothesis` talks about *every* zero in $\mathbb{C}$ outside `GRH.trivialZeros χ`.

A separate bridge file (`comparator/FormalConjecturesBridge.lean`), written by reviewers using only Mathlib and `src/Statements.lean` (never importing the submission's proof), proves that the two statements are logically equivalent (`↔`). Because the external challenge file `FormalConjecturesChallenge.lean` is frozen and checked by Comparator against `FormalConjecturesSolution.lean`, the proof cannot silently redefine any symbol or weaken the theorem.

![Stage 00 formal statement and Comparator audit architecture linking Statements, Challenge, Main, FormalConjectures, and external kernels](assets/stage00-verification-bridge.svg)

### Where this analogy breaks

Treating the `formal-conjectures` Comparator check as a simple "string comparison" of theorem text misses why files like `FormalConjecturesStatement.lean` need subtle Lean-specific care. In Lean 4, two definitions that look identical to human eyes can fail Comparator's constant-matching check if an anonymous typeclass instance gets a different auto-generated name, or if a `module` versus non-`module` file elaborates a proof term (`by rfl`) into a slightly different auxiliary constant. Statement equivalence in Lean operates on fully elaborated kernel terms, not surface text.

## Level 2 — Undergrad Bridge

The `formal-conjectures` setup (`comparator/formal-conjectures.json`) separates four files so that the submission can never interfere with how the target statement is elaborated:

1. **`comparator/FormalConjecturesChallenge.lean`** (frozen, imports only `Mathlib`):
   Copies the exact declarations from `formal-conjectures` at commit `d8d031d4998c78575159ccab9a7a9e95ce043883`:
   - `DirichletCharacter.instDecidablePredOddOfDecidableEq_formalConjecturesForMathlib`
   - `RiemannHypothesis.riemannHypothesis : RiemannHypothesis` (`by sorry`)
   - `GRH.trivialZeros`
   - `GRH.generalized_riemann_hypothesis` (`by sorry`)

2. **`comparator/FormalConjecturesStatement.lean`** (solution-side definitions, imports only `Mathlib.NumberTheory.LSeries.DirichletContinuation`):
   Provides the solution side's exact copies of the two definitions not in Mathlib (`GRH.trivialZeros` and its `Odd` decidability instance), because the solution cannot import the challenge file (which contains `sorry`).

   Why does `GRH.trivialZeros` depend on that oddly specific instance name `DirichletCharacter.instDecidablePredOddOfDecidableEq_formalConjecturesForMathlib`? Look at its body:
   ```lean
   noncomputable def GRH.trivialZeros {q : ℕ} (χ : DirichletCharacter ℂ q) : Set ℤ :=
     if q = 1 then {-2 * (n + 1) | (n : ℕ) } else
       if χ.Odd then { -2 * n - 1 | (n : ℕ) } else
         { - 2 * n | (n : ℕ) }
   ```
   The `if χ.Odd then ... else ...` expression requires a `Decidable (χ.Odd)` witness inside the elaborated term of `GRH.trivialZeros`. In `formal-conjectures`, that instance was unnamed, so Lean auto-generated the name `instDecidablePredOddOfDecidableEq_formalConjecturesForMathlib` from the file path; giving it that exact name in both `FormalConjecturesStatement.lean` and `FormalConjecturesChallenge.lean`—and using Lean's `module` system so the `by rfl` proof elaborates to the identical statement `ψ (-1) = -1 ↔ ψ.Odd`—ensures Comparator matches the constants identically.

3. **`comparator/FormalConjecturesBridge.lean`** (reviewer-authored, imports only `Statements`, `FormalConjecturesStatement`, and `Mathlib`):
   Proves `primitiveDirichletLZerosOnLine_iff : PrimitiveDirichletLZerosOnLine ↔ GeneralizedRiemannHypothesis` in two directions:
   - **Forward (`generalized_riemann_hypothesis_of_primitive`):** For primitive $\chi$ and $L(\chi, s) = 0$:
     - $\operatorname{Re}(s) \ge 1$ is ruled out by `LFunction_ne_zero_of_one_le_re_of_isPrimitive` (using Mathlib's `LFunction_ne_zero_of_one_le_re` and `riemannZeta_one_ne_zero` at $q = 1, s = 1$).
     - $\operatorname{Re}(s) \le 0$ forces $s \in \mathrm{Int.cast}\;''\;\mathrm{GRH.trivialZeros}\;\chi$ via `mem_trivialZeros_of_re_nonpos`: apply the functional equation to $\chi^{-1}$ (also primitive) and use $L(\chi^{-1}, 1 - s) \neq 0$ for $\operatorname{Re}(1 - s) \ge 1$ to show $\Lambda(\chi, s) \neq 0$, forcing `gammaFactor χ s = 0` and splitting on `q = 1`, `χ.Even`, and `χ.Odd`.
     - What remains satisfies $0 < \operatorname{Re}(s) < 1$, where `PrimitiveDirichletLZerosOnLine` gives $\operatorname{Re}(s) = 1/2$.
   - **Converse (`primitive_of_generalizedRiemannHypothesis`):** Every element of `GRH.trivialZeros χ` is an integer $z \le 0$, so no point with $0 < \operatorname{Re}(s)$ can be a trivial zero.

4. **`comparator/FormalConjecturesSolution.lean`** (imports `Main` and `FormalConjecturesBridge`):
   Proves `RiemannHypothesis.riemannHypothesis` from `_root_.riemannHypothesis` and `GRH.generalized_riemann_hypothesis` via `type_of%` applied to `FormalConjecturesBridge.generalized_riemann_hypothesis_of_primitive _root_.LFunction_eq_zero_re_of_isPrimitive`. Using `type_of%` guarantees that the statement type was elaborated in `FormalConjecturesBridge` *without* the submission in scope, preventing any instance declared by the submission from altering the theorem's meaning.

## Level 3 — Formal Lean Contract

In `comparator/formal-conjectures.json`, `FormalConjecturesChallenge` is the frozen challenge module (`2` `sorry` placeholders at `comparator/FormalConjecturesChallenge.lean:84, 108`) and `FormalConjecturesSolution` is the solution module, checked against the three permitted axioms (`propext`, `Quot.sound`, `Classical.choice`) and external kernels (`leanchecker-paranoid` and `nanoda`).

The two Stage 00 skeleton modules below capture the exact declarations from upstream `comparator/FormalConjecturesStatement.lean` (with `preserve_defs: true`) and `comparator/FormalConjecturesChallenge.lean` at pinned commit `c900e67c2fcd1fdb041685820bbd2b867fbc5a4b`.

**Verification status (`audit/verification-status.json`):** Local Lean build verification is **`unverified-locally`**. Local checks verify commit provenance, SHA-256 digests of all six files under `comparator/`, and entry static scanning (`0` forbidden tokens across `FormalConjecturesStatement.lean`, `FormalConjecturesBridge.lean`, and `FormalConjecturesSolution.lean`; `2` expected `sorry` tokens in `FormalConjecturesChallenge.lean`).

<!-- rh:lean-node:00_statement/formal_conjectures_statement -->

<!-- rh:lean-node:00_statement/formal_conjectures_challenge -->
