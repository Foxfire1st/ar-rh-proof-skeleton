---
slug: formal-verification
term: formal verification
aliases:
  - proof assistant
inline_glimpse: "Checking every logical step of a mathematical proof by a small computer kernel against explicit axioms, paired with statement-equivalence checks ensuring the proved theorem matches the intended conjecture."
visual_ref: assets/stage00-verification-bridge.svg
where_analogy_breaks: "Comparing formal verification to running a compiler or spell-checker misses two crucial boundaries: a kernel checks logical validity given the definitions and axioms, so it cannot catch a subtly weakened theorem statement unless an independent comparator checks the statement—and a repository's proof is only locally verified once you actually build and check it with the kernel."
sources:
  - title: "The Lean 4 Theorem Prover and Programming Language"
    citation: "L. de Moura and S. Ullrich, CADE 28, Springer LNCS 12699 (2021), pp. 625–635."
    kind: paper
  - title: "The Lean Mathematical Library"
    citation: "The mathlib Community, Proceedings of the 9th ACM SIGPLAN International Conference on Certified Programs and Proofs (CPP 2020), pp. 367–381."
    kind: paper
  - title: "Formal Conjectures Repository"
    citation: "Google DeepMind, formal-conjectures (`github.com/google-deepmind/formal-conjectures`, pinned commit d8d031d4998c78575159ccab9a7a9e95ce043883)."
    kind: reference
lean_anchors:
  - 00_statement/challenge.lean#riemannHypothesis
child_concepts:
  - riemann-zeta-function
  - dirichlet-l-function
---

# Formal Verification

## Level 1 — First-Principles

In traditional mathematics, a referee reads a paper and checks whether each step follows from earlier theorems without hidden gaps. For a proof spanning thousands of pages or hundreds of thousands of machine-generated lemmas, human line-by-line checking cannot catch every subtle omission. **Formal verification** translates both the statement and every deduction into a strict symbolic language checked by a **proof assistant** (here, Lean 4). A small trusted core program—the kernel—verifies every single inference rule back to a tiny list of foundational axioms.

Checking the steps, however, solves only half the trust problem. If a proof proves a statement with a subtle typo—for instance, accidentally dropping cases from {{concept:riemann-zeta-function|the global function extending $\sum_{n=1}^{\infty} n^{-s}$}} or weakening the claim for {{concept:dirichlet-l-function|its twisted periodic generalizations $L(\chi, s)$}}—the kernel happily accepts a proof of the wrong theorem. To prevent that, Stage 00 pairs every solution file with a frozen *challenge* file and is designed to run Comparator: a tool that exports the proof environment, verifies that no forbidden shortcuts (`sorry`, custom axioms) appear, checks that the submitted statements match the frozen challenge declarations type-for-type, and replays the proof term through independent external kernels.

### Limits of the intuition

Comparing a proof assistant to a software test suite or syntax checker understates its logical rigor (it checks all infinitely many cases via universal quantification, not sampled inputs), while calling it "absolute certainty" overstates what any single artifact proves on its own. Two external audits remain essential: confirming that the formal definitions match standard mathematics (the statement-equivalence role of `formal-conjectures` and Comparator), and actually executing the kernel check rather than relying on static text inspection alone.

## Level 2 — Undergrad Bridge

Lean 4 implements a dependent type theory (the Calculus of Inductive Constructions with quotient types and classical choice). Under the Curry–Howard correspondence, a proposition is a type `P : Prop` and a proof is a term `t : P`. The Lean kernel verifies `t : P` using only three standard axioms permitted by Mathlib:
- `propext` (propositional extensionality),
- `Quot.sound` (soundness of quotient types),
- `Classical.choice` (the axiom of choice).

A *challenge-vs-solution* Comparator workflow splits trust into two isolated environments:
1. **Challenge module** (root `Challenge.lean`, importing only `Statements` which imports only Mathlib): states the five target theorems with `by sorry` and is frozen against edits.
2. **Solution module** (`src/Main.lean`): imports the proof library (`New.Main`) and supplies actual proofs for the exact same declaration names.

(Upstream's parallel `formal-conjectures` challenge/solution pair—`comparator/FormalConjecturesChallenge.lean` and `comparator/FormalConjecturesSolution.lean`—is archived on `alpoge@c900e67c2` pending re-integration on condensed `main`.)

Comparator checks that every target theorem in the solution has the exact same elaborated type as in the challenge (with no hijacked typeclass instances or shadowed names), uses only the three permitted axioms (`propext`, `Quot.sound`, `Classical.choice`), and replays the exported proof term through an independent external kernel (`"enable_nanoda": true` in `comparator.json`).

## Level 3 — Formal Lean Contract

Stage 00 anchors the challenge entry point in:
- `00_statement/challenge.lean#riemannHypothesis` (upstream root `Challenge.lean:7-8`), checked against `Main` via `comparator.json` (`"enable_nanoda": true`):
  ```lean
  theorem riemannHypothesis : RiemannHypothesis := by
    sorry
  ```

The parallel `formal-conjectures` target (`comparator/FormalConjecturesChallenge.lean` and `comparator/formal-conjectures.json`) is archived on upstream `alpoge@c900e67c2` pending re-integration on condensed `main`.

**Honest verification status (`audit/verification-status.json`):** Local Lean compilation and kernel replay in this repository are currently **`unverified-locally`**. See the page's live verification banner (`audit/verification-status.json`) for the currently pinned upstream snapshot (`leanprover/lean4:v4.37.0-rc1`, Mathlib rev `31ce06eaadf4a200c0e1407c5f10953cf606b8f8`), entry-file static token scanning (`0` forbidden tokens across solution files, `5` expected `sorry` tokens in root `Challenge.lean`), and SHA-256 digests of `comparator.json` and upstream's proof guides (`GUIDE.md`, `INDEX.md`).
