---
slug: rallis-inner-product
term: Rallis inner product formula
aliases: ["Rallis inner product", "Rallis formula", "Rallis product", "lift determinant", "triangle of lifts"]
inline_glimpse: "Relating the squared norm (and Gram determinant) of a theta lift or m-step chain of lifts to spherical Hecke operators and single-lift determinants—factoring Z(theta)/Z(1) across m lifts up to quotient L-value ratios."
visual_ref: assets/stage05-triangle-and-two-grid-turns.svg
where_analogy_breaks: "Thinking of the Rallis inner product formula as a single closed-form scalar identity for any theta lift hides two crucial subtleties in Stage 05: first, the m-step chain Y_0 -> Y_m leaves the convergent Rallis range after the first step, so NormOfChain must slide lifts past descents across a triangle of spaces using the theta square law; second, the intermediate predicate HasAbsUpToNormPower allows C = 0, so strict positivity (C > 0, chainDeterminant_pos, liftDeterminant_pos) must be proved separately via injectivity of the first lifts."
sources:
  - title: "Injectivity properties of liftings associated to Weil representations"
    citation: "S. Rallis, Compositio Math. 52 (1984), 139–169."
    kind: paper
  - title: "Non-vanishing theorems for the cohomology of certain arithmetic quotients"
    citation: "J.-S. Li, J. Reine Angew. Math. 428 (1992), 177–217."
    kind: paper
  - title: "The regularized Siegel–Weil formula (the second term identity) and the Rallis inner product formula"
    citation: "W. T. Gan, Y. Qiu & S. Takeda, Invent. Math. 198 (2014), 739–831."
    kind: paper
lean_anchors:
  - 05_determinant/norm_of_chain.lean#New.Data.exists_normDet_goodLift_sq_eq
  - 05_determinant/norm_of_chain.lean#New.Data.exists_adjoint_chain_comp_eq_smul_rallisProduct
  - 05_determinant/norm_of_chain.lean#New.Data.exists_chainDeterminant_pos_and_div_chainDeterminant_one_eq
  - 05_determinant/norm_of_chain.lean#New.Data.liftDeterminant_pos
  - 05_determinant/norm_of_chain.lean#New.Data.chainDeterminant_pos
child_concepts:
  - gram-determinant
  - theta-correspondence
  - hecke-operator
  - trace-comparison
  - theta-square-law
  - hecke-l-value-ratio
---

# Rallis Inner Product Formula

## Level 1 — First-Principles

Given a linear map $u : U \to V$ between finite-dimensional inner-product spaces, how much does $u$ stretch volumes? Its squared volume-stretching factor is the {{concept:gram-determinant|squared parallelotope volume computed as $\det(u^* u)$}}. If $u$ is injective, $\det(u^* u) > 0$; if $u$ collapses any nonzero vector to zero, $\det(u^* u) = 0$. In automorphic forms, when $u$ is a {{concept:theta-correspondence|unitary dual-pair theta lift}}, the **Rallis inner product formula** expresses the inner product $\langle u(x), u(y) \rangle$ on the target space directly back on the source space as $\langle x, R(y) \rangle$, where $R$ is an explicit convergent series acting as a spherical {{concept:hecke-operator|averaging operator away from the level}}.

In Stage 05, the target space of Theorem C consists of good forms on the indefinite space $Y_0$, and the chain map $\Theta_\vartheta : Y_0 \to Y_m$ applies $m$ theta lifts in sequence for a twist $\vartheta$. Measuring the volume $\det(\Theta_\vartheta^* \Theta_\vartheta)$ directly in one jump fails because the chain quickly leaves the convergent range of the classical **Rallis formula**. Instead, Stage 05 splits the problem into two coordinated halves:

1. **Sliding across a triangle of lifts:** Place the intermediate spaces into a **triangle of lifts** indexed by intervals $(i, n)$. Repeatedly applying the {{concept:theta-square-law|commuting theta square}} slides lifts past descents until the Gram operator $\Theta_\vartheta^* \Theta_\vartheta$ collapses onto a scalar multiple of the **Rallis product** $R_\vartheta = R_1 \cdots R_m$ of $m$ first-step Rallis operators on $Y_0$, with scalar norm controlled by the {{concept:hecke-l-value-ratio|pairwise quotient ratios of Hecke L-values}} for $1 \le i < j \le m$.
2. **Transferring determinants across signatures:** Taking determinants turns $\Theta_\vartheta^* \Theta_\vartheta = c(\vartheta) c_Q^m R_\vartheta$ into a product of $\det(R_j|_{Y_0})$. By Stage 04's {{concept:trace-comparison|trace comparison between inner forms of different real signatures}} (`det_rallisOnGood_eq`), each $\det(R_j|_{Y_0})$ equals $\det(R_j|_{W_j})$ on the *definite* companion space $W_j$—which by the single-step **Rallis inner product** (`exists_normDet_goodLift_sq_eq`) is proportional to the **lift determinant** $Z_j(\vartheta^j) = \det(u_j^* u_j)$ of the single good lift $u_j$ from $W_j$ to $V_j$.

Dividing the twisted chain volume $Z(\vartheta)$ by the untwisted chain volume $Z(1)$ cancels all unknown base constants $C$ and $A$, leaving the exact factorization (`exists_chainDeterminant_pos_and_div_chainDeterminant_one_eq`):
$$\frac{Z(\vartheta)}{Z(1)} = \Bigl(\mathrm{Nm}(Q)^{e/2} \prod_{1 \le i < j \le m} \mathrm{quotientRatio}_{i,j}(\vartheta)\Bigr)^{d} \prod_{j=1}^{m} \frac{Z_j(\vartheta^j)}{Z_j(1)}.$$
That factorization reduces the $m$-step chain volume needed for Theorem C directly to $m$ single-lift volume ratios bounded in Stage 06.

### Limits of the intuition

Treating the **Rallis inner product formula** as an automatic positivity certificate hides a delicate logical split in `NormOfChain.lean`. The commutation calculus across the **triangle of lifts** yields `HasAbsUpToNormPower`, which states only $\|c(\vartheta)\|^2 = C \cdot \mathrm{Nm}(Q)^k (\prod \mathrm{quotientRatio})^2$ with a real constant $C$ that could *a priori* be $0$ (inheriting the uncomputed constant $\kappa$ from `OneSquare.lean`). Strict positivity ($C > 0$, `liftDeterminant_pos`, and `chainDeterminant_pos`) does not come from the square law—it is proved separately from the injectivity of the lifts (`lowerChainAtLevel_injective` in `ChainNorms.lean`) in the convergent range of the **Rallis inner product formula**. Notice also a minor naming convention: upstream docstrings call `u.normDet ^ 2 = det(u* u)` the *"volume"* of $u$, while Lean names the definitions `liftDeterminant` and `chainDeterminant`.

## Level 2 — Undergrad Bridge

In `src/New/Determinant/Chain/NormOfChain.lean` (backed by `ChainNorms.lean`, `TriangleLifts.lean`, and `TriangleNorms.lean`), the determinant factorization unfolds in five clean algebraic lemmas:

1. **Linear-algebra volume rule (`New.normDet_sq_eq_pow_mul_det`):**
   If $u : U \to V$ satisfies $\langle u x, u y \rangle = a \langle x, S y \rangle$ for $S \in \mathrm{End}_{\mathbb{C}}(U)$, then $u^* u = a S$ and hence $u.\mathrm{normDet}^2 = \det(u^* u) = a^{\dim U} \det(S)$.
2. **Single-lift Rallis determinant transferred from $W_j$ to $Y_0$ (`exists_normDet_goodLift_sq_eq`, `normDet_goodLift_sq_pos`, `liftDeterminant_pos`):**
   For each $1 \le j \le m$, `D.goodLift ϑ j` maps `D.goodForms (D.spaceW j)` to `(D.descentPair ϑ j).FormV`. Note the argument convention: `D.goodLift ϑ j` takes a `D.TameChar`, whereas `D.liftDeterminant ϑ j` takes a `D.MFoldTwist` and evaluates the $j$-th lift at $\vartheta^j$. Applying the single-step Rallis formula on $W_j$ and Stage 04's `D.det_rallisOnGood_eq` to replace `(D.rallisOnGood (D.spaceW j) ϑ j).det` by `(D.rallisOnGood (D.spaceY 0) ϑ j).det` gives a nonzero constant $A$ independent of $\vartheta$ such that
   $$\bigl((D.\mathrm{goodLift}\ \vartheta\ j).\mathrm{normDet}\bigr)^2 = \bigl(A \cdot c_Q\bigr)^{d} \cdot \det\bigl(D.\mathrm{rallisOnGood}\ Y_0\ \vartheta\ j\bigr),$$
   with $c_Q = (D.\mathrm{descentPair}\ \vartheta\ j).\mathrm{twistFactor}$ and $d = D.\mathrm{targetDim}$. Injectivity gives `liftDeterminant_pos : 0 < D.liftDeterminant ϑ j`.
3. **Triangle Gram identity (`exists_inner_weightedChain_eq`, `exists_inner_chain_eq`, `exists_adjoint_chain_comp_eq_smul_rallisProduct`):**
   Using the two square commutation relations in `ChainNorms.lean` (`exists_isTriangle`), the Gram form of the weighted chain across the triangle evaluates to a multiple of `D.rallisProduct ϑ` (the ordered product $\prod_{j=1}^m D.\mathrm{rallisOnGood}\ Y_0\ (\vartheta^j)\ j$). Rescaling from the weighted chain to `D.Θ ϑ` gives `exists_inner_chain_eq`, and combining with `chainDeterminant_pos` rules out $c(1) = 0$ so `HasAbsUpToNormPower.exists_norm_div_eq_mul_prod_quotientRatio` upgrades the scalar to a strictly positive constant $C > 0$ and integer $e$:
   $$(D.\Theta\ \vartheta)^* \circ (D.\Theta\ \vartheta) = \bigl(c(\vartheta)\, c_Q^m\bigr) \cdot D.\mathrm{rallisProduct}\ \vartheta, \qquad \|c(\vartheta)\| = C \cdot \mathrm{Nm}(Q)^{e/2} \prod_{1 \le i < j \le m} \mathrm{quotientRatio}_{i,j}(\vartheta).$$
4. **Determinant factorization (`exists_chainDeterminant_eq_pow_mul_prod_liftDeterminant`, `exists_div_chainDeterminant_one_eq`, `exists_chainDeterminant_pos_and_div_chainDeterminant_one_eq`):**
   Taking determinants in `exists_adjoint_chain_comp_eq_smul_rallisProduct` and substituting `exists_normDet_goodLift_sq_eq` cancels the $c_Q^m$ twist factors identically, leaving $C^d$ times the quotient-ratio power times $\prod_{j=1}^m D.\mathrm{liftDeterminant}\ \vartheta\ j$. Dividing by $\vartheta = 1$ (where `prod_quotientRatio_one` gives $1$) cancels $C^d$ as well.
5. **Height bound on the quotient-ratio scalar (`HasSmallRatios.heightLe_inv_rpow_mul_prod_quotientRatio`):**
   Each of the $\binom{m}{2} \le m^2$ pairs $(i, j)$ is a ramified ratio (`isRamifiedRatio_of_mem_sigma`) of $t$ critical $L$-value ratios, so under `HasSmallRatios θ` the inverse scalar has height at most $m^2 t x_1$ (`LongForms.lean` upgrades this to $\le b$ via `D.le_of_hasLogBound`).

## Level 3 — Formal Lean Contract

Stage 05 extracts the determinant factorization into `skeleton/05_determinant/norm_of_chain.lean` (24 declarations; 1 intermediate lemma `exists_inner_weightedChain_eq` at L86 truncates at `let u :=`, quoted verbatim in [Chain Determinant Factorization and Two-Grid Auxiliary Forms](../stages/05_determinant/chain-determinant-and-auxiliary-forms.md); all headline theorems below extract untruncated):

- `05_determinant/norm_of_chain.lean#New.Data.exists_normDet_goodLift_sq_eq` (upstream `NormOfChain.lean:118-141`):
  ```lean
  lemma New.Data.exists_normDet_goodLift_sq_eq (j : D.LiftIndex) :
      ∃ A : ℂ, A ≠ 0 ∧ ∀ ϑ : D.TameChar,
        ((D.goodLift ϑ j).normDet : ℂ) ^ 2 =
          (A * (D.descentPair ϑ j).twistFactor) ^ D.targetDim *
            (D.rallisOnGood (D.spaceY 0) ϑ j).det
  ```
- `05_determinant/norm_of_chain.lean#New.Data.exists_adjoint_chain_comp_eq_smul_rallisProduct` (upstream `NormOfChain.lean:295-346`):
  ```lean
  theorem New.Data.exists_adjoint_chain_comp_eq_smul_rallisProduct (D : Data P) :
      ∃ (C : ℝ) (e : ℤ) (c : D.MFoldTwist → ℂ),
        0 < C ∧
        (∀ ϑ : D.MFoldTwist, ‖c ϑ‖ = C * ((ϑ.normQ : ℝ) ^ (e / 2 : ℝ) *
          ∏ i ∈ Ioc 0 D.m, ∏ j ∈ Ioc i D.m, D.quotientRatio i j ϑ)) ∧
        ∀ ϑ : D.MFoldTwist, (D.Θ ϑ).adjoint ∘ₗ D.Θ ϑ =
          (c ϑ * (D.descentPair ϑ 1).twistFactor ^ D.m) • D.rallisProduct ϑ
  ```
- `05_determinant/norm_of_chain.lean#New.Data.exists_chainDeterminant_pos_and_div_chainDeterminant_one_eq` (upstream `NormOfChain.lean:418-435`):
  ```lean
  theorem New.Data.exists_chainDeterminant_pos_and_div_chainDeterminant_one_eq :
      ∃ e : ℤ, ∀ ϑ : D.MFoldTwist,
        0 < D.chainDeterminant ϑ ∧
        (∀ j : D.LiftIndex, 0 < D.liftDeterminant ϑ j) ∧
        D.chainDeterminant ϑ / D.chainDeterminant 1 =
          (((ϑ.normQ : ℝ) ^ (e / 2 : ℝ) *
              ∏ i ∈ Ioc 0 D.m, ∏ j ∈ Ioc i D.m, D.quotientRatio i j ϑ : ℝ) : ℂ) ^ D.targetDim *
            ∏ j ∈ Icc 1 D.m, D.liftDeterminant ϑ j / D.liftDeterminant 1 j
  ```
- Also anchored: `05_determinant/norm_of_chain.lean#New.Data.liftDeterminant_pos` (upstream `NormOfChain.lean:88-92`) and `05_determinant/norm_of_chain.lean#New.Data.chainDeterminant_pos` (upstream `NormOfChain.lean:110-116`).

See the page's live `<VerificationBanner />` (`audit/verification-status.json`) for the pinned upstream commit and local Lean build status (`build_status: unverified-locally`).
