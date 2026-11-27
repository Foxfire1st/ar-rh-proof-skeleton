---
slug: divided-lattice-gluing
term: divided-lattice gluing
aliases: ["divided lattice", "first factor", "period lift", "glued form"]
inline_glimpse: "Splitting the definite target space V_j = V'' + V^o along a congruence-glued lattice to pair a twist-carrying first factor on V'' with arbitrary forms on V^o, producing a linear map glue_theta whose adjoint isolates the twist as a single nonzero scalar in c'_theta * glue_theta^* * Theta_{j,theta} = Theta^o."
visual_ref: assets/stage06-three-exchanges-and-gluing.svg
where_analogy_breaks: "Imagining divided-lattice gluing as a plain tensor product of two independent spaces misses why the lattice L_Lambda must sit strictly between varpi O^n and O^n at the chosen places: without congruence gluing across V'' and V^o, the theta kernel would not decouple cleanly modulo varpi^2, and scaling by M^{l_1} is required to repay the denominators of the gluing matrix while keeping the map integral away from Q."
sources:
  - title: "Integrality of a ratio of Petersson norms and level-lowering congruences"
    citation: "K. Prasanna, Ann. of Math. 163 (2006), 901–941."
    kind: paper
  - title: "Yoshida lifts and Selmer groups"
    citation: "S. Böcherer, N. Dummigan & R. Schulze-Pillot, J. Math. Soc. Japan 64 (2012), 1353–1401."
    kind: paper
  - title: "The Rallis inner product formula and p-adic L-functions"
    citation: "M. Harris, J.-S. Li & C. M. Skinner, Automorphic Representations, L-functions and Applications, OSU Math. Res. Inst. Publ. 11 (2005), 225–255."
    kind: paper
lean_anchors:
  - 06_one_lift/gluing_maps.lean#New.Rows.glue
  - 06_one_lift/gluing_maps.lean#New.Rows.firstFactor
  - 06_one_lift/gluing_maps.lean#New.Rows.periodLift
  - 06_one_lift/gluing_maps.lean#New.Rows.twistFactor_smul_adjoint_glue_comp
  - 06_one_lift/gluing_maps.lean#New.Rows.periodLift_injective
  - 06_one_lift/gluing_maps.lean#New.Rows.twistFactor_ne_zero
child_concepts:
  - seesaw-duality
  - weyl-gauge
  - carrier-form
  - false-lift-period
---

# Divided-Lattice Gluing

## Level 1 — First-Principles

Suppose we have a single-step theta lift $\Theta_{j,\vartheta}$ mapping good forms on $W_j$ (rank $r$) into forms on a larger definite space $V_j$ (rank $r + t = 6r$). We want a linear map that builds **glued forms** on $V_j$ from a fixed twist-independent space of inputs, preserves integrality away from the twist modulus $Q$, stays small when $\vartheta = 1$, and pushes all twist dependence out into one scalar when paired back against $\Theta_{j,\vartheta}$.

**Divided-lattice gluing** achieves this by cutting the target space into two complementary orthogonal blocks:
$$V_j = V'' \oplus V^\circ, \qquad \operatorname{rk}(V'') = r + 9m + 1, \quad \operatorname{rk}(V^\circ) = t - 9m - 1.$$
Rather than taking the naive direct sum of the standard lattices on $V''$ and $V^\circ$, each choice index $i$ picks a **divided lattice** $L_\Lambda = \mathsf{g}\mathcal{O}^{r+t}$ (`glueLattice`) lying between $\varpi \mathcal{O}^{r+t}$ and $\mathcal{O}^{r+t}$ at the chosen places, glued across the two blocks by a congruence matrix $\Lambda$ modulo $\varpi^2$ (`isGlued_glueLattice`).

On the first block $V''$, Stage 06 takes the theta-series output of {{concept:carrier-form|three-exchange reduction}} and multiplies it by one twist-independent nonzero scalar $c''$ (`firstFactorScale`) to form the **first factor** $\varphi''_{\vartheta, i} = \mathrm{firstFactor}\ j\ \vartheta\ i$. Its values are integral in {{concept:weyl-gauge|the prime-by-prime Weyl representation norm}} away from $Q$, and for $\vartheta = 1$ their conjugates have squared norm at most $\exp(f m b \log^2 b)$.

Given a tuple $\zeta \in \mathrm{FormTuples}\ j$ of forms on the smaller complementary block $V^\circ$ (one copy per choice $i$), the gluing map $\mathrm{glue}_\vartheta(\zeta)$ forms the Cartan product $\varphi''_{\vartheta,i}(h)\cdot \zeta_i(h^\circ)$ on the class of $(h, h^\circ)\mathsf{g}$, sums over classes, and multiplies by $M^{l_1}$ (`D.M ^ denominatorExponent`) to repay the denominators of $\mathsf{g}$. That single linear map delivers four decisive guarantees:
1. **Twist-isolating seesaw identity (`twistFactor_smul_adjoint_glue_comp`):** Applying {{concept:seesaw-duality|dual-pair adjoint transfer}} to $\mathrm{glue}_\vartheta^* \circ \Theta_{j,\vartheta}$ yields
   $$c'_\vartheta \cdot \mathrm{glue}_\vartheta^* \circ \Theta_{j,\vartheta} = \Theta^\circ,$$
   where $\Theta^\circ = \mathrm{periodLift}\ j$ is the **period lift** into $\mathrm{FormTuples}\ j$—completely independent of the twist $\vartheta$—and $\|c'_\vartheta\|^2 = C / \Gamma_j(\vartheta)$.
2. **Injectivity and non-vanishing (`periodLift_injective`, `twistFactor_ne_zero`):** Because products of good forms with the twist-free choice functions span (`ProductsSpan.lean`), the **period lift** $\Theta^\circ$ is injective. Combining injectivity with Stage 04's guarantee that the target space is nonzero (`D.finrank_targetForms_ne_zero`) proves $c'_\vartheta \neq 0$, retroactively certifying $C \neq 0$ from `ThreeExchanges`!
3. **Integrality and smallness (`glue_mem_integralForms`, `normSqAt_glue_le`):** At any prime $\ell \nmid \mathrm{Nm}(Q)$, $\mathrm{glue}_\vartheta$ maps integral tuples on $V^\circ$ into integral forms on $V_j$; at $\vartheta = 1$, it inflates squared norms by at most $\exp(2 f m b \log^2 b)$.
4. **Orthogonality to false lifts (`isOrthogonalToFalseLifts_glue_periodLift`):** Because {{concept:false-lift-period|false-lift periods over the complementary block $V^\circ$ vanish}}, every glued form $\mathrm{glue}_\vartheta(\Theta^\circ y)$ is orthogonal to all false lifts on $V_j$.

### Limits of the intuition

Calling **divided-lattice gluing** a "block-diagonal product map" glosses over why the congruence matrix $\mathsf{g}$ is indispensable. On an unglued direct sum $V'' \oplus V^\circ$, the local level data at the chosen places $\varpi$ cannot match the row-stabilizer level `gluingLevel` $J_\Lambda$ produced by the third exchange. Gluing through $L_\Lambda$ introduces denominators bounded by $M^{l_1}$, so `glue` has to multiply by $M^{l_1}$ up front and absorb $(M^{l_1})^2$ inside the archimedean bound $\exp(2 f m b \log^2 b)$ (`M_pow_denominatorExponent_sq_mul_le_exp`).

## Level 2 — Undergrad Bridge

In Prasanna (2006), Harris–Li–Skinner (2005), and Böcherer–Dummigan–Schulze-Pillot (2012), pullback and seesaw identities pair a theta lift on a larger group against a fixed form on a split subgroup to extract special $L$-values while preserving $p$-adic integrality. `src/New/OneLift/ComparisonOperator/GluingMaps.lean` (68 declarations, 0 stub truncations) builds this in full rank:

1. **Splitting and glued lattices (`splittingV`, `gluingLevel`, `glueLattice`, `isGlued_glueLattice`):**
   The splitting `splittingV j ϑ` divides $V_j$ (`D.RowIndex ⊕ Fin D.t`, rank $6r$) into $V''$ (`FirstFactorIndex j`, rank $r + 9m + 1$) and $V^\circ$ (`D.SecondFactorIndex`, rank $t - 9m - 1$), with the divided directions `D.CarrierIndex` embedded in $V''$. Both Young diagrams $\mu^+$ (`colsPlus`) and $\mu^-$ (`colsMinus`) satisfy `HasColumnsBelow`, so Cartan products embed cleanly. The lattice `glueLattice j ϑ i` is chosen once at $\vartheta = 1$ and reused across all twists (`isGlued_splittingV_of_isGlued`).
2. **First factor arithmetic (`firstFactorScale`, `firstFactor`, `isIntegralAndSmall_firstFactor`, `weylGauge_firstFactor_le_one`, `weylNormSq_firstFactor_le`):**
   Setting $\varphi''_{\vartheta,i} = \overline{c'' \Upsilon_{\vartheta,i}}$ with $c'' = \mathrm{firstFactorScale}\ j \neq 0$ makes $\mathrm{firstFactorPairing}\ j\ \vartheta\ i = c'' \cdot \mathrm{thirdExchangeLeftSide}\ \vartheta\ j\ i$, so `twistFactor j ϑ` ($c'_\vartheta$) satisfies $c'_\vartheta \Xi_{\vartheta, i} = c'' \mathsf{F}_i$ and $\|c'_\vartheta\|^2 = C / \Gamma_j(\vartheta)$ (`exists_norm_twistFactor_sq_eq`).
3. **Seesaw adjoint computation (`inner_adjoint_glue`, `conj_normalizedDescent_glue`, `inner_adjoint_glue_goodLift_eq_sum_integral`, `twistFactor_smul_adjoint_glue_comp`):**
   Writing the descent of $\mathrm{glue}_\vartheta(\zeta)$ as a sum over $i \in \mathrm{ChoiceIndex}\ j$ and $u \in \bar{J}^1 = \mathrm{congruenceGroup}$ (using invariance of good forms under $\bar{J}^1$ via `goodForm_mul_liftMod_congruenceGroup`) gives
   $$c'_\vartheta \langle \zeta, \mathrm{glue}_\vartheta^*(\Theta_{j,\vartheta} y) \rangle = \sum_i c^\circ_i \int_{[U(W_j)]} y(g)\,\mathsf{F}_i(g)\,\langle \zeta_i, \theta^\circ(g, \cdot)\rangle\,dg = \langle \zeta, \Theta^\circ y\rangle,$$
   where $c^\circ_i = M^{l_1} c_{\mathsf{g}} \#\bar{J}^1 c'' \neq 0$ (`glueFactor_ne_zero`).
4. **False-lift orthogonality, injectivity, and $c'_\vartheta \neq 0$ (`sum_slice_eq_zero_of_mem_falseLiftsAt`, `thetaKernelPairing_adjoint_glue_eq_zero`, `isOrthogonalToFalseLifts_glue_periodLift`, `periodLift_injective`, `twistFactor_ne_zero`):**
   For any false lift $\Xi$, `PeriodsOfFalseLifts` forces every $V^\circ$-slice of $\Xi$ against $\theta^\circ$ to vanish, hence `thetaKernelPairing` of $\mathrm{glue}_\vartheta^*(\Xi)$ is zero, making $\langle \Xi, \mathrm{glue}_\vartheta(\Theta^\circ y)\rangle = 0$. Finally, `ProductsSpan.lean`'s `eq_zero_of_integral_choiceFn_thetaKernelPairing` proves `periodLift_injective`, and `D.finrank_targetForms_ne_zero` + `D.targetDim_eq_finrank_goodForms_spaceW` rule out $\Theta^\circ = 0$, proving `twistFactor_ne_zero`.

## Level 3 — Formal Lean Contract

All six anchors live in `skeleton/06_one_lift/gluing_maps.lean` (68 declarations, 0 stub truncations):

- `06_one_lift/gluing_maps.lean#New.Rows.firstFactor` (upstream `GluingMaps.lean:242-246`), `06_one_lift/gluing_maps.lean#New.Rows.glue` (upstream `GluingMaps.lean:344-385`), and `06_one_lift/gluing_maps.lean#New.Rows.periodLift` (upstream `GluingMaps.lean:1009-1013`).
- `06_one_lift/gluing_maps.lean#New.Rows.twistFactor_smul_adjoint_glue_comp` (upstream `GluingMaps.lean:1024-1030`):
  ```lean
  theorem New.Rows.twistFactor_smul_adjoint_glue_comp :
      twistFactor j ϑ • (glue j ϑ).adjoint ∘ₗ D.goodLift ϑ j = periodLift j
  ```
- `06_one_lift/gluing_maps.lean#New.Rows.periodLift_injective` (upstream `GluingMaps.lean:1046-1059`):
  ```lean
  theorem New.Rows.periodLift_injective : Function.Injective (periodLift j)
  ```
- `06_one_lift/gluing_maps.lean#New.Rows.twistFactor_ne_zero` (upstream `GluingMaps.lean:1061-1067`):
  ```lean
  theorem New.Rows.twistFactor_ne_zero : twistFactor j ϑ ≠ 0
  ```

See the page's live `<VerificationBanner />` (`audit/verification-status.json`) for the pinned upstream commit and local Lean build status (`build_status: unverified-locally`).
