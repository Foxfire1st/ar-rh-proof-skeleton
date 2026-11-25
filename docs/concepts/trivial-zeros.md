---
slug: trivial-zeros
term: trivial zero
aliases: []
inline_glimpse: "A zero at a non-positive integer forced purely by poles of the gamma factor in the functional equation, carrying no hidden information about prime fluctuations."
visual_ref: assets/complex-plane-critical-strip.svg
where_analogy_breaks: "The word 'trivial' means structurally explained by the gamma factor alone, not 'absent from the function' or 'identical for every L-function': the exact set of trivial zeros shifts depending on whether the character is trivial, even, or odd, and omitting $s = 0$ for $\\zeta(s)$ matters."
sources:
  - title: "The Theory of the Riemann Zeta-Function"
    citation: "E. C. Titchmarsh, 2nd ed., Oxford University Press (1986), Chapter 2, §§2.1–2.12."
    kind: book
  - title: "Multiplicative Number Theory"
    citation: "H. Davenport, 3rd ed., Springer Graduate Texts in Mathematics 74 (2000), Chapters 9 and 12."
    kind: book
  - title: "Analytic Number Theory"
    citation: "H. Iwaniec and E. Kowalski, AMS Colloquium Publications Vol. 53 (2004), Chapter 5."
    kind: book
lean_anchors:
  - 00_statement/formal_conjectures_statement.lean#GRH.trivialZeros
child_concepts:
  - riemann-zeta-function
  - analytic-continuation
  - dirichlet-l-function
  - critical-strip
---

# Trivial Zero

## Level 1 — Abitur / First-Principles

When {{concept:riemann-zeta-function|the global function built from $\sum n^{-s}$}} is extended to the whole plane by {{concept:analytic-continuation|the unique complex-differentiable extension beyond $\operatorname{Re}(s) > 1$}}, it satisfies a mirror symmetry linking its value at $s$ to its value at $1 - s$. That mirror equation includes a built-in scaling factor (the gamma factor) that blows up to infinity at every negative even integer $s = -2, -4, -6, \dots$. Because the completed product stays finite and nonzero at those points, the zeta function itself is forced to equal $0$ there to cancel the blow-up.

A zero forced solely by that gamma-factor blow-up is a **trivial zero**. Because trivial zeros are already completely accounted for by the symmetry factor, they tell us nothing mysterious about primes. Every other zero—the ones that actually govern prime fluctuations—must live inside {{concept:critical-strip|the vertical band $0 < \operatorname{Re}(s) < 1$}}. The same classification applies to {{concept:dirichlet-l-function|twisted cousins of the zeta function weighted by periodic arithmetic signs}}, except the exact list of non-positive integers depends on whether the twist is even or odd.

### Limits of the intuition

Calling these zeros "trivial" suggests they are unimportant edge cases one can ignore casually. In formal statements, getting the exact exclusion set wrong makes the theorem either false or vacuous: for $\zeta(s)$, $s = 0$ is *not* a zero ($\zeta(0) = -1/2$), whereas for a nontrivial even character $s = 0$ *is* a trivial zero, and for an odd character the trivial zeros shift to the negative odd integers $-1, -3, -5, \dots$.

## Level 2 — Undergrad Bridge

For a primitive Dirichlet character $\chi \bmod q$, define the parity parameter $\mathfrak{a} \in \{0, 1\}$ by $\chi(-1) = (-1)^{\mathfrak{a}}$ ($\mathfrak{a} = 0$ for even $\chi$, $\mathfrak{a} = 1$ for odd $\chi$). The completed $L$-function is
$$\Lambda(\chi, s) = \left(\frac{q}{\pi}\right)^{(s + \mathfrak{a})/2} \Gamma\!\left(\frac{s + \mathfrak{a}}{2}\right) L(\chi, s).$$
The functional equation relates $\Lambda(\chi, s)$ to $\Lambda(\overline{\chi}, 1 - s)$ by a nonzero root-number constant of modulus $1$. For $\operatorname{Re}(s) \le 0$, we have $\operatorname{Re}(1 - s) \ge 1$, where $L(\overline{\chi}, 1 - s) \neq 0$ and $\Gamma((1 - s + \mathfrak{a})/2)$ has no zeros, so $\Lambda(\chi, s) \neq 0$ (aside from the pole at $s = 0$ when $q = 1$, where the simple pole of $\Lambda(s)$ cancels the simple pole of $\Gamma(s/2)$ to leave $\zeta(0) = -1/2 \neq 0$).

Hence $L(\chi, s) = 0$ for $\operatorname{Re}(s) \le 0$ occurs if and only if the gamma factor $\Gamma((s + \mathfrak{a})/2)$ has a pole and $(q, s) \neq (1, 0)$:
- **$q = 1$ ($\chi = 1$, giving $\zeta$):** $s \in \{-2(n+1) : n \in \mathbb{N}\} = \{-2, -4, -6, \dots\}$;
- **$q > 1$, $\chi$ odd ($\chi(-1) = -1$):** $s \in \{-2n - 1 : n \in \mathbb{N}\} = \{-1, -3, -5, \dots\}$;
- **$q > 1$, $\chi$ even ($\chi(-1) = 1$):** $s \in \{-2n : n \in \mathbb{N}\} = \{0, -2, -4, \dots\}$.

For an *imprimitive* character $\chi \bmod N$ induced by a primitive character $\chi^* \bmod q$ ($q \mid N$), $L(\chi, s) = L(\chi^*, s) \prod_{p \mid N} (1 - \chi^*(p) p^{-s})$ acquires additional zeros on the vertical line $\operatorname{Re}(s) = 0$ whenever $\chi^*(p) \neq 0$; this is why `formal-conjectures` states GRH for primitive characters when excluding only `GRH.trivialZeros`.

## Level 3 — Formal Lean Contract

Stage 00 anchors the exact `formal-conjectures` definition in `00_statement/formal_conjectures_statement.lean#GRH.trivialZeros` (upstream `comparator/FormalConjecturesStatement.lean:70-79`):

```lean
noncomputable def GRH.trivialZeros {q : ℕ} (χ : DirichletCharacter ℂ q) : Set ℤ :=
  if q = 1 then {-2 * (n + 1) | (n : ℕ) } else
    if χ.Odd then { -2 * n - 1 | (n : ℕ) } else
      { - 2 * n | (n : ℕ) }
```

In `comparator/FormalConjecturesBridge.lean`, `mem_trivialZeros_of_re_nonpos` proves that for every primitive `χ`, any zero of `LFunction χ` with `s.re ≤ 0` belongs to `Int.cast '' GRH.trivialZeros χ`, bridging the strip formulation `PrimitiveDirichletLZerosOnLine` to `GRH.generalized_riemann_hypothesis`. Upstream provenance is pinned at commit `c900e67c2fcd1fdb041685820bbd2b867fbc5a4b` (`build_status: unverified-locally`).
