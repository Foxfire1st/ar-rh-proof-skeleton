---
slug: von-mangoldt-function
term: von Mangoldt function
aliases: []
inline_glimpse: "The arithmetic weight Λ(n) equal to log p when n is a prime power pᵏ (k ≥ 1) and 0 otherwise, converting multiplicative prime factorizations into additive logarithms."
visual_ref: assets/stage01-one-sided-landau-funnel.svg
where_analogy_breaks: "Treating $\\Lambda(n)$ as a pure prime detector overlooks higher prime powers $p^2, p^3, \\dots$: although those powers contribute only $O(\\sqrt{x})$ to the Chebyshev sum $\\psi(x) = \\sum_{n \\le x} \\Lambda(n)$, retaining them is essential for the exact identity $\\sum_{d \\mid n} \\Lambda(d) = \\log n$ and the logarithmic derivative $-\\zeta'(s)/\\zeta(s) = \\sum_{n=1}^{\\infty} \\Lambda(n) n^{-s}$."
sources:
  - title: "Multiplicative Number Theory"
    citation: "H. Davenport, 3rd ed., Springer Graduate Texts in Mathematics 74 (2000), Chapters 17–18."
    kind: book
  - title: "Multiplicative Number Theory I: Classical Theory"
    citation: "H. L. Montgomery and R. C. Vaughan, Cambridge University Press (2007), §2.1 and Chapter 11."
    kind: book
  - title: "Mathlib4: ArithmeticFunction.vonMangoldt"
    citation: "Mathlib.NumberTheory.ArithmeticFunction.VonMangoldt (`ArithmeticFunction.vonMangoldt`, `vonMangoldt_apply_pow`, `vonMangoldt_mul_zeta`)."
    kind: mathlib
lean_anchors:
  - 01_reduction/criterion.lean#New.LFunction_ne_zero_of_primeRemainder_le
  - 01_reduction/one_sided_bound.lean#New.eventually_primeRemainder_le_mul_rpow
child_concepts:
  - prime-number
  - infinite-series
  - riemann-zeta-function
  - dirichlet-l-function
  - quadratic-twist
  - landaus-lemma
---

# von Mangoldt function

## Level 1 — Abitur / First-Principles

Counting {{concept:prime-number|integers greater than $1$ divisible only by $1$ and themselves}} with a flat weight of $1$ makes multiplication awkward, because the count of primes does not match the structure of factorizations. The **von Mangoldt function** $\Lambda(n)$ fixes that by assigning a logarithmic weight to every prime power:
$$\Lambda(n) = \begin{cases} \log p & \text{if } n = p^k \text{ for some prime } p \text{ and integer } k \ge 1, \\ 0 & \text{otherwise.} \end{cases}$$
Thus $\Lambda(1) = 0$, $\Lambda(2) = \log 2$, $\Lambda(3) = \log 3$, $\Lambda(4) = \log 2$, $\Lambda(6) = 0$, and $\Lambda(9) = \log 3$.

This choice turns unique factorization into a clean addition law over divisors:
$$\sum_{d \mid n} \Lambda(d) = \log n.$$
For example, the divisors of $12 = 2^2 \cdot 3$ are $1, 2, 3, 4, 6, 12$, and summing $\Lambda(d)$ gives $0 + \log 2 + \log 3 + \log 2 + 0 + 0 = 2\log 2 + \log 3 = \log 12$.

In analytic number theory, weighting integers by $\Lambda(n)$ links prime distribution directly to complex analysis. Forming the {{concept:infinite-series|unending sum over positive integers}} $\sum_{n=1}^{\infty} \Lambda(n) n^{-s}$ produces the negative logarithmic derivative $-\zeta'(s)/\zeta(s)$ of {{concept:riemann-zeta-function|the function extending $\sum_{n=1}^{\infty} n^{-s}$ across the plane}}, turning zeros of $\zeta(s)$ into poles of a prime-power sum. Stage 01 of the proof builds a nonnegative residue-class weight $\lambda(n) = \Lambda(n)(1 - \chi_K(n))$ on $n \equiv 1 \pmod{q_0}$ by pairing $\Lambda(n)$ with {{concept:quadratic-twist|a $\pm 1$-valued prime-modulus sign factor $(n/d)$}}. Its smoothed partial-sum remainder captures the zeros of {{concept:dirichlet-l-function|character-weighted generalizations $L(\psi, s)$ of $\zeta(s)$}} and feeds them into {{concept:landaus-lemma|a sign-based pole-exclusion theorem}}.

### Limits of the intuition

Calling $\Lambda(n)$ a "prime detector" misses two structural features. First, $\Lambda(n)$ remains nonzero on higher prime powers $p^2, p^3, \dots$; discarding those terms would break the exact divisor identity $\sum_{d \mid n} \Lambda(d) = \log n$ and sever the exact logarithmic-derivative link to {{term:riemann-zeta-function|the Riemann zeta function}} and {{term:dirichlet-l-function|Dirichlet L-functions}}. Second, $\Lambda(n)$ does not treat all {{term:prime-number|primes}} equally: its jumps grow like $\log p$ and occur on sparse integers, so raw partial sums oscillate with jump size $\log x$. Stage 01 therefore smooths the sum using the kernel $(1/n - 1/x)$ inside the remainder `primeRemainder`.

## Level 2 — Undergrad Bridge

In the ring of arithmetic functions under Dirichlet convolution $(f * g)(n) = \sum_{d \mid n} f(d) g(n/d)$, the von Mangoldt function is uniquely determined by
$$\Lambda * \mathbf{1} = \log, \qquad \Lambda = \mu * \log,$$
where $\mathbf{1}(n) = 1$ and $\mu$ is the Möbius function. Taking the logarithmic derivative of the Euler product of a Dirichlet $L$-function for $\operatorname{Re}(s) > 1$ yields
$$-\frac{L'(\chi, s)}{L(\chi, s)} = \sum_{n=1}^{\infty} \frac{\chi(n)\Lambda(n)}{n^s},$$
with the special case $\chi = 1$ giving $-\zeta'(s)/\zeta(s) = \sum_{n=1}^{\infty} \Lambda(n) n^{-s}$.

Stage 01 (`New.Reduction.Zeros.PrimeError`) adapts this machinery to a residue class $n \equiv 1 \pmod q$ twisted by the Legendre symbol $\chi_K(n) = (n/d)$ of a prime $d$:
$$\lambda(n) = \begin{cases} \Lambda(n)\bigl(1 - (n/d)\bigr) & \text{if } n \equiv 1 \pmod q, \\ 0 & \text{otherwise.} \end{cases}$$
Because $(n/d) \in \{-1, 0, 1\}$, we have $0 \le \lambda(n) \le 2\Lambda(n)$ for every $n$. Character orthogonality modulo $q$ expresses its Dirichlet series for $\operatorname{Re}(s) > 1$ as
$$\sum_{n=1}^{\infty} \frac{\lambda(n)}{n^s} = \frac{1}{\varphi(q)} \sum_{\psi \bmod q} \left(-\frac{L'(\psi, s)}{L(\psi, s)} + \frac{L'(\psi \cdot (\cdot/d), s)}{L(\psi \cdot (\cdot/d), s)}\right) = \frac{s}{\varphi(q)(s - 1)} + G_q(s),$$
where $G_q$ is holomorphic at $s = 1$. Integrating against the smoothing kernel $(1/n - 1/x)$ defines the Stage 01 error term
$$R_q(x) = \sum_{n \le x} \lambda(n)\left(\frac{1}{n} - \frac{1}{x}\right) - \frac{\log x}{\varphi(q)} - \operatorname{Re} G_q(1).$$
Under $x = e^v$, the Laplace transform $\int_0^{\infty} R_q(e^v) e^{(1 - s)v}\,dv$ has at most simple poles in $\operatorname{Re}(s) > 0$, converting zeros of $L(\psi, s)$ into poles of residue $(n(s) - n'(s)) / (\varphi(q)\,s(1 - s))$.

## Level 3 — Formal Lean Contract

In Mathlib4 (`Mathlib.NumberTheory.ArithmeticFunction.VonMangoldt`), `ArithmeticFunction.vonMangoldt : ArithmeticFunction ℝ` (notation `Λ`) formalizes $\Lambda(n)$, together with `vonMangoldt_nonneg`, `vonMangoldt_le_log`, `vonMangoldt_apply_prime`, `vonMangoldt_apply_pow`, and `vonMangoldt_mul_zeta : vonMangoldt * zeta = log`.

Upstream `src/New/Reduction/Zeros/PrimeError.lean` builds the Stage 01 weighted sum directly on `vonMangoldt`:
- `New.lambdaKResidueClass d q n : ℝ := residueClass (1 : ZMod q) n * (1 - chiK d n)`, where `residueClass` restricts `vonMangoldt` to $n \equiv 1 \pmod q$.
- `New.lambdaKResidueClass_nonneg` and `New.lambdaKResidueClass_le_two_mul_vonMangoldt : lambdaKResidueClass d q n ≤ 2 * vonMangoldt n`.
- `New.primeRemainder d q x := lambdaKSum d q 0 x - lambdaKSum d q 1 x - x.log / φ q - re (logDerivMean d q 1)`.
- `New.sum_vonMangoldt_div_le : ∑ n ∈ Ioc 0 N, vonMangoldt n / n ≤ 2 * Real.log N`.

Stage 01 anchors this weighted remainder in two main theorems:
- `01_reduction/criterion.lean#New.LFunction_ne_zero_of_primeRemainder_le` (upstream `src/New/Reduction/Zeros/Criterion.lean:35-90`):
  ```lean
  theorem New.LFunction_ne_zero_of_primeRemainder_le {q₀ : ℕ} [NeZero q₀] (hq₀ : 8 ∣ q₀) {d₀ : ℕ}
      (hd₀_mod : d₀ % 8 = 7) (hd₀_cop : d₀.Coprime q₀) {β : ℝ} (hβ : 1 / 2 < β)
      (h_bound : ∀ d : Primes, (d : ℕ) ≡ d₀ [MOD q₀] →
        ∃ A : ℝ, ∀ᶠ x : ℝ in atTop, primeRemainder d q₀ x ≤ A * x ^ (β - 1))
      (ψ : DirichletCharacter ℂ q₀) {ρ : ℂ} (hρ_ne : ρ ≠ 1) (hρ_re : β < ρ.re) :
      ψ.LFunction ρ ≠ 0
  ```
- `01_reduction/one_sided_bound.lean#New.eventually_primeRemainder_le_mul_rpow` (upstream `src/New/Reduction/OneSidedBound.lean:201-235`):
  ```lean
  theorem New.eventually_primeRemainder_le_mul_rpow {q₀ : ℕ} [NeZero q₀] (hq₀ : 18480 ∣ q₀) {d : ℕ}
      [hd_prime : Fact d.Prime] (hd : d ≡ q₀ / 2 - 1 [MOD q₀]) {ε : ℝ} (hε : 0 < ε) :
      ∀ᶠ y : ℝ in atTop, primeRemainder d q₀ y ≤ (128 * q₀.totient + 32) * y ^ (-1 / 2 + ε)
  ```

See the page's live verification banner (`audit/verification-status.json`) for the currently pinned upstream snapshot and local Lean build status (`build_status: unverified-locally`).
