---
slug: ribet-clearing-operator
term: Ribet clearing operator
aliases: ["common Ribet element", "lift generator", "weak lift", "lift valuation bound", "clearing bound", "first-order Ribet method"]
inline_glimpse: "A linear operator Lambda on the theta-lift subspace Z built from a common Ribet element and Taylor–Wiles tie relations whose eigenvalues satisfy liftValuationBound and whose rescaled determinant norm is at least l^(-dim Z * r * clearingBound D l), clearing all l-adic projection denominators on integral forms orthogonal to false lifts."
visual_ref: assets/stage07-ribet-triangle-to-regular-family.svg
where_analogy_breaks: "Picturing the Ribet clearing operator as simply multiplying the orthogonal projection by a single scalar congruence number misses two subtleties: each lift generator v_0 may have a different Galois eigensystem, so Stage 07 constructs a diagonal operator Lambda whose eigenvalues vary by eigensystem, and pairing integrality against forms integral at all places above l requires scaling by an extra factor N = D.chosenGroupOrder (adding +v_l(N) inside clearingBound D l)."
sources:
  - title: "The Eisenstein ideal at prime level"
    citation: "S. Dasgupta, M. Kakde, J. Silliman & J. Wang, arXiv:2307.11021 (2023)."
    kind: reference
  - title: "Families of Galois Representations and Selmer Groups"
    citation: "J. Bellaïche & G. Chenevier, Astérisque 324, Soc. Math. France (2009)."
    kind: book
  - title: "Selmer groups and the Eisenstein–Klingen ideal"
    citation: "E. Urban, Duke Math. J. 106 (2001), 485–525."
    kind: paper
lean_anchors:
  - 07_congruences/denominator_bound.lean#New.Selmer.IsLiftGenerator
  - 07_congruences/denominator_bound.lean#New.Selmer.liftValuationBound
  - 07_congruences/denominator_bound.lean#New.Selmer.exists_isCommonRibetElement_ellVal_le
  - 07_congruences/denominator_bound.lean#New.Selmer.typeProjection_sub_starProjection_mem_of_isOrthogonalToFalseLifts
  - 07_congruences/denominator_bound.lean#New.Selmer.exists_operator_ellVal_le_liftValuationBound
  - 07_congruences/denominator_bound.lean#New.Selmer.denominatorLe_clearingBound
child_concepts:
  - taylor-wiles-prime-triangle
  - anticyclotomic-selmer-twisting
  - crystalline-dual-defect
  - false-lift-period
  - regular-twist-family
---

# Ribet Clearing Operator

## Level 1 — First-Principles

Inside {{concept:regular-twist-family|a regular twist family}}, the hardest obligation to satisfy at a prime $\ell$ is {{term:regular-twist-family|projection clearing}} (`ClearsProjection`): when an $\ell$-integral form $y$ on $V_j$ is projected orthogonally onto the subspace $Z$ of theta lifts, congruences between $Z$ and $Z^\perp$ can introduce $\ell$-adic denominators. We need a linear operator $\Lambda : Z \to Z$ — the **Ribet clearing operator** — whose determinant has controlled $\ell$-adic size and which restores $\ell$-integrality to $\Lambda(\mathrm{pr}_Z y)$ whenever $y$ is integral and satisfies {{concept:false-lift-period|false-lift orthogonality}}.

Stage 07 constructs $\Lambda$ by the **first-order Ribet method** in four clean stages (`FirstOrder/DenominatorBound.lean`):

1. **False-lift orthogonality strips away non-weak lifts:** Suppose $Z$ is spanned by **lift generators** (`IsLiftGenerator`)—forms in $Z$ of admissible local type lifting Hecke eigensystems whose twisted {{concept:anticyclotomic-selmer-twisting|divisible crystalline Selmer groups}} have length at most $\mathrm{sel}$. Because $y$ is orthogonal to every false lift, its difference from its projection $y - \mathrm{pr}_Z y$ has zero component on every **weak lift** outside $Z$ (`typeProjection_sub_starProjection_mem_of_isOrthogonalToFalseLifts`).
2. **A common Ribet element (`exists_isCommonRibetElement_ellVal_le`):** Feeding a {{concept:taylor-wiles-prime-triangle|Taylor–Wiles prime triangle}} $Q_j$ into the trace ideal $\mathcal{J}$ produces a **common Ribet element**—a polynomial family $f$ in the tie variables $X_j$ that multiplies every determinant family $x \mapsto \det(1 - N_x(g))$ into $\mathcal{J}$, kills every monomial properly dividing $\prod_j X_j$ on the lift eigensystems $\mathcal{X}$, and has leading coefficient $d$ at the target eigensystem $x_0$ bounded by the **lift valuation bound**:
   $$v_\ell\bigl(N^2 d\bigr) \;\le\; \mathrm{liftValuationBound}(\ell, O, \mathrm{ram}, \mathrm{sel}) \;=\; \mathrm{sel} + 2 t'\,\mathrm{ram} + r'\,\mathrm{localDefectBound}(D, \ell) + \mathrm{denominatorError}(D, \ell),$$
   combining the twisted Selmer length, auxiliary ramification, {{concept:crystalline-dual-defect|local crystalline dual defect}}, and odd-class/level error terms (with zero dependence on even Tate-twist classes!).
3. **Diagonal clearing operator on $Z$ (`exists_operator_ellVal_le_liftValuationBound`):** Turning the **common Ribet element** into a Hecke-diagonal operator on base-level forms (`exists_ellVal_le_and_diagOp_mem_ellIntegralForms`) and assembling across all **lift generators** builds an endomorphism $\Lambda_0$ on $Z$ whose every eigenvalue $\mu$ satisfies $v_\ell(\iota_\ell \mu) \le \mathrm{liftValuationBound}$ and which sends $\mathrm{pr}_Z y$ into `integralForms`.
4. **Rescaling for pairing integrality (`exists_operator_det_clearingBound` $\Rightarrow$ `denominatorLe_clearingBound`):** Multiplying $\Lambda_0$ by $N = D.\mathrm{chosenGroupOrder}$ ensures that $\Lambda(\mathrm{pr}_Z y)$ not only is integral at $\iota_\ell$, but also has integral inner products against forms integral at *all* places above $\ell$. Setting $\mathrm{ram} = \mathrm{sel} = e'_\ell$ (`twistError ℓ D`) folds every term into the **clearing bound**
   $$\mathrm{clearingBound}(D, \ell) \;=\; \mathrm{localDefectBound}(D, \ell) + (1 + 2t)\,e'_\ell + \mathrm{denominatorError}(D, \ell) + v_\ell(N),$$
   yielding $\|\iota_\ell(\det \Lambda)\| \ge \exp(-(\dim Z)\, r'\, \mathrm{clearingBound}(D, \ell)\log\ell)$ (`New.Selmer.denominatorLe_clearingBound`).

### Limits of the intuition

Viewing the **Ribet clearing operator** as a single scalar multiple of the identity on $Z$ misses why `exists_operator_ellVal_le_liftValuationBound` works eigensystem by eigensystem: different **lift generators** in $Z$ belong to different Hecke eigensystems $x_0$, so $\Lambda$ acts diagonally with eigenvalues individually bounded by **lift valuation bound**, and only after taking $\det \Lambda$ does the product over $\dim Z$ dimensions collapse into the uniform exponent $\dim Z \cdot r' \cdot \mathrm{clearingBound}(D, \ell) \log \ell$.

## Level 2 — Undergrad Bridge

`src/New/Congruences/FirstOrder/DenominatorBound.lean` (37 declarations, 0 stub truncations) builds the **Ribet clearing operator** from four components:

1. **Error constants (`TraceRelations.lean` + `DenominatorBound.lean`):**
   - `localDefectBound D ℓ = 4 * D.f * defectBound D ℓ`
   - `denominatorError D ℓ = D.t * v_ℓ(oddClassBound D) + (8 * D.f * D.t + 2 + (4 * D.t + 1) * h₂²² + h₂) * v_ℓ(D.chosenGroupOrder)`
   - `liftValuationBound ℓ O ram sel = sel + 2 * O.rankDiff * ram + card r * localDefectBound D ℓ + denominatorError D ℓ`
   - `clearingBound D ℓ = localDefectBound D ℓ + (1 + 2 * D.t) * twistError ℓ D + denominatorError D ℓ + v_ℓ(D.chosenGroupOrder)`
   Note `add_denominatorError_le_mul_clearingBound`: whenever $1 \le r' \le D.r$, $t' \le D.t$, and $\mathrm{sel}, \mathrm{ram} \le e'_\ell$, we have $\mathrm{liftValuationBound} + v_\ell(N) \le r' \cdot \mathrm{clearingBound}(D, \ell)$.
2. **Trace ideal and common Ribet element (`exists_isCommonRibetElement_ellVal_le`):**
   Pairs `IsPrimeTriangle` with `IsTraceIdeal` (containing $\ell^\nu$ times all traces plus the tie relations linking tame generators `tame j` to Frobenii `φ j` via polynomials `e j` with `e j (1) = 1`), invoking `RibetElement.lean::ribet_columns` to bound $v_\ell(N^2 d) \le \mathrm{liftValuationBound}$.
3. **Separating non-weak lifts (`exists_heckePolyOnForms_eq_zero`, `typeProjection_sub_starProjection_mem_of_isOrthogonalToFalseLifts`):**
   Uses `exists_prod_sub_det_one_sub_eq_zero` to kill non-weak lifts outside `𝓧` and uses false-lift orthogonality (`O.IsOrthogonalToFalseLifts E Zᗮ y`) to ensure `typeProjection O c (y - Z.starProjection y)` lies strictly outside weak lifts, so `exists_ellVal_le_and_diagOp_mem_ellIntegralForms` yields a diagonal operator `B.diagOp s` keeping `N² • B.diagOp s y` integral.
4. **Assembly and `DenominatorLe` (`exists_linearMap_eq_smul_mem_integralForms` $\to$ `exists_operator_ellVal_le_liftValuationBound` $\to$ `exists_operator_det_clearingBound` $\to$ `denominatorLe_clearingBound`):**
   Builds one operator `G` per lift generator `v₀`, takes a basis of `Z` in `exists_operator_ellVal_le_liftValuationBound`, scales by $N = D.\mathrm{chosenGroupOrder}$ via `exp_le_norm_det_smul_of_ellVal_le` to clear pairing denominators in `exists_operator_det_clearingBound`, and packages the result as `DenominatorLe ι.ιℓ O E Z (clearingBound D ℓ * log ℓ)`.

## Level 3 — Formal Lean Contract

All six anchors extract completely untruncated in `skeleton/07_congruences/denominator_bound.lean` (37 declarations, 0 stub truncations):

- `07_congruences/denominator_bound.lean#New.Selmer.IsLiftGenerator` (upstream `DenominatorBound.lean:744-764`).
- `07_congruences/denominator_bound.lean#New.Selmer.liftValuationBound` (upstream `DenominatorBound.lean:179-186`).
- `07_congruences/denominator_bound.lean#New.Selmer.exists_isCommonRibetElement_ellVal_le` (upstream `DenominatorBound.lean:468-521`).
- `07_congruences/denominator_bound.lean#New.Selmer.typeProjection_sub_starProjection_mem_of_isOrthogonalToFalseLifts` (upstream `DenominatorBound.lean:909-940`).
- `07_congruences/denominator_bound.lean#New.Selmer.exists_operator_ellVal_le_liftValuationBound` (upstream `DenominatorBound.lean:1116-1181`).
- `07_congruences/denominator_bound.lean#New.Selmer.denominatorLe_clearingBound` (upstream `DenominatorBound.lean:1269-1293`):
  ```lean
  theorem New.Selmer.denominatorLe_clearingBound {P : Parameters} {D : Data P} {ℓ : ℕ} [ThePrime ℓ]
      (ι : PadicIso ℓ) (ϑ : D.Twist)
      (hϑ : IsAuxTwist (badPlaces D ℓ) ϑ.Q ϑ.invAvatar (twistError ℓ D))
      {r n : Type} [Fintype r] [DecidableEq r] [Fintype n] [DecidableEq n] {O : DescentPair D.E r n}
      {E : MonoidAlgebra ℂ (O.ChosenGL r)} {Z : Submodule ℂ O.FormV}
      (h_span : Z ≤ span ℂ {y : O.FormV | IsLiftGenerator ϑ O E Z (twistError ℓ D) y}) :
      DenominatorLe ι.ιℓ O E Z (clearingBound D ℓ * log ℓ)
  ```

See the page's live `<VerificationBanner />` (`audit/verification-status.json`) for the pinned upstream commit and local Lean build status (`build_status: unverified-locally`).
