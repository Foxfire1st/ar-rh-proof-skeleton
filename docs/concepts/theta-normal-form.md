---
slug: theta-normal-form
term: theta normal form
aliases:
  - partner place
  - Frobenius coupling
inline_glimpse: "A p-adic factorization of rescaled multivariable theta products into an integral power series times an exponential of Frobenius-power couplings pairing each infinite place with its shifted partner place."
visual_ref: assets/stage02-frobenius-normal-form-pipeline.svg
where_analogy_breaks: "Picturing a theta normal form as factoring a function into a smooth amplitude times an oscillating phase $W(z) e^{i\\phi(z)}$ captures the separation between harmless integral clutter and the sole source of $p$-adic denominators, but misses the non-archimedean mechanism: $W$ is integral in $\\mathcal{O}[[z]]$, the exponent is scaled by $c_\\varpi = \\varpi^{p+1}/p$ sitting just past the $p$-adic exponential radius $p^{-1/(p-1)}$, and each Frobenius coupling $\\Gamma_e = \\sum z_{\\tau, c}^{p^e} z_{\\tau', c}$ pairs distinct infinite places $\\tau$ and $\\tau' = (-p^e)\\tau$ with asymmetric degrees $p^e$ and $1$."
sources:
  - title: "Algebraic theta functions and the p-adic interpolation of Eisenstein-Kronecker numbers"
    citation: "K. Bannai and S. Kobayashi, Duke Mathematical Journal 153 (2010)."
    kind: paper
  - title: "Integral structures on p-adic Fourier theory"
    citation: "K. Bannai and S. Kobayashi, Annales de l'Institut Fourier 66 (2016)."
    kind: paper
  - title: "Congruences entre nombres de Bernoulli-Hurwitz dans le cas supersingulier"
    citation: "M. Chellali, Journal of Number Theory 35 (1990)."
    kind: paper
lean_anchors:
  - 02_derivatives/theta_products.lean#New.Parameters.partner
  - 02_derivatives/theta_products.lean#New.Parameters.exists_hasNormalForm
  - 02_derivatives/count.lean#New.Parameters.norm_jet_minorProd_le_of_charges
child_concepts:
  - formal-group-law
  - elliptic-theta-function
  - tight-frame
  - schur-jet
  - arithmetic-height
---

# Theta Normal Form

## Level 1 — First-Principles

Suppose you have a complicated multivariable function built by multiplying together many copies of an {{concept:elliptic-theta-function|analytic building block on a curved lattice}} evaluated along different linear directions, and you want to know how divisible its high-order Taylor derivatives are by a {{concept:prime-number|indivisible whole number $p \in \{2, 3, 5, 7, \dots\}$}}. Expanding the product blindly produces a tangle of overlapping coefficients where denominators from one factor could cancel or compound against another. What you want is a canonical normal form that isolates every source of non-integrality into a single transparent expression.

A **theta normal form** achieves exactly that separation at every prime $p$ that does not split in the underlying imaginary quadratic field. Rescaling all variables by a single calibrated constant $\varpi$ derived from the curve's {{concept:formal-group-law|power-series addition law and its $p$-th-power congruences}}, the multivariable {{term:elliptic-theta-function|theta product}} splits cleanly into two pieces:
$$F(\varpi z) = W(z) \cdot \exp\biggl(c_\varpi \sum_{e, j} \lambda_{e, j} \Gamma_e(z)^{p^j}\biggr).$$
The front factor $W(z)$ has completely integral coefficients—it introduces zero $p$-adic denominators on its own. All the $p$-adic action lives inside the exponential, which is built out of a sparse family of **Frobenius couplings** $\Gamma_e(z) = \sum_{\tau, c} z_{\tau, c}^{p^e} z_{\tau', c}$. Each coupling links every embedding $\tau$ of the cyclotomic field to its **partner place** $\tau' = (-p^e)\tau$, obtained by shifting $\tau$ inside the Galois group by the residue class of $-p^e$.

Why do all the tangled linear forms collapse into such simple pairwise terms? Because the vectors directing the factors form a {{concept:tight-frame|balanced spanning family whose weighted outer products reconstruct the identity matrix}}: modulo the local prime element $\pi_p$, the Frobenius-twisted sum $\sum_j m_j (u_j^\dagger z)^{p^e}(u_j^\dagger z)$ simplifies straight into $\Gamma_e(z)$, and anything divisible by $\pi_p$ inside the exponent stays integral once exponentiated and can be absorbed into $W(z)$. That clean factorization turns derivative counting into a matching puzzle across pairs $(\tau, \tau')$: whenever a linear change of variables makes every relevant $\Gamma_e$ divisible by $\pi_p$, the whole exponential becomes integral up to degree $B$, yielding an exact divisibility floor for every {{concept:schur-jet|matrix-minor derivative evaluation of the series}} that feeds directly into Stage 02's global {{concept:arithmetic-height|accounting of ordinary size against $p$-adic divisibility}}.

### Limits of the intuition

Comparing a theta normal form to separating a wave into a well-behaved amplitude $W(z)$ and a simple phase factor $e^{\phi(z)}$ accurately explains why $W$ can be ignored when tracking denominators and why only the exponent controls divisibility. The analogy breaks down in two places. First, the factor $c_\varpi = \varpi^{p+1}/p$ in front of the exponent is *not* bounded in $p$-adic size; it sits just outside the convergence radius of the $p$-adic exponential, so $e^{c_\varpi \Gamma_e}$ has genuine denominators unless a change of variables introduces an extra factor of $\pi_p$ into every coupling $\Gamma_e$ of degree at most $B$. Second, the **Frobenius coupling** $\Gamma_e$ is neither symmetric nor quadratic in general: it pairs $p^e$ powers of a variable at $\tau$ against a single power at its **partner place** $\tau'$, creating the asymmetric $1/p^e$ ratios that govern the exact divisibility count.

## Level 2 — Undergrad Bridge

Fix parameters $P$ specifying a prime $d \equiv 7 \pmod 8$, the imaginary quadratic field $K = \mathbb{Q}(\sqrt{-d})$, and a level-$s$ cyclotomic extension $E = K(\zeta_{q_1})$ with infinite-place set $\Phi = \Phi_s$ carrying a free transitive action of $\operatorname{Gal}(E/K) \cong (\mathbb{Z}/q_1\mathbb{Z})^\times$. Fix a non-split prime $p$ ($\chi_K(p) \neq 1$), an embedding $\iota_p : \mathbb{C} \hookrightarrow \mathbb{C}_p$ with valuation ring $\mathcal{O}$, and the local prime element $\pi_p$ ($\sqrt{-d}$ if $p = d$, $p$ if $p$ is inert) of valuation $v_p(\pi_p)$ and norm $q_p$ ($p$ or $p^2$).

For each exponent $e \in \mathbb{N}$ and each infinite place $\tau \in \Phi$, the **partner place** is
$$\tau' = \operatorname{partner}(s, p, e, \tau) = \operatorname{placeOfInt}(s, -p^e) \cdot \tau \in \Phi,$$
defined as multiplication by the class of $-p^e$ in $(\mathbb{Z}/q_1\mathbb{Z})^\times$ (or the identity if $-p^e$ is not coprime to $q_1$). Attached to this partner rule on variables $z = (z_{\tau, c})_{(\tau, c) \in \Phi \times C}$ is the homogeneous degree-$(p^e + 1)$ **Frobenius coupling**
$$\Gamma_e(z) = \sum_{(\tau, c) \in \Phi \times C} z_{\tau, c}^{p^e}\, z_{\tau', c} \in \mathcal{O}[[z]],$$
and for a set of exponents $S \subseteq \mathbb{N}$ and coefficients $\lambda_{e, j} \in \mathcal{O}$ the convergent coupling sum
$$\Xi_0(z) = \sum_{e \in S} \sum_{j = 0}^{\infty} \lambda_{e, j}\, \Gamma_e(z)^{p^j} \in \mathcal{O}[[z]].$$

A multivariable power series $F \in \mathbb{C}_p[[z]]$ has the **theta normal form** `HasNormalForm S partner ϖ l F` if there exists an integral power series $W \in \mathcal{O}[[z]]$ such that, with $c_\varpi = \varpi^{p + 1}/p$,
$$F(\varpi z) = W(z)\,\exp\bigl(c_\varpi\, \Xi_0(z)\bigr).$$
Here $v_p(\varpi) = \rho_p = v_p(\pi_p)/(q_p - 1)$ is chosen so that
$$v_p(c_\varpi \pi_p) = (p + 1)\rho_p - 1 + v_p(\pi_p) = \frac{1}{p - 1}.$$
Because the $p$-adic exponential series $\exp(u) = \sum_{k \ge 0} u^k / k!$ has integral coefficients whenever $v_p(u) \ge 1/(p - 1)$, any integral discrepancy $\Xi - \Xi_0$ divisible by $\pi_p$ yields an integral exponential $\exp(c_\varpi(\Xi - \Xi_0)) \in \mathcal{O}[[z]]$ that can be absorbed directly into $W$ (`HasNormalForm.of_dvd_sub`).

When $u_j \in E^n$ (with multiplicities $m_j \in \mathbb{N}$) are integral above $p$ and form a resolution of the identity $z = \sum_j m_j (u_j^\dagger z) u_j$, the arithmetic Frobenius congruence $\tau(\alpha)^{p^e} \equiv \overline{\tau'(\alpha)} \pmod{\pi_p}$ for $e \in I_p^\circ$ (odd $e$ when $p$ is inert; all $e \ge 0$ when $p = d$) implies
$$\sum_j m_j (u_j^\dagger z)^{p^e}(u_j^\dagger z) \equiv \Gamma_e(z) \pmod{\pi_p}.$$
Combining this with the single-curve formal-group factorization $\theta(\varpi t) = W_1(t)\exp(c_\varpi \Xi_1(t))$ (cf. Chellali 1990; Bannai–Kobayashi 2010, 2016) proves `exists_hasNormalForm`: every product $\bigl(\prod_j \theta(u_j^\dagger z)^{m_j}\bigr) \cdot H$ with $H$ integral in the translated coordinates $y(u_j^\dagger z)$ satisfies `HasNormalForm` with $S = I_p^\circ$, and at inert primes the leading coefficients $\lambda_{e, 0}$ for odd $e$ are $p$-adic units (a sharpness input used later in `bound_attained.lean`).

The payoff arrives in `Count.lean` (`norm_jet_minorProd_le_of_charges`). Given a Young-diagram family $\kappa$, a Weyl filling $i'$, integral matrices $y_\tau$, a set of marked places, and row charges $p^{w(\tau, h, b)}$ covering every exempt pair, one constructs a linear change of variables $z(s)$ whose directions at $\tau$ and $\tau'$ satisfy $\pi_p \mid \Gamma_e(z(s))$ for every $e \in I_p^\circ$ with $e \le \lfloor\log_p B\rfloor$ (where $B$ is the total number of boxes in columns of height $< N = |n'|$). By `HasNormalForm.norm_coeff_subst_le_one`, the substituted series $F(\varpi z(s))$ has integral coefficients in all degrees $\le B$. Applying Legendre's factorial bound to the homogeneous column-determinant polynomial $P_*(s)$ and pulling back by the chain rule forces the $p$-adic valuation of the jet $(P_y(\partial)F)(0)$ plus the filling charge $\sum w$ to be at least the predicted divisibility of the reduced diagram $\kappa^\circ$ (columns of height $1$ erased at marked places), plus $\gamma_p$ per removed box, minus the logarithmic error $f N^3 (1 + \log_p \max(B, 1))$.

## Level 3 — Formal Lean Contract

Stage 02 extracts the partner-place map, the existence of the theta normal form, and the main counting bound with charges into `skeleton/02_derivatives/theta_products.lean` and `skeleton/02_derivatives/count.lean` (from upstream `src/New/Derivatives/FormalGroup/ThetaProducts.lean` and `src/New/Derivatives/Divisibility/Count.lean`, building on `New.coupling`, `New.couplingSum`, and `New.HasNormalForm` in `src/New/Derivatives/Divisibility/NormalForm.lean`):

- `02_derivatives/theta_products.lean#New.Parameters.partner` (upstream `src/New/Derivatives/FormalGroup/ThetaProducts.lean:106-112`):
  ```lean
  noncomputable def New.Parameters.partner (P : Parameters) (s p e : ℕ) (τ : P.Φ s) : P.Φ s
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
- `02_derivatives/count.lean#New.Parameters.norm_jet_minorProd_le_of_charges` (upstream `src/New/Derivatives/Divisibility/Count.lean:641-684`):
  > **Skeleton stub truncation note:** Because the return type of `norm_jet_minorProd_le_of_charges` introduces local abbreviations `let N : ℕ := card n'` and `let B : ℕ := boxes κ N`, the automated skeleton extractor in `skeleton/02_derivatives/count.lean` truncates the type right after `let N : ℕ := by sorry`. For full transparency, the complete upstream Lean statement (`src/New/Derivatives/Divisibility/Count.lean:658-672`) is quoted below:
  ```lean
  theorem New.Parameters.norm_jet_minorProd_le_of_charges [DecidableEq n'] (hp : chiK P.d p ≠ 1)
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
            card (P.Φ s) * N ^ 3 * (1 + Real.logb p (max B 1 : ℕ))) : ℝ)
  ```

Here `HasNormalForm S pr ϖ l F` (`src/New/Derivatives/Divisibility/NormalForm.lean:55-59`) asserts `∃ W : MvPowerSeries (Φ × c) (valRingCp p), F.rescale (fun _ ↦ ϖ) = seriesToCp W * expSubst ((ϖ ^ (p + 1) / p) • seriesToCp (couplingSum S pr l))`, and `norm_jet_minorProd_le_of_hasNormalForm` (the uncharged count immediately following in `Count.lean:686-713`) is the specialization with `marked = ∅` and `w = 0`. See the page's live verification banner (`audit/verification-status.json`) for the currently pinned upstream snapshot and local Lean build status (`build_status: unverified-locally`).
