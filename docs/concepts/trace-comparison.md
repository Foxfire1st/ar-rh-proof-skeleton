---
slug: trace-comparison
term: trace comparison
aliases: ["multiplicity equality", "Jacquet–Langlands transfer", "Jacquet-Langlands transfer", "inner form", "formal degree"]
inline_glimpse: "Matching traces of spherical Hecke operators between an indefinite unitary group and its definite inner form via a cuspidal local test function—proving equal eigensystem multiplicities, Rallis determinant equality, and the target-dimension bound."
visual_ref: assets/stage04-second-moment-spreading-funnel.svg
where_analogy_breaks: "Picturing trace comparison as swapping two identical finite matrices with different labels hides why an arbitrary modular form on the indefinite space Y_0 cannot be compared directly to one on the definite space W_m: the trace formula only simplifies to equal traces once the test function is strictly cuspidal at a split finite place (enforced by a primitive polynomial label in `goodTupleFamilies`) and the archimedean Harish-Chandra parameter has a large gap `D.gap` ensuring discrete-series isolation across both signatures."
sources:
  - title: "Conditional base change for unitary groups"
    citation: "M. Harris & J.-P. Labesse, Asian J. Math. 8 (2004), 653–684."
    kind: paper
  - title: "Changement de base CM et séries discrètes"
    citation: "J.-P. Labesse, in On the Stabilization of the Trace Formula (2011), Thms. 5.4–5.5."
    kind: paper
  - title: "Simple Algebras, Base Change, and the Advanced Theory of the Trace Formula"
    citation: "J. Arthur & L. Clozel, Ann. of Math. Stud. 120, Princeton Univ. Press (1989)."
    kind: book
lean_anchors:
  - 04_spanning/equal_multiplicities.lean#New.Data.trace_heckeOnGood_eq
  - 04_spanning/equal_multiplicities.lean#New.Data.finrank_goodForms_inf_eigenPart_eq
  - 04_spanning/equal_multiplicities.lean#New.Data.targetDim_eq_finrank_goodForms_spaceW
  - 04_spanning/spanning_theorem.lean#New.Data.targetDim_le_card_mul_besselDegree_mul_mass
child_concepts:
  - complex-plane
  - hecke-operator
  - central-idempotent
  - theta-correspondence
  - hilbert-schmidt-norm
---

# Trace Comparison

## Level 1 — First-Principles

Two square matrices over {{concept:complex-plane|the two-dimensional plane of complex coordinates $\mathbb{C}$}} have the same eigenvalues with the same multiplicities whenever every power of the first matrix has the same trace (sum of diagonal entries) as the corresponding power of the second. When we cannot write down a basis for a hard space $V$ directly—and cannot even count its dimension $\dim V$—we can still measure $V$ completely if we find an easier twin space $V'$ where every commuting symmetry operator has the exact same trace. That transfer of spectral data by equating traces is **trace comparison** (the unitary-group counterpart of classical **Jacquet–Langlands transfer** between two **inner forms**—groups that look identical at every finite prime and differ only in their real-place signature).

In Stage 04, the target space of Theorem B consists of good automorphic forms on the *indefinite* unitary space $Y_0$, where counting the target dimension $d_T = D.\mathrm{targetDim}$ directly is blocked by non-compactness. Meanwhile, the definite spaces $W_1, \dots, W_m$ (which share the exact same finite-adelic group, congruence level, and weight parameter as $Y_0$, differing only in their signs at the infinite places) are compact at infinity: on the definite space $W_m$, the principal congruence level acts freely, so forms of principal level are finite tables of vectors whose total dimension is an exact product $\#\bar{G} \cdot \mathrm{deg} \cdot \mathrm{mass}_r$ of the finite group size, the **formal degree** of the weight representation, and the lattice mass.

Stage 04 bridges $Y_0$ and $W_m$ in four clean moves:

1. **Isolate cuspidal local labels:** By projecting onto good forms with a {{concept:central-idempotent|orthogonal projection built from finite-field polynomial labels including at least one primitive polynomial}}, every component test function becomes strictly cuspidal at a split finite place above the level. Cuspidality kills all non-elliptic orbital integrals in the trace formula.
2. **Equate spherical Hecke traces (`trace_heckeOnGood_eq`):** For any spherical {{concept:hecke-operator|averaging operator built from finite matrix cosets away from the level}} $T$, the trace of $T$ on the good forms of $Y_0$ equals the trace of $T$ on the good forms of any companion space $W_j$.
3. **Deduce characteristic polynomials, determinants, and multiplicity equality:** Because the spherical Hecke operators form a commuting algebra, equal traces on all powers of $T$ force equal characteristic polynomials (`charpoly_heckeOnGood_eq`) and equal determinants (`det_heckeOnGood_eq`). Taking $T = 1$ gives `targetDim_eq_finrank_goodForms_spaceW` ($d_T = \dim(\mathrm{goodForms}\ W_j)$), while taking $T$ to isolate a single eigensystem $q$ gives **multiplicity equality** (`finrank_goodForms_inf_eigenPart_eq`).
4. **Serve two pillars at once:** Bounding the good forms on $W_m$ by all principal-level forms on $W_m$ gives $d_T \le \#\bar{G}\cdot \mathrm{deg}\cdot \mathrm{mass}_r$ (`targetDim_le_card_mul_besselDegree_mul_mass`), which plugs into the {{concept:hilbert-schmidt-norm|Welch-type second-moment spreading bound}} for Theorem B. Simultaneously, realizing the Rallis inner-product series of a {{concept:theta-correspondence|unitary dual-pair lift between automorphic forms}} as a spherical Hecke operator gives `det_rallisOnGood_eq`, which feeds Stage 05's determinant factorization (`05_determinant/norm_of_chain`) for Theorem C.

### Limits of the intuition

Picturing **trace comparison** as checking that two finite matrices have matching traces of powers captures Newton's identities (`charpoly_heckeOnGood_eq`) and **multiplicity equality**, but omits the analytic price paid to equate traces across **inner forms**. Two unitary groups with different real signatures do *not* have identical traces on all automorphic forms: non-discrete spectrum and endoscopic terms would spoil the comparison. Equality holds in `EqualMultiplicities.lean` only after imposing two strict filters: the archimedean Harish-Chandra parameter `D.paramW` must have a large gap `D.gap` for both signatures (isolating discrete-series packets), and the finite-place component must lie in `D.goodTupleFamilies`, guaranteeing at least one primitive polynomial label whose local idempotent produces a cuspidal test function (`trace_eq_of_isCuspidalAt`).

## Level 2 — Undergrad Bridge

The trace comparison for inner forms of unitary groups over a CM field $E$ descends from the simple trace formula of Arthur–Clozel (1989) and its unitary discrete-series specialization by Harris–Labesse (2004) and Labesse (2011, Thms. 5.4–5.5). Let $V$ and $V'$ be two Hermitian spaces of the same rank $r = |D.\mathrm{RowIndex}|$ over $E$ with sign tuples $s, s'$ at the infinite places such that `toHCParam s (D.paramW v)` and `toHCParam s' (D.paramW v)` both satisfy `HasGap D.gap`.

### 1. From primitive labels to equal traces (`EqualMultiplicities.lean`)

1. **Primitive polynomial witness (`exists_isPrimitivePolynomial`):** Every good label family $c \in D.\mathrm{goodTupleFamilies}$ attaches a monic allowed degree-$r$ polynomial $c_x \in \mathbb{F}_{w_x}[X]$ to each chosen place $x$ above $p_0, p_1$, with at least one $c_x$ primitive (irreducible, with a root generating $\mathbb{F}_{q_x^r}^\times$).
2. **Decomposition into label idempotents (`trace_heckeOnGood_eq_sum`):** Because $e_0 = \sum_{c \in D.\mathrm{goodTupleFamilies}} e_c$ projects onto `D.goodForms V`, for every spherical Hecke operator $T \in D.\mathrm{heckeAlgebra}$ away from $M = p_0 p_1$,
   $$\operatorname{tr}_{\mathbb{C}}\bigl(\mathrm{heckeOnGood}\ V\ T\bigr) = \sum_{c \in D.\mathrm{goodTupleFamilies}} \operatorname{tr}_{\mathbb{C}}\bigl(T_V \cdot \rho_V(e_c)\bigr).$$
3. **Simple trace comparison (`trace_mul_tupleIdem_eq` $\Rightarrow$ `trace_heckeOnGood_eq`):** At the place $x$ where $c_x$ is primitive, the local matrix coefficient attached to $e_{c_x}$ is supercuspidal (`Lib.Adelic.TraceFormula.CuspidalType`), so the test function for $T e_c$ is cuspidal at one split place. Invoking `New.trace_eq_of_isCuspidalAt` equates each summand on $V$ and $V'$, yielding
   $$\operatorname{tr}_{\mathbb{C}}\bigl(\mathrm{heckeOnGood}\ V\ T\bigr) = \operatorname{tr}_{\mathbb{C}}\bigl(\mathrm{heckeOnGood}\ V'\ T\bigr).$$

### 2. Spectral consequences and the Rallis operator

- **Characteristic polynomials and determinants:** Applying `trace_heckeOnGood_eq` to all powers $T^k$ ($k \ge 1$) and invoking Newton's identities (`TauCeti.LinearAlgebra.Matrix.Charpoly.Newton`) yields `(D.heckeOnGood V T).charpoly = (D.heckeOnGood V' T).charpoly` (`charpoly_heckeOnGood_eq`) and equal determinants (`det_heckeOnGood_eq`).
- **Multiplicity equality (`finrank_goodForms_inf_eigenPart_eq`):** Because `D.heckeAlgebra` acts semisimply with finitely many joint eigensystems $q$, choosing $T \in D.\mathrm{heckeAlgebra}$ equal to $1$ on `eigenPart V q` and $0$ on all other eigensystems (`trace_heckeOnGood_eq_finrank`) shows
  $$\dim_{\mathbb{C}}\bigl(D.\mathrm{goodForms}\ V \cap D.\mathrm{eigenPart}\ V\ q\bigr) = \dim_{\mathbb{C}}\bigl(D.\mathrm{goodForms}\ V' \cap D.\mathrm{eigenPart}\ V'\ q\bigr).$$
- **Target dimension (`targetDim_eq_finrank_goodForms_spaceW` + `targetDim_le_card_mul_besselDegree_mul_mass`):** Setting $T = 1$ with $V = Y_0$ and $V' = W_j$ equates $d_T = D.\mathrm{targetDim} = \dim_{\mathbb{C}}(D.\mathrm{goodForms}\ W_j)$. On the definite space $W_m$, the principal congruence level `D.chosenLevel` has trivial class stabilizers (`classAut = ⊥`), index $\#\bar{G}$ in the maximal compact level (whose class sum is `mass D.E D.r`), and local weight dimension equal to the product of compact **formal degrees** `D.filling.besselDegree` (via Weyl's dimension polynomial). Hence
  $$d_T = \dim(D.\mathrm{goodForms}\ W_m) \le \dim(D.\mathrm{principalForms}\ W_m) = \#\bar{G} \cdot \mathrm{deg} \cdot \mathrm{mass}_r.$$
- **Cross-pillar bridge to Stage 05 (`det_rallisOnGood_eq`):** Each term `D.rallisTerm ϑ j W z y i` of Rallis' inner-product formula is realized as a matrix coefficient $\langle z, p_i y \rangle$ of a single spherical Hecke operator $p_i$ independent of $W$ (`exists_rallisTerm_eq_inner`), and summing the series produces one operator $p \in D.\mathrm{heckeAlgebra}$ acting as `D.rallisOperator ϑ j` on both $V$ and $V'$ (`exists_hecke_eq_rallisOperator`). Restricting to good forms (`D.rallisOnGood`) therefore yields equal determinants `(D.rallisOnGood V ϑ j).det = (D.rallisOnGood V' ϑ j).det`, used directly in Stage 05 (`05_determinant/norm_of_chain`).

## Level 3 — Formal Lean Contract

Stage 04 extracts the trace comparison into `skeleton/04_spanning/equal_multiplicities.lean` and its target-dimension consequence into `skeleton/04_spanning/spanning_theorem.lean`:

- `04_spanning/equal_multiplicities.lean#New.Data.trace_heckeOnGood_eq` (upstream `src/New/Spanning/Multiplicities/EqualMultiplicities.lean:273-280`):
  ```lean
  theorem New.Data.trace_heckeOnGood_eq [hs : ∀ v : D.Φ, (toHCParam s (D.paramW v)).HasGap D.gap]
      [hs' : ∀ v : D.Φ, (toHCParam s' (D.paramW v)).HasGap D.gap] (T : D.heckeAlgebra) :
      trace ℂ (D.goodForms V) (D.heckeOnGood V T) =
        trace ℂ (D.goodForms V') (D.heckeOnGood V' T)
  ```
- `04_spanning/equal_multiplicities.lean#New.Data.finrank_goodForms_inf_eigenPart_eq` (upstream `src/New/Spanning/Multiplicities/EqualMultiplicities.lean:376-401`):
  ```lean
  theorem New.Data.finrank_goodForms_inf_eigenPart_eq
      [hs : ∀ v : D.Φ, (toHCParam s (D.paramW v)).HasGap D.gap]
      [hs' : ∀ v : D.Φ, (toHCParam s' (D.paramW v)).HasGap D.gap] (q : FinPlace D.E × ℕ → ℂ) :
      finrank ℂ (D.goodForms V ⊓ D.eigenPart V q : Submodule ℂ (D.principalForms V)) =
        finrank ℂ (D.goodForms V' ⊓ D.eigenPart V' q : Submodule ℂ (D.principalForms V'))
  ```
- `04_spanning/equal_multiplicities.lean#New.Data.targetDim_eq_finrank_goodForms_spaceW` (upstream `src/New/Spanning/Multiplicities/EqualMultiplicities.lean:411-415`):
  ```lean
  theorem New.Data.targetDim_eq_finrank_goodForms_spaceW (j : D.LiftIndex) :
      D.targetDim = finrank ℂ (D.goodForms (D.spaceW j))
  ```
- `04_spanning/spanning_theorem.lean#New.Data.targetDim_le_card_mul_besselDegree_mul_mass` (upstream `src/New/Spanning/SecondMoment/SpanningTheorem.lean:91-102`):
  ```lean
  theorem New.Data.targetDim_le_card_mul_besselDegree_mul_mass :
      D.targetDim ≤ Nat.card D.chosenGL * D.filling.besselDegree * mass D.E D.r
  ```

See the page's live `<VerificationBanner />` (`audit/verification-status.json`) for the pinned upstream commit and local Lean build status (`build_status: unverified-locally`).
