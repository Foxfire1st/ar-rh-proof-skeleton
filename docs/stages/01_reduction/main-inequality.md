# The Main Inequality & Theorems A and B

This page explains how `New.star_inequality` (`src/New/Reduction/MainInequality.lean`) turns three structural inputs—shortness and small index (Theorem A), near-full-rank spanning (Theorem B), and a lower bound on integral lifts (Theorem C, supplied by Stages 05–07)—into an upper bound on the weighted prime-sum remainder `D.totalRemainder`, and how `ShortAndEnough.lean` and `PrimeSums.lean` assemble the upper-bound half of that comparison. Return to the [Stage 01 Overview](./overview.md), see [Landau's Lemma & the One-Sided Criterion](./one-sided-criterion.md) for how this bound rules out off-line zeros, and continue to [Twisted Lattices & A Twist for Each Prime](./twisted-lattices.md) for the lattice-and-height engine behind Theorem C.

## Level 1 — Abitur / First-Principles

Imagine stacking $n$ rigid shipping crates inside a warehouse whose total dimension is $d_T$. Suppose you obtain two opposing estimates for the total volume of the stacked crates:
- **A ceiling (upper bound):** each crate is individually small, so by multiplying their individual sizes you get an upper bound on the total volume they enclose. Moreover, whenever a weighted sum over {{concept:prime-number|indivisible whole numbers greater than $1$}} drifts too far upward, the crates shrink even faster—lowering the ceiling by an extra penalty proportional to that drift.
- **A floor (lower bound):** because the crates nearly align with an integer grid, geometry forbids squashing them flat. As long as your $n$ crates fill a substantial fraction of the warehouse's directions, their total volume (after paying a small fee for how far they miss the exact grid) cannot fall below a fixed geometric floor.

Crucially, the ceiling and the floor share the *exact same* gigantic baseline term—called the **main amount**. When you place the floor beneath the ceiling, that enormous baseline cancels out completely. What remains is a direct inequality: the upward drift of the prime-sum remainder cannot exceed the sum of the small approximation errors. Plugging that one-sided upper bound into {{concept:landaus-lemma|the positivity test that forbids a pole-free series with nonnegative weights from having zeros to the right of $\operatorname{Re}(s) = 1/2$}} closes the entire reduction.

Building the ceiling requires solving two distinct engineering problems at once (Theorems A and B):
1. **Making the crates short without breaking integrality (Theorem A).** Raw vectors built from source data carry huge extra divisibility at small primes, but they are initially too large in ordinary length. Multiplying every vector by a carefully engineered rational rescaling factor $c_B$ trades that extra prime-power divisibility for smaller Euclidean size—like exchanging heavy coins for lighter bills of the same value. The trade is exact up to a small rounding fee (an "index loss") tracked inside a {{concept:twisted-lattice|grid structure that monitors divisibility across many auxiliary versions of the space at once}}. Once each vector is short, {{concept:gram-determinant|the squared $n$-dimensional volume of a family of vectors, bounded above by the product of their squared lengths}} obeys an explicit ceiling per dimension.
2. **Preventing dimensional collapse (Theorem B).** Having millions of short vectors does no good if they all lie inside one narrow plane; the floor only kicks in when you span nearly the entire warehouse. Theorem B proves that only a modest number of group translates suffices to sweep out at least $(1 - 4/b^2) d_T$ independent directions of the target space. Because the number of translates stays small, the per-translate index fee stays tiny compared to the signal, while $n \ge (1 - 4/b^2) d_T \ge d_T / 2$ guarantees you can safely divide both sides of the volume comparison by $n$.

Finally, how does this geometric inequality see the prime sums used in the [companion positivity-reduction page](./one-sided-criterion.md)? Each vector's prime-power divisibility is governed by {{concept:von-mangoldt-function|the step weight putting $\log p$ on powers of a prime $p$ and zero elsewhere}}, filtered by {{concept:quadratic-twist|a $\pm 1$ sign weight separating primes modulo a fixed prime $d$}}. Summing the discrepancy between observed divisibility and its expected main term over the selected infinite places cancels every constant and leading logarithm, leaving a clean linear combination of prime-sum remainders at two cutoffs $x$ and $x_1$.

![Two-sided volume balance canceling the main amount between the lower bound and the upper bound from short, spread-out vectors](assets/stage01-main-inequality-balance.svg)

### Where this analogy breaks

A single warehouse with ordinary real coordinates hides three features that make the formal proof work. First, the vectors live in vector spaces over {{concept:complex-plane|the two-dimensional system of real and imaginary coordinates}}, and the upper bound on each vector's length must hold simultaneously under *every* field automorphism $\sigma$ of those coordinates—not just the standard embedding—so that {{term:gram-determinant|Hadamard's inequality}} bounds every conjugate of the {{term:gram-determinant|covolume}}. Second, divisibility is not measured in a single classical integer grid; it is measured in a {{term:twisted-lattice}}, where pairing a vector with its complex conjugate forces the clearing penalty ("index") to enter with a factor of $2$. Third, the floor (Theorem C, `integral_forms_long`) is not proved inside Stage 01: Stage 01 constructs the abstract {{term:twisted-lattice}} machinery (see the [companion page on prime-by-prime twists](./twisted-lattices.md)) and executes the comparison in `New.star_inequality`, while Stages 05–07 supply the determinant and height estimates that prove Theorem C.

## Level 2 — Undergrad Bridge

### 1. The two-sided sandwich in `New.star_inequality`

Fix parameters $P = (q_0, d)$ with $18480 \mid q_0$ and prime $d \equiv q_0/2 - 1 \pmod{q_0}$, and admissible data $D : \mathrm{Data}\ P$ carrying scales $(\log b)^{100} \le x$, $x^{10^6} \le x_1$, $x_1^{10^6} \le b$, and $\log\log b \ge 10^{12} P.\mathrm{constantBound}$, with $a = x b$, $f = D.f$ infinite places of the CM field $E$, $m = D.m$ theta lifts per chain, and target dimension $d_T = D.\mathrm{targetDim}$. Set the three bookkeeping quantities (`src/New/Reduction/MainInequality.lean:99-101`):
$$\mathrm{loss} := f a \log^4 b, \qquad \mathrm{logNormSq} := \mathrm{mainAmount} - m \cdot \mathrm{totalRemainder} + \mathrm{loss}, \qquad \mathrm{indexPerDim} := 10 \log(1 + 2b^2) \cdot \mathrm{errorUnit},$$
where $\mathrm{errorUnit} := f a \log^3 b$.

The proof of `New.star_inequality` (`D.m * D.totalRemainder ≤ D.f * D.a * D.logB ^ 5`) runs in five steps:

1. **Select a spanning basis of short integral images.** By **Theorem B** (`D.enough_forms`), $d_T > 0$ and there is a finite set $t \subseteq \bar{G} = D.\mathrm{chosenGL}$ with
   $$\#t \cdot \dim \mathrm{src} \le 5 d_T \log(1 + 2b^2), \qquad \left(1 - \frac{4}{b^2}\right) d_T \le \dim \bigvee_{g \in t} \operatorname{im}(D.\mathrm{normalizedPoincare}\ g),$$
   where $\mathrm{src} = D.\mathrm{ramifiedSourceForms}$. By **Theorem A (a)** (`D.span_short_integralRamified_eq_top`), the subset $\mathrm{short} \subseteq D.\mathrm{integralRamified}$ of integral ramified source forms satisfying $\|\Theta_1(g\mathfrak{p}\varphi)\|_\sigma^2 \le \exp(\mathrm{logNormSq})$ for all $g \in \bar{G}$ and $\sigma \in \operatorname{Aut}(\mathbb{C})$ spans $\mathrm{src}$. Taking a basis $y : \mathrm{Fin}\ n \to D.\mathrm{targetForms}$ of $\operatorname{span}_{\mathbb{C}}\bigl(\bigcup_{g \in t} g\mathfrak{p}(\mathrm{short})\bigr)$ produces linearly independent vectors with $(1 - 4/b^2) d_T \le n \le d_T$.

2. **Upper bound on covolume (Hadamard).** Each $y_i$ has $\|\Theta_1 y_i\|_\sigma^2 \le \exp(\mathrm{logNormSq})$ for every $\sigma \in \operatorname{Aut}(\mathbb{C})$, so `TwistedLattice.covolumeLe_of_normSqAt_le` gives
   $$D.\mathrm{targetLattice.CovolumeLe}\ y\ (n \cdot \mathrm{logNormSq}).$$

3. **Upper bound on index (Theorems A (b) + B).** Since each $y_i$ lies in $g\mathfrak{p}(D.\mathrm{integralRamified})$ for some $g \in t$, **Theorem A (b)** (`D.indexLe_normalizedPoincare`) yields an index bound of $\#t \cdot \dim\mathrm{src} \cdot 2\,\mathrm{errorUnit}$. Combining this with Theorem B's bound on $\#t \cdot \dim\mathrm{src}$ gives
   $$D.\mathrm{targetLattice.IndexLe}\ y\ (d_T \cdot \mathrm{indexPerDim}), \qquad 2 \cdot \mathrm{indexPerDim} = 20 \log(1 + 2b^2) \cdot f a \log^3 b.$$

4. **Comparison against Theorem C (Stage 05–07 input) and cancellation of `mainAmount`.** Applying Theorem C (`D.integral_forms_long`, proved in Stages 05–07 from the twisted-lattice product formula `TwistedLattice.covolume_add_index_nonneg` and the determinant height bound `heightLe_chainDeterminant_one_div`) to the family $y$ produces the lower bound
   $$d_T \bigl(\mathrm{mainAmount} - 3\,\mathrm{loss}\bigr) \le n \cdot \mathrm{logNormSq} + 2 d_T \cdot \mathrm{indexPerDim}.$$
   Because $\mathrm{mainAmount} \ge 0$ (`D.mainAmount_nonneg`), $\mathrm{loss} > 0$, and $n \le d_T$, we have
   $$n(\mathrm{mainAmount} + \mathrm{loss}) \le d_T(\mathrm{mainAmount} + \mathrm{loss}).$$
   Substituting $\mathrm{logNormSq} = (\mathrm{mainAmount} + \mathrm{loss}) - m \cdot \mathrm{totalRemainder}$ cancels $d_T \cdot \mathrm{mainAmount}$ cleanly from both sides and leaves
   $$n \cdot m \cdot \mathrm{totalRemainder} \le d_T \cdot \mathrm{err}, \qquad \mathrm{err} := 4\,\mathrm{loss} + 2\,\mathrm{indexPerDim}.$$
   The three per-dimension error terms contributing to $\mathrm{err}$ are:

   | Source | Theorem | Per-dimension amount |
   |---|---|---|
   | Archimedean size | Theorem A (a) | $f a \log^4 b$ |
   | Doubled index | Theorems A (b) & B | $20 \log(1 + 2b^2) \cdot f a \log^3 b$ |
   | Covolume lower bound | Theorem C (Stages 05–07) | $3 f a \log^4 b$ |

5. **Division by $n \ge d_T / 2$.** From the scale bounds, $4/b^2 \le 1/2$, so Theorem B implies
   $$d_T = 2\left(1 - \frac{1}{2}\right) d_T \le 2\left(1 - \frac{4}{b^2}\right) d_T \le 2n.$$
   Since $d_T > 0$, we have $n > 0$. Dividing $n \cdot m \cdot \mathrm{totalRemainder} \le d_T |\mathrm{err}| \le n \cdot |2\,\mathrm{err}|$ by $n$ and using the asymptotic scale inequality $|2\,\mathrm{err}| \le f a \log^5 b$ (`D.le_of_hasLogBound`) gives
   $$m \cdot \mathrm{totalRemainder} \le f a \log^5 b.$$

### 2. Inside `ShortAndEnough.lean`: trading divisibility for size (Theorem A) and guaranteeing spread (Theorem B)

The map from ramified source forms to target forms is built in three layers (`src/New/Reduction/ShortAndEnough.lean:43-143`):
- **Seesaw scaling (`seesawConst`, `scaledInclusion`):** `D.seesawConst : ℂˣ` is the nonzero constant $c_{\mathrm{ss}}$ witnessing `D.seesaw_identity`, absorbed into `scaledInclusion` as $c_{\mathrm{ss}}^{-1}$ so that for every twist $\vartheta$ there exists a chain datum $\Psi$ with $\iota_\vartheta(c_{\mathrm{ss}}^{-1} \mathcal{P}\varphi) = B_\vartheta\varphi$ (`exists_lowerChain_poincare_eq`). Here `projectedPoincare s` is $\varphi \mapsto \#\bar{G} \cdot e_0 s c_{\mathrm{ss}}^{-1} \mathcal{P}\varphi$.
- **Divisibility rescaling (`gainExponent`, `invGainFactor`):** For each prime $l$, define the integer exponent $g_l := \lfloor \mathrm{gain}_l - \mathrm{err}_l \rfloor$ (`D.gainExponent l`, where $\mathrm{err}_l = D.\mathrm{allowedLoss}\ l$), and form the positive rational number
  $$c_B := D.\mathrm{invGainFactor} = \prod_{l \in \mathrm{smallNonsplitPrimes} \cup \{N\}} l^{-g_l} \in \mathbb{Q}_{>0}.$$
- **Normalized Poincaré map (`normalizedPoincare`):** For $s \in \bar{G}$, define
  $$s\mathfrak{p} := D.\mathrm{normalizedPoincare}\ s = c_B \cdot \#\bar{G} \cdot e_0 s c_{\mathrm{ss}}^{-1} \mathcal{P}.$$

This single rational factor $c_B$ simultaneously governs both halves of Theorem A:

- **Archimedean trade-off — Theorem A (a) (`span_short_integralRamified_eq_top`):**
  Because $c_B \in \mathbb{Q}_{>0}$ is fixed by every $\sigma \in \operatorname{Aut}(\mathbb{C})$, `normSqAt_normalizedPoincare` scales every conjugate squared norm by $\exp(2 \log c_B)$:
  $$\|\Theta_1(s\mathfrak{p}\varphi)\|_\sigma^2 = \exp(2 \log c_B) \cdot \|\Theta_1(\mathrm{projectedPoincare}\ s\ \varphi)\|_\sigma^2.$$
  Taking logarithms and using $-g_l \le -\mathrm{gain}_l + \mathrm{err}_l + 1$, together with $\mathrm{gain}_N = 0$ (since $N \ge x_1$), $\sum \mathrm{err}_l \log l \le \mathrm{errorUnit}$, and the floor-rounding bound $\sum_{l \in \mathrm{smallNonsplitPrimes} \cup \{N\}} \log l \le b$ (`sum_log_smallNonsplitPrimes_union_le`), gives `log_invGainFactor_le`:
  $$\log c_B \le -\sum_{l \in \mathrm{smallNonsplitPrimes}} \mathrm{gain}_l \log l + \mathrm{errorUnit} + b.$$
  For every short source form $\varphi$ in the spanning set `span_isShort_ramifiedSourceForms_eq_top`, the seesaw identity and `weylNormSq_compositeLift_le` bound the trivial-twist lift exponent by $\mathrm{errorUnit} + \beta$, where $\beta = \mathrm{mainAmount} - m\cdot\mathrm{totalRemainder} + 2\sum_{l \in \mathrm{smallNonsplitPrimes}} \mathrm{gain}_l \log l + \mathrm{errorUnit}$; passing to `#Ḡ · e₀ s` adds $x^{99}$. Adding $2\log c_B$ cancels the entire gain sum $2\sum \mathrm{gain}_l \log l$, and $x^{99} + 4\,\mathrm{errorUnit} + 2b \le f a \log^4 b$ yields $\|\Theta_1(s\mathfrak{p}\varphi)\|_\sigma^2 \le \exp(\mathrm{mainAmount} - m\cdot\mathrm{totalRemainder} + f a \log^4 b)$ for all $s, \sigma$.

- **$p$-adic integrality and index — Theorem A (b) (`indexLe_normalizedPoincare`):**
  At any prime $l$, `norm_invGainFactor` shows $\|c_B\|_{\mathbb{C}_l} = \|l\|_{\mathbb{C}_l}^{-g_l}$ (since $g_l = 0$ outside $\mathrm{smallNonsplitPrimes} \cup \{N\}$). Thus whenever a ramified source form $\varphi$ has $l$-adic valuation of $B_\vartheta\varphi$ at least $\mathrm{gain}_l - \mathrm{err}_l \ge g_l$ for all twists $\vartheta$ coprime to $l$, multiplying by $c_B$ clears $l^{g_l}$ and ensures $s\mathfrak{p}\varphi \in D.\mathrm{targetLattice.integralVectors}\ \iota$ (`normalizedPoincare_mem_integralVectors`). Local divisibility (`exists_colengthDvd_gaugeLe_compositeLift`) supplies a submodule $S$ of colength dividing $l^j$ with $j \le D_X \cdot \mathrm{err}_l$ on which that valuation bound holds, where $D_X = \dim D.\mathrm{SourceForm} \le 2\dim\mathrm{src}$ (`finrank_sourceForm_le_two_mul`, using $f/(N-1) \le 1/2$). Feeding $e(l) = D_X \cdot \mathrm{err}_l$ into `TwistedLattice.indexLe_of_colengthDvd` gives per-translate index $\le \dim\mathrm{src} \cdot 2\,\mathrm{errorUnit}$ (`indexLe_of_mem_image_normalizedPoincare`), and summing over $s \in t$ via `TwistedLattice.indexLe_card_mul` produces Theorem A (b):
  $$D.\mathrm{targetLattice.IndexLe}\ y\ \bigl(\#t \cdot \dim\mathrm{src} \cdot 2 f a \log^3 b\bigr).$$

- **Dimensional spread — Theorem B (`enough_forms`):**
  The spanning theorem `D.isSpread_goodPoincare.exists_few_le_finrank_iSup_range` applied with $\varepsilon = (8b^2)^{-1}$ provides a finite subset $t \subseteq \bar{G}$ satisfying
  $$\#t \cdot D_X \le \frac{5}{2}\left(1 + \log\frac{1}{16\varepsilon}\right) d_T \le 5 d_T \log(1 + 2b^2), \qquad \left(1 - \frac{2}{b^2}\right) d_T \le \dim \bigvee_{s \in t} \operatorname{im}(D.\mathrm{goodPoincare}\ s),$$
  using `five_halves_mul_one_add_log_le` for the first inequality. Because $c_B$ and $c_{\mathrm{ss}}^{-1}$ are nonzero, `(D.normalizedPoincare s).range = D.ramifiedSourceForms.map (D.goodPoincare s)` (`range_normalizedPoincare`). Passing from all `SourceForm` to the subspace `ramifiedSourceForms` drops at most $D_X - \dim\mathrm{src} \le \frac{f}{N-1} D_X \le b^{-8} D_X$ dimensions per translate, for a cumulative loss of at most
  $$\#t (D_X - \dim\mathrm{src}) \le b^{-8} \cdot 5 \log(1 + 2b^2) \, d_T \le \frac{2}{b^2} d_T.$$
  Subtracting this from $(1 - 2/b^2) d_T$ leaves at least $(1 - 4/b^2) d_T$ independent dimensions inside $\bigvee_{s \in t} \operatorname{im}(s\mathfrak{p})$, while `#t * dim src ≤ #t * D_X ≤ 5 d_T log (1 + 2 b²)` holds immediately. Together with `targetDim_pos` ($d_T > 0$), this proves `D.enough_forms`.

### 3. Connecting `totalRemainder` to $R_{q_0}(x)$, $R_{q_0}(x_1)$, and $R_1(x_1)$ (`PrimeSums.lean`)

At each infinite place $\tau \in \Phi$ of $E$, the weight $\kappa(\tau) = D.\mathrm{weight}\ \tau$ equals $b = a/x$ on the $b$-places $\Phi_b$ (a subset of places $\equiv -1 \pmod{q_0}$) and $a/x_1$ elsewhere. The prime sum at $\tau$ is
$$s_{\mathrm{fin}}(a, \tau) = \sum_{n=1}^{a} \Lambda_K(n) \left(\frac{a}{n} - \kappa(-n\tau)\right)_+, \qquad \Lambda_K(n) = \Lambda(n)(1 - \chi_K(n)),$$
and its expected value over the $a$-places $\Phi_a$ ($\tau \equiv 1 \pmod{q_0}$) is $s_{\mathrm{main}} = a(\log a - f^{-1}\sum_{\tau \in \Phi}\log\kappa(\tau) + c_1)$ with $c_1 = \operatorname{Re}(\mathrm{logDerivMean}\ d\ 1\ 1)$.

When $\tau \in \Phi_a$, the shifted place $-n\tau$ lies in $\Phi_b$ if and only if $n \equiv 1 \pmod{q_0}$ (`mem_aPlaces_iff_of_mul_mem_bPlaces`), and for each such $n$ there are exactly $\#\Phi_b$ places $\tau \in \Phi_a$ with $-n\tau \in \Phi_b$ (`card_filter_mem_bPlaces`). Summing $s_{\mathrm{fin}}(a, \tau)$ over $\tau \in \Phi_a$ (`sum_aPlaces_primeSum`) therefore separates into a modulus-$1$ sum at cutoff $x_1$ weighted by $\#\Phi_a$ plus a modulus-$q_0$ correction between cutoffs $x$ and $x_1$ weighted by $\#\Phi_b$. Using $f = \#\Phi_a \cdot \varphi(q_0)$ (`f_eq_card_aPlaces_mul`), the product $\#\Phi_a \cdot s_{\mathrm{main}}$ (`card_aPlaces_mul_mainPrimeSum`) equals the exact main term of `sum_aPlaces_primeSum`, so subtracting them yields `totalRemainder_eq` (`src/New/Reduction/PrimeSums.lean:180-185`):
$$D.\mathrm{totalRemainder} = \sum_{\tau \in \Phi_a} \bigl(s_{\mathrm{fin}}(a, \tau) - s_{\mathrm{main}}\bigr) = a\Bigl(\#\Phi_b \bigl(R_{q_0}(x) - R_{q_0}(x_1)\bigr) + \#\Phi_a R_1(x_1)\Bigr).$$
This identity directly feeds `New.star_inequality` into the one-sided remainder bound of [Landau's Lemma & the One-Sided Criterion](./one-sided-criterion.md).

## Level 3 — Formal Lean Contract

The extracted Stage 01 skeletons below correspond verbatim to `src/New/Reduction/MainInequality.lean` (lines `86–160`) and `src/New/Reduction/ShortAndEnough.lean` (lines `43–392`) at pinned upstream commit `41c2716658c42ba08eb0648994a2efe83082a95c` (short `41c2716658c4`), with proof bodies replaced by deterministic extraction stubs recording upstream line ranges and SHA-256 digests. In the dependency graph, `New.star_inequality` consumes `D.enough_forms`, `D.span_short_integralRamified_eq_top`, and `D.indexLe_normalizedPoincare` from Stage 01, `TwistedLattice.covolumeLe_of_normSqAt_le` from [Twisted Lattices & A Twist for Each Prime](./twisted-lattices.md), and `D.integral_forms_long` (Theorem C) from Stages 05–07 (`New.Determinant.Twist.LongForms`).

**Verification status (`audit/verification-status.json`):** Local Lean build verification is **`unverified-locally`** (`scan_scope: entry`, pinned commit `41c2716658c42ba08eb0648994a2efe83082a95c`, `0` forbidden tokens in `src/Main.lean`, `5` expected `sorry` tokens in root `Challenge.lean`).

<!-- rh:lean-node:01_reduction/main_inequality -->

<!-- rh:lean-node:01_reduction/short_and_enough -->
