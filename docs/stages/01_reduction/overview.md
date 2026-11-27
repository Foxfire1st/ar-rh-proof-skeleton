# Stage 01 — The Theorem and Its Reduction

Stage 01 is the architectural bridge between the formal statement contracts of [Stage 00](../00_statement/overview.md) and the deep automorphic, $p$-adic, and Galois-theoretic engines of Stages 02–07. It proves in full how a single upper bound on a smoothed prime-counting remainder implies the Generalized Riemann Hypothesis and the Riemann Hypothesis, and how that upper bound itself follows once a balanced cancellation inequality traps the prime remainder between an upper bound on lifted vectors and an arithmetic lower bound on their determinant.

## Level 1 — First-Principles

Finding every zero of a function across an entire two-dimensional plane looks hopeless if you try to hunt them one point at a time. Stage 01 trades that hunt for a much sharper target: controlling how far the distribution of {{concept:prime-number|whole numbers greater than $1$ with no smaller positive divisors}} can surge above its expected average.

The bridge runs in four linked steps, reading from the final headline theorems back down to the core inequality:

1. **Mirror reflection to the right half-plane:** Across {{concept:complex-plane|the two-dimensional grid of points $s = \sigma + it$ combining a real horizontal coordinate and an imaginary vertical coordinate}}, each {{concept:dirichlet-character|periodic multiplicative weight $\chi$ modulo $N$}} defines {{concept:dirichlet-l-function|a global function $L(\chi, s)$ built by extending $\sum_{n=1}^{\infty} \chi(n) n^{-s}$}}, with modulus $N = 1$ giving {{concept:riemann-zeta-function|the global function $\zeta(s)$}}. As Stage 00 establishes, ruling out zeros anywhere strictly to the right of {{concept:critical-line|the central vertical line $\operatorname{Re}(s) = 1/2$}} automatically forces every zero inside {{concept:critical-strip|the open vertical band $0 < \operatorname{Re}(s) < 1$}} onto the {{term:critical-line}} by functional-equation symmetry.
2. **One-sided prime bound to zero-free half-plane:** Rather than bounding prime fluctuations from both sides, Stage 01 uses {{concept:landaus-lemma|a classical analytic principle turning a one-sided ceiling on a weighted prime error into a zero-free vertical strip}}. Specifically, Stage 01 weighs prime powers by {{concept:von-mangoldt-function|the logarithmic weight $\Lambda(n)$ equal to $\log p$ on prime powers $p^k$ and $0$ elsewhere}} multiplied by a {{concept:quadratic-twist|sign-flipping factor $(1 - (n/d))$ built from quadratic residues modulo an auxiliary prime $d$}} that keeps every weight nonnegative. If the resulting smoothed prime remainder $R_{q_0}(y)$ stays bounded above by a constant times $y^{-1/2 + \varepsilon}$ for all large $y$ and every prime $d$ in a chosen residue class, then no {{term:dirichlet-l-function}} modulo $q_0$ vanishes with $\operatorname{Re}(s) > 1/2 + \varepsilon$.
3. **Two-cutoff reduction from the main inequality:** The core engine does not isolate $R_{q_0}(x)$ directly; it bounds a combination pairing $R_{q_0}(x)$ at a primary cutoff $x$ against remainders at a vastly larger second cutoff $x_1$ (with $x^{10^6} \le x_1$ and $x_1^{10^6} \le b$). Because $x_1$ is so huge relative to $x$, the classical Prime Number Theorem crushes the $x_1$ remainders far below $x^{-2}$, leaving only the target ceiling on $R_{q_0}(x)$ with a saving of $1/m \approx x^{-1/2}$ from running a chain of $m \approx \sqrt{x}$ lifts.
4. **The three-way cancellation balance:** Inside the main inequality, Stage 01 builds a {{concept:twisted-lattice|vector space equipped with an ordinary inner product for sizes alongside prime-by-prime integrality rules across a family of character twists}}. A family of $n$ lifted target vectors has a combined squared volume measured by its {{concept:gram-determinant|squared parallelotope volume computed as the determinant of pairwise inner products}}, corrected by an index tracking the denominators needed to make those vectors integral. Three theorems then collide:
   - **Theorem A (Shortness & Small Index):** A spanning set of integral source forms maps into lifted target vectors whose ordinary lengths shrink whenever the prime remainder $R_{q_0}(x)$ is large, while their denominator index stays small.
   - **Theorem B (Enough Translates):** A small collection of group translates of those source forms spans almost the entire target space, supplying $n \ge (1 - 4/b^2)\dim$ linearly independent short vectors.
   - **Theorem C (Longness):** Powered by {{concept:arithmetic-height|the global balance law stating that a nonzero algebraic number cannot simultaneously be tiny in ordinary size and divisible by huge prime powers}}, any spanning integral family has combined volume plus twice its index at least a large baseline called the *main amount*.

When Theorems A and B supply the upper ceiling and Theorem C supplies the lower floor on the same volume-plus-index quantity, the huge *main amount* cancels identically from both sides. What remains is an upper bound on $m$ times the total prime remainder.

![Balance of the main inequality showing Theorems A and B bounding covolume plus twice the index from above while Theorem C bounds it from below so the main amount cancels](assets/stage01-main-inequality-balance.svg)

**What Stage 01 proves in full versus what Stages 02–07 supply:** Stage 01 proves the entire reduction shell end to end — the {{term:twisted-lattice}} height and {{term:arithmetic-height|product formula}}, the assembly of Theorems A and B from their automorphic inputs, the cancellation in the main inequality, the two-cutoff passage via the Prime Number Theorem to the one-sided bound, the quadratic-twist zero-free half-plane criterion, and the final reflection to the {{term:critical-line}}. Four deeper engines are taken here as typed black-box inputs and unpacked in later stages:
- **[Stage 02 (`02_derivatives`)](../02_derivatives/overview.md):** Taylor-coefficient ("jet") $p$-adic divisibility and integral vacuum bounds for theta functions on cyclotomic CM fields.
- **[Stage 03 (`03_upper_bound`)](../03_upper_bound/overview.md):** Prime-by-prime composite-lift divisibility, the iterated seesaw identity, and archimedean size estimates feeding Theorem A.
- **[Stage 04 (`04_spanning`)](../04_spanning/overview.md):** Second-moment Petersson unfolding and sieve estimates proving that few translates of the Poincaré map spread across almost all of the target space (feeding Theorem B).
- **[Stage 05 (`05_determinant`)](../05_determinant/overview.md) and Stages 06–07 (`06_one_lift`, `07_congruences`):** Chain determinant factorization, single-lift volume height ratios, and existence of regular $m$-fold twist families proving Theorem C (`D.integral_forms_long`).

Explore Stage 01 across its three landmark essays, or zoom out to the [Full Proof Journey & Alpöge's 20-Step Map](../00_statement/proof-journey.md):
- [From a One-Sided Prime Bound to a Zero-Free Half-Plane](./one-sided-criterion.md) — `Criterion`, `OneSidedBound`, and `New.Main`: how the smoothed prime remainder, quadratic-twist zero avoidance, two-cutoff elimination, and functional-equation reflection turn `star_inequality` into `New.grh` and zeros on the {{term:critical-line}}.
- [The Main Cancellation Inequality and Theorems A & B](./main-inequality.md) — `MainInequality` and `ShortAndEnough`: how `D.mainAmount` cancels between Theorems A, B, and C to yield $m \cdot \text{rem} \le f a \log^5 b$, and how the error budget closes.
- [Twisted Lattices, Covolumes, and the Arithmetic Product Formula](./twisted-lattices.md) — `TwistedLattice`: how twist-independent denominator clearing combines untwisted complex sizes with twisted $p$-adic valuations in `TwistedLattice.covolume_add_index_nonneg`.

### Where this analogy breaks

Describing the main inequality as a single balance scale where a large baseline cancels out cleanly captures the exact cancellation structure of `New.star_inequality`. The picture breaks on uniformity of scales and spaces: the cancellation does not take place on one fixed Euclidean space, nor at a single prime cutoff. The number of lifts $m \in [\sqrt{x}, 2\sqrt{x}]$, the cyclotomic tower degree $q_1 / q_0$, the Behrend-sparse set of $b$-places $\Phi_b$, and the prime level $N$ all grow together as functions of $x$, while the {{term:twisted-lattice}} simultaneously couples complex conjugates across every embedding $\sigma : \mathbb{C} \simeq \mathbb{C}$ with local rings at every prime embedding $\iota \mid \ell$.

## Level 2 — Undergrad Bridge

Stage 01 organizes the top-down reduction into six landmark modules inside `src/New/Main.lean` and `src/New/Reduction/`, forming a direct chain of implications:

$$\text{TwistedLattice} + \text{Stages 02–07} \;\Longrightarrow\; \text{Theorems A, B, C} \;\Longrightarrow\; \texttt{star\_inequality} \;\Longrightarrow\; R_{q_0}(y) \le C y^{-1/2 + \varepsilon} \;\Longrightarrow\; \texttt{New.grh} \;\Longrightarrow\; \text{RH \& GRH}.$$

Reading each layer in mathematical detail:

1. **Twisted lattices (`New/Reduction/Lattices/TwistedLattice.lean`):**
   Given a complex vector space $V$ with linear lifts $\Theta_\xi : V \to W_\xi$ indexed by twists $\xi$, integral lattices at every prime-place isomorphism $\iota : \mathbb{C} \simeq \overline{\mathbb{Q}}_\ell$, and a rational inner product on $W_1$, `TwistedLattice.covolume_add_index_nonneg` proves a Bost–Laurent type height inequality: the untwisted squared Gram covolume plus twice the twist-independent index of a family $y$ is bounded below by completing $y$ to a basis and comparing the resulting determinant against the twisted volume ratio $Z(1)/Z(\xi_\iota)$ via the product formula. This single lemma underpins Theorem C (`D.integral_forms_long` in Stage 05) and supplies Hadamard's upper bound (`TwistedLattice.covolumeLe_of_normSqAt_le`) for Theorem A.

2. **Theorems A and B (`New/Reduction/ShortAndEnough.lean`):**
   For admissible data $D : \text{Data } P$ (fixing $q_0$ a multiple of $18480$, prime $d \equiv q_0/2 - 1 \pmod{q_0}$, scales $(\log b)^{100} \le x$, $x^{10^6} \le x_1$, $x_1^{10^6} \le b$, $m \in [\sqrt{x}, 2\sqrt{x}]$, $a = x b$, and $f = \varphi(q_0)\#\Phi_a$ infinite places of the CM field $E = K(\zeta_{q_1})$):
   - **Theorem A** combines `03_upper_bound/local_divisibility`, `03_upper_bound/seesaw_identity`, and `03_upper_bound/archimedean_size` with `05_determinant/norm_of_chain` to define the normalized Poincaré translates `D.normalizedPoincare g` (rescaled by the rational inverse gain factor $c_B = \prod \ell^{-g_\ell}$) and prove:
     - `D.span_short_integralRamified_eq_top`: integral ramified source forms whose lifts $\Theta_1(g\mathfrak{p}\varphi)$ satisfy $\|\Theta_1(g\mathfrak{p}\varphi)\|_\sigma^2 \le \exp(\text{mainAmount} - m\cdot\text{rem} + f a \log^4 b)$ for all $g \in \bar{G}$ and $\sigma \in \operatorname{Aut}(\mathbb{C})$ span the entire ramified source space;
     - `D.indexLe_normalizedPoincare`: any family selected from $t$ translates has twisted-lattice index at most $2\#t\cdot\dim(\text{source})\cdot\text{errorUnit}$, where $\text{errorUnit} = f a \log^3 b$.
   - **Theorem B** (`D.enough_forms`) invokes `04_spanning/spanning_theorem` to produce a finite subset $t \subseteq \bar{G}$ with $\#t \cdot \dim(\text{source}) \le 5 d_T \log(1 + 2 b^2)$ whose translates span a subspace of dimension at least $(1 - 4/b^2) d_T$ in the target space of dimension $d_T = D.\text{targetDim}$.

3. **The Main Inequality (`New/Reduction/MainInequality.lean`):**
   `New.star_inequality` selects a linearly independent family $y_1, \dots, y_n$ of short images from $t$ with $(1 - 4/b^2)d_T \le n \le d_T$. Hadamard's inequality gives a covolume bound $V = n(\text{mainAmount} - m\cdot\text{rem} + f a \log^4 b)$, Theorem A(b) + Theorem B give an index bound $\delta = 10 d_T \log(1 + 2b^2) f a \log^3 b$, and Theorem C (`D.integral_forms_long` from `05_determinant/long_forms`) gives the lower bound:
   $$d_T \cdot \text{mainAmount} - 3 d_T f a \log^4 b \le V + 2\delta.$$
   Dividing by $d_T$, canceling `D.mainAmount` using `mainAmount_nonneg` and $n \le d_T$, and using $n \ge d_T/2$ yields:
   $$D.m \cdot D.\text{totalRemainder} \le D.f \cdot D.a \cdot D.\text{logB}^5.$$

4. **Two-cutoff prime-remainder bound (`New/Reduction/OneSidedBound.lean`):**
   Expanding `D.totalRemainder` over the $a$-places and $b$-places isolates $R_{q_0}(x)$ up to $|R_{q_0}(x_1)|$ and $(\log b)|R_1(x_1)|$ (`New.Data.primeRemainder_le`). Choosing scales with $\log b \le 2 x^\varepsilon$, $\sqrt{x} \le m$, and $x^{\varepsilon/12} \le \log x_1$ (`New.eventually_exists_data_scales`, relying on admissible-data existence and the theta vacuum input `02_derivatives/integral_vacuum`), and applying the classical Prime Number Theorem at $x_1$ with exponent $B \ge 24/\varepsilon$ (`New.eventually_abs_primeRemainder_exp_le`) suppresses both $x_1$ terms below $x^{-2}$. Passing from integer $x$ to all real $y$ yields `New.eventually_primeRemainder_le_mul_rpow`:
   $$R_{q_0}(y) \le (128\varphi(q_0) + 32)\,y^{-1/2 + \varepsilon} \qquad (y \to \infty).$$

5. **One-sided criterion and half-plane GRH (`New/Reduction/Zeros/Criterion.lean`, `New/Reduction/OneSidedBound.lean`, `New/Main.lean`):**
   For any $\chi \bmod q$ and $\rho$ with $\operatorname{Re} \rho > 1/2$, `New.grh` handles $\rho = 1$ by Dirichlet's theorem, induces $\chi$ to $q_0 = 18480 q$ with $d_0 = 9240 q - 1$, applies `New.LFunction_ne_zero_of_primeRemainder_le` (which uses quadratic-twist zero avoidance to remove twisted poles and Landau's lemma to rule out poles at $\rho$), and peels off the finite Euler factor by `LFunction_changeLevel`. Finally, `New.zeros_on_line` and `New.zeta_zeros_on_line` in `src/New/Main.lean` reflect $\operatorname{Re} s > 1/2$ across $s \mapsto 1 - s$ to complete all five entry theorems of Stage 00.

## Level 3 — Formal Lean Contract

Stage 01 covers **18 upstream files** (`src/New/Main.lean` plus `17` files under `src/New/Reduction/`), represented in the condensed 41-landmark DAG (`dag/dag.json`) by **6 extracted landmark skeletons** derived from pinned upstream commit `41c2716658c42ba08eb0648994a2efe83082a95c` (`41c2716658c4`, `leanprover/lean4:v4.37.0-rc1`, Mathlib rev `31ce06eaadf4a200c0e1407c5f10953cf606b8f8`):

| Skeleton File | Upstream Path | Direct Landmark Dependencies (`dag/dag.json`) | Key Declarations |
| --- | --- | --- | --- |
| `01_reduction/new_main.lean` | `src/New/Main.lean` | `01_reduction/one_sided_bound` | `New.zeros_on_line`, `New.zeta_zeros_on_line` |
| `01_reduction/one_sided_bound.lean` | `src/New/Reduction/OneSidedBound.lean` | `00_statement/statements`, `01_reduction/criterion`, `01_reduction/main_inequality`, `02_derivatives/integral_vacuum` | `New.Data.primeRemainder_le`, `New.eventually_primeRemainder_le_mul_rpow`, `New.grh` |
| `01_reduction/criterion.lean` | `src/New/Reduction/Zeros/Criterion.lean` | *(none — imports `src/Lib/` and `PrimeError`)* | `New.LFunction_ne_zero_of_primeRemainder_le` |
| `01_reduction/main_inequality.lean` | `src/New/Reduction/MainInequality.lean` | `01_reduction/short_and_enough`, `05_determinant/long_forms` | `New.star_inequality` |
| `01_reduction/short_and_enough.lean` | `src/New/Reduction/ShortAndEnough.lean` | `01_reduction/twisted_lattice`, `03_upper_bound/{archimedean_size,local_divisibility,seesaw_identity}`, `04_spanning/spanning_theorem`, `05_determinant/norm_of_chain` | `New.Data.span_short_integralRamified_eq_top`, `New.Data.indexLe_normalizedPoincare`, `New.Data.enough_forms` |
| `01_reduction/twisted_lattice.lean` | `src/New/Reduction/Lattices/TwistedLattice.lean` | *(none — imports `src/Lib/` and `Lattices/{Colength,Heights}`)* | `New.TwistedLattice`, `New.TwistedLattice.IndexLe`, `New.TwistedLattice.CovolumeLe`, `New.TwistedLattice.covolume_add_index_nonneg` |

**Typed black-box boundary into Stages 02–07:** Every external proof dependency entering Stage 01 in `dag/dag.json` lands on an explicit landmark node from downstream stages:
- `02_derivatives/integral_vacuum` $\to$ `01_reduction/one_sided_bound` (via `New.Reduction.Data.Existence`),
- `03_upper_bound/local_divisibility`, `03_upper_bound/seesaw_identity`, `03_upper_bound/archimedean_size` $\to$ `01_reduction/short_and_enough`,
- `04_spanning/spanning_theorem` $\to$ `01_reduction/short_and_enough`,
- `05_determinant/norm_of_chain` $\to$ `01_reduction/short_and_enough` and `05_determinant/long_forms` (Theorem C, backed by `06_one_lift` and `07_congruences`) $\to$ `01_reduction/main_inequality`.

**Verification status (`audit/verification-status.json`):** Local Lean compilation and kernel replay remain **`unverified-locally`** at pinned commit `41c2716658c42ba08eb0648994a2efe83082a95c` (`41c2716658c4`). Static token scanning in this repository currently has **`scan_scope: entry`**, covering only the three Stage 00 entry files (`Challenge.lean`, `src/Main.lean`, `src/Statements.lean`), where it records `0` forbidden tokens in solution files and `5` expected `sorry` tokens in `Challenge.lean`. Stage 01 proof files under `src/New/Main.lean` and `src/New/Reduction/` have not been locally compiled, kernel-checked, or statically scanned here; every theorem signature and proof outline above reflects direct source inspection of the pinned upstream files.

<!-- rh:stage-minimap:01_reduction -->
