---
slug: formal-group-law
term: formal group law
aliases:
  - Frobenius congruence
  - formal parameter
inline_glimpse: "A power-series addition law encoding an elliptic curve's group structure near the origin, whose Lubin–Tate scaling and Frobenius congruence isolate the exact p-adic powers controlling theta-function derivatives."
visual_ref: assets/stage02-frobenius-normal-form-pipeline.svg
where_analogy_breaks: "Thinking of a formal group law as a small-angle Taylor approximation like $u + v + O(u^2v)$ captures why multiplication by a prime $p$ acts cleanly on a local coordinate near the origin, but misses its rigid arithmetic structure: on a supersingular CM curve at a non-split prime $p$, the formal group has height $2$, so multiplication by $\\pi_p$ satisfies the Lubin–Tate relation $T(\\pi_p z) = \\pi_p T + b_1 T^{q_p} + \\cdots$ with $b_1$ a $p$-adic unit, forcing a unique rescaling radius $v(\\varpi) = \\rho_p$ where the logarithm of the theta function obeys a rigid Frobenius congruence modulo $\\pi_p$."
sources:
  - title: "The Arithmetic of Elliptic Curves"
    citation: "J. H. Silverman, 2nd ed., Springer Graduate Texts in Mathematics 106 (2009), Chapter IV."
    kind: book
  - title: "Formal complex multiplication in local fields"
    citation: "J. Lubin and J. Tate, Annals of Mathematics 81 (1965)."
    kind: paper
  - title: "Divisibilities, congruences, and Cartier duality"
    citation: "N. M. Katz, Journal of the Faculty of Science, University of Tokyo 28 (1981)."
    kind: paper
  - title: "Radius of convergence of the p-adic sigma function"
    citation: "K. Bannai, S. Kobayashi, and T. Yasuda (2017)."
    kind: paper
lean_anchors:
  - 02_derivatives/theta_products.lean#New.Parameters.norm_pow_sub_conj_partner_le
  - 02_derivatives/theta_products.lean#New.Parameters.exists_hasNormalForm
child_concepts:
  - prime-number
  - complex-multiplication
  - elliptic-theta-function
  - theta-normal-form
  - arithmetic-height
---

# Formal Group Law

## Level 1 — First-Principles

Near the origin of the ordinary number line, adding two small numbers $u$ and $v$ is just $u + v$. On a curved geometric group—like points near the identity on a circle or an elliptic curve—you can still write the group addition purely as a power series $F(u, v) = u + v + c_{1,1}uv + \cdots$ in a single local coordinate $T$ chosen to vanish at the identity. That algebraic power series is a **formal group law**, and the local coordinate $T$ is its **formal parameter**. Because the entire group law is encoded in the coefficients of $F(u, v)$ without ever evaluating a limit, you can inspect the curve's addition law modulo any {{concept:prime-number|indivisible whole number $p \in \{2, 3, 5, 7, \dots\}$}}, even when classical calculus over the real numbers says nothing about divisibility.

When you multiply a point by $p$ inside a group, repeated addition becomes a single power series $[p](T) = pT + \cdots$. Modulo $p$, the linear term $pT$ vanishes, so the first surviving term has a power $T^{p^h}$—a shadow of the freshman's-dream identity $(u + v)^p \equiv u^p + v^p \pmod{p}$. On an elliptic curve with {{concept:complex-multiplication|extra geometric symmetries given by an imaginary quadratic integer ring}}, a prime $p$ that does not split into two separate factors in that ring produces a rigid endomorphism $\pi_p$ whose power series has no intermediate terms: its leading terms are $\pi_p T$ and a unit multiple of $T^{q_p}$, where $q_p$ is the norm of the prime above $p$ ($p^2$ when $p$ is inert, $p$ when $p$ ramifies). Rescaling the analytic coordinate $z$ by a carefully calibrated factor $\varpi$ balances those two leading terms so that the rescaled formal parameter satisfies $Y - Y^{q_p} \equiv z \pmod{\pi_p}$, unlocking the expansion $Y \equiv \sum_{j \ge 0} z^{q_p^j} \pmod{\pi_p}$.

This same algebraic symmetry governs the curve's {{concept:elliptic-theta-function|quasi-periodic analytic building block whose zeros lie on the curve's lattice}}. Applying the endomorphism $\pi_p$ to the theta function yields a **Frobenius congruence**: the ratio $\theta(\pi_p z) / \theta(z)^{q_p}$ is congruent to $1$ modulo $\pi_p$ inside the ring of integral power series in the formal parameter. Taking logarithms turns that multiplicative ratio into a linear functional equation for $g = -\log\theta$, which forces the non-integral part of $\theta(\varpi z)$ to collapse into a single exponential whose exponent is built solely from Frobenius-power pairings. Combined with the Galois-side congruence relating $p^e$-th powers at one embedding to complex conjugates at another, this formal-group machinery produces the {{concept:theta-normal-form|canonical factorization of multivariable products into an integral factor times an exponential of Frobenius pairings}} that drives all of Stage 02's divisibility bounds via {{concept:arithmetic-height|the prime-by-prime accounting of ordinary size against $p$-adic divisibility}}.

### Limits of the intuition

Thinking of a formal group law as a local Taylor approximation to curved addition explains why the identity neighborhood can be studied purely with power-series coefficients and why multiplication by $p$ creates $p$-th-power congruences. The intuition falls short in two crucial ways. First, a formal group law is not just an asymptotic truncation: its coefficients live in the local ring of integers $\mathcal{O}$ of $\mathbb{C}_p$, and the unit leading coefficient $b_1$ of $T^{q_p}$ reflects an exact arithmetic invariant—the height of the formal group—rather than a generic Taylor coefficient. Second, in Stage 02 the **Frobenius congruence** works on two distinct levels simultaneously: analytically on one curve (linking $\theta(\pi_p z)$ to $\theta(z)^{q_p}$ in $\mathcal{O}[[T]]$), and arithmetically across the infinite places of the cyclotomic tower $E_s = K(\zeta_{q_1})$, where raising an algebraic integer to the $p^e$-th power at one place matches complex conjugation at a shifted place modulo $\pi_p$.

## Level 2 — Undergrad Bridge

Let $K = \mathbb{Q}(\sqrt{-d})$ with $d \equiv 7 \pmod 8$ prime, and let $C$ be a CM lattice in $\mathbb{C}$ stable under $\mathcal{O}_K$ equipped with a $2$-torsion point $t$ and normalized so that $\wp''(t) = 2$ and $a_1^2 = 12\wp(t)$. Fixing an embedding $\iota_p : \mathbb{C} \hookrightarrow \mathbb{C}_p$ with valuation ring $\mathcal{O} = \{x \in \mathbb{C}_p : \|x\|_p \le 1\}$, the translated Weierstrass coordinates
$$x(z) = \wp(z + t) - \wp(t), \qquad y(z) = \frac{\wp'(z + t) - a_1 x(z)}{2}$$
have Taylor expansions at $z = 0$ over $\mathbb{C}_p$. Because $y(z) = z + O(z^2)$ generates the local ring of the curve at the origin (Silverman 2009, Ch. IV), the ring $\mathcal{O}[[y]]$ is the ring of integral formal functions on the formal group $\widehat{E}$ of $C$. Averaging $y(z)$ over the $(q_p - 1)$-st roots of unity isolates its part $T(z)$ in degrees $\equiv 1 \pmod{q_p - 1}$, which still generates $\mathcal{O}[[y]] = \mathcal{O}[[T]]$ and serves as the canonical **formal parameter**.

Let $p$ be a prime that does not split in $K$ ($\chi_K(p) \neq 1$), so $p$ is odd (since $d \equiv 7 \pmod 8$ implies $2$ splits). Let $\pi_p = \sqrt{-d}$ if $p = d$ (with $q_p = p$, $v_p(\pi_p) = 1/2$) and $\pi_p = p$ if $p$ is inert (with $q_p = p^2$, $v_p(\pi_p) = 1$). Because $\pi_p \in \mathcal{O}_K$ acts as an endomorphism of $C$ of degree $q_p$, its action on the formal parameter $T$ is a Lubin–Tate endomorphism (Lubin–Tate 1965):
$$T(\pi_p z) = \pi_p T(z) + b_1 T(z)^{q_p} + \cdots, \qquad b_1 \in \mathcal{O}^\times,$$
where the fact that $b_1$ is a unit expresses the exact height bound of the supersingular formal group. Choosing $\varpi \in \mathbb{C}_p$ with $\varpi^{q_p - 1} = -\pi_p / b_1$ pins its $p$-adic valuation to the critical radius
$$\rho_p = \frac{v_p(\pi_p)}{q_p - 1} = \begin{cases} \dfrac{1}{p^2 - 1} & \text{if } p \text{ is inert}, \\[6pt] \dfrac{1}{2(p - 1)} & \text{if } p = d. \end{cases}$$
The rescaled formal parameter $Y(z) = T(\varpi z)/\varpi$ is integral over $\mathcal{O}$ and satisfies $Y - Y^{q_p} \equiv z \pmod{\pi_p}$, hence
$$Y(z) \equiv \sum_{j = 0}^{\infty} z^{q_p^j} \pmod{\pi_p}.$$
Consequently, every series $H \in \mathcal{O}[[y]]$ becomes integral in the analytic variable $z$ after rescaling to $H(\varpi z)$—this is the precise $p$-adic radius of convergence controlled by the formal group (compare Katz 1981; Bannai–Kobayashi–Yasuda 2017).

On the theta function $\theta(z)$ of $C$ (the translated sigma function normalized to $\theta(0) = 1$), the isogeny $\pi_p$ produces an algebraic identity $\theta(\pi_p z) = \theta(z)^{q_p} R(x(z))$ with $\deg R < q_p/2$, from which Stage 02 proves the **Frobenius congruence**
$$\frac{\theta(\pi_p z)}{\theta(z)^{q_p}} \equiv 1 \pmod{\pi_p \mathcal{O}[[y]]}.$$
For $g = -\log \theta$, this reads $q_p g(z) - g(\pi_p z) \in \pi_p \mathcal{O}[[y]]$. Averaging $g$ over $(q_p - 1)$-st roots of unity splits off its congruence part $g^\flat$ in degrees $\equiv p + 1 \pmod{q_p - 1}$: the discrepancy $\exp(g^\flat - g) = \theta \exp(g^\flat)$ lies in $\mathcal{O}[[y]]$ (hence is integral at $\varpi z$), while the rescaled exponent $\Xi_1(z) = -g^\flat(\varpi z)/c_\varpi$ with $c_\varpi = \varpi^{p + 1}/p$ is integral and satisfies
$$\Xi_1(z) \equiv \begin{cases} \lambda\, Y(z)^{p + 1} \pmod{\pi_p} & \text{if } p \text{ is inert } (\lambda \in \mathcal{O}^\times), \\ \lambda\bigl(Y(z)^2 - \tfrac{1}{2}z^2\bigr) \pmod{\pi_p} & \text{if } p = d. \end{cases}$$
Expanding $Y(z) \equiv \sum_j z^{q_p^j}$ turns $\Xi_1$ modulo $\pi_p$ into a sum of terms $(z^{p^e} \cdot z)^{p^j}$ with $e$ running over the partner-exponent set $I_p^\circ$ (odd $e$ for inert $p$, all $e \ge 0$ for $p = d$).

Finally, to pass from one variable to a multivariable theta product over the cyclotomic tower $E_s = K(\zeta_{q_1})$, Stage 02 pairs this formal-group congruence with an arithmetic Frobenius congruence on $\mathcal{O}_{E_s}$. The infinite places $\tau \in \Phi_s$ form a torsor for $\operatorname{Gal}(E_s/K) \cong (\mathbb{Z}/q_1\mathbb{Z})^\times$, and for each $e \in I_p^\circ$ the shifted place $\tau' = (-p^e)\tau$ satisfies
$$\iota_p(\tau(\alpha))^{p^e} \equiv \iota_p\bigl(\overline{\tau'(\alpha)}\bigr) \pmod{\pi_p} \qquad (\alpha \in \mathcal{O}_{E_s}).$$
Indeed, on $\zeta_{q_1}$ raising to $p^e$ agrees with conjugating after multiplying the place by $-p^e$, while on $\sqrt{-d}$ the factor $\chi_K(p)^e \cdot (-1)$ equals $+1$ because $e$ is odd when $\chi_K(p) = -1$ (and $\sqrt{-d} = \pi_p$ vanishes modulo $\pi_p$ when $p = d$). Together with the critical valuation identity $v_p(c_\varpi \pi_p) = 1/(p - 1)$—the exact threshold where $\exp(c_\varpi \pi_p G)$ stays integral—these two congruences collapse any resolution-of-the-identity theta product into its canonical normal form.

## Level 3 — Formal Lean Contract

In `skeleton/02_derivatives/theta_products.lean` (extracted from upstream `src/New/Derivatives/FormalGroup/ThetaProducts.lean`, building on the single-curve formal-parameter and Frobenius-congruence modules `src/Lib/CM/FormalGroup/FormalParameter.lean`, `src/New/Derivatives/FormalGroup/FrobeniusCongruence.lean`, and `src/New/Derivatives/FormalGroup/Exponent.lean`), the arithmetic Frobenius congruence across places and the resulting existence of the normal form have the following signatures:

- `02_derivatives/theta_products.lean#New.Parameters.norm_pow_sub_conj_partner_le` (upstream `src/New/Derivatives/FormalGroup/ThetaProducts.lean:136-178`):
  ```lean
  lemma New.Parameters.norm_pow_sub_conj_partner_le {P : Parameters} {s p : ℕ} [Fact p.Prime] [PadicPlace p] {e : ℕ}
      (he : e ∈ partnerExponents P.d p) (τ : P.Φ s) (α : 𝓞 (P.E s)) :
      ‖ιₚ (Φ τ α) ^ p ^ e - ιₚ (conj (Φ (P.partner s p e τ) α))‖ ≤ ‖ιₚ (piPrime P.d p)‖
  ```
- `02_derivatives/theta_products.lean#New.Parameters.exists_hasNormalForm` (upstream `src/New/Derivatives/FormalGroup/ThetaProducts.lean:439-477`):
  ```lean
  lemma New.Parameters.exists_hasNormalForm (P : Parameters) (s : ℕ) [PadicPlace p] (hp : chiK P.d p ≠ 1) :
      ∃ (ϖ : ℂ_[p]) (l : ℕ → ℕ → valRingCp p),
      ‖ϖ‖ = p ^ (-varpiValuation P.d p) ∧
      (chiK P.d p = -1 → ∀ e : ℕ, Odd e → IsUnit (l e 0)) ∧
      ∀ {n J : Type} [Fintype n] [DecidableEq n] [Fintype J] (u : J → n → P.E s) (m : J → ℕ),
        IsResolution u m → (∀ (j : J) (i : n), IsIntegralAbove p (u j i)) →
          ∀ H ∈ P.cmCurve.multiSeriesRing (daggerForms u),
            HasNormalForm (partnerExponents P.d p) (P.partner s p) ϖ l
              (P.cmCurve.thetaProd (daggerForms u) m * H)
  ```

Internally, `exists_hasNormalForm` invokes the single-curve formal-group theorem `New.CMLattice.exists_exponent_tsum` (`src/New/Derivatives/FormalGroup/Exponent.lean`), which factors `θ(ϖ z) = W * exp(c_ϖ Ξ)` using the formal parameter `T = C.formalParam` and the Frobenius congruence `q_p g(z) - g(π_p z) ∈ π_p 𝒪[[y]]`, and then feeds `norm_pow_sub_conj_partner_le` through `piPrimeInt_dvd_sum_linearSeries_sub_coupling` and `norm_pow_succ_div_mul_piPrimeInt` (`‖ϖ ^ (p + 1) / p * π_p‖ = p ^ (-1 / (p - 1))`) to absorb all higher-order discrepancy terms modulo `π_p` into the integral prefactor `W`. See the page's live verification banner (`audit/verification-status.json`) for the currently pinned upstream snapshot and local Lean build status (`build_status: unverified-locally`).
