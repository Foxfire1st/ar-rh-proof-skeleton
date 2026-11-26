# The Five Entry Theorems (`Main.lean` & `Challenge.lean`)

This landmark page explains the five headline theorems exposed at the top of the proof (`src/Main.lean`), their frozen counterparts in root `Challenge.lean`, and why proving zero-freeness strictly to the right of $\operatorname{Re}(s) = 1/2$ suffices to pin every nontrivial zero onto $\operatorname{Re}(s) = 1/2$. Return to the [Stage 00 Overview](./overview.md) for the full stage map.

## Level 1 — First-Principles

Imagine a mirror running vertically down the exact center of a hallway. Suppose you know two facts: every stain in the hallway has a twin reflected across the center mirror, and the entire right half of the hallway is completely clean. Without ever inspecting the left half directly, you know the left half must also be clean—otherwise a stain on the left would cast a reflected stain onto the clean right half. The only place any stain can exist is directly on the center line itself.

That mirror logic drives the five entry theorems in `src/Main.lean`. Working across {{concept:complex-plane|the two-dimensional grid of real and imaginary coordinates}}, the core proof engine is built to establish one master fact first: for every {{concept:dirichlet-character|periodic multiplicative weight $\chi$ modulo $N$}}, its {{concept:dirichlet-l-function|twisted global function $L(\chi, s)$}} never equals $0$ anywhere strictly to the right of {{concept:critical-line|the vertical center line $\operatorname{Re}(s) = 1/2$}} (skipping the single pole at $s = 1$ when $\chi = 1$). Inside {{concept:critical-strip|the open vertical band $0 < \operatorname{Re}(s) < 1$}}, reducing any character to its underlying {{concept:primitive-character|minimal-period character}} and reflecting $s \mapsto 1 - s$ pairs any hypothetical zero in the left half ($0 < \operatorname{Re}(s) < 1/2$) with a zero in the forbidden right half ($1/2 < \operatorname{Re}(s) < 1$). Therefore every zero inside the {{term:critical-strip}} must sit on the {{term:critical-line}}. Specializing to modulus $N = 1$ recovers {{concept:riemann-zeta-function|the global function extending $\sum_{n=1}^{\infty} n^{-s}$}}, ruling out every zero off the center line other than {{concept:trivial-zeros|the predictable negative even integers $-2, -4, -6, \dots$}}.

In {{concept:formal-verification|computer-checked proof systems}}, proving five theorems means little unless an independent file locks down what those five names mean. Upstream separates the contract into two files:
- Root `Challenge.lean` imports only `Statements` (which imports only Mathlib), states the five theorems with placeholder `sorry` proofs, and never touches the submission.
- `src/Main.lean` imports `New.Main` from the proof library and fills in all five proofs.

Comparator is designed to check that every theorem name in `Main` has the exact same type as in `Challenge`, without custom axioms or `sorry`.

![Unique smooth extension of zeta(s) past Re(s)=1 with overlapping disks and symmetry reflection across Re(s)=1/2](assets/analytic-continuation-bridge.svg)

### Where this analogy breaks

The mirror-hallway analogy makes reflection look like a self-reflection of a single function $L(\chi, s)$. When $\chi$ takes non-real complex values, the functional equation does not reflect $L(\chi, s)$ to itself—it reflects $L(\chi, s)$ to the *inverse* character's function $L(\chi^{-1}, 1 - s)$, and only after replacing $\chi$ with its primitive character $\chi^*$. That is why the master nonvanishing theorem cannot be stated for one character at a time: it must clear the right half-plane for *all* characters simultaneously so that $(\chi^*)^{-1}$ is covered as well.

## Level 2 — Undergrad Bridge

The five theorems in `src/Main.lean` (lines `8–26`, via `public import New.Main`) form a tight dependency tree rooted at `New.grh` (`src/New/Reduction/OneSidedBound.lean:239-268`):

1. **`LFunction_ne_zero_of_half_lt_re : DirichletLNonvanishingRightOfHalf`**
   Proved directly as `New.grh` (the endpoint of Stages 01–07, treated as a typed black box in Stage 00). It asserts that for all $N \ge 1$, all $\chi \bmod N$, and all $s \in \mathbb{C}$ with $\operatorname{Re}(s) > 1/2$ and $(\chi \neq 1 \lor s \neq 1)$, we have $L(\chi, s) \neq 0$.

2. **`LFunction_eq_zero_re : DirichletLZerosOnLine`**
   Proved by `New.zeros_on_line New.grh` (`src/New/Main.lean:32-36`). Given $0 < \operatorname{Re}(s) < 1$ and $L(\chi, s) = 0$, apply trichotomy between $\operatorname{Re}(s)$ and $1/2$:
   - If $\operatorname{Re}(s) > 1/2$: $s \neq 1$ (since $\operatorname{Re}(s) < 1$), contradicting `New.grh` immediately.
   - If $\operatorname{Re}(s) = 1/2$: done.
   - If $\operatorname{Re}(s) < 1/2$: using `New.gammaFactor_eq_zero_or_LFunction_one_sub_eq_zero` and `Complex.Gammaℝ_ne_zero_of_re_pos`, the gamma factor cannot vanish for $\operatorname{Re}(s) > 0$, so functional-equation reflection across $s \mapsto 1 - s$ forces $L(\chi^{-1}, 1 - s) = 0$ (passing through the underlying primitive character inside the strip). Since $\operatorname{Re}(1 - s) = 1 - \operatorname{Re}(s) > 1/2$ and $1 - s \neq 1$ (because $s \neq 0$), this contradicts `New.grh`.

3. **`LFunction_eq_zero_re_of_isPrimitive : PrimitiveDirichletLZerosOnLine`**
   Proved by the one-line lambda `fun N _ χ _ ↦ LFunction_eq_zero_re N χ`, accepting and ignoring the extra `χ.IsPrimitive` hypothesis to expose the exact predicate used by the external `formal-conjectures` equivalence (archived on upstream's `alpoge` branch pending re-integration).

4. **`riemannHypothesis : RiemannHypothesis`**
   Proved by `New.zeta_zeros_on_line New.grh` (`src/New/Main.lean:41-44`). Given $\zeta(s) = 0$, $s \neq 1$, and $s \notin \{-2(n+1) : n \in \mathbb{N}\}$:
   - Specializing `New.grh` at modulus $N = 1$ and the trivial character $\chi = 1$ at both $s$ and $1 - s$ rules out $\operatorname{Re}(s) > 1/2$ directly and rules out $\operatorname{Re}(1 - s) > 1/2$ (that is, $\operatorname{Re}(s) < 1/2$) once reflection is applied.
   - At $s = 0$, `riemannZeta_zero` gives $\zeta(0) = -1/2 \neq 0$.
   - For $s \neq 0$, `riemannZeta_def_of_ne_zero`, `completedRiemannZeta_one_sub`, and `Complex.Gammaℝ_eq_zero_iff` show that the only zeros coming from $\Gamma_{\mathbb{R}}(s)$ poles are the non-positive even integers (excluded by `htriv` and $s \neq 0$), while any other zero with $\operatorname{Re}(s) < 1/2$ reflects to a zero of $\zeta(1 - s)$ with $\operatorname{Re}(1 - s) > 1/2$, contradicting `New.grh`. Hence $\operatorname{Re}(s) = 1/2$.

5. **`riemannZeta_eq_zero_re : RiemannZetaZerosOnLine`**
   Proved in one identifier (`riemannHypothesis`), because `RiemannZetaZerosOnLine` is definitionally equal to Mathlib's `RiemannHypothesis`.

## Level 3 — Formal Lean Contract

In root `comparator.json`, `Challenge` is the frozen challenge module (root `Challenge.lean`, which declares `import Statements` at line 1 rather than duplicating the definitions and contains `5` `sorry` placeholders at lines `8, 11, 14, 17, 20`) and `Main` is the solution module (`src/Main.lean:8-26`, delegating via `public import New.Main` to `New.zeta_zeros_on_line New.grh`, `riemannHypothesis`, `New.grh`, `New.zeros_on_line New.grh`, and `fun N _ χ _ ↦ LFunction_eq_zero_re N χ`), checked against the three permitted axioms (`propext`, `Quot.sound`, `Classical.choice`) with Lean's kernel and `enable_nanoda: true`. Note that in the skeleton files below, proof bodies are intentionally replaced by extraction stubs while keeping exact upstream line ranges and SHA-256 digests.

**Verification status (`audit/verification-status.json`):** Local Lean build verification is **`unverified-locally`** (pinned upstream commit `41c2716658c42ba08eb0648994a2efe83082a95c`, short `41c2716658c4`; entry static scan reports `0` forbidden tokens in `src/Main.lean` and `5` expected `sorry` tokens in root `Challenge.lean`). See [Formal Statement Contracts](./statement-definitions.md) for the predicate definitions in `src/Statements.lean` and [Independent Statement Equivalence](./formal-conjectures.md) for the archived `formal-conjectures` Comparator target pending re-integration.

<!-- rh:lean-node:00_statement/main -->

<!-- rh:lean-node:00_statement/challenge -->
