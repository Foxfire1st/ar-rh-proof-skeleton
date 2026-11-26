---
slug: schur-jet
term: Schur jet
aliases: ["jet series", "minor product", "predicted divisibility"]
inline_glimpse: "The evaluation at the origin of a symmetry-adapted Schur differential operator on a theta function's multivariate Taylor expansion, whose prime-by-prime divisibility is controlled through minor products and matched by an explicit sharpness construction."
visual_ref: assets/stage02-partner-couplings-and-divisibility-count.svg
where_analogy_breaks: "Thinking of a Schur jet as a single high-order Taylor coefficient $F^{(k)}(0)$ captures why differentiation creates extra prime divisibility, but misses three layers: the differential operators are matrix Schur polynomials indexed by Young diagrams and Weyl fillings across all infinite places simultaneously; passing to a minor product is a non-archimedean norm inequality over $\\mathbb{C}_p$ (not an identity for a fixed $y$) via residue-field independence; and the predicted divisibility includes a logarithmic loss $f N^3 (1 + \\log_p \\max(B, 1))$ plus an $e = 0$ half-term at $p = d$, while sharpness is attained on twelve-copy indicator data rather than an arbitrary single Schur jet."
sources:
  - title: "Divisibilities, congruences, and Cartier duality"
    citation: "N. M. Katz, J. Fac. Sci. Univ. Tokyo 28 (1981)."
    kind: paper
  - title: "Sur certains groupes d'opérateurs unitaires"
    citation: "A. Weil, Acta Math. 111 (1964)."
    kind: paper
  - title: "Representation Theory"
    citation: "W. Fulton & J. Harris, Graduate Texts in Mathematics 129, Springer (1991)."
    kind: book
lean_anchors:
  - 02_derivatives/count.lean#New.minorProd
  - 02_derivatives/count.lean#New.predictedDivisibility
  - 02_derivatives/count.lean#New.Parameters.norm_jet_minorProd_le_of_hasNormalForm
  - 02_derivatives/count.lean#New.exists_norm_schurJet_le_norm_jet_minorProd
  - 02_derivatives/bound_attained.lean#New.Data.exists_rpow_neg_localPrimeSumTotal_le_norm_coeff
child_concepts:
  - prime-number
  - theta-normal-form
  - elliptic-theta-function
  - tight-frame
  - arithmetic-height
  - von-mangoldt-function
---

# Schur Jet

## Level 1 — First-Principles

To probe a smooth multivariable function near the origin, you take partial derivatives and evaluate them at $0$. Doing this for a single polynomial in partial derivatives $P(\partial)$ produces a single number $(P(\partial) F)(0)$ called a *jet*. When the variables of $F$ are organized into a collection of matrices $z_\tau$—one matrix for each embedding of our symmetry field—ordinary coordinate derivatives scramble under basis changes. Representation theory tells us which differential polynomials respect matrix symmetries: determinants of square submatrices (minors), products of such determinants (**minor products**), and the canonical symmetry-adapted combinations called Schur polynomials, indexed by stair-step box shapes (Young diagrams) together with row fillings. Applying a Schur polynomial of partial derivatives at the origin to a theta function's multivariate Taylor expansion (its **jet series**) produces a **Schur jet**.

Why does Stage 02 spend its entire climax measuring the divisibility of Schur jets? Because the global proof balances ordinary geometric size against divisibility at every {{concept:prime-number|indivisible whole number $p \in \{2, 3, 5, 7, \dots\}$}} via {{concept:arithmetic-height|the global balance between ordinary magnitude and prime-by-prime divisibility}}. At each non-split prime $p$, a product of {{concept:elliptic-theta-function|quasi-periodic building-block functions attached to a two-dimensional lattice}} can be rewritten in {{concept:theta-normal-form|canonical exponential factorization whose exponent sums universal power-coupling terms across paired embeddings}}. Whenever a differential operator hits that exponential factor, the chain rule pulls down powers of coupled variables whose exponents differ by powers of $p$. If the Young diagram at one embedding has many more boxes than its paired embedding can absorb, the surplus derivatives are forced to strike the integral part or accumulate factorial factors, guaranteeing high divisibility by $p$.

Measuring that exact divisibility takes two conceptual leaps:

1. **From Schur jets to minor products:** A general Schur polynomial is a tangled sum of determinants, hard to differentiate directly. Instead, Stage 02 expands a simpler **minor product** $P_y(z)$—built by multiplying $z_\tau$ by an auxiliary test matrix $y_\tau$ and taking one determinant per column of the Young diagram—as a linear combination of Schur polynomials. Because the coefficient polynomials stay linearly independent even modulo $p$, there is always an integral choice of $y_\tau$ that prevents cancellation and makes the jet along $P_y$ at least as large (in $p$-adic size) as the target **Schur jet**.
2. **From minor products to the predicted divisibility:** Next, a tailored linear change of variables turns the jet along $P_y$ into a clean determinant derivative while making every exponential coupling vanish modulo $p$ (or modulo $\sqrt{-d}$ when $p = d$). Once the exponential factor is integral, Legendre's classical formula for how many times $p$ divides a factorial yields an explicit lower bound on the divisibility exponent—the **predicted divisibility**—which depends purely on counting height-$1$ columns, taller columns, and boxes across paired embeddings.

Crucially, this divisibility bound is not merely a loose overestimate. A companion sharpness theorem constructs twelve-copy theta data using a {{concept:tight-frame|balanced overcomplete system of integral directions reconstructing the inner product}} and shows that some Taylor coefficient actually attains the predicted exponent at every prime simultaneously (up to a tiny, globally bounded normalizing factor). That matching upper-and-lower divisibility ledger carries the exact prime-power weights—closely aligned with the {{concept:von-mangoldt-function|step function assigning $\log p$ to every prime power $p^k$ and zero elsewhere}}—needed by later stages of the proof.

### Limits of the intuition

Thinking of a **Schur jet** as a single high-order derivative $F^{(k)}(0)$ captures why differentiation harvests prime divisibility from factorials and exponential couplings, but breaks down in three concrete ways. First, the differential operators are matrix Schur polynomials acting across all infinite places $\tau \in \Phi$ simultaneously, so the divisibility count depends sensitively on the shape of the Young diagrams $\kappa(\tau)$—specifically the mismatch between height-$1$ columns and taller columns at paired embeddings—rather than just the total derivative order. Second, replacing a **Schur jet** by a **minor product** jet is a one-way $p$-adic norm inequality over the completed algebraic closure $\mathbb{C}_p$, not an algebraic identity for a fixed matrix $y$; it relies on residue-field linear independence of the dual Weyl basis. Third, the **predicted divisibility** is only proved up to an explicit logarithmic error term $f N^3 (1 + \log_p \max(B, 1))$ arising from Legendre's formula on the $f N^3$ auxiliary direction variables, and at the ramified prime $p = d$ it subtracts a half-weight correction for $e = 0$; likewise, sharpness is proved for twelve-copy indicator data rather than for every individual Schur jet entry.

## Level 2 — Undergrad Bridge

Let $R$ be a commutative ring, $\sigma$ a finite index set, $F \in R[[X_\sigma]]$ a formal power series, and $P \in R[X_\sigma]$ a polynomial. The **jet pairing** is the bilinear evaluation
$$\operatorname{jet}_R(P, F) = (P(\partial) F)(0) = \sum_{d : \sigma \to_0 \mathbb{N}} ([X^d] P)\cdot d!\cdot ([X^d] F), \qquad d! = \prod_{x \in \sigma} (d_x)!.$$
For a vector $f$ in the finite part of a positive-definite adelic Heisenberg space $V$ (Weil 1964), its **jet series** `V.jetSeries a f` is the multivariate Taylor expansion at $0$ of $z \mapsto \operatorname{entireTheta}(f)(a z)$.

Now specialize the variable set to $\sigma = \Phi \times n' \times n$, representing a tuple of $N \times |n|$ matrices $z_\tau = (z_{\tau, b, a})$ indexed by the infinite places $\tau \in \Phi$ of the CM field $E$, with $|n| \le |n'| = N$. Let $\kappa : \Phi \to \mathbb{N} \to \mathbb{N}$ assign to each place $\tau$ a Young diagram having $\kappa(\tau, h)$ columns of height $h$, satisfying `FitsRanks` (no column height strictly between $|n|$ and $N$).

1. **Minor products and Schur jets (`minorProd`, `exists_norm_schurJet_le_norm_jet_minorProd`):** A standard Weyl filling $i' \in \operatorname{WeylIndex}(n', \kappa)$ assigns a row $(i'(\tau))_{h,j,b} \in n'$ to each box $b < h$ of column $j < \kappa(\tau, h)$ at place $\tau$. Given auxiliary matrices $y_\tau$ of size $n \times \mathbb{N}$, the **minor product** is
   $$P_y(z) = \operatorname{minorProd}(i', y) = \prod_{\tau \in \Phi} \prod_{h < N} \prod_{j < \kappa(\tau, h)} \det\!\left(\sum_{a \in n} y_{\tau, a, k}\, z_{\tau, (i'(\tau))_{h,j,b}, a}\right)_{0 \le b, k < h}.$$
   By Schur–Weyl duality (Fulton & Harris 1991), $P_y(z)$ expands in row $i'$ of the Schur matrix $\operatorname{schur}(\kappa, z)$ (`minorProd_eq_sum`):
   $$P_y(z) = \sum_{i \in \operatorname{WeylIndex}(n, \kappa)} \bigl(\operatorname{nablaBasis}_i(y)\cdot \operatorname{detPow}(y)\bigr)\,\operatorname{schur}(\kappa, z)_{i', i}.$$
   Over $\mathcal{O}_{\mathbb{C}_p}$, the polynomials $\operatorname{nablaBasis}_i \cdot \operatorname{detPow}$ remain linearly independent after reduction to the residue field. Applying the linear-independence norm lemma (`exists_norm_le_norm_sum_mul_eval`) to the numbers $\alpha_i = \operatorname{schurJet}_\kappa(F)_{i', i} = \operatorname{jet}(\operatorname{schur}(\kappa, z)_{i', i}, F)$ yields integral matrices $y_\tau$ over $\mathcal{O}_{\mathbb{C}_p}$ with
   $$\|\operatorname{schurJet}_\kappa(F)_{i', i}\|_p \le \|\operatorname{jet}_{\mathbb{C}_p}(P_y, F)\|_p.$$

2. **Direction substitution and predicted divisibility (`predictedDivisibility`, `norm_jet_minorProd_le_of_hasNormalForm`):** Let $p$ be non-split in $K = \mathbb{Q}(\sqrt{-d})$ ($\chi_K(p) \neq 1$), $\varpi \in \mathbb{C}_p$ satisfy $v_p(\varpi) = \rho_p = v_p(\pi_p)/(q_p - 1)$, and suppose $F$ has normal form `HasNormalForm` for $I_p^\circ$ and partner map $\operatorname{pr}_e(\tau) = -p^e \tau$. Let $a_\tau = \kappa(\tau, 1)$ count height-$1$ columns, $a'_\tau = \sum_{h < N, h \neq 1} \kappa(\tau, h)$ count other columns below height $N$, $l_\tau = \sum_{h < N, h \neq 1} h\,\kappa(\tau, h)$ count boxes in those other columns, and $B = \sum_\tau (a_\tau + l_\tau)$ be the total box count below height $N$.

   Introducing at most $f N^3$ auxiliary variables $s_{\tau, h, k, b}$ ($h < N$, $k < h$), we substitute $z_{\tau, b, a} = \sum_{h, k} p^{w_{\tau, h, b}} \beta_{\tau, h, k, a} s_{\tau, h, k, b}$ with $\beta_{\tau, h, k, a} = \sum_{k'} (g_{\tau, h})_{k, k'} y_{\tau, a, k'}$. Choosing the row charges $w$ and integral transformation matrices $g_{\tau, h}$ so that all Frobenius couplings $\Gamma_e(z(s))$ become divisible by $\pi_p$ (`dvd_subst_coupling_directionSubst_of_charges`) renders $F(\varpi z(s))$ an integral power series over $\mathcal{O}_{\mathbb{C}_p}$ (echoing the $p$-adic differential congruence mechanism of Katz 1981). Applying Legendre's bound $v_p(d!) \ge |d|/(p-1) - |\sigma|(1 + \lfloor \log_p |d|\rfloor)$ to the homogeneous determinant product `columnDetProd` of degree $B$ and dividing out the chain-rule factors $\varpi^B$, $p^{-\operatorname{fillingCharge}(w, i')}$, and $\prod \det(g_{\tau, h})^{\kappa(\tau, h)}$ (`norm_jet_columnDetProd_subst`) produces the **predicted divisibility**:
   $$\operatorname{predictedDivisibility}(d, p, \operatorname{pr}, \kappa, N) = v_p(\pi_p)\!\sum_{e \in I_p}\!\sum_{\tau \in \Phi}\!\left[\left(\frac{a_\tau}{p^e} - a_{\tau'} - a'_{\tau'}\right)_+ + \left(\frac{l_\tau}{p^e} - l_{\tau'}\right)_+ - \frac{a'_\tau}{p^e}\right] - \mathbf{1}_{p=d}\,\frac{1}{2}\sum_{\tau \in \Phi}\Bigl(\min(a_\tau, a_{\tau_0} + a'_{\tau_0}) + a'_\tau + \min(l_\tau, l_{\tau_0})\Bigr),$$
   where $I_p = \text{positivePartnerExponents}(d, p)$, $\tau' = \operatorname{pr}_e(\tau)$, $\tau_0 = \operatorname{pr}_0(\tau)$, and $x_+ = \max(x, 0)$. The main divisibility count then states:
   $$\|\operatorname{jet}_{\mathbb{C}_p}(P_y, F)\|_p \le p^{-\bigl(\operatorname{predictedDivisibility} - f N^3 (1 + \log_p \max(B, 1))\bigr)}.$$

3. **Global sharpness (`exists_rpow_neg_localPrimeSumTotal_le_norm_coeff`):** To verify that the local divisibility count gives up nothing essential, Stage 02 works on `twelveSpace` = $\operatorname{Hom}(E, E^{12})$ (twelve parallel copies of the one-column space). For admissible data $D$ and any degree vector $l : \Phi \to \mathbb{N}$ in the window $\kappa \le l \le \kappa + x^{42}$, lifting one-dimensional curve sections along a tight frame yields integral twelve-copy theta functions (`exists_integral_isThetaFunction_twelveTaylor_eq`) whose coefficients are dominated by finite-modulus indicator vectors `indicatorDatum 𝔪 y₀` ($\mathfrak{m} = (\pi_M)$). Assembling the global denominator factor $t \neq 0$ with all Galois conjugates bounded by $|\sigma(t)| \le \exp(f m b / k)$ gives, at *every* prime $p$ and place $\iota_p$, a multi-index $\gamma$ with $\sum_{b < 12} \gamma(\tau, b, ()) = l_\tau$ and a class $y_0 \pmod{\mathfrak{m}}$ attaining
   $$p^{-S_p} \le \left\|\iota_p\!\left(\frac{\operatorname{vacuumScale}^{12}}{t} \Bigl(\prod_{\tau \in \Phi} (l_\tau)!\Bigr)\,\operatorname{coeff}_\gamma\bigl(\operatorname{twelveTaylor}(\operatorname{indicatorDatum}\ \mathfrak{m}\ y_0)\bigr)\right)\right\|_p,$$
   where $S_p = D.\operatorname{localPrimeSumTotal}(p)$ is the predicted local divisibility total.

## Level 3 — Formal Lean Contract

Stage 02 extracts the Schur-jet divisibility count into `skeleton/02_derivatives/count.lean` (from upstream `src/New/Derivatives/Divisibility/Count.lean`) and the matching sharpness theorem into `skeleton/02_derivatives/bound_attained.lean` (from upstream `src/New/Derivatives/Sharpness/BoundAttained.lean`):

- `02_derivatives/count.lean#New.minorProd` (upstream `src/New/Derivatives/Divisibility/Count.lean:111-118`):
  ```lean
  noncomputable def New.minorProd (i' : WeylIndex n' κ) (y : Φ → n → ℕ → R) :
      MvPolynomial (Φ × n' × n) R
  ```
- `02_derivatives/count.lean#New.predictedDivisibility` (upstream `src/New/Derivatives/Divisibility/Count.lean:411-428`):
  ```lean
  noncomputable def New.predictedDivisibility [Fact d.Prime] : ℝ
  ```
- `02_derivatives/count.lean#New.Parameters.norm_jet_minorProd_le_of_hasNormalForm` (upstream `src/New/Derivatives/Divisibility/Count.lean:686-713`):
  *Stub truncation note:* Because the upstream return type binds `let N : ℕ := card n'`, the automated extractor truncates the skeleton stub in `skeleton/02_derivatives/count.lean` at `let N : ℕ := by sorry`. The full upstream statement (lines 703–710 of `src/New/Derivatives/Divisibility/Count.lean`) is:
  ```lean
  theorem New.Parameters.norm_jet_minorProd_le_of_hasNormalForm [DecidableEq n'] (hp : chiK P.d p ≠ 1)
      (hϖ : ‖ϖ‖ = p ^ (-varpiValuation P.d p))
      (hF : HasNormalForm (partnerExponents P.d p) (P.partner s p) ϖ l F)
      (i' : WeylIndex n' κ) (y : P.Φ s → n → ℕ → valRingCp p) :
      let N : ℕ := card n'
      ‖jet ℂ_[p] (minorProd i' fun (τ : P.Φ s) (a : n) (k : ℕ) ↦ (y τ a k : ℂ_[p])) F‖ ≤
        p ^ (-(predictedDivisibility P.d p (P.partner s p) κ N -
          card (P.Φ s) * N ^ 3 * (1 + Real.logb p (max (boxes κ N) 1 : ℕ))) : ℝ)
  ```
- `02_derivatives/count.lean#New.exists_norm_schurJet_le_norm_jet_minorProd` (upstream `src/New/Derivatives/Divisibility/Count.lean:790-821`):
  ```lean
  lemma New.exists_norm_schurJet_le_norm_jet_minorProd
      (hκ : FitsRanks (fun _ : Φ ↦ n) (fun _ : Φ ↦ n') κ)
      (F : MvPowerSeries (Φ × n' × n) ℂ_[p]) (i' : WeylIndex n' κ) (i : WeylIndex n κ) :
      ∃ y : Φ → n → ℕ → valRingCp p,
        ‖schurJet κ F i' i‖ ≤ ‖jet ℂ_[p]
          (minorProd i' fun (τ : Φ) (a : n) (k : ℕ) ↦ (y τ a k : ℂ_[p])) F‖
  ```
- `02_derivatives/bound_attained.lean#New.Data.exists_rpow_neg_localPrimeSumTotal_le_norm_coeff` (upstream `src/New/Derivatives/Sharpness/BoundAttained.lean:375-448`):
  ```lean
  theorem New.Data.exists_rpow_neg_localPrimeSumTotal_le_norm_coeff {P : Parameters} (D : Data P) (l : D.Φ → ℕ)
      (hl : ∀ τ : D.Φ, (D.weight τ : ℝ) ≤ l τ ∧ l τ ≤ (D.weight τ : ℝ) + D.x ^ 42) :
      ∃ t : ℂ, t ≠ 0 ∧
        (∀ σ : ℂ ≃+* ℂ, ‖σ t‖ ≤ exp (D.f * D.m / D.k * D.b)) ∧
        ∀ 𝔪 : Modulus D.E, 𝔪.ideal = .span {D.piM} → ∀ (p : ℕ) [Fact p.Prime] [PadicPlace p],
          ∃ (γ : D.Φ × Fin 12 × Unit →₀ ℕ) (y₀ : Matrix (Fin 12) Unit 𝔪.ResidueRing),
            (∀ τ : D.Φ, ∑ b : Fin 12, γ (τ, b, ()) = l τ) ∧
            p ^ (-D.localPrimeSumTotal p) ≤ ‖ιₚ
              (P.vacuumScale D.towerExp ^ 12 / t * (∏ τ : D.Φ, (l τ)!) *
              coeff γ (D.twelveTaylor (D.indicatorDatum 𝔪 y₀)))‖
  ```

Notice how `exists_norm_schurJet_le_norm_jet_minorProd` composes directly with `norm_jet_minorProd_le_of_hasNormalForm` (itself an immediate specialization of `norm_jet_minorProd_le_of_charges` with empty marked set and zero charges) to bound every Schur-jet entry, while `exists_rpow_neg_localPrimeSumTotal_le_norm_coeff` supplies the complementary sharpness lower bound at every prime simultaneously. See the page's live verification banner (`audit/verification-status.json`) for the currently pinned upstream snapshot and local Lean build status (`build_status: unverified-locally`).
