# Stage 00 — Formal Statements & Challenge Contracts

Stage 00 establishes what the Riemann Hypothesis and the Generalized Riemann Hypothesis actually assert in Lean 4, why proving nonvanishing strictly to the right of $\operatorname{Re}(s) = 1/2$ forces every nontrivial zero onto $\operatorname{Re}(s) = 1/2$, and how independent Comparator specifications are designed to confirm that the submitted theorems match standard mathematical definitions.

## Level 1 — First-Principles

Counting {{concept:prime-number|whole numbers greater than $1$ that have no smaller positive divisors}} up to a bound $x$ looks simple at first: roughly $x / \ln x$ numbers below $x$ are prime. Yet the exact count $\pi(x)$ wavers above and below its smooth approximation $\operatorname{Li}(x) = \int_2^x \frac{dt}{\ln t}$. In 1859, Bernhard Riemann showed that those irregular prime wavers behave like sound waves whose frequencies and amplitudes are read off from a single function defined across {{concept:complex-plane|the two-dimensional grid of points $s = \sigma + it$ combining a real horizontal coordinate and an imaginary vertical coordinate}}.

That master function starts as {{concept:infinite-series|an unending sum of terms weighted by $n^{-s}$}}, namely $\sum_{n=1}^{\infty} n^{-s}$, which converges when $\operatorname{Re}(s) > 1$ and factors into a product over every {{term:prime-number}}. Extending that formula past $\operatorname{Re}(s) = 1$ by {{concept:analytic-continuation|the unique complex-differentiable extension beyond the sum's convergence region}} yields {{concept:riemann-zeta-function|the global function $\zeta(s)$ defined on the entire plane except $s = 1$}}. Every place where $\zeta(s) = 0$ falls into one of two categories:
- Known {{concept:trivial-zeros|predictable negative-integer zeros at $-2, -4, -6, \dots$ forced purely by the symmetry factor}}, which carry no hidden prime fluctuations;
- nontrivial zeros, all trapped inside {{concept:critical-strip|the open vertical band $0 < \operatorname{Re}(s) < 1$}}, where each zero at $\rho = \sigma + it$ adds an oscillation of size $x^{\sigma}$ to the prime-counting error.

The Riemann Hypothesis asserts that every nontrivial zero sits on {{concept:critical-line|the central vertical line $\operatorname{Re}(s) = 1/2$}}, making the error as tight as square-root cancellation allows.

To track primes in arithmetic progressions—such as primes leaving remainder $1$ or $3$ modulo $4$—number theorists twist each term $n^{-s}$ by {{concept:dirichlet-character|a periodic multiplicative weight $\chi(n)$ modulo $N$}}. That produces {{concept:dirichlet-l-function|a family of twisted functions $L(\chi, s)$ generalizing $\zeta(s)$}}, each governed by an underlying {{concept:primitive-character|minimal-period character that cannot be inherited from a smaller dividing modulus}}. The Generalized Riemann Hypothesis asserts that every nontrivial zero of every {{term:dirichlet-l-function}} also lies on the {{term:critical-line}}.

Because a modern machine-checked proof can span hundreds of thousands of lines, {{concept:formal-verification|checking every logical step in a computer kernel against explicit axioms}} only settles the conjecture if the statement proved by the machine matches the classical conjecture with zero loopholes. Stage 00 examines that entire statement contract across three landmark pages, plus a bridge page mapping the full proof engine ahead:
- [Full Proof Journey & Alpöge's 20-Step Map](./proof-journey.md) — how the eight mathematical pillars in `src/New/` combine Theorems A, B, and C to drive `New.grh`, and how the condensed readable edition aligns with Alpöge's original 20-step roadmap.
- [The Five Entry Theorems](./main-theorems.md) — how `src/Main.lean` reduces all five headline claims to a single right-half-plane nonvanishing theorem, and how root `Challenge.lean` locks the target signatures.
- [Formal Statement Contracts](./statement-definitions.md) — why each of the four predicates in `src/Statements.lean` carries its exact exclusions and strip bounds.
- [Independent Statement Equivalence](./formal-conjectures.md) — how the `comparator/` files (archived on upstream's `alpoge` branch while temporarily pending re-integration on condensed `main`) bridge the submission's strip formulation to the external `formal-conjectures` statements of RH and GRH.

**What Stage 00 leaves out:** Stage 00 treats the core proof engine `New.grh` (defined in `src/New/Reduction/OneSidedBound.lean` and imported by `src/Main.lean` via `New.Main`, whose stated type asserts nonvanishing of $L(\chi, s)$ on $\operatorname{Re}(s) > 1/2$) as a typed black box. The full proof engine is unpacked across all seven downstream pillars: [Stage 01 — The Theorem and Its Reduction](../01_reduction/overview.md) (one-sided prime-sum reduction, main inequality, and {{concept:twisted-lattice|families of lifted inner-product spaces that pair complex norms with $p$-adic integrality across twists}}), [Stage 02 — Derivatives of Theta Functions](../02_derivatives/overview.md), [Stage 03 — Upper Bound (Theorem A)](../03_upper_bound/overview.md), [Stage 04 — Spanning (Theorem B)](../04_spanning/overview.md), [Stage 05 — Determinant (Theorem C, Pt 1)](../05_determinant/overview.md), [Stage 06 — One Lift (Theorem C, Pt 2)](../06_one_lift/overview.md), and [Stage 07 — Congruences & Galois Representations (Theorem C, Pt 3)](../07_congruences/overview.md) (while the 9-file discrete-series supplement `src/New/Appendix/DiscreteSeries/` and the 495-file literature library `src/Lib/` sit outside the 41 primary landmarks and are cited directly where consumed).

![Complex s-plane showing convergence half-plane, critical strip, critical line, pole at s=1, trivial zeros, and first nontrivial zeros](assets/complex-plane-critical-strip.svg)

![Prime counting staircase pi(x) compared against Li(x) and illustrative zeta-zero error envelope](assets/prime-wave-zeta-zeros.svg)

### Where this analogy breaks

Describing nontrivial zeros as "sound waves" building the prime-counting error accurately conveys that each zero $\rho = \sigma + it$ contributes an oscillatory term $x^{\sigma} e^{it \ln x} / \rho$ whose amplitude grows with $\sigma$ and whose frequency grows with $t$. The analogy breaks on independence and synthesis: unlike audio harmonics that can be tuned independently on a synthesizer, the zeros of $\zeta(s)$ and $L(\chi, s)$ are rigidly locked together by the arithmetic Euler product and global analyticity, and the upstream submission does not inspect zeros individually—its strategy aims for global nonvanishing in the entire half-plane $\operatorname{Re}(s) > 1/2$.

## Level 2 — Undergrad Bridge

Stage 00 organizes the logical architecture at the summit of the proof into a short chain of implications that uses only classical complex analysis and Mathlib's $L$-function library:

$$\text{DirichletLNonvanishingRightOfHalf} \;\Longrightarrow\; \text{DirichletLZerosOnLine} \;\Longrightarrow\; \text{PrimitiveDirichletLZerosOnLine} \;\Longleftrightarrow\; \text{GRH (formal-conjectures)}$$
$$\text{DirichletLNonvanishingRightOfHalf} \;\Longrightarrow\; \text{RiemannHypothesis} \;=\; \text{RiemannZetaZerosOnLine}.$$

Three classical ingredients carry every arrow in Stage 00:
1. **Conductor reduction inside the open strip:** For any character $\chi \bmod N$ induced by its primitive character $\chi^* \bmod q$ ($q \mid N$), $L(\chi, s) = L(\chi^*, s)\prod_{p \mid N} (1 - \chi^*(p) p^{-s})$. Because $|p^{-s}| = p^{-\operatorname{Re}(s)} < 1$ on $\operatorname{Re}(s) > 0$, the finite Euler factor never vanishes in $0 < \operatorname{Re}(s) < 1$, so $L(\chi, s) = 0 \implies L(\chi^*, s) = 0$.
2. **Functional-equation reflection across $\operatorname{Re}(s) = 1/2$:** For primitive $\chi^*$, the completed function $\Lambda(\chi^*, s)$ satisfies $\Lambda(\chi^*, s) = \varepsilon(\chi^*)\Lambda((\chi^*)^{-1}, 1 - s)$ with $\varepsilon(\chi^*) \neq 0$, and the gamma factor has no zeros in $\operatorname{Re}(s) > 0$. Any zero $s$ of $L(\chi^*, s)$ with $0 < \operatorname{Re}(s) < 1/2$ therefore reflects to a zero $1 - s$ of $L((\chi^*)^{-1}, \cdot)$ with $1/2 < \operatorname{Re}(1 - s) < 1$, contradicting right-half-plane nonvanishing.
3. **Whole-plane equivalence for primitive characters:** Classical nonvanishing on $\operatorname{Re}(s) \ge 1$ (`LFunction_ne_zero_of_one_le_re`, plus $\zeta(1) \neq 0$ in Mathlib's pole convention) rules out zeros to the right of the strip, while reflection from $\operatorname{Re}(s) \le 0$ to $\operatorname{Re}(1 - s) \ge 1$ shows every zero in $\operatorname{Re}(s) \le 0$ comes from a pole of the gamma factor—matching `GRH.trivialZeros χ` exactly.

Everything below `DirichletLNonvanishingRightOfHalf` belongs to Stages 01–07; everything inside Stage 00 is a transparent, self-contained statement and reduction contract (whose local Lean/Comparator build verification remains `unverified-locally`, and whose `formal-conjectures` bridge is currently archived on upstream's `alpoge` branch pending re-integration on condensed `main`).

## Level 3 — Formal Lean Contract

On November 25, 2026, upstream `leanprover-community/riemann-hypothesis` condensed `main` to the readable edition (`GUIDE.md`, `INDEX.md`, `src/New/`, and `src/Lib/`), moving `Challenge.lean` to the repository root and temporarily leaving `comparator/` Formal Conjectures pending re-integration (archived on upstream's `alpoge` branch at `c900e67c2fcd1fdb041685820bbd2b867fbc5a4b`).

Stage 00 covers three active extracted skeleton modules derived from upstream `leanprover-community/riemann-hypothesis` at pinned commit `41c2716658c42ba08eb0648994a2efe83082a95c` (`41c2716658c4`, `leanprover/lean4:v4.37.0-rc1`, Mathlib rev `31ce06eaadf4a200c0e1407c5f10953cf606b8f8`):

| Skeleton File | Upstream Path | Role in Stage 00 |
| --- | --- | --- |
| `00_statement/statements.lean` | `src/Statements.lean` | Four `Prop` definitions specifying RH and GRH contracts |
| `00_statement/challenge.lean` | `Challenge.lean` | Frozen Mathlib-only challenge (`import Statements`; `5` target theorems with `sorry` at lines 8, 11, 14, 17, 20) |
| `00_statement/main.lean` | `src/Main.lean` | Solution entry module (`public import New.Main`) proving all `5` theorems from `New.grh` (`src/New/Reduction/OneSidedBound.lean`) |

**Live direct-import DAG topology:** In the condensed 41-landmark DAG (`dag/dag.json`), root `Challenge.lean` directly declares `import Statements`, so `00_statement/challenge` depends directly on `00_statement/statements`. Meanwhile, `src/Main.lean` declares `public import New.Main`, connecting `00_statement/main` directly to `01_reduction/new_main` (which in turn imports `01_reduction/one_sided_bound` and `00_statement/statements`).

**Verification status (`audit/verification-status.json`):** Local Lean compilation and kernel replay are currently **`unverified-locally`**. Local checks in this repository verify commit provenance (`41c2716658c42ba08eb0648994a2efe83082a95c`), entry-file static scanning (`0` forbidden tokens across `src/Statements.lean` and `src/Main.lean`; `5` expected `sorry` tokens in root `Challenge.lean` at lines 8, 11, 14, 17, 20), and parsed root `comparator.json` metadata (`challenge_module: "Challenge"`, `solution_module: "Main"`, permitted axioms `propext`, `Quot.sound`, `Classical.choice`, `enable_nanoda: true` alongside Lean's kernel). The `comparator/` Formal Conjectures harness is temporarily absent on condensed `main@41c2716658c4` and remains archived on `alpoge@c900e67c2fcd1fdb041685820bbd2b867fbc5a4b`.

<!-- rh:stage-minimap:00_statement -->
