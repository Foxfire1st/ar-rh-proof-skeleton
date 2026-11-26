# Sharpness: Proving the Valuation Bound Is Attained

This page explains how Stage 02 proves that its prime-by-prime divisibility rate $S_p = D.\mathrm{localPrimeSumTotal}\ p$ is sharp (`src/New/Derivatives/Sharpness/BoundAttained.lean`, built on `LeastValuation.lean` and `Cases.lean`), producing a single global nonzero scalar $t \in \mathbb{C}$ with uniformly bounded conjugates that witnesses the attained valuation bound across all primes simultaneously. Return to the [Stage 02 Overview](./overview.md), review the divisibility lower bound in [Formal Groups, Theta Normal Form & Prime-Counting Divisibility](./formal-group-and-divisibility.md), or revisit the vacuum construction in [Tight Frames, Chowla–Selberg Scaling & the Integral Vacuum $\theta^\circ$](./tight-frames-and-integral-vacuum.md).

## Level 1 — First-Principles

Earlier in Stage 02, the counting argument proved a *divisibility lower bound*: for any integral theta function, every derivative of degree $l$ at a non-split {{concept:prime-number|indivisible whole number greater than $1$}} is divisible by at least $p^{S_p}$. That high divisibility shrinks vectors inside the {{concept:twisted-lattice|structure that measures lengths in one fixed standard space while checking divisibility across a family of prime-dependent spaces}}, which is exactly what Theorem A needs. Later in Stage 06, however, the proof needs the *reverse* inequality: to control the {{concept:arithmetic-height|global accounting rule balancing a number's ordinary size against its divisibility at every prime}} of rank-one theta-lift Hecke $L$-values, we must put an *upper bound* on their {{term:arithmetic-height|p-adic valuation}}. In other words, we must prove that the divisibility rate $S_p$ is *sharp*—that some honest theta derivative actually achieves valuation essentially no larger than $S_p$, rather than hiding an unexpected extra factor of $p$ everywhere.

At first glance, proving sharpness looks straightforward: on a single one-dimensional {{concept:complex-multiplication|elliptic curve with extra imaginary-quadratic symmetries}} (a {{term:complex-multiplication|CM curve}}), you can write down a power series in the curve's {{concept:formal-group-law|power-series addition law near the origin encoding arithmetic congruences modulo a prime}} whose {{term:formal-group-law|formal parameter}} achieves the exact Katz divisibility limit. The catch is that an honest adelic theta function never looks like an arbitrary power series in that coordinate: it always comes multiplied by an unavoidable prefactor $\Theta = \prod \theta(\mu_i z)^{c_i}$, a {{concept:elliptic-theta-function|quasi-periodic entire function product built from one-dimensional theta sections}} along a {{concept:tight-frame|redundant collection of directions whose one-dimensional projections sum back to the exact identity metric}}. You cannot simply divide by $\Theta$ inside one copy without breaking the theta-function transformation law.

The solution is a **6 + 6 = 12 copy** cancellation construction powered by Lagrange's four-square theorem:

1. **Neutralize $\Theta$ with six copies.** By Lagrange's theorem extended to $p$-adic integers, $-1$ is always a sum of four $p$-adic squares $a_0^2 + a_1^2 + a_2^2 + a_3^2 = -1$. Taking the six $p$-adic scales $\delta = (1, a_0, a_1, a_2, a_3, 0)$ gives $\sum_{c=0}^5 \delta_c^2 = 0$. When you multiply six scaled copies $\Theta(\delta_0 z)\cdots\Theta(\delta_5 z)$, the quadratic anomaly in the exponent is proportional to $\sum \delta_c^2 = 0$ and cancels identically—turning the 6-copy product into an *invertible unit* in the integral power-series ring.
2. **Spend the first 6 copies on partner couplings.** Neutralizing $\Theta$ once frees the first 6 copies to implement the {{concept:theta-normal-form|canonical coordinate form aligning a place with its Frobenius partner}}, capturing all the valuation savings from near pairs of {{term:theta-normal-form|partner places}} for the coupled degree $\nu' + \nu''$.
3. **Spend the second 6 copies on the remainder.** Neutralizing $\Theta$ a second time frees the remaining 6 copies to achieve the single-curve Katz limit on the leftover degree $\eta = l - (\nu' + \nu'')$. Multiplying the two 6-copy series gives **$6 + 6 = 12$ copies**, attaining the sharp valuation bound in total degree $l$ at every prime $p$.
4. **Lift back to one global multiplier $t$.** Finally, the proof lifts these 12-copy monomial products back from the {{term:complex-multiplication|CM curve}} into a 12-copy adelic space (`twelveSpace`) using the {{term:tight-frame}} resolution theorem, applies finite Fourier inversion modulo the level ideal $\mathfrak{m} = (\pi_M)$ to isolate an explicit congruence indicator (`indicatorDatum`) whose {{concept:schur-jet|multivariate Taylor series recording all partial derivatives at the origin}} attains the bound, and collects the level factor $M^{12f}$, the squared factorial-bridging loss products $\prod_\tau B_\tau^2$, and a single nonzero value $c_0$ of the integral vacuum $\theta^\circ$ into **one single nonzero** {{concept:complex-plane|number with real and imaginary parts}} $t \in \mathbb{C}$. Every conjugate of $t$ is bounded by $\exp(f m b / k)$, yet that one fixed $t$ works simultaneously at *every* prime $p$ and *every* $p$-adic embedding.

![Paired coordinate couplings and the 6 plus 6 equals 12 copy mechanism attaining the valuation count at every prime](assets/stage02-partner-couplings-and-divisibility-count.svg)

### Where this analogy breaks

Saying that six copies "cancel $\Theta$" can make it sound as if the final 12-copy theta function has stripped away $\Theta$ altogether. In reality, the $p$-adic scales $\delta_c \in \mathbb{Z}_p$ are *only* used as a diagonal substitution (`copySubst`) to bound the least valuation among the coefficients of an *unscaled* 12-copy product $\prod_{b=0}^{11} (H_b \Theta)(z_b)$: because every coefficient of the scaled diagonal series is an integral combination of coefficients of the unscaled 12-copy series, exhibiting one diagonal coefficient of small {{term:arithmetic-height|p-adic valuation}} forces some unrestricted 12-copy coefficient to have valuation at least as small.

Nor are the witnessing derivative multi-index $\gamma$ and congruence class $y_0 \pmod{\mathfrak{m}}$ the same at every prime. Only the global multiplier $t$ (with its uniform archimedean bound across all symmetries of the {{term:complex-plane}}) is fixed once and for all across all primes; for each prime $p$ and embedding $\iota_p$, the theorem supplies a prime-specific 12-copy exponent $\gamma$ (whose 12 parts sum to $l$) and residue class $y_0$ attaining $S_p$.

Finally, the attained valuation is $S_p + v_p(t)$ rather than bare $S_p$: there is an extra finite discrepancy $v_p(t)$ contributed by the level power $M^{12f}$, the squared loss products $B_\tau^2$ bridging split factorials back to $l!$, and the vacuum value $c_0$. Because $t$ is a single global number whose archimedean conjugates stay below $\exp(f m b / k)$—negligible relative to the main height budget—its contribution is absorbed cleanly in Stage 06.

## Level 2 — Undergrad Bridge

### 1. Why sharpness is needed and the cross-pillar DAG edge

Stage 02's divisibility count (`Count.lean`) establishes a *lower bound* on $p$-adic valuations (an upper bound on $p$-adic norms), which makes integral theta derivatives highly divisible and produces short vectors for Theorem A. In Stage 06 (`06_one_lift/twisted_value_heights`), however, controlling the arithmetic height of rank-one theta-lift Hecke $L$-values requires the opposite direction: an *upper bound* on $p$-adic valuations (a *lower bound* on $p$-adic norms) showing that the divisibility count $S_p = D.\mathrm{localPrimeSumTotal}\ p$ is actually attained.

This creates an explicit cross-pillar DAG edge in the proof architecture:
$$\text{\texttt{03\_upper\_bound/short\_columns}} \;\longrightarrow\; \text{\texttt{02\_derivatives/bound\_attained}} \;\longrightarrow\; \text{\texttt{06\_one\_lift/twisted\_value\_heights}}.$$
Here `03_upper_bound/short_columns` defines the purely combinatorial prime-sum rate $S_p = D.\mathrm{localPrimeSumTotal}\ p$ attached to admissible data $D$; `02_derivatives/bound_attained` proves that $S_p$ is attained by a 12-copy indicator theta derivative up to a single global conjugate-bounded factor $t$; and `06_one_lift/twisted_value_heights` consumes `New.Data.exists_rpow_neg_localPrimeSumTotal_le_norm_coeff` to bound rank-one lift heights.

### 2. Why 12 copies: Lagrange's four-square theorem and the 6 + 6 decomposition

In `src/New/Derivatives/Sharpness/LeastValuation.lean`, for a CM lattice $C$, linear forms $\mu_j$ over $\mathbb{C}_p$, multiplicities $m_j$, and $\Theta = C.\mathrm{thetaProd}\ \mu\ m = \prod_j \theta(\mu_j z)^{m_j}$, the relation **`C.LeastValLE μ m n ν r`** (written $\operatorname{val}_n(\nu) \le r$) asserts that for some integral power series $H_0, \dots, H_{n-1} \in C.\mathrm{multiSeriesRing}\ \mu$ (integral series in the $y(\mu_j z)$) and some $n$-copy exponent $\gamma$ summing across the $n$ copies to $\nu$, the coefficient of $z^\gamma$ in $\prod_{c=0}^{n-1} (H_c \Theta)(z_c)$ has $p$-adic norm at least $p^{-r}$ (valuation $\le r$).

**Why 6 copies cancel $\Theta$ (`LeastValuation.lean:489–505`).**
By Lagrange's four-square theorem on $\mathbb{N}$ and compactness of $\mathbb{Z}_p^4$, **`New.padicInt_sum_four_squares`** represents $-1 \in \mathbb{Z}_p$ as $\sum_{i=0}^3 a_i^2 = -1$. Defining the six $p$-adic scales
$$\delta := (1,\, a_0,\, a_1,\, a_2,\, a_3,\, 0) : \mathrm{Fin}\ 6 \to \mathbb{Z}_p$$
gives $\delta_0 = 1$ and $\sum_{c=0}^5 \delta_c^2 = 0$. In **`exists_scaledThetaProd_mem_mul_eq_one`**, this zero sum of squares cancels the quadratic exponential factor in $\theta$, making the 6-copy scaled product
$$C.\mathrm{scaledThetaProd}\ \mu\ m\ \delta = \prod_{c=0}^5 \Theta(\delta_c z)$$
an element of `C.multiSeriesRing μ` with constant term $1$ that admits a multiplicative inverse $V \in C.\mathrm{multiSeriesRing}\ \mu$.

**Single-curve Katz limit on 6 copies (`leastValLE_of_unit_coefficients`, `LeastValuation.lean:513–545`).**
If some form $\mu_{i_0}$ has unit coefficients ($\|\mu_{i_0}(x)\| = 1$ for all $x$), choose $g \in C.\mathrm{integralSeriesRing}$ attaining `C.katz_divisibility_limit` in degree $|\nu|$. Setting $H_0 := g(\mu_{i_0} z) \cdot V$ and $H_c := 1$ for $c \neq 0$, rescaling copy $c$ by $\delta_c$ collapses the 6-copy product identically to $\prod_{c=0}^5 (H_c \Theta)(\delta_c z) = g(\mu_{i_0} z)$ (using $\delta_0 = 1$ and $V \cdot \mathrm{scaledThetaProd} = 1$). Because integral diagonal rescaling can only increase valuations (`norm_coeff_copySubst_le` / `leastValLE_of_rescale`), this yields
$$\operatorname{val}_6(\nu) \le \gamma_p |\nu| - v_p(\nu!),$$
where $\gamma_p = \mathrm{gammaPrime}\ d\ p = \frac{1}{p-1} - \rho_p$ equals $0$, $\frac{p}{p^2 - 1}$, or $\frac{1}{2p - 2}$ according as $p$ is split, inert, or ramified in $K = \mathbb{Q}(\sqrt{-d})$.

**Combining 6 + 6 = 12 copies across all primes (`Cases.lean:505–670`).**
For any degree $l$ in the near-weight window $\kappa_\tau \le l_\tau \le \kappa_\tau + x^{42}$ (`D.IsNearWeights l`):
- **Non-split primes $p \le k$ (`leastValLE_twelve_localPrimeSumTotal_of_le`, `Cases.lean:602–650`):** Let $\Phi_0 = \{\tau \in D.\Phi \mid \exists e,\, D.\mathrm{IsNearPair}\ p\ \tau\ e\}$ be the starts of near pairs, and couple each $\tau \in \Phi_0$ with its partner $-p^{e_\tau}\tau$ exactly $j_\tau = \lfloor l_\tau / p^{e_\tau} \rfloor$ times. Writing $\nu' = \mathrm{powerDegree}$ and $\nu'' = \mathrm{partnerDegree}$ for the two coupled multi-indices, split the target degree as
  $$l = (\nu' + \nu'') + \eta.$$
  1. The **first 6 copies** harvest the Frobenius partner couplings via **`leastValLE_six_of_isNearPair`**:
     $$v_p(\nu'!\,\nu''!) + \operatorname{val}_6(\nu' + \nu'') \le \gamma_p |\nu' + \nu''| - \sum_{\tau \in \Phi_0} j_\tau.$$
  2. The **second 6 copies** supply the remaining degree $\eta$ at the single-curve Katz rate via **`leastValLE_of_unit_coefficients`**:
     $$v_p(\eta!) + \operatorname{val}_6(\eta) \le \gamma_p |\eta|.$$
  3. Combining the two via `LeastValLE.add` produces a 12-copy bound for $l$, and comparing $v_p(l!)$ against $v_p(\nu'!) + v_p(\nu''!) + v_p(\eta!)$ introduces the loss products $B_\tau = D.\mathrm{lossProduct}(l_\tau) = (l_\tau^- + 1)\cdots(l_\tau^- + n)$ (with $n = \lceil 6b/k \rceil$, $l_\tau^- = (l_\tau - n)_+$), giving $v_p(l!) + \operatorname{val}_{12}(l) \le S_p + \sum_\tau v_p(B_\tau)$.
- **Split primes (`leastValLE_twelve_of_split`):** Here $S_p = 0$ and $\gamma_p = 0$, so `leastValLE_of_unit_coefficients` on 6 copies plus `leastValLE_zero` on 6 copies gives $v_p(l!) + \operatorname{val}_{12}(l) \le 0 = S_p$.
- **Large inert primes $p > k$ (`leastValLE_twelve_of_lt`):** All places are coupled at $e_\tau = 1$ in the first 6 copies (`leastValLE_six_of_isCouplingFamily`), while the second 6 copies absorb $\eta$ at valuation $\le 0$ (`leastValLE_six_zero_of_inert`), yielding $v_p(l!) + \operatorname{val}_{12}(l) \le S_p + 2\sum_\tau v_p(B_\tau)$.

Unifying all three regimes gives **`New.Data.leastValLE_twelve`** (`Cases.lean:655–669`): for *every* prime $p$,
$$v_p(l!) + \operatorname{val}_{12}(l) \le S_p + 2 \sum_{\tau \in D.\Phi} v_p(B_\tau).$$

### 3. Lifting `val₁₂` from the curve back to adelic Schwartz data

`src/New/Derivatives/Sharpness/BoundAttained.lean` translates `leastValLE_twelve` from $p$-adic power series on `P.cmCurve` back to explicit adelic Schwartz functions on 12 copies:
- **`New.Data.twelveSpace`**: the standard hermitian space $\operatorname{Hom}(E, E^{12})$ indexed by `Fin 12 × Unit` with all signs $+1$.
- **`New.Data.twelveTaylor`**: `Space.jetSeries` on `D.twelveSpace` at scale $\pi_M \cdot \mathrm{jetScale}$.
- **`New.Data.indicatorDatum 𝔪 y₀`**: the vector in $\mathcal{S}(D.\mathrm{twelveSpace})$ that is the Gaussian `Fock.vacVector` at infinity, the indicator of the matrix class $y_0 \in M_{12 \times 1}(\mathcal{O}_E / \mathfrak{m})$ at places dividing $\mathfrak{m} = (\pi_M)$, and `Space.vacElem` at all other finite places.

Three lemmas carry the bound from the curve to `indicatorDatum`:
1. **Tight-frame realization (`exists_integral_isThetaFunction_twelveTaylor_eq`, `BoundAttained.lean:158–218`):** Whenever $3 e_{b,i} \le c_i$, applying `exists_isResolutionProduct_of_isTightFrame` builds a vector $F \in \mathcal{S}(D.\mathrm{twelveSpace})$ that is $p$-adically integral at all finite points, is a theta function for $\bar{\pi}_M \mathfrak{a}_0^{12}$, and satisfies
   $$(D.\mathrm{twelveTaylor}\ F).\mathrm{map}\ \iota_p = \prod_{b : \mathrm{Fin}\ 12} \mathrm{rename}(\mathrm{copyEmbedding}\ b)\!\left(\left(\prod_i y(\mu_i z)^{e_{b,i}}\right) \Theta\right)$$
   (assembled via `map_subst_linearSeries_copy` and `coeff_prod_rename_copyEmbedding`).
2. **Finite Fourier inversion (`exists_norm_le_norm_indicatorDatum`, `BoundAttained.lean:220–274`):** Because $F$ is integral at finite adelic vectors and fixed by signed translations along $\bar{\pi}_M \mathfrak{a}_0^{12}$, expanding $F$ modulo $\mathfrak{m} = (\pi_M)$ shows that for every linear form $L : \mathcal{S}(D.\mathrm{twelveSpace}) \to \mathbb{C}$ and every finite adelic vector $y$, there exists $y_0 \in M_{12 \times 1}(\mathcal{O}_E/\mathfrak{m})$ with
   $$\bigl\|\iota_p\bigl(M^{12f} \cdot L(F) \cdot D.\mathrm{twelveSpace}.\mathrm{vacElem}(\mathrm{finVec}\ y)\bigr)\bigr\| \le \bigl\|\iota_p\bigl(L(D.\mathrm{indicatorDatum}\ \mathfrak{m}\ y_0)\bigr)\bigr\|.$$
3. **Reduction to the curve (`exists_le_norm_coeff_indicatorDatum`, `BoundAttained.lean:276–325`):** Taking $L = \mathrm{coeff}\ \gamma \circ D.\mathrm{twelveTaylor}$, assuming $c_i \ge 3|l|$ and `LeastValLE ... 12 l r`, and multiplying through by `vacuumScale ^ 12` (turning `vacElem` into `intVacuum`), there exist $\gamma$ summing across the 12 copies to $l$ and $y_0 \pmod{\mathfrak{m}}$ such that
   $$p^{-r} \cdot \bigl\|\iota_p\bigl(M^{12f} \cdot \mathrm{intVacuum}\ D.\mathrm{twelveSpace}\ (\mathrm{finVec}\ y)\bigr)\bigr\| \le \bigl\|\iota_p\bigl(\mathrm{vacuumScale}^{12} \cdot \mathrm{coeff}\ \gamma\ (D.\mathrm{twelveTaylor}\ (D.\mathrm{indicatorDatum}\ \mathfrak{m}\ y_0))\bigr)\bigr\|.$$

### 4. One global conjugate-bounded scalar $t$ for all primes

Pick once and for all a finite adelic vector $y$ where $c_0 := \mathrm{intVacuum}\ D.\mathrm{twelveSpace}\ (\mathrm{finVec}\ y) \neq 0$ (`exists_intVacuum_ne_zero`), and define the single global nonzero scalar
$$t := M^{12f} \left(\prod_{\tau \in D.\Phi} B_\tau^2\right) c_0 \in \mathbb{C}^\times.$$
By **`New.Data.norm_map_mul_intVacuum_le`** (`BoundAttained.lean:327–371`), combining `norm_ringEquiv_intVacuum_le` ($\|\sigma c_0\| \le \exp(12 C q_1^2)$) with $M^{12f}$ and $B_\tau \le (2b + n)^n$ via the data log-bound (`D.le_of_hasLogBound`) bounds every automorphism conjugate uniformly:
$$\forall \sigma \in \operatorname{Aut}(\mathbb{C}), \qquad \|\sigma t\| \le \exp\!\left(\frac{f m}{k} b\right).$$

Finally, `D.exists_isTightFrame` supplies a single tight frame with multiplicities $c_i \ge 6fb \ge 3|l|$ that has a unit linear form at *every* prime $p$. Feeding `leastValLE_twelve` into `exists_le_norm_coeff_indicatorDatum` and rewriting the $p$-adic norms of $\prod B_\tau^2$ and $\prod l_\tau!$ via `norm_natCast_prod_eq_rpow_neg_sum` proves the headline sharpness theorem **`New.Data.exists_rpow_neg_localPrimeSumTotal_le_norm_coeff`** (`BoundAttained.lean:375–448`): for every degree $l$ with $\kappa_\tau \le l_\tau \le \kappa_\tau + x^{42}$, there exists a single nonzero $t \in \mathbb{C}$ with $\|\sigma t\| \le \exp(f m b / k)$ for all $\sigma \in \operatorname{Aut}(\mathbb{C})$ such that, simultaneously for **every** prime $p$, **every** embedding $\iota_p : \mathbb{C} \hookrightarrow \mathbb{C}_p$, and every modulus $\mathfrak{m}$ with $\mathfrak{m}.\mathrm{ideal} = (\pi_M)$, there exist $\gamma$ with $\sum_{b=0}^{11} \gamma(\tau, b, ()) = l_\tau$ and $y_0 \in M_{12 \times 1}(\mathcal{O}_E/\mathfrak{m})$ satisfying
$$p^{-D.\mathrm{localPrimeSumTotal}\ p} \le \left\|\iota_p\!\left(\frac{\mathrm{vacuumScale}^{12}}{t} \left(\prod_{\tau \in D.\Phi} l_\tau!\right) \mathrm{coeff}\ \gamma\bigl(D.\mathrm{twelveTaylor}(D.\mathrm{indicatorDatum}\ \mathfrak{m}\ y_0)\bigr)\right)\right\|.$$

## Level 3 — Formal Lean Contract

The extracted Stage 02 skeleton below reproduces the complete public API of `src/New/Derivatives/Sharpness/BoundAttained.lean` (lines `54–448`) at pinned upstream commit `41c2716658c42ba08eb0648994a2efe83082a95c` (`41c2716658c4`), backed by the 6-copy unit lemma (`exists_scaledThetaProd_mem_mul_eq_one`, `leastValLE_of_unit_coefficients`) in `LeastValuation.lean` (lines `489–556`) and the 12-copy unification (`leastValLE_twelve`) in `Cases.lean` (lines `505–670`). Across the proof DAG, this node imports `D.localPrimeSumTotal` from `03_upper_bound/short_columns` and exports `New.Data.exists_rpow_neg_localPrimeSumTotal_le_norm_coeff` directly to `06_one_lift/twisted_value_heights`.

**Verification status (`audit/verification-status.json`):** `build_status: unverified-locally`; `scan_scope: entry` covering only `Challenge.lean`, `src/Main.lean`, and `src/Statements.lean` (`0` forbidden tokens in solution files, `5` expected `sorry` tokens in `Challenge.lean`) at pinned commit `41c2716658c42ba08eb0648994a2efe83082a95c` (`41c2716658c4`). Stage 02 proof bodies have not been locally compiled or verified.

<!-- rh:lean-node:02_derivatives/bound_attained -->
