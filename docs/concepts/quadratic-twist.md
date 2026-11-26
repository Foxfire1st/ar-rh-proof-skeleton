---
slug: quadratic-twist
term: quadratic twist
aliases:
  - Legendre symbol
inline_glimpse: "Multiplying a Dirichlet character χ modulo N by the Legendre symbol (·/d) of an odd prime d coprime to N, yielding a non-principal character modulo N·d whose L-function family admits zero-density control."
visual_ref: assets/stage01-one-sided-landau-funnel.svg
where_analogy_breaks: "A quadratic twist $\\chi \\cdot (\\cdot/d)$ does not remove zeros of $L(\\chi, s)$ directly: it introduces a second family of poles with opposite sign into the remainder's transform, so Stage 01 works only because a zero-density estimate selects a prime $d$ in the required congruence class whose twisted $L$-functions have no zeros in the target box."
sources:
  - title: "Multiplicative Number Theory"
    citation: "H. Davenport, 3rd ed., Springer Graduate Texts in Mathematics 74 (2000), Chapters 5 and 9."
    kind: book
  - title: "On mean values of Dirichlet polynomials with real characters"
    citation: "M. Jutila, Acta Arithmetica 27 (1975), pp. 191–198."
    kind: paper
  - title: "A mean value estimate for real character sums"
    citation: "D. R. Heath-Brown, Acta Arithmetica 72 (1995), pp. 235–275."
    kind: paper
  - title: "Mathlib4: legendreSym"
    citation: "Mathlib.NumberTheory.LegendreSymbol.Basic (`legendreSym`, `quadraticChar_isQuadratic`)."
    kind: mathlib
lean_anchors:
  - 01_reduction/criterion.lean#New.LFunction_ne_zero_of_primeRemainder_le
  - 01_reduction/one_sided_bound.lean#New.eventually_primeRemainder_le_mul_rpow
child_concepts:
  - prime-number
  - dirichlet-character
  - dirichlet-l-function
  - von-mangoldt-function
  - landaus-lemma
---

# Quadratic twist

## Level 1 — First-Principles

Fix an odd {{concept:prime-number|integer greater than $1$ divisible only by $1$ and itself}} $d$. The **Legendre symbol** $(n/d)$ answers a basic modular-arithmetic question about any integer $n$: is $n$ a perfect square modulo $d$?
$$\left(\frac{n}{d}\right) = \begin{cases} 0 & \text{if } d \text{ divides } n, \\ +1 & \text{if } d \nmid n \text{ and } n \equiv a^2 \pmod d \text{ for some integer } a, \\ -1 & \text{otherwise.} \end{cases}$$
For $d = 7$, the nonzero squares modulo $7$ are $1^2 \equiv 1$, $2^2 \equiv 4$, and $3^2 \equiv 2$, so $(1/7) = (2/7) = (4/7) = +1$ while $(3/7) = (5/7) = (6/7) = -1$.

Now take {{concept:dirichlet-character|a periodic multiplicative weight $\chi(n)$ modulo $N$}} and an odd {{term:prime-number|prime}} $d$ coprime to $N$. Multiplying the two weights gives a new periodic multiplicative weight $\chi_d(n) = \chi(n)(n/d)$ of period $N d$. This product is the **quadratic twist** of $\chi$ by $d$, and its associated {{concept:dirichlet-l-function|weighted sum and continuation $L(\chi_d, s)$}} is the twisted $L$-function.

Stage 01 relies on **quadratic twists** to keep its prime-power remainder nonnegative while avoiding accidental pole cancellations. On $n \equiv 1 \pmod{q_0}$, the proof weights each integer by $\lambda(n) = \Lambda(n)(1 - (n/d))$, combining {{concept:von-mangoldt-function|the logarithmic prime-power weight $\Lambda(n)$}} with $1 - (n/d) \in \{0, 1, 2\}$. Expanding $1 - (n/d)$ splits the transform of the remainder into a positive term from the untwisted {{term:dirichlet-l-function|Dirichlet L-functions}} modulo $q_0$ and a negative term from their **quadratic twists** by $d$. Because $d$ can vary freely over all primes in a chosen residue class modulo $q_0$, a zero-density estimate guarantees that at least one prime $d$ in that class has no twisted zeros anywhere inside a prescribed box to the right of $\operatorname{Re}(s) = 1/2$. Fixing such a prime $d$ clears out the negative poles in that box, leaving only untwisted poles that {{concept:landaus-lemma|the sign-based pole-exclusion theorem}} then forces to vanish.

### Limits of the intuition

Treating the **quadratic twist** as a single static filter misses two essential requirements. First, for a poorly chosen prime $d$, a twisted zero could sit directly on top of a zero $\rho$ of $L(\psi, s)$ and cancel its pole in the difference of logarithmic derivatives; the reduction succeeds only because the Stage 01 upper bound holds for *every* prime $d \equiv d_0 \pmod{q_0}$, allowing the proof to pick a good prime $d$ after $\rho$ is given. Second, the congruence condition $d \equiv 7 \pmod 8$ (strengthened in the one-sided bound to $d \equiv q_0/2 - 1 \pmod{q_0}$ with $18480 \mid q_0$) is structurally necessary: it pins down the quadratic-reciprocity sign of $(\cdot/d)$ and ensures that $-d$ is the discriminant of an imaginary quadratic field in which $2$ splits, feeding directly into both the zero-density proof and the downstream geometric stage.

## Level 2 — Undergrad Bridge

For a Dirichlet character $\chi \bmod N$ and an odd prime $d$ with $\gcd(N, d) = 1$, the quadratic twist $\chi_d = \chi \cdot (\cdot/d)$ is a Dirichlet character modulo $N d$. It is never principal (`quadTwist_ne_one`): choosing a quadratic non-residue $a \bmod d$ and solving $n \equiv 1 \pmod N$, $n \equiv a \pmod d$ via the Chinese Remainder Theorem yields $\chi_d(n) = -1$. Consequently $L(\chi_d, s)$ is entire on $\mathbb{C}$, with no pole at $s = 1$.

In the Stage 01 error term `primeRemainder`, the factor $1 - (n/d)$ couples the untwisted and twisted logarithmic derivatives: for $\operatorname{Re}(s) > 1$,
$$\sum_{\substack{n=1 \\ n \equiv 1 \pmod q}}^{\infty} \frac{\Lambda(n)\bigl(1 - (n/d)\bigr)}{n^s} = \frac{1}{\varphi(q)} \sum_{\psi \bmod q} \left(-\frac{L'(\psi, s)}{L(\psi, s)} + \frac{L'(\psi \cdot (\cdot/d), s)}{L(\psi \cdot (\cdot/d), s)}\right).$$
Accordingly, the Laplace transform of $v \mapsto R_q(e^v)$ has at most simple poles in $\operatorname{Re}(s) > 0$, none at $s = 1$, and residue
$$r(s) = \frac{n(s) - n'(s)}{\varphi(q)\,s(1 - s)},$$
where $n(s) = \sum_{\psi \bmod q} \operatorname{ord}_s(\operatorname{entireL}(\psi))$ and $n'(s) = \sum_{\psi \bmod q} \operatorname{ord}_s(\operatorname{entireL}(\psi \cdot (\cdot/d)))$.

To make $n'(s) = 0$ on a box $\operatorname{Re}(s) \ge \beta = 1/2 + 9\delta$, $|\operatorname{Im}(s)| \le H$, upstream `Lib.Analytic.QuadraticTwist` proves a zero-density estimate in the spirit of Jutila (1975) and Heath-Brown (1995), replacing the full real-character large sieve by a smoothed theta-series mean square bounded via the Pólya–Vinogradov inequality:
1. For a primitive character $\chi \neq 1$ of conductor $q \le T^2$, a zero in the box forces the theta-series tail integral $J(\chi, T) = \int_{1/T}^{\infty} (|\theta_\chi(u)| + |\theta_{\mu\chi}(u)|)\,du/u$ to exceed $T^{2\delta}$, via a truncated mollifier and Hadamard's three-lines theorem.
2. When $d \equiv 7 \pmod 8$, quadratic reciprocity rewrites $(n/d) = (-d/n^\circ)$ in terms of the odd part $n^\circ$ of $n$; averaging the squared theta series over $v \le T^2$ bounds $\sum J$ by $O(T^{2 + \delta})$, showing that at most $O(X^{1 - \delta/2})$ primes $d \le X$ in the class are "bad" (possess a twisted zero in the box).
3. Since $\sum_{p \equiv a \pmod N} (\log p)/p$ diverges by Dirichlet's theorem, not all primes of the invertible class can be bad, yielding a prime $d$ with $n'(s) = 0$ throughout the box.

## Level 3 — Formal Lean Contract

In Mathlib4 (`Mathlib.NumberTheory.LegendreSymbol.Basic`), `legendreSym d n : ℤ` implements $(n/d)$ for `[Fact d.Prime]` via `quadraticChar (ZMod d)`, and `quadraticChar_isQuadratic` certifies that `legendreSym d n ∈ {0, 1, -1}`.

Upstream formalizes the quadratic twist and its zero-density selection across three modules:
- `src/Lib/Analytic/EntireLFunction.lean`: `New.quadTwist_apply : quadTwist χ d n = χ n * legendreSym d n` and `New.quadTwist_ne_one` (for `d ≠ 2` and `N.Coprime d`).
- `src/Lib/Analytic/QuadraticTwist.lean`: `New.zero_density_quadTwist` (the $O(X^{1 - \delta/2})$ bound on `IsBadPrime`) and `New.exists_entireL_quadTwist_ne_zero`, which produces `d : Primes` in the residue class with `entireL (quadTwist ξ d) s ≠ 0` on `β ≤ s.re`, `|s.im| ≤ H`.
- `src/New/Reduction/Zeros/PrimeError.lean`: `New.one_sub_chiK_mem_Icc` (`1 - (chiK d n : ℝ) ∈ Set.Icc 0 2`) and `New.isPolar_primeRemainder`.

Stage 01 anchors the twist in both top-level reduction theorems:
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
