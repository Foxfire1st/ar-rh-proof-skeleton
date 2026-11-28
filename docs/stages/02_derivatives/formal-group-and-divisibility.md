# Formal Groups, Theta Normal Form & Prime-Counting Divisibility

This page unpacks the two core divisibility landmarks of Stage 02 — `src/New/Derivatives/FormalGroup/ThetaProducts.lean` (`02_derivatives/theta_products`) and `src/New/Derivatives/Divisibility/Count.lean` (`02_derivatives/count`) — together with their immediate upstream formal-group and direction-elimination helpers (`FrobeniusCongruence.lean`, `Exponent.lean`, `NullFrames.lean`) and downstream Stage 03 bridge (`Vacuum/Divisibility.lean`). Return to the [Stage 02 Overview](./overview.md), or continue to [Tight Frames, Resolution Products & the Integral Vacuum](./tight-frames-and-integral-vacuum.md) and [Sharpness: Attaining the Divisibility Bound](./sharpness-bound-attained.md).

## Level 1 — First-Principles

Take a smooth function $F(z)$ whose Taylor series has integer coefficients. Differentiating $F$ $B$ times at $z = 0$ pulls down a factorial $B!$, so the $B$-th derivative is automatically divisible by roughly $p^{B/(p-1)}$ for every {{concept:prime-number|whole number greater than $1$ with no smaller positive divisors}} $p$. That baseline comes purely from calculus — it treats every prime identically.

Now replace a generic series by the {{concept:elliptic-theta-function|quasi-periodic entire function attached to an elliptic curve}} with {{concept:complex-multiplication|extra ring symmetries from the integers of an imaginary quadratic field}} $K = \mathbb{Q}(\sqrt{-d})$, where $d \equiv 7 \pmod 8$ is prime. Measuring divisibility by powers of $p$ — its {{concept:arithmetic-height|exact prime-power divisibility exponent}} (the {{term:arithmetic-height|p-adic valuation}}) — reveals a striking split governed by the {{concept:quadratic-twist|sign-flipping quadratic character $\chi_K(p)$ recording how $p$ factors in $K$}} (the {{term:quadratic-twist|Legendre symbol}}). Near $z = 0$, the curve's addition law defines a {{concept:formal-group-law|power-series group law with a local coordinate $T$}} that satisfies a **{{term:formal-group-law|Frobenius congruence}}**:

- When $p$ **splits** ($\chi_K(p) = 1$), the {{term:formal-group-law}} has height $1$, so rescaling by a number $\varpi$ of valuation $\rho_p = \frac{1}{p-1}$ wipes out the entire factorial gain: the net slope is $\gamma_p = 0$.
- When $p$ is **inert** ($\chi_K(p) = -1$), the curve is supersingular modulo $p$ (height $2$), so the {{term:formal-group-law|Frobenius congruence}} jumps by $q_p = p^2$ instead of $p$:
  $$\frac{\theta(p z)}{\theta(z)^{p^2}} \equiv 1 \pmod{p}.$$
  Because $p^2 - 1$ is vastly larger than $p - 1$, rescaling the variable by $\varpi$ of valuation only $\rho_p = \frac{1}{p^2 - 1}$ already pulls almost all of $\theta(\varpi z)$ into an integral series, leaving a single controlled exponential tail. Every derivative therefore keeps an extra divisibility slope
  $$\gamma_p = \frac{1}{p-1} - \frac{1}{p^2-1} = \frac{p}{p^2 - 1} = \frac{1}{p} + \frac{1}{p^3} + \frac{1}{p^5} + \cdots.$$
- When $p = d$ is **ramified** ($\chi_K(d) = 0$), the uniformizer is $\pi_d = \sqrt{-d}$ of valuation $1/2$ and norm $q_d = d$, costing $\rho_d = \frac{1}{2(d-1)}$ and leaving extra slope
  $$\gamma_d = \frac{1}{d-1} - \frac{1}{2(d-1)} = \frac{1}{2(d-1)} = \frac{1}{2}\left(\frac{1}{d} + \frac{1}{d^2} + \frac{1}{d^3} + \cdots\right).$$

In every case, $2 \gamma_p \log p$ equals $\sum_{e \ge 1} \Lambda(p^e)(1 - \chi_K(p^e)) p^{-e}$, matching the exact {{concept:von-mangoldt-function|prime-power logarithmic weight $\Lambda(n)$}} twisted by $(1 - \chi_K(n))$.

![Pipeline from the one-curve congruence modulo p through Galois-shifted places and spanning vector families to the factored normal form and derivative divisibility count](assets/stage02-frobenius-normal-form-pipeline.svg)

Passing from one variable to the multi-variable theta functions over the cyclotomic CM field $E = K(\zeta_{q_1})$ takes four clean algebraic moves:

1. **Collapse along a spanning frame:** Suppose a family of vectors $u_j$ with multiplicities $m_j$ forms a {{concept:tight-frame|weighted spanning family reconstructing every vector from its projections $u_j^\dagger z$}} (a {{term:tight-frame|resolution of the identity}}). Taking the product $\prod_j \theta(u_j^\dagger z)^{m_j}$ multiplies the one-variable exponential tails together into a single exponent $\sum_j m_j \Xi(u_j^\dagger z)$.
2. **Pair each place with its Galois partner:** Modulo $\pi_p$, raising a coordinate at an infinite place $\tau$ to the $p^e$-th power acts like complex conjugation at the shifted place $\tau' = -p^e \tau$. That congruence collapses the sum over all frame vectors $u_j$ into a universal {{concept:theta-normal-form|canonical factorization expressing $F(\varpi z)$ as an integral series times the exponential of cross-place couplings $\Gamma_e$ pairing each place $\tau$ with its shifted partner $\tau'$}}. Crucially, the resulting {{term:theta-normal-form}} forgets every detail of the specific frame $u_j$.
3. **Kill the couplings by changing directions:** The exponential factor $\exp(c_\varpi \sum \lambda_{e,j} \Gamma_e^{p^j})$ is the *only* obstruction preventing $F(\varpi z)$ from having integral coefficients. Because $c_\varpi \pi_p$ has valuation $\frac{1}{p-1}$, making every active {{term:theta-normal-form|Frobenius coupling}} $\Gamma_e$ divisible by $\pi_p$ forces the entire exponential — and hence $F(\varpi z)$ — to become integral up to degree $B$. Stage 02 achieves this by applying a linear change of variables that steers the column directions into null spaces modulo $\pi_p$, paying a controlled determinant "deduction" for each imposed congruence.
4. **Read off the derivative bound and lift to Schur matrices:** Once $G(s) = F(\varpi z(s))$ is integral in degree $B$, Legendre's factorial bound gives $v_p(\text{jet of } G) \ge B/(p-1) - O(f N^3 \log_p B)$. Dividing by the chain-rule scaling factor $\varpi^B$ and the change-of-direction determinants leaves $\gamma_p B$ minus the deduction sum — the exact {{concept:schur-jet|derivative divisibility lower bound on matrix minors and Schur-matrix polynomials}} (the {{term:schur-jet|predicted divisibility}} of every {{term:schur-jet|minor product}} and {{term:schur-jet}} entry).

![How Galois-shifted place pairings drive direction substitution, determinant deductions, marked-place charges, and the passage from minor determinants to matrix derivatives](assets/stage02-partner-couplings-and-divisibility-count.svg)

### Where this analogy breaks

Saying that "killing the couplings turns $F(\varpi z)$ into an integral series" hides two subtle features that make the formal proof work. First, you do *not* need to kill infinitely many {{term:theta-normal-form|Frobenius couplings}} $\Gamma_e$: because $\Gamma_e$ is homogeneous of degree $p^e + 1$, only exponents $e \le \lfloor \log_p B \rfloor$ can ever touch Taylor coefficients of degree $B$, so the direction elimination solves only finitely many congruences. Second, in Stage 03 some places $\tau$ carry exceptional height-$1$ columns ("marked places") that cannot be eliminated cheaply by linear algebra alone; instead, Stage 02 exempts those pairs from the congruence conditions and pays for them explicitly with row-level prime-power "charges" $p^{w(\tau,h,b)}$ that add cleanly to the exponent bound.

## Level 2 — Undergrad Bridge

### 1. Prime splitting and the supersingular formal group (`FrobeniusCongruence.lean`, `Exponent.lean`)

Fix $P : \mathrm{Parameters}$, so $d = P.d$ is a prime with $d \equiv 7 \pmod 8$ and $q_0$ is a multiple of $18480 = 16 \cdot 3 \cdot 5 \cdot 7 \cdot 11$ with $d \equiv q_0/2 - 1 \pmod{q_0}$. Since $d \equiv 7 \pmod 8$, the prime $2$ splits in $K = \mathbb{Q}(\sqrt{-d})$ ($\chi_K(2) = 1$), so every non-split prime $p$ is odd; moreover, in the proof's admissible data all primes dividing $q_0$ split in $K$, so every non-split prime satisfies $p \ge 13$.

Let $C = P.\mathrm{cmCurve}$ be the CM lattice $\Omega_0 \mathcal{O}_K$ scaled so that $\wp''(t) = 2$ at its chosen $2$-torsion point $t$ with $a_1^2 = 12\wp(t)$, and write $\theta, x, y$ for the Taylor series at $0$ (embedded into $\mathbb{C}_p$ via $\iota_p : \mathbb{C} \to \mathbb{C}_p$) of $C.\theta$, $C.\mathrm{translatedX}(z) = \wp(z + t) - \wp(t)$, and $C.\mathrm{translatedY}(z) = (\wp'(z + t) - a_1 x(z))/2$, with $g = -\log \theta$. The ring $\mathcal{O}[[y]] = \mathcal{O}[[T]]$ is `C.integralSeriesRing` over the valuation ring $\mathcal{O} = \mathrm{valRingCp}\ p$.

For any prime $p$ with $\chi_K(p) \neq 1$, define:

| Prime Type | $\chi_K(p)$ | $\pi_p$ | $v(\pi_p)$ | $q_p$ | $\rho_p = \frac{v(\pi_p)}{q_p - 1}$ | $\gamma_p = \frac{1}{p-1} - \rho_p$ | $I_p^\circ$ (`partnerExponents`) | $I_p$ (`positivePartnerExponents`) |
| --- | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| Split | $+1$ | $p$ | $1$ | $p$ | $\frac{1}{p-1}$ | $0$ | $\emptyset$ | $\emptyset$ |
| Inert (height $2$) | $-1$ | $p$ | $1$ | $p^2$ | $\frac{1}{p^2 - 1}$ | $\frac{p}{p^2 - 1} = \sum_{e \ge 1,\text{ odd}} p^{-e}$ | $\{1, 3, 5, \dots\}$ | $\{1, 3, 5, \dots\}$ |
| Ramified ($p = d$) | $0$ | $\sqrt{-d}$ | $\frac{1}{2}$ | $d$ | $\frac{1}{2(d-1)}$ | $\frac{1}{2(d-1)} = \frac{1}{2}\sum_{e \ge 1} d^{-e}$ | $\mathbb{N}$ (includes $0$) | $\{1, 2, 3, \dots\}$ |

The one-curve formal-group pipeline then runs in two modules:
1. **`FrobeniusCongruence.lean`:** Proves the Frobenius congruence $\theta(\pi_p z)/\theta(z)^{q_p} \equiv 1 \pmod{\pi_p}$ in $\mathcal{O}[[y]]$ (`New.CMLattice.exists_rescale_theta_eq_pow_mul_one_add_smul`), hence $q_p g(z) - g(\pi_p z) \in \pi_p \mathcal{O}[[y]]$ (`New.CMLattice.exists_negLogTheta_frobenius_congruence`). Independently, a recursion proves that at an inert prime $p$ the coefficient of $z^{p-1}$ in $x(z)$ is a $p$-adic unit (`New.CMLattice.norm_coeff_taylorVia_translatedX_eq_one`).
2. **`Exponent.lean`:** Averaging $\zeta^{-p-1} g(\zeta z)$ over the $(q_p - 1)$-th roots of unity isolates the part $g^\flat$ supported in degrees $\equiv p + 1 \pmod{q_p - 1}$. Solving the resulting functional equation degree by degree for $\Xi_1 = -g^\flat(\varpi z)/c_\varpi$ (where $v(\varpi) = \rho_p$ and $c_\varpi = \varpi^{p+1}/p$) yields `New.CMLattice.exists_exponent_tsum`:
   $$\theta(\varpi z) = W(z) \exp\bigl(c_\varpi \Xi_1(z)\bigr), \qquad \Xi_1(z) \equiv \sum_{e \in I_p^\circ} \sum_{j \ge 0} \lambda_{e,j}\bigl(z^{p^e} z\bigr)^{p^j} \pmod{\pi_p},$$
   with $W, \Xi_1 \in \mathcal{O}[[z]]$, and when $p$ is inert, $\lambda_{e,0} \in \mathcal{O}^\times$ for all odd $e$ (a unit property later essential for sharpness in `BoundAttained.lean`). Note the exact balancing identity (`New.Parameters.norm_pow_succ_div_mul_piPrimeInt`):
   $$v(c_\varpi \pi_p) = (p + 1)\rho_p - 1 + v(\pi_p) = \frac{1}{p - 1},$$
   which sits right at the threshold where $\exp(c_\varpi \pi_p \cdot \text{integral series})$ has integral coefficients.

### 2. Partner places, Frobenius couplings, and the theta normal form (`ThetaProducts.lean`)

Let $E = P.E\ s = K(\zeta_{q_1})$ with infinite places $\Phi_s = P.\Phi\ s \cong \operatorname{Gal}(E/K) \cong (\mathbb{Z}/q_1\mathbb{Z})^\times$. For $e \in \mathbb{N}$ and $\tau \in \Phi_s$, define the **partner place** (`New.Parameters.partner`):
$$\tau' = P.\mathrm{partner}\ s\ p\ e\ \tau = (-p^e) \cdot \tau \in \Phi_s.$$
Comparing Frobenius on the generators of $\mathcal{O}_E$ proves `New.Parameters.norm_pow_sub_conj_partner_le` (and its conjugate form `norm_conj_pow_sub_partner_le` for elements integral above $p$): for every $e \in I_p^\circ$ (odd $e$ when $p$ is inert; all $e \ge 0$ when $p = d$) and $\alpha \in \mathcal{O}_E$,
$$\tau(\alpha)^{p^e} \equiv \overline{\tau'(\alpha)} \pmod{\pi_p}, \qquad \overline{\tau(y)}^{p^e} \equiv \tau'(y) \pmod{\pi_p}.$$

Now let vectors $u_j \in E^n$ (integral above $p$) with multiplicities $m_j \in \mathbb{N}$ form a **resolution of the identity** (`IsResolution u m`), meaning $z = \sum_j m_j (u_j^\dagger z) u_j$, where $(u_j^\dagger z) = \sum_{(\tau, c)} \overline{\tau(u_{j,c})} z_{(\tau, c)}$ has integral coefficients in $\mathbb{C}_p$ (`norm_daggerForms_le_one`). By `dvd_pow_sub_sum_partner`, $(u_j^\dagger z)^{p^e} \equiv \sum_{(\tau, c)} \tau'(u_{j,c}) z_{(\tau, c)}^{p^e} \pmod{\pi_p}$. Multiplying by $m_j (u_j^\dagger z)$ and summing over $j$ applies the resolution identity $\sum_j m_j \tau'(u_{j,c}) (u_j^\dagger z) = z_{(\tau', c)}$ to give `New.Parameters.piPrimeInt_dvd_sum_linearSeries_sub_coupling`:
$$\sum_{j \in J} m_j (u_j^\dagger z)^{p^e}(u_j^\dagger z) \equiv \Gamma_e := \sum_{(\tau, c) \in \Phi_s \times n} z_{(\tau, c)}^{p^e} z_{(\tau', c)} \pmod{\pi_p} \qquad (e \in I_p^\circ).$$
Because $\theta(\varpi z) = W \exp(c_\varpi \Xi_1)$, multiplying over all $j$ (`rescale_thetaProd_eq_mul_expSubst`) and lifting the congruence term-by-term through $p^j$-th powers (`dvd_sum_subst_sub_couplingSum`) while checking that any integral series $H \in C.\mathrm{multiSeriesRing}(\mathrm{daggerForms}\ u)$ becomes integral at $\varpi z$ (`rescale_mem_range_of_mem_multiSeriesRing`) produces the headline theorem of `ThetaProducts.lean`, **`New.Parameters.exists_hasNormalForm`**:
$$\Bigl(\prod_{j \in J} \theta(u_j^\dagger z)^{m_j} \cdot H\Bigr)(\varpi z) = W_{\mathrm{tot}}(z)\,\exp\!\Bigl(c_\varpi \sum_{e \in I_p^\circ}\sum_{j \ge 0} \lambda_{e,j}\,\Gamma_e^{p^j}\Bigr), \qquad W_{\mathrm{tot}} \in \mathcal{O}[[z]].$$

### 3. Direction substitution, Legendre's bound, deductions, and charges (`NullFrames.lean`, `Count.lean`)

Now specialize the variable index set to $\Phi_s \times n' \times n$, representing a family of $N \times |n|$ matrices $z_\tau$ ($N = |n'|$). Let $\kappa : \Phi_s \to \mathbb{N} \to \mathbb{N}$ specify Young diagrams with $\kappa(\tau, h)$ columns of height $h$, let $i' : \mathrm{WeylIndex}\ n'\ \kappa$ fill each column with rows in $n'$, and let $y_\tau$ be integral matrices with rows indexed by $n$. The **minor product** (`New.minorProd i' y`) is the homogeneous polynomial of degree $B = \mathrm{boxes}(\kappa, N)$ given by
$$P_y(z) = \prod_{\tau \in \Phi_s} \prod_{h < N} \prod_{j < \kappa(\tau, h)} \det\bigl((z_\tau y_\tau)_{\text{rows of }(i'\,\tau,h,j),\; \text{first } h \text{ cols}}\bigr).$$

To bound $v_p\bigl(P_y(\partial)F(0)\bigr)$ for any series $F$ satisfying `HasNormalForm`, `Count.lean` runs a three-step machine:

1. **Direction substitution and null-frame elimination (`NullFrames.lean`, `Count.lean`):**
   Introduce auxiliary variables $s_{(\tau, h, k, b)}$ (`DirectionVar`, at most $f N^3$ variables by `card_directionVar_le`) and substitute
   $$z_{(\tau, b, a)}(s) = \sum_{h < N}\sum_{k < h} p^{w(\tau, h, b)} \beta_{(\tau, h, k, a)}\, s_{(\tau, h, k, b)}, \qquad \beta_{(\tau, h, k, a)} = \sum_{k' < h} (g_{\tau, h})_{k, k'} y_{(\tau, a, k')}$$
   via `directionSubst` and `newDirection g y`. Fix a finite set `marked` of marked places; a pair of heights $(h, h')$ at $(\tau, \tau')$ is **exempt** (`IsExemptPair`) if $h = 1$ and either $\tau \in \mathrm{marked}$ or ($h' = 1$ and $\tau' \in \mathrm{marked}$).
   - For the active exponents $S = \{e \in I_p^\circ \mid e \le \lfloor \log_p B \rfloor\}$, `NullFrames.lean::exists_newDirection_dvd` constructs integral matrices $g_{\tau, h}$ such that $\pi_p$ divides every non-exempt Frobenius pairing $\sum_a \beta_{\tau,h,k,a}^{p^e} \beta_{\tau',h',k',a}$, while the determinant product costs at most
     $$\prod_{\tau \in \Phi_s}\prod_{h < N} \|\det(g_{\tau, h})\|^{\kappa(\tau, h)} \ge \|\pi_p\|^D, \qquad D = \sum_{e \in S} \sum_{\tau \in \Phi_s} \mathrm{deductionTerm}_p(\kappa, N, \mathrm{marked}, \tau, \tau', e).$$
   - For the exempt pairs, the row-charge condition `IsChargeExponent` ensures $w(\tau, 1, b) > 0$ or $w(\tau', h', b) > 0$, so $p$ (hence $\pi_p$) divides the scaled pairing. By `dvd_subst_coupling_directionSubst_of_charges`, $\pi_p \mid \Gamma_e(z(s))$ for every $e \in S$.
   - Since $v(c_\varpi \pi_p) = \frac{1}{p-1}$ (`norm_pow_succ_div_mul_piPrimeInt`), `HasNormalForm.norm_coeff_subst_le_one` guarantees that $G(s) = F(\varpi z(s))$ has integral coefficients in degree $B$.

2. **Legendre's factorial bound and the chain rule (`norm_jet_le_of_isHomogeneous`, `norm_jet_columnDetProd_subst`):**
   Let $P_*(s) = \mathrm{columnDetProd}\ i'$ be the product of the $h \times h$ determinants $\det(s_{(\tau, h, k, r)})$, homogeneous of degree $B$ (`isHomogeneous_columnDetProd`). By Legendre's bound on multinomial factorials (`norm_prod_factorial_le`), `norm_jet_le_of_isHomogeneous` gives
   $$\|\mathrm{jet}(P_*, G)\| \le p^{\,f N^3 (\lfloor \log_p B \rfloor + 1) - B/(p - 1)}.$$
   On the other hand, the chain rule (`norm_jet_columnDetProd_subst`) computes
   $$\|\mathrm{jet}(P_*, G)\| = \|\varpi\|^B \cdot p^{-\mathrm{fillingCharge}(w, i')} \cdot \Bigl(\prod_{\tau, h} \|\det g_{\tau, h}\|^{\kappa(\tau, h)}\Bigr) \cdot \|\mathrm{jet}(P_y, F)\|.$$
   Dividing through by $\|\varpi\|^B = p^{-\rho_p B}$ turns $B/(p-1)$ into $\gamma_p B$, proving **`New.Parameters.norm_jet_minorProd_le_of_isChargeExponent`**.

3. **Relating deductions to `predictedDivisibility` (`mul_tsum_deductionAt`, `mul_sum_deductionTerm_le`):**
   Write $a_\tau = \mathrm{heightOneColumns}\ \kappa\ N\ \tau$, $a'_\tau = \mathrm{lowColumnCount}\ \kappa\ N\ \tau$, and $l_\tau = \mathrm{lowBoxCount}\ \kappa\ N\ \tau$, so $B = \sum_\tau (a_\tau + l_\tau)$ (`boxes_eq_sum`). Because $x - \min(x, y) = (x - y)_+$ and $\sum_{e \in I_p} v(\pi_p) p^{-e} = \gamma_p$, `New.mul_tsum_deductionAt` proves the exact identity
   $$v(\pi_p)\sum_{e \in I_p^\circ} \mathrm{deductionAt}(e) = \gamma_p B - \mathrm{predictedDivisibility}_p(\kappa, N),$$
   where
   $$\mathrm{predictedDivisibility}_p(\kappa, N) = v(\pi_p)\sum_{e \in I_p}\sum_{\tau \in \Phi_s}\Bigl(\bigl(a_\tau / p^e - a_{\tau'} - a'_{\tau'}\bigr)_+ + \bigl(l_\tau / p^e - l_{\tau'}\bigr)_+ - a'_\tau / p^e\Bigr) - \mathbf{1}_{p = d}\cdot \frac{1}{2}\sum_{\tau \in \Phi_s}\bigl(\min(a_\tau, a_{\tau'_0} + a'_{\tau'_0}) + a'_\tau + \min(l_\tau, l_{\tau'_0})\bigr).$$
   Marking `marked` has the exact same effect on deductions as erasing height-$1$ columns at `marked` (`deductionTerm_eraseHeightOne`), yielding `mul_sum_deductionTerm_le` and hence **`norm_jet_minorProd_le_of_charges`** (and, with `marked = ∅` and $w = 0$, **`norm_jet_minorProd_le_of_hasNormalForm`**).

### 4. From minor products to Schur-matrix jets and Stage 03 (`Count.lean`, `Vacuum/Divisibility.lean`)

When $\kappa$ fits the ranks $|n| \le |n'|$ (`FitsRanks`), `New.minorProd_eq_sum` expands the minor product $P_y(z)$ in the $i'$-th row of the Schur matrix `schur κ z`:
$$P_y(z) = \sum_{i : \mathrm{WeylIndex}\ n\ \kappa} \mathrm{eval}_y\bigl(\mathrm{nablaBasis}\ i \cdot \mathrm{detPow}\bigr) \cdot \mathrm{schur}(\kappa, z)_{i', i}.$$
Because the polynomials $\mathrm{nablaBasis}\ i \cdot \mathrm{detPow}$ remain linearly independent over the residue field of $\mathcal{O}$, `New.exists_norm_le_norm_sum_mul_eval` produces an integral point $y$ at which the linear combination has norm at least the target coefficient, proving **`New.exists_norm_schurJet_le_norm_jet_minorProd`**:
$$\exists y : \Phi \to n \to \mathbb{N} \to \mathcal{O}, \qquad \|\mathrm{schurJet}\ \kappa\ F\ i'\ i\| \le \|\mathrm{jet}(P_y, F)\|.$$

Combining this with `exists_hasNormalForm_jetSeries` and `norm_jet_minorProd_le_of_charges` in `src/New/Derivatives/Vacuum/Divisibility.lean` produces **`New.Parameters.norm_schurJet_jetSeries_le_of_charges`**, the exact entry point consumed by Stage 03 (`src/New/UpperBound/Divisibility/ChainValuation.lean` and `ValuationAtNonSplitPrimes.lean`) to bound valuations of iterated unitary theta lifts at all non-split primes.

## Level 3 — Formal Lean Contract

The extracted Stage 02 skeletons embedded below correspond to `src/New/Derivatives/FormalGroup/ThetaProducts.lean` and `src/New/Derivatives/Divisibility/Count.lean` at pinned upstream commit `41c2716658c42ba08eb0648994a2efe83082a95c` (`41c2716658c4`).

### Extractor Stub-Truncation Disclosure (`skeleton/02_derivatives/count.lean`)

> **Deterministic `rh-extract` stub truncation on three `let`-bound return types:** In `src/New/Derivatives/Divisibility/Count.lean` (lines `581–713`), the three headline theorems `New.Parameters.norm_jet_minorProd_le_of_isChargeExponent`, `New.Parameters.norm_jet_minorProd_le_of_charges`, and `New.Parameters.norm_jet_minorProd_le_of_hasNormalForm` introduce `let N : ℕ := card n'` inside their Lean return types. Because `rh-extract` splits proof bodies at the first depth-0 `:=` token after the signature colon, those three generated stubs in `skeleton/02_derivatives/count.lean` truncate at `let N : ℕ := by sorry` rather than displaying the full inequality.

For complete formal transparency, the **exact, verbatim upstream theorem statements** from `src/New/Derivatives/Divisibility/Count.lean` (lines `581–593`, `658–672`, and `703–710` at commit `41c2716658c42ba08eb0648994a2efe83082a95c`) are reproduced below:

```lean
-- src/New/Derivatives/Divisibility/Count.lean:581-593
theorem norm_jet_minorProd_le_of_isChargeExponent [DecidableEq n'] (hp : chiK P.d p ≠ 1)
    (hϖ : ‖ϖ‖ = p ^ (-varpiValuation P.d p))
    (hF : HasNormalForm (partnerExponents P.d p) (P.partner s p) ϖ l F)
    (marked : Finset (P.Φ s)) (w : P.Φ s → ℕ → n' → ℕ)
    (hw : ∀ e ∈ partnerExponents P.d p, IsChargeExponent (P.partner s p e) marked w)
    (i' : WeylIndex n' κ) (y : P.Φ s → n → ℕ → valRingCp p) :
    let N : ℕ := card n'
    let B : ℕ := boxes κ N
    ‖jet ℂ_[p] (minorProd i' fun (τ : P.Φ s) (a : n) (k : ℕ) ↦ (y τ a k : ℂ_[p])) F‖ ≤
      p ^ (fillingCharge w i' - (gammaPrime P.d p * B -
        piPrimeValuation P.d p * ∑ e ≤ p.log B with e ∈ partnerExponents P.d p,
          ∑ τ : P.Φ s, deductionTerm p κ N marked τ (P.partner s p e τ) e -
        card (P.Φ s) * N ^ 3 * (p.log B + 1)) : ℝ) := by
```

```lean
-- src/New/Derivatives/Divisibility/Count.lean:658-672
theorem norm_jet_minorProd_le_of_charges [DecidableEq n'] (hp : chiK P.d p ≠ 1)
    (hϖ : ‖ϖ‖ = p ^ (-varpiValuation P.d p))
    (hF : HasNormalForm (partnerExponents P.d p) (P.partner s p) ϖ l F)
    (marked : Finset (P.Φ s)) (w : P.Φ s → ℕ → n' → ℕ)
    (hw : ∀ e ∈ partnerExponents P.d p, ∀ (τ : P.Φ s) (h : ℕ) (b : n'),
      κ τ 1 ≠ 0 → κ (P.partner s p e τ) h ≠ 0 → τ ∈ marked ∨ h = 1 ∧ P.partner s p e τ ∈ marked →
        w τ 1 b ≠ 0 ∨ w (P.partner s p e τ) h b ≠ 0)
    (i' : WeylIndex n' κ) (y : P.Φ s → n → ℕ → valRingCp p) :
    let N : ℕ := card n'
    let B : ℕ := boxes κ N
    ‖jet ℂ_[p] (minorProd i' fun (τ : P.Φ s) (a : n) (k : ℕ) ↦ (y τ a k : ℂ_[p])) F‖ ≤
      p ^ (fillingCharge w i' -
        (predictedDivisibility P.d p (P.partner s p) (eraseHeightOne κ marked) N +
          gammaPrime P.d p * (B - boxes (eraseHeightOne κ marked) N) -
          card (P.Φ s) * N ^ 3 * (1 + Real.logb p (max B 1 : ℕ))) : ℝ) := by
```

```lean
-- src/New/Derivatives/Divisibility/Count.lean:703-710
theorem norm_jet_minorProd_le_of_hasNormalForm [DecidableEq n'] (hp : chiK P.d p ≠ 1)
    (hϖ : ‖ϖ‖ = p ^ (-varpiValuation P.d p))
    (hF : HasNormalForm (partnerExponents P.d p) (P.partner s p) ϖ l F)
    (i' : WeylIndex n' κ) (y : P.Φ s → n → ℕ → valRingCp p) :
    let N : ℕ := card n'
    ‖jet ℂ_[p] (minorProd i' fun (τ : P.Φ s) (a : n) (k : ℕ) ↦ (y τ a k : ℂ_[p])) F‖ ≤
      p ^ (-(predictedDivisibility P.d p (P.partner s p) κ N -
        card (P.Φ s) * N ^ 3 * (1 + Real.logb p (max (boxes κ N) 1 : ℕ))) : ℝ) := by
```

**Verification status (`audit/verification-status.json`):** Local Lean compilation and kernel replay remain **`build_status: unverified-locally`** at pinned commit `41c2716658c42ba08eb0648994a2efe83082a95c` (`41c2716658c4`). Static token scanning currently has **`scan_scope: entry`**, covering only `Challenge.lean`, `src/Main.lean`, and `src/Statements.lean` (`0` forbidden tokens in solution files, `5` expected `sorry` tokens in `Challenge.lean`). Neither `ThetaProducts.lean` nor `Count.lean` has been locally compiled, kernel-checked, or statically scanned here.

<!-- rh:lean-node:02_derivatives/theta_products -->

<!-- rh:lean-node:02_derivatives/count -->
