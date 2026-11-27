---
slug: theta-square-law
term: theta square law
aliases: ["square law", "commuting theta square", "up-then-down lift", "edge family"]
inline_glimpse: "Commutation of theta lifts around a four-corner square (U_1, U_2, A, A'): summing down-then-up over matched matrix data equals (kappa / c) times summing up-then-down, with |c|^2 = C * L_*(t) != 0 and kappa independent of twist."
visual_ref: assets/stage05-exchange-and-one-square.svg
where_analogy_breaks: "Calling the theta square law a commuting diagram of linear maps hides two crucial caveats: first, individual point-datum lifts do not commute—commutation holds only after summing over matched matrix functions (Psi, Psi') modulo m; second, OneSquare.lean proves kappa is independent of the character, modulus, and forms, but does NOT compute kappa or prove kappa != 0 (non-vanishing is deduced later from Rallis injectivity in ChainNorms.lean and NormOfChain.lean)."
sources:
  - title: "θ-series and invariant theory"
    citation: "R. Howe, Proc. Sympos. Pure Math. 33 (1979), Part 1, 275–285."
    kind: paper
  - title: "Seesaw dual reductive pairs"
    citation: "S. S. Kudla, in Automorphic Forms of Several Variables (Taniguchi Symposium, Katata, 1983), Progr. Math. 46, Birkhäuser (1984), 244–268."
    kind: paper
  - title: "A proof of the Howe duality conjecture"
    citation: "W. T. Gan & S. Takeda, J. Amer. Math. Soc. 29 (2016), 473–493."
    kind: paper
lean_anchors:
  - 05_determinant/one_square.lean#New.Exchange.IsSquare
  - 05_determinant/one_square.lean#New.Exchange.exists_sum_pairLift_comp_pairTransposedLift_eq_smul
  - 05_determinant/one_square.lean#New.Exchange.equivariant_and_exists_sum_eq_of_areMatched
  - 05_determinant/one_square.lean#New.Exchange.one_square_identity_of_isEdgeFamily
child_concepts:
  - complex-plane
  - theta-correspondence
  - seesaw-duality
  - siegel-weil-exchange
  - rallis-inner-product
---

# Theta Square Law

## Level 1 — First-Principles

Imagine a diamond of four spaces of automorphic forms: a starting space $U_1$ on the left, a destination space $U_2$ on the right, a larger intermediate space $A$ at the top (rank $|n| + 2t$), and a smaller intermediate space $A'$ at the bottom (rank $|n|$). Using {{concept:theta-correspondence|unitary dual-pair theta lifts}}, there are two competing routes from $U_1$ to $U_2$:
- **Top route (up-then-down lift $\Theta_i(x)$):** Lift a form from $U_1$ up to the larger space $A$, then take the adjoint/reverse lift down from $A$ to $U_2$, using the two column blocks of a matrix $x$ modulo $\mathfrak{m}_i$.
- **Bottom route (down-then-up $\Theta'_i(y)$):** Descend from $U_1$ down to the smaller space $A'$, then lift back up from $A'$ to $U_2$, using the two row blocks of a matrix $y$ modulo $\mathfrak{m}_i$.

The **theta square law** proves that these two routes commute—not matrix by matrix, but after summing over any pair of {{concept:siegel-weil-exchange|matched functions $(\Psi, \Psi')$ modulo $\mathfrak{m}_i$}}:
$$\sum_y \Psi'(y)\,\Theta'_i(y) = \frac{\kappa}{c_i} \sum_x \Psi(x)\,\Theta_i(x), \qquad \|c_i\|^2 = C \cdot L_*(t) \neq 0.$$
Here $C > 0$ and $\kappa \in \mathbb{C}$ (in {{concept:complex-plane|the complex plane}}) are fixed numbers completely independent of the index $i$, the tame character $\nu_i$, the modulus $\mathfrak{m}_i$, and the forms being lifted, while $c_i \neq 0$ carries all dependence on the twist through the partial $L$-value product $L_*(t)$.

How does Stage 05 turn {{term:siegel-weil-exchange}} into a **commuting theta square**? By the doubling trick of {{concept:seesaw-duality|pairing a direct sum of two groups against one shared partner so a product of two kernels becomes a single kernel on the sum}}: set $W = U_1 \oplus U_2$. A pair of forms $(f_1, f_2)$ on $U_1 \times U_2$ integrates against the theta kernel of $(W, A)$ as the inner product of $f_2$ with the **up-then-down lift** $\Theta_i(x) f_1$, and against $(A', W)$ as the inner product of $f_2$ with the bottom route $\Theta'_i(y) f_1$. Applying `exchange_identity` to the complementary triple $(W, A, A')$ immediately forces the two weighted sums to agree up to $\kappa / c_i$.

Both Stage 05 engines sit directly on top of this single square:
- In `NormOfChain.lean`, tiling a triangular array of spaces with **commuting theta squares** slides every lift past every descent until the Gram form of the $m$-step chain factors into a product of $m$ {{concept:rallis-inner-product|single-lift Rallis inner-product operators}}.
- In `SpanningForms.lean`, sweeping a path across two square grids via $m^2$ square turns transfers period integrals from the lower chain to short deep-level forms.

### Limits of the intuition

Calling this a **commuting theta square** can mislead a reader into assuming the proportionality constant $\kappa / c_i$ is automatically nonzero. In fact, `OneSquare.lean` explicitly allows $\kappa = 0$! Because the real Schwartz vector $w$ produced by `exchange_identity` is only known to exist abstractly (approximated by polynomial Fock vectors via `exists_eq_mul_of_forall_polyVector`), `one_square_identity_of_isEdgeFamily` guarantees only that $\kappa$ does not vary with the character, modulus, or forms—not that $\kappa \neq 0$. Non-vanishing of the composite chain scalar is established later in `ChainNorms.lean` and `NormOfChain.lean` (`chainDeterminant_pos`) from the injectivity of the first lifts in the convergent Rallis range.

## Level 2 — Undergrad Bridge

`src/New/Determinant/Exchange/OneSquare.lean` translates the pointwise identity of `TheoremForPartners.lean` on $W = U_1 \oplus U_2$ into a linear-operator law packaged as `SquareLaw`:

1. **Equivariance under $\mathrm{GL}$ modulo $\mathfrak{m}$ (`pairWeil_modDatum_single_mul_left`, `pairWeil_modDatum_single_mul_right`, `pairTransposedLift_comp_pairLift_mul`, `pairLift_comp_pairTransposedLift_mul`):**
   Left-multiplying $x \in \mathrm{Mat}_{n', n_1 \oplus n_2}(\mathcal{O}_E / \mathfrak{m}_i)$ by $g \in \mathrm{GL}_{n'}(\mathcal{O}_E / \mathfrak{m}_i)$ scales $\Theta_i(x)$ by $\chi_i(\det g)$; right-multiplying $y \in \mathrm{Mat}_{n_1 \oplus n_2, n}(\mathcal{O}_E / \mathfrak{m}_i)$ by $g \in \mathrm{GL}_n(\mathcal{O}_E / \mathfrak{m}_i)$ scales $\Theta'_i(y)$ by $\chi_i(\det g)$.
2. **Factorization of kernels on $W = U_1 \oplus U_2$ (`modKernel_single_sum_left`, `modKernel_single_sum_right`, `integral_upIntegrand`, `integral_downIntegrand`):**
   For a product of Fock polynomials $P_1 \cdot P_2$ and point-mass matrix datum $\mathbf{1}_x$ (resp. $\mathbf{1}_y$), the theta kernel on $(U_1 \oplus U_2, A)$ (resp. $(A', U_1 \oplus U_2)$) splits cleanly as a product of the two edge kernels on $(U_1, A)$ and $(U_2, A)$ (resp. $(A', U_1)$ and $(A', U_2)$) up to the character $\nu_0(\det g_2)$ linking the two sides ($b_2 \nu_0 = b_1$ on $A$, $b'_1 \nu_0 = b'_2$ on $A'$).
3. **Extending from polynomials to arbitrary Schwartz vectors (`exists_integrable_kernel_bound`, `exists_upIntegrand_bound`, `exists_norm_integral_upIntegrand_sub_le`, `exists_eq_mul_of_forall_polyVector`, `exists_integral_upIntegrand_eq_mul_integral_lift`):**
   Because `exchange_identity` produces a general Schwartz Fock vector $w$ rather than a polynomial vector, `OneSquare.lean` bounds `upIntegrand` by a finite Fock moment `fockMoment N w`, proves Lipschitz continuity in $w$, and passes from polynomials to all Schwartz vectors via density—producing the single twist-independent scalar $\kappa \in \mathbb{C}$ (possibly $0$).
4. **The square structure and edge-family specialization (`IsSquare`, `exists_sum_pairLift_comp_pairTransposedLift_eq_smul`, `equivariant_and_exists_sum_eq_of_areMatched`, `one_square_identity_of_isEdgeFamily`):**
   `IsSquare` bundles the rank gap $|n'| = |n| + 2t$, negative-sign additivity $\mathrm{negCount}(U_1 \oplus U_2) = \mathrm{negCount}(A) + \mathrm{negCount}(A')$, the weight/sign window $2N + 2 \le |n_1 \oplus n_2| \le 2\mu\tau + 2N$, tame ramification `IsTameFor (𝔪 i) (ν i) μ (χ i)`, four `IsPairEdge` conditions, and four `spaceChar` product identities matching $(\nu_i).\mathrm{baseChange}^{-1}$. When the characters come from two **edge families** (`IsEdgeFamily`), `one_square_identity_of_isEdgeFamily` packages both $\mathrm{GL}$-equivariance and the matched-sum identity into `SquareLaw (χ i) σ (κ / c)` for every form $f$ and linear functional $l$.

## Level 3 — Formal Lean Contract

Stage 05 extracts the square commutation law into `skeleton/05_determinant/one_square.lean` (32 declarations). **Stub-truncation honesty notice:** All three headline theorems (`exists_sum_pairLift_comp_pairTransposedLift_eq_smul` at L449, `equivariant_and_exists_sum_eq_of_areMatched` at L505, and `one_square_identity_of_isEdgeFamily` at L544), along with two integrability lemmas (`exists_integrable_kernel_bound` at L73, `exists_norm_integral_upIntegrand_sub_le` at L198), truncate at their `let ... :=` return-type bindings in the extracted skeleton. Their complete untruncated statements from upstream `src/New/Determinant/Exchange/OneSquare.lean` are quoted verbatim in [Siegel–Weil Exchange and the Commuting Theta Square](../stages/05_determinant/exchange-and-one-square.md).

- `05_determinant/one_square.lean#New.Exchange.IsSquare` (untruncated; upstream `OneSquare.lean:684-722`): bundles the 13 structural fields (`negCount_eq`, `card_eq`, `negCount_lt_half`, `card_le_weight`, `tame`, `edge₁`, `edge₂`, `edge₁'`, `edge₂'`, `spaceChar_mul_up`, `spaceChar_mul_down`, `spaceChar_mul_first`, `spaceChar_mul_second`).
- `05_determinant/one_square.lean#New.Exchange.exists_sum_pairLift_comp_pairTransposedLift_eq_smul` (upstream `OneSquare.lean:747-771`): proves `∃ C > 0, ∃ κ, ∀ i, ∃ c, ‖c‖^2 = C * lValueProd (𝔪 i).places (ν i) t ∧ ... ∧ ∑ y, Ψ' y • Θ' i y = (κ / c) • ∑ x, Ψ x • Θ i x`.
- `05_determinant/one_square.lean#New.Exchange.equivariant_and_exists_sum_eq_of_areMatched` (upstream `OneSquare.lean:896-929`): joins $\mathrm{GL}$-equivariance of $\Theta$ and $\Theta'$ with the matched-sum commutation identity.
- `05_determinant/one_square.lean#New.Exchange.one_square_identity_of_isEdgeFamily` (upstream `OneSquare.lean:972-1003`): specializes to `IsEdgeFamily` and outputs `SquareLaw (χ i) σ (κ / c)`.

See the page's live `<VerificationBanner />` (`audit/verification-status.json`) for the pinned upstream commit and local Lean build status (`build_status: unverified-locally`).
