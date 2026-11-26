---
slug: landaus-lemma
term: Landau's lemma
aliases:
  - one-sided criterion
inline_glimpse: "A complex-analytic theorem stating that the Laplace or Dirichlet transform of a nonnegative function must have a singularity on the real axis at its abscissa of convergence, turning one-sided error bounds into zero-free half-planes."
visual_ref: assets/stage01-one-sided-landau-funnel.svg
where_analogy_breaks: "Landau's lemma cannot rule out off-axis poles on its own if a real singularity sits further right: a one-sided bound $E(v) \\le A e^{(\\beta - 1)v}$ guarantees a nonnegative density for $\\Psi(s) = K/(s - \\beta) - \\Phi(s)$, so excluding poles in $\\operatorname{Re}(s) > \\beta$ still requires verifying that every real residue at $x > \\beta$ satisfies $\\operatorname{Re}(r(x)) \\ge 0$."
sources:
  - title: "Über einen Satz von Tschebyschef"
    citation: "E. Landau, Mathematische Annalen 61 (1905), pp. 527–550."
    kind: paper
  - title: "Multiplicative Number Theory I: Classical Theory"
    citation: "H. L. Montgomery and R. C. Vaughan, Cambridge University Press (2007), Lemma 15.1 and §§15.1–15.2."
    kind: book
  - title: "On variants of Chebyshev's conjecture"
    citation: "M. Suzuki, arXiv:2411.07436 (2024)."
    kind: paper
lean_anchors:
  - 01_reduction/criterion.lean#New.LFunction_ne_zero_of_primeRemainder_le
child_concepts:
  - complex-plane
  - analytic-continuation
  - dirichlet-l-function
  - von-mangoldt-function
  - quadratic-twist
  - critical-line
---

# Landau's lemma

## Level 1 — Abitur / First-Principles

Suppose a signal $f(v) \ge 0$ stays nonnegative for all $v > 0$, and we pack it into the exponential integral
$$F(s) = \int_0^{\infty} f(v) e^{-sv}\,dv$$
defined on a right half of {{concept:complex-plane|the two-dimensional grid of real and imaginary coordinates}}. Because every contribution $f(v) \ge 0$ pushes in the same positive direction when $s$ is real, nothing can cancel along the real axis. **Landau's lemma** states the consequence: if $F(s)$ converges for $\operatorname{Re}(s) > a$ and diverges for $\operatorname{Re}(s) < a$, then $F(s)$ cannot stay smooth at the real boundary point $s = a$—it must hit a genuine singularity there.

Turn that statement around and it becomes an engine for proving zero-free regions. If $F(s)$ has an {{concept:analytic-continuation|unique complex-differentiable extension past its initial convergence boundary}} that stays free of singularities along the real segment $x > \beta$, then the integral must actually converge all the way down to $\operatorname{Re}(s) > \beta$. As a result, $F(s)$ cannot have any pole anywhere in the half-plane $\operatorname{Re}(s) > \beta$, even far away from the real axis.

Crucially, this principle extends to oscillating error signals $E(v)$ that obey only a *one-sided* upper bound $E(v) \le A e^{(\beta - 1)v}$ for large $v$. Multiplying by $e^v$ and subtracting from a large enough baseline $K e^{\beta v}$ makes the shifted density $K e^{\beta v} - E(v) e^v \ge 0$ nonnegative everywhere on $v > 0$. Applying **Landau's lemma** to the shifted transform $K/(s - \beta) - \Phi(s)$ yields the **one-sided criterion** of Stage 01: an upper bound on the prime-power remainder built from {{concept:von-mangoldt-function|the logarithmic prime-power weight $\Lambda(n)$}} and {{concept:quadratic-twist|a $\pm 1$-valued prime-modulus sign factor $(n/d)$}} rules out all zeros of {{concept:dirichlet-l-function|the character-weighted functions $L(\psi, s)$ modulo $q_0$}} in $\operatorname{Re}(s) > \beta$. Letting $\beta$ approach $1/2$ forces every zero inside the strip onto {{concept:critical-line|the central vertical line $\operatorname{Re}(s) = 1/2$}}.

### Limits of the intuition

**Landau's lemma** turns one-sided bounds into zero-free half-planes only when the real axis stays unobstructed. If the transform $\Phi(s)$ had a real pole at some $a > \beta$ with a negative residue, then $K/(s - \beta) - \Phi(s)$ would have a genuine positive-residue real singularity at $s = a$, stopping convergence right there and allowing off-axis poles with $\operatorname{Re}(\rho) \le a$. In Stage 01, two safeguards keep the real axis clear: on real $x > \beta$, the factor $x(1 - x)$ is positive on $(0, 1)$ while no $L(\psi, x)$ vanishes for $x \ge 1$, ensuring $\operatorname{Re}(r(x)) \ge 0$; and any negative-sign poles contributed by twisted functions are cleared from the target box by picking $d$ via a {{term:quadratic-twist|quadratic twist}} zero-density bound.

## Level 2 — Undergrad Bridge

Let $\mu$ be a nonnegative Borel measure on $[0, \infty)$ whose Laplace transform $F(s) = \int_0^{\infty} e^{-sv}\,d\mu(v)$ has finite abscissa of convergence $\sigma_c$. Landau's theorem (1905; Montgomery–Vaughan, Lemma 15.1) asserts that $F$ cannot be analytically continued to a neighborhood of $s = \sigma_c$. The proof rests on Taylor-series positivity: for any real $z > \sigma_c$ and real $\sigma \le z$, expanding $e^{-\sigma v} = e^{(z - \sigma)v} e^{-zv}$ produces nonnegative terms
$$\frac{((z - \sigma)v)^k}{k!} e^{-zv} \ge 0.$$
If $F$ were analytic at $\sigma_c$, the Taylor series around $z$ would converge at some real $\sigma < \sigma_c$. Applying Tonelli's theorem to swap sum and integral would then show $\int_0^{\infty} e^{-\sigma v}\,d\mu(v) < \infty$, contradicting the definition of $\sigma_c$.

In Stage 01, this theorem is packaged as a polar-transform criterion (`New.IsPolar.landau`). Suppose $E : \mathbb{R} \to \mathbb{R}$ is measurable with $|E(v)| \le C e^v$ for $v \ge 0$, its transform $\Phi(s) = \int_0^{\infty} E(v) e^{(1 - s)v}\,dv$ continues meromorphically to $\mathbb{C}$ with at most simple poles of residue $r(\rho)$ in $\operatorname{Re}(\rho) > 0$, and $E(v) \le A e^{(\beta - 1)v}$ for large $v$ with $\beta > 0$. Choosing $K$ so that $K e^{\beta v} - E(v) e^v \ge 0$ for all $v > 0$ produces a nonnegative measure $\mu$ whose Laplace transform agrees with $\Psi(s) = K/(s - \beta) - \Phi(s)$ for $\operatorname{Re}(s) > \beta + 2$. Let $a$ be the supremum of $\beta$ and the complement of the real convergence set $S$ of $\mu$. If $a > \beta$, then $\Psi(x) \ge 0$ for real $x > a$ forces the residue $-r(a)$ of $\Psi$ at $a$ to satisfy $\operatorname{Re}(-r(a)) \ge 0$. Combining this with the real-residue hypothesis $\operatorname{Re}(r(a)) \ge 0$ yields $r(a) = 0$, so $\Psi$ continues analytically across $a$, contradicting Landau's lemma. Hence $a = \beta$, $\Psi$ is holomorphic on $\operatorname{Re}(s) > \beta$, and $r(\rho) = 0$ for every $\rho$ with $\operatorname{Re}(\rho) > \beta$.

Setting $E(v) = R_{q_0}(e^v)$ and choosing a prime $d \equiv d_0 \pmod{q_0}$ whose quadratic twists have no zeros in $\{\operatorname{Re}(s) \ge \beta,\; |\operatorname{Im}(s)| \le |\operatorname{Im}(\rho)|\}$, the residue in that box reduces to
$$r(s) = \frac{n(s)}{\varphi(q_0)\,s(1 - s)}, \qquad n(s) = \sum_{\chi \bmod q_0} \operatorname{ord}_s\bigl(\operatorname{entireL}(\chi)\bigr).$$
For real $x > \beta$, either $x \le 1$ (so $x(1 - x) \ge 0$) or $x \ge 1$ (where $n(x) = 0$), giving $\operatorname{Re}(r(x)) \ge 0$. Applying `IsPolar.landau` forces $r(\rho) = 0$, hence $n(\rho) = 0$, and therefore $L(\psi, \rho) \neq 0$.

## Level 3 — Formal Lean Contract

Upstream `src/Lib/Analytic/EntireLFunction.lean` defines `New.IsPolar (E : ℝ → ℝ) (r : ℂ → ℂ) : Prop`, packaging measurability, $|E(v)| \le C e^v$, meromorphic continuation of $\int_0^{\infty} E(v) e^{(1 - s)v}\,dv$, and simple-pole residues $r(\rho)$ in $\operatorname{Re}(\rho) > 0$. Upstream `src/Lib/Analytic/Landau.lean` proves the analytic engine:
- `New.mem_integrableExpSet_of_summable_taylor`: derives real convergence of `complexMGF X μ` from summability of the Taylor series at $z \ge \sigma$ via `lintegral_tsum`.
- `New.exists_measure_laplace_eq`: turns `∀ᶠ v in atTop, E v ≤ A * Real.exp ((β - 1) * v)` into a nonnegative measure `μ` with density `(K * Real.exp (β * v) - E v * Real.exp v).toNNReal`.
- `New.IsPolar.landau`: proves `r ρ = 0` for `β < ρ.re` whenever `∀ x : ℝ, β < x → 0 ≤ (r x).re` and the one-sided exponential bound holds.

Stage 01 anchors the full criterion in:
- `01_reduction/criterion.lean#New.LFunction_ne_zero_of_primeRemainder_le` (upstream `src/New/Reduction/Zeros/Criterion.lean:35-90`):
  ```lean
  theorem New.LFunction_ne_zero_of_primeRemainder_le {q₀ : ℕ} [NeZero q₀] (hq₀ : 8 ∣ q₀) {d₀ : ℕ}
      (hd₀_mod : d₀ % 8 = 7) (hd₀_cop : d₀.Coprime q₀) {β : ℝ} (hβ : 1 / 2 < β)
      (h_bound : ∀ d : Primes, (d : ℕ) ≡ d₀ [MOD q₀] →
        ∃ A : ℝ, ∀ᶠ x : ℝ in atTop, primeRemainder d q₀ x ≤ A * x ^ (β - 1))
      (ψ : DirichletCharacter ℂ q₀) {ρ : ℂ} (hρ_ne : ρ ≠ 1) (hρ_re : β < ρ.re) :
      ψ.LFunction ρ ≠ 0
  ```

See the page's live verification banner (`audit/verification-status.json`) for the currently pinned upstream snapshot and local Lean build status (`build_status: unverified-locally`).
