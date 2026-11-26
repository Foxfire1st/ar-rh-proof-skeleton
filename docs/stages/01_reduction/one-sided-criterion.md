# From a One-Sided Prime Bound to a Zero-Free Half-Plane

This landmark page traces the top half of Stage 01 — from `New.star_inequality` through two-cutoff elimination (`New.Reduction.OneSidedBound`), Landau and Suzuki's one-sided criterion (`New.Reduction.Zeros.Criterion`), and functional-equation reflection (`New.Main`) to the Generalized Riemann Hypothesis and the Riemann Hypothesis. Return to the [Stage 01 Overview](./overview.md) for the full stage map, or continue to [The Main Cancellation Inequality and Theorems A & B](./main-inequality.md) to see how `New.star_inequality` itself is proved.

## Level 1 — Abitur / First-Principles

Suppose a suspension bridge is monitored by a single pressure gauge that measures how much traffic piles up above the long-term average. If a hidden resonant wave existed inside the bridge, it would force the pressure gauge to swing wildly both above and below zero with a growing amplitude. Therefore, you do not need to cap swings in both directions: if you can prove a **one-sided ceiling** — showing that the pressure never spikes far *above* zero — no destructive wave can exist above that growth rate.

That one-sided strategy turns a statement about {{concept:prime-number|whole numbers greater than $1$ with no smaller positive divisors}} into a zero-free region in {{concept:complex-plane|the two-dimensional grid of points $s = \sigma + it$}}. Instead of counting raw primes, Stage 01 tracks a gently tapered sum over integers leaving remainder $1$ modulo $q_0$. Each integer $n$ receives {{concept:von-mangoldt-function|the logarithmic prime-power weight $\Lambda(n)$, equal to $\log p$ when $n = p^k$ and $0$ otherwise}} multiplied by {{concept:quadratic-twist|a residue factor $(1 - (n/d))$ built from squares and non-squares modulo an auxiliary prime $d$}}. Because $(n/d)$ takes only the values $-1$, $0$, and $+1$, the combined weight $\lambda(n) = \Lambda(n)(1 - (n/d))$ is never negative, while every prime that splits in $\mathbb{Q}(\sqrt{-d})$ drops out with weight $0$.

Multiplying each term by the smooth taper $(1/n - 1/x)$ and subtracting the expected smooth growth $\log x / \varphi(q_0) + c_{q_0}$ produces a single remainder $R_{q_0}(x)$ that drifts slowly toward $0$. Four steps turn a ceiling on $R_{q_0}(x)$ into the full theorem:

1. **Eliminating the huge second cutoff:** The main inequality bounds $R_{q_0}(x)$ only after mixing it with error terms measured at a vastly larger scale $x_1 \ge x^{10^6}$. Because $x_1$ dwarfs $x$, the classical Prime Number Theorem crushes the $x_1$ error terms below $x^{-2}$, leaving a pure upper bound $R_{q_0}(y) \le C y^{-1/2 + \varepsilon}$ for every real $y$ large enough.
2. **Dodging unwanted zeros by choosing $d$:** The smoothed transform of $R_{q_0}(x)$ detects zeros of both the original functions — each {{concept:dirichlet-character|periodic multiplicative weight $\chi$ modulo $q_0$}} produces {{concept:dirichlet-l-function|a global function $L(\chi, s)$}} — and their partners twisted by $(n/d)$. Those two sets of zeros enter with *opposite signs*, so a twisted zero could accidentally cancel an untwisted zero. Because the one-sided bound holds for **all** primes $d$ in the chosen congruence class, Stage 01 uses a zero-density estimate to pick one prime $d$ whose twisted functions have no zeros at all inside the box reaching up to the candidate point $\rho$.
3. **Springing the one-sided trap:** Once the twisted zeros are cleared from the box, every remaining pole on the real axis above $\beta$ has a nonnegative sign automatically. Applying {{concept:landaus-lemma|the classical analytic principle forbidding a nonnegative-coefficient transform from having a boundary pole without also exploding on the real axis}} forces the residue at $\rho$ to vanish, so no $L(\chi, s)$ modulo $q_0$ can equal $0$ anywhere to the right of $\operatorname{Re}(s) = \beta$.
4. **Inducing and reflecting to the center line:** Given any character modulo an arbitrary $q$, Stage 01 lifts it to modulus $q_0 = 18480 q$, applies the zero-free half-plane bound there, and divides out the finitely many extra prime factors at primes dividing $18480$. That clears the entire half-plane $\operatorname{Re}(s) > 1/2$ for every character. Finally, functional-equation reflection across {{concept:critical-line|the central vertical line $\operatorname{Re}(s) = 1/2$}} rules out zeros on the left half of {{concept:critical-strip|the open vertical band $0 < \operatorname{Re}(s) < 1$}} as well, pinning every nontrivial zero of every {{term:dirichlet-l-function}} — and of {{concept:riemann-zeta-function|the global function $\zeta(s)$}} away from {{concept:trivial-zeros|the predictable negative even integers $-2, -4, -6, \dots$}} — directly onto the {{term:critical-line}}.

![One-sided Landau funnel showing star_inequality feeding two-cutoff PNT elimination, real-variable smoothing, quadratic-twist zero avoidance, Landau's lemma, and New.grh reflection](assets/stage01-one-sided-landau-funnel.svg)

### Where this analogy breaks

Comparing $R_{q_0}(x)$ to a mechanical pressure gauge suggests that any one-sided upper bound immediately prevents every wave from growing. In reality, {{term:landaus-lemma}} requires an exact sign condition on the real axis: if a real zero of $L(\chi, s)$ happened to produce a *negative* residue along the real line, it could mask complex oscillations and break the one-sided trap. What makes this specific smoothed kernel work without assuming "no real zeros" is that the pole denominator $s(1 - s)$ is nonnegative on $(\beta, 1]$, while classical nonvanishing rules out real zeros on $[1, \infty)$ — and even that sign positivity only survives after the {{term:quadratic-twist}} prime $d$ is chosen to wipe out all negatively signed twisted zeros inside the box.

## Level 2 — Undergrad Bridge

### 1. The Smoothed Prime Remainder and Its Laplace Transform

In `src/New/Reduction/Zeros/PrimeError.lean`, fix a prime $d$ and a modulus $q \ge 1$. Write $\chi_K(n) = (n/d)$ for the Legendre symbol modulo $d$, and define the nonnegative arithmetic weight (`New.lambdaKResidueClass`):

$$\lambda(n) = \begin{cases} \Lambda(n)(1 - \chi_K(n)) & \text{if } n \equiv 1 \pmod q, \\ 0 & \text{otherwise.} \end{cases}$$

Since $\chi_K(n) \in \{-1, 0, 1\}$, we have $0 \le 1 - \chi_K(n) \le 2$ and $0 \le \lambda(n) \le 2\Lambda(n)$. The smoothed prime remainder (`New.primeRemainder`) is

$$R_q(x) = \sum_{1 \le n \le x} \lambda(n)\left(\frac{1}{n} - \frac{1}{x}\right) - \frac{\log x}{\varphi(q)} - c_q,$$

where $c_q = \operatorname{Re} G_q(1)$ and $G_q(s)$ (`New.logDerivMean d q`) is the mean logarithmic-derivative difference across all Dirichlet characters $\psi \bmod q$:

$$G_q(s) = \frac{1}{\varphi(q)}\left(\sum_{\psi \bmod q} \left(\frac{(\operatorname{entireL}(\psi\chi_K))'}{\operatorname{entireL}(\psi\chi_K)}(s) - \frac{(\operatorname{entireL}\psi)'}{\operatorname{entireL}\psi}(s)\right) - 1\right),$$

with $\operatorname{entireL}\psi$ equal to $L(s, \psi)$ multiplied by $(s - 1)$ when $\psi$ is principal.

Why smooth by the linear taper $(1/n - 1/x)$ rather than using the sharp counting error $\mathcal{E}_q(t) = \sum_{n \le t} \lambda(n) - t/\varphi(q)$ directly? Partial summation (`primeRemainder_eq_integral_countError_sub`) shows

$$R_q(x) = \int_1^x \frac{\mathcal{E}_q(t)}{t^2}\,dt - c_q.$$

Integrating once by $t^{-2}$ buys two crucial analytic properties simultaneously:
- **Slow variation:** Chebyshev's bound $|\mathcal{E}_q(t)| \le 13 t$ (`abs_countError_le`) immediately gives $|R_q(z) - R_q(y)| \le 13(z - y)/y$ for $1 \le y \le z$ (`abs_primeRemainder_sub_le`), which later bridges integers $\lfloor y \rfloor$ to all real $y$.
- **Simple-pole Laplace transform (`New.isPolar_primeRemainder`):** Changing variables to $x = e^v$, the Laplace integral $\int_0^\infty R_q(e^v)\,e^{(1-s)v}\,dv$ converges absolutely on $\operatorname{Re} s > 1$ (where the Dirichlet series of $\lambda$ equals $s/(\varphi(q)(s-1)) + G_q(s)$) and continues meromorphically to $\operatorname{Re} s > 0$ with at most simple poles, **no pole at $s = 1$**, and exact residue at every $s \neq 1$:
  $$r(s) = \frac{n(s) - n'(s)}{\varphi(q)\,s(1 - s)},$$
  where $n(s) = \sum_{\psi \bmod q} \operatorname{ord}_s(\operatorname{entireL}\psi)$ and $n'(s) = \sum_{\psi \bmod q} \operatorname{ord}_s(\operatorname{entireL}(\psi\chi_K))$.

Under the substitution $x = e^v$, an eventual upper bound $R_{q_0}(x) \le A x^{\beta - 1}$ becomes $R_{q_0}(e^v) \le A e^{(\beta - 1)v}$. For $\beta = 1/2 + \varepsilon$, the exponent $\beta - 1 = -1/2 + \varepsilon$ is precisely the abscissa at which the Laplace integrand $e^{(\beta - 1)v} e^{(1 - s)v} = e^{(\beta - s)v}$ decays exponentially throughout $\operatorname{Re} s > \beta$.

### 2. Why `Criterion` Quantifies Over All Primes $d \equiv d_0 \pmod{q_0}$ — and Needs No "No Real Zeros" Hypothesis

In `src/New/Reduction/Zeros/Criterion.lean`, `New.LFunction_ne_zero_of_primeRemainder_le` assumes $8 \mid q_0$, $d_0 \equiv 7 \pmod 8$, $\gcd(d_0, q_0) = 1$, $\beta > 1/2$, and that **for every prime** $d \equiv d_0 \pmod{q_0}$ there exists $A \in \mathbb{R}$ with $R_{q_0}(x) \le A x^{\beta - 1}$ eventually.

Why must the hypothesis hold for *all* primes $d$ in that residue class rather than one fixed $d$? Look at the numerator $n(s) - n'(s)$ of the residue $r(s)$:
- A zero of an untwisted function $L(s, \psi)$ contributes positively to $n(s)$, while a zero of a twisted function $L(s, \psi\chi_K)$ contributes negatively via $-n'(s)$.
- If we fixed a single $d$ in advance, a zero of $L(s, \psi\chi_K)$ could coincide with $\rho$ (canceling $n(\rho)$) or sit on the real interval $(\beta, 1)$ (making $\operatorname{Re} r(x) < 0$ and invalidating Landau's lemma).
- To prevent both hazards, `Criterion.lean` calls `exists_entireL_quadTwist_ne_zero` (`src/Lib/Analytic/QuadraticTwist.lean`), which proves by a theta-series tail integral, Hadamard's three-lines theorem, and Pólya–Vinogradov mean-square estimates that at most $O(X^{1 - \delta/2})$ primes $d \le X$ are "bad" (having any twisted zero in $\operatorname{Re} s \ge \beta$, $|\operatorname{Im} s| \le |\operatorname{Im} \rho|$). Since $\sum_{p \equiv d_0 \pmod{q_0}} (\log p)/p$ diverges, there exists at least one good prime $d \equiv d_0 \pmod{q_0}$ with `entireL (quadTwist χ d) s ≠ 0` throughout the entire box $\operatorname{Re} s \ge \beta$, $|\operatorname{Im} s| \le |\operatorname{Im} \rho|$.

For that chosen prime $d$, we have $n'(s) = 0$ identically inside the box, so $r(s) = n(s)/(\varphi(q_0)\,s(1-s))$. Now check real points $x > \beta$:
- If $\beta < x \le 1$: $n(x) \in \mathbb{N}$ is nonnegative, $\varphi(q_0) > 0$, and $x(1 - x) \ge 0$, so $\operatorname{Re} r(x) = n(x)/(\varphi(q_0)\,x(1-x)) \ge 0$.
- If $x \ge 1$: `entireL_ne_zero` guarantees $\operatorname{entireL}\chi(x) \neq 0$ for every $\chi \bmod q_0$, so $n(x) = 0$ and $r(x) = 0$.

Thus $\operatorname{Re} r(x) \ge 0$ holds automatically for **every** real $x > \beta$, with no separate hypothesis excluding real zeros in $(\beta, 1)$. Landau's lemma (`h_polar.landau`) then forces $r(\rho) = 0$, which (since $\rho \neq 0, 1$) forces $n(\rho) = 0$, hence $\operatorname{entireL}\psi(\rho) \neq 0$ and $L(\rho, \psi) \neq 0$.

### 3. Two-Cutoff Elimination ($x \ll x_1 \ll b$) and Real-Variable Passage

`src/New/Reduction/OneSidedBound.lean` connects `New.star_inequality` ($m \cdot \text{rem} \le f a \log^5 b$) to the hypothesis of `Criterion` in five cleanly separated lemmas:

1. **Isolating $R_{q_0}(x)$ (`New.Data.primeRemainder_le`, lines `37–71`):**
   Expanding `D.totalRemainder_eq` expresses `D.totalRemainder` in terms of $R_{q_0}(x)$, $R_{q_0}(x_1)$, and $R_1(x_1)$. Dividing `star_inequality` by $m a \#\Phi_b$ and substituting $f = \varphi(q_0)\#\Phi_a$ together with $\#\Phi_a / \#\Phi_b \le \log b$ (`D.towerDeg_le_card_bPlaces_mul`) yields
   $$R_{q_0}(x) \le \frac{\varphi(q_0)\log^6 b}{m} + |R_{q_0}(x_1)| + (\log b)|R_1(x_1)|.$$

2. **Admissible scale selection (`New.eventually_exists_data_scales`, lines `78–93`):**
   For any $0 < \varepsilon < 1/100$ and all sufficiently large integers $x$, `eventually_exists_data` produces admissible data $D : \text{Data } P$ with $D.x = x$, $\log b \le 2 x^\varepsilon$, $\sqrt{x} \le m$, and $x^{\varepsilon/12} \le \log x_1$ (while preserving the hierarchy $(\log b)^{100} \le x$, $x^{10^6} \le x_1$, $x_1^{10^6} \le b$).

3. **Crushing the $x_1$ remainders by PNT (`New.eventually_abs_primeRemainder_exp_le`, lines `99–121`):**
   Classical PNT in arithmetic progressions (`New.eventually_abs_primeRemainder_le` in `PrimeNumberTheorem.lean`) saves every power of the logarithm: $|R_q(y)| \le (\log y)^{-B}$ for any $B \in \mathbb{N}$ and large $y$. Choosing $B \ge 24/\varepsilon$, any $\ell \ge x^{\varepsilon/12}$ satisfies
   $$\frac{1}{(\log e^\ell)^B} = \frac{1}{\ell^B} \le x^{-(\varepsilon/12)B} \le x^{-2}.$$
   Applying this at $\ell = \log x_1$ bounds both $|R_{q_0}(x_1)| \le x^{-2}$ and $|R_1(x_1)| \le x^{-2}$.

4. **Integer-scale bound (`New.eventually_primeRemainder_natCast_le`, lines `129–159`):**
   Substituting the scale inequalities and the $x^{-2}$ PNT bounds into `Data.primeRemainder_le` gives:
   $$R_{q_0}(x) \le \frac{\varphi(q_0)(2 x^\varepsilon)^6}{x^{1/2}} + x^{-2} + 2 x^\varepsilon \cdot x^{-2} \le (64\varphi(q_0) + 3)\,x^{-1/2 + 6\varepsilon}.$$

5. **Real-variable bridge (`New.eventually_primeRemainder_le_mul_rpow`, lines `204–235`):**
   *(Note on exact naming: the Lean theorem at `src/New/Reduction/OneSidedBound.lean:204` is `New.eventually_primeRemainder_le_mul_rpow`; upstream's summary table `INDEX.md:21` contains a minor typo printing `eventually_primeRemainder_le_rpow`.)*
   Choose $\delta = \min(1/200, \varepsilon/6) \in (0, 1/100)$ so $t = -1/2 + 6\delta \in [-1, 0]$ and $t \le -1/2 + \varepsilon$, and set $C = 64\varphi(q_0) + 3$. For all large real $y \ge 2$:
   - `primeRemainder_sub_primeRemainder_floor_le` bounds the fractional jump: $R_{q_0}(y) - R_{q_0}(\lfloor y \rfloor) \le 13(y - \lfloor y \rfloor)/\lfloor y \rfloor \le 26/y = 26 y^{-1} \le 26 y^t$.
   - `natFloor_rpow_le_two_mul_rpow` converts $\lfloor y \rfloor^t$ to $y^t$: since $\lfloor y \rfloor \ge y/2$ and $-1 \le t \le 0$, $\lfloor y \rfloor^t \le (y/2)^t = y^t / 2^t \le 2 y^t$.
   Combining both gives
   $$R_{q_0}(y) \le C(2 y^t) + 26 y^t = (128\varphi(q_0) + 32)\,y^t \le (128\varphi(q_0) + 32)\,y^{-1/2 + \varepsilon}.$$

### 4. Assembling `New.grh` and Reflecting to the Critical Line

`New.grh : DirichletLNonvanishingRightOfHalf` (`src/New/Reduction/OneSidedBound.lean:239-268`) takes any modulus $q \ge 1$, any character $\chi \bmod q$, and any $\rho \in \mathbb{C}$ with $\operatorname{Re} \rho > 1/2$ and $(\chi \neq 1 \lor \rho \neq 1)$:

1. **The boundary point $\rho = 1$:** Dispatched immediately by Dirichlet's theorem (`χ.LFunction_ne_zero_of_one_le_re h_pole le_rfl`).
2. **Induction to $q_0 = 18480 q$:** For $\rho \neq 1$, pick $\beta \in (1/2, \operatorname{Re} \rho)$ and set $q_0 = 18480 q$ and $d_0 = 9240 q - 1 = q_0/2 - 1$. Then:
   - $8 \mid q_0$ (since $18480 = 8 \times 2310$),
   - $d_0 \equiv 7 \pmod 8$ (since $8 \mid 9240$),
   - $\gcd(d_0, q_0) = 1$ (since $d_0$ is odd and $q_0 = 2(d_0 + 1)$),
   - every prime dividing $q_0$ splits in $K = \mathbb{Q}(\sqrt{-d})$ (for every prime $d \equiv d_0 \pmod{q_0}$, $-d \equiv 1 \pmod 8$ splits $2$ and $-d \equiv 1 \pmod p$ is a square for every odd prime $p \mid q_0$),
   - for every prime $d \equiv d_0 \pmod{q_0}$, `dvd_two_mul_succ_and_not_dvd_succ` verifies $q_0 \mid 2(d + 1)$ and $q_0 \nmid (d + 1)$, which is the exact divisibility requirement in `Parameters`.
3. **Applying the criterion and peeling off Euler factors:** Setting $\varepsilon = \beta - 1/2 > 0$ so $\beta - 1 = -1/2 + \varepsilon$, `eventually_primeRemainder_le_mul_rpow` supplies the uniform one-sided bound with constant $A = 128\varphi(18480 q) + 32$. `LFunction_ne_zero_of_primeRemainder_le` then yields $L(\chi_{\mathrm{ind}}, \rho) \neq 0$, where $\chi_{\mathrm{ind}} = \chi.\text{changeLevel}(q \mid 18480 q)$. Rewriting by `LFunction_changeLevel _ χ h_pole` expresses $L(\chi_{\mathrm{ind}}, \rho)$ as $L(\chi, \rho)$ multiplied by the finite Euler-factor product $\prod_{p \mid 18480 q, p \nmid q} (1 - \chi(p) p^{-\rho})$. Applying `left_ne_zero_of_mul` concludes $L(\chi, \rho) \neq 0$ directly — in this direction, nonvanishing of a product implies nonvanishing of its left factor without needing any separate check that the Euler factors are nonzero.
4. **Functional-equation reflection (`src/New/Main.lean:32-44`):**
   - `New.zeros_on_line` applies `New.grh` to both $s$ and $1 - s$ via `New.gammaFactor_eq_zero_or_LFunction_one_sub_eq_zero` and `Complex.Gammaℝ_ne_zero_of_re_pos`, ruling out both $\operatorname{Re} s > 1/2$ and $0 < \operatorname{Re} s < 1/2$.
   - `New.zeta_zeros_on_line` specializes `New.grh` to modulus $1$, checks $\zeta(0) = -1/2 \neq 0$ via `riemannZeta_zero`, and uses `completedRiemannZeta_one_sub` and `Complex.Gammaℝ_eq_zero_iff` to show every zero of $\zeta$ outside $\{1, -2, -4, \dots\}$ has $\operatorname{Re} s = 1/2$.

## Level 3 — Formal Lean Contract

The three extracted skeleton modules below correspond to `src/New/Reduction/Zeros/Criterion.lean`, `src/New/Reduction/OneSidedBound.lean`, and `src/New/Main.lean` at pinned upstream commit `41c2716658c42ba08eb0648994a2efe83082a95c` (`41c2716658c4`, `leanprover/lean4:v4.37.0-rc1`, Mathlib rev `31ce06eaadf4a200c0e1407c5f10953cf606b8f8`). Note that `New.eventually_primeRemainder_le_mul_rpow` (`src/New/Reduction/OneSidedBound.lean:204`) is the live declaration name in Lean, whereas upstream's `INDEX.md:21` prints `eventually_primeRemainder_le_rpow`.

**Verification status (`audit/verification-status.json`):** Local Lean compilation and kernel replay remain **`unverified-locally`**; local static token scanning has **`scan_scope: entry`** (covering only `Challenge.lean`, `src/Main.lean`, and `src/Statements.lean`), so these Stage 01 modules have not been locally compiled or statically scanned. Proof bodies in the embedded skeletons below are intentionally replaced with extraction stubs carrying exact upstream line ranges and SHA-256 digests. Continue to [The Main Cancellation Inequality and Theorems A & B](./main-inequality.md) to examine `New.star_inequality`.

<!-- rh:lean-node:01_reduction/criterion -->

<!-- rh:lean-node:01_reduction/one_sided_bound -->

<!-- rh:lean-node:01_reduction/new_main -->
