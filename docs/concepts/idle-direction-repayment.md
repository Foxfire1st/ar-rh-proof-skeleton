---
slug: idle-direction-repayment
term: idle direction repayment
aliases: ["idle direction", "single-lift cost", "lift volume height"]
inline_glimpse: "The exact negative-height accounting on Z_j(1) / Z_j(xi_iota^j) where k/2 squared main-pair L-value ratios repay k idle directions at cost d * k * (log b * denominatorBudget - directionAmount), overcoming the naive 2/3 barrier to produce -d * D.mainAmount inside Theorem C."
visual_ref: assets/stage06-comparison-operator-volume.svg
where_analogy_breaks: "Speaking of each lift as 'paying back k idle directions' is a helpful accounting metaphor, but the repayment does not cancel error terms directly inside a single determinant: it combines two separate height bounds—the compressed comparison determinant ratio (height <= d * (3A + r * denominatorBudget)) and the d-th power of the inverse Hecke product ratio (height <= d * (10 m x_1 + k * (6 * denominatorBudget - directionAmount)))—and then absorbs all positive errors into d * k * log b * denominatorBudget via D.le_of_hasLogBound."
sources:
  - title: "Congruences of cusp forms and special values of their zeta functions"
    citation: "H. Hida, Invent. Math. 63 (1981), 225–261."
    kind: paper
  - title: "Yoshida lifts and Selmer groups"
    citation: "S. Böcherer, N. Dummigan & R. Schulze-Pillot, J. Math. Soc. Japan 64 (2012), 1353–1401."
    kind: paper
  - title: "Integrality of a ratio of Petersson norms and level-lowering congruences"
    citation: "K. Prasanna, Ann. of Math. 163 (2006), 901–941."
    kind: paper
lean_anchors:
  - 06_one_lift/lift_volume_height.lean#New.Data.heightLe_liftDeterminant_ratio
  - 06_one_lift/lift_volume_height.lean#New.Data.IsComparisonOperator.norm_padic_det_liftCompression_le
  - 06_one_lift/lift_volume_height.lean#New.Data.IsComparisonOperator.norm_map_det_liftCompression_le
  - 06_one_lift/twisted_value_heights.lean#New.Data.exists_heckeValueProd_eq_mul_prod_ratio
  - 06_one_lift/twisted_value_heights.lean#New.Data.heightLe_inv_heckeValueProd_div
child_concepts:
  - arithmetic-height
  - gram-determinant
  - lift-comparison-operator
  - character-theta-lift
  - regular-twist-family
---

# Idle Direction Repayment

## Level 1 — First-Principles

Why did Levent Alpöge's roadmap flag the **single-lift cost** (Step 14) as the barrier separating an elementary $2/3$ exponent from the $1/2$ exponent required for GRH? In the $m$-step chain of lifts, each step moves from a Hermitian space $W_j$ of rank $r$ to a larger space $V_j$ of rank $r + t$ ($t = 5r$), while the source space has rank $k = r + 1 - m$. Only $k$ of the lifted directions correspond to active derivatives of the source forms; the remaining directions sit as **idle directions** whose naive archimedean volume would swallow the prime-divisibility gain unless each lift's {{concept:gram-determinant|Gram determinant}} ratio repays them cleanly.

**Idle direction repayment** is the exact negative-{{concept:arithmetic-height|arithmetic height}} budget established by Stage 06 (`New.Data.heightLe_liftDeterminant_ratio`): across {{concept:regular-twist-family|a regular twist family}} $\theta$, the **lift volume height** of the $j$-th single-lift ratio $Z_j(1) / Z_j(\theta(\iota)^j)$ is bounded above by
$$d \cdot k \cdot \bigl(\log b \cdot \mathrm{denominatorBudget} - D.\mathrm{directionAmount}\bigr).$$
Because $D.\mathrm{directionAmount}$ is vastly larger than $\log b \cdot \mathrm{denominatorBudget}$, this height ceiling is *strongly negative*: every one of the $k$ directions contributes a full credit of $-D.\mathrm{directionAmount}$, and summing over all $m$ lifts in Stage 05 (`LongForms.lean`) yields $-d \cdot m \cdot k \cdot D.\mathrm{directionAmount} = -d \cdot D.\mathrm{mainAmount}$, creating the exact lower floor of Theorem C.

How does Stage 06 assemble that exact coefficient $k$ and keep every error term inside $\log b \cdot \mathrm{denominatorBudget}$? Using {{concept:lift-comparison-operator|the lift comparison operator}}, Stage 06 splits the single-lift determinant ratio into two factors:
$$\frac{Z_j(1)}{Z_j(\xi_\iota^j)} = \frac{\Delta(\xi_\iota^j)}{\Delta(1)} \cdot \rho_{\xi_\iota^j}^{-d}.$$
1. **Compressed determinant ratio $\Delta(\xi_\iota^j) / \Delta(1)$:** At infinity, eigenvalue bounds on the untwisted {{term:lift-comparison-operator|lift compression}} give $\|\sigma(\Delta(1))\| \le \exp(3 d A)$ where $A = f m b \log^2 b$ (`norm_map_det_liftCompression_le`). At each prime $\ell$, `ClearsProjection` gives $\|\iota(\Delta(\xi_\iota^j))\| \le \exp(d r \beta_\ell)$ (`norm_padic_det_liftCompression_le`). Together, the intermediate height of $\Delta(\xi_\iota^j)/\Delta(1)$ is at most $d(3A + r\,\mathrm{denominatorBudget})$.
2. **Inverse Hecke value ratio $\rho_{\xi_\iota^j}^{-d}$:** Factoring $\Gamma_j(\vartheta) / \Gamma_j(1)$ over the three {{concept:carrier-form|exchange pairs from the three-exchange reduction}} (`exists_heckeValueProd_eq_mul_prod_ratio`) splits the squared $L$-value factors into **exact pair counts**:
   - $\#A = k / 2$ {{concept:hecke-l-value-ratio|main pairs}} (with $7(m/2) < z \le 7(m/2) + k/2$, sign $s = +1$), squared to give **$k$ copies** of {{concept:character-theta-lift|the negative main-pair height}} $6\,\mathrm{denominatorBudget} - D.\mathrm{directionAmount}$;
   - $\#B = 5m$ {{term:hecke-l-value-ratio|ramified pairs}} ($m/2$ at sign $-1$, $m$ at sign $+1$, and $7(m/2)$ at sign $+1$), squared to give **$10m$ copies** of the `HasSmallRatios` bound $x_1$.
   Raising to the $d$-th power yields height $d\bigl(10 m x_1 + k(6\,\mathrm{denominatorBudget} - D.\mathrm{directionAmount})\bigr)$ (`heightLe_inv_heckeValueProd_div`).
3. **Final absorption (`D.le_of_hasLogBound`):** Adding both intermediate heights gives
   $$d\Bigl(3A + r\,\mathrm{denominatorBudget} + 10 m x_1 + 6 k\,\mathrm{denominatorBudget} - k\,D.\mathrm{directionAmount}\Bigr).$$
   Because $\log b$ towers over $r/k$, $A/(k\,\mathrm{denominatorBudget})$, and $m x_1 / (k\,\mathrm{denominatorBudget})$, the scale bound `D.le_of_hasLogBound` folds all four positive error terms cleanly into $k \log b \cdot \mathrm{denominatorBudget}$, leaving $d k (\log b \cdot \mathrm{denominatorBudget} - D.\mathrm{directionAmount})$.

### Limits of the intuition

Do not conflate the intermediate height $d(3A + r\,\mathrm{denominatorBudget})$ of $\Delta(\xi_\iota^j)/\Delta(1)$ with the final absorbed error $d k \log b \cdot \mathrm{denominatorBudget}$: the extra factor of $\log b$ on `denominatorBudget` is not produced by any single determinant bound, but is the slack needed by `D.le_of_hasLogBound` to absorb $3A$ ($3 f m b \log^2 b$), $r\,\mathrm{denominatorBudget}$, $10 m x_1$, and $6 k\,\mathrm{denominatorBudget}$ all at once—and in Stage 05 `LongForms.lean`, $k \log b \cdot \mathrm{denominatorBudget}$ simplifies identically to $f a \log^4 b / m$.

## Level 2 — Undergrad Bridge

In Hida (1981), Prasanna (2006), and Böcherer–Dummigan–Schulze-Pillot (2012), congruence numbers link special $L$-values to norm ratios of modular forms; here, the same logic operates at rank $r \to 6r$ across $m$ lifts to defeat the $2/3$ barrier:

1. **Exact pair counts in `exists_heckeValueProd_eq_mul_prod_ratio` (`TwistedValueHeights.lean:1025-1062`):**
   The three `exchangePair j i` cutoffs are $z_{-1} = m/2$, $z_{+1} = m$, and $z_* = \mathrm{mainPairPoint} = 7(m/2) + k/2$. Since $m$ and $k$ are even:
   - Main pairs $A = (7(m/2),\, 7(m/2) + k/2]$ on `i = none` (all sign $+1$): $\#A = k/2$, so $2\#A = k$. Every $a \in A$ satisfies `IsMainPair j` (`isMainPair_lowerPoint_exchangePair_none`).
   - Remaining ramified pairs $B$: $(0, m/2]$ on `some (-1)` (sign $-1$), $(0, m]$ on `some 1` (sign $+1$), and $(0, 7(m/2)]$ on `none` (sign $+1$). Total cardinality: $\#B = 7(m/2) + m + m/2 = 5m$, so $2\#B = 10m$. Every $b \in B$ satisfies `IsRamified` (`isRamified_lowerPoint_exchangePair`).
   - Conductor power: $\mathrm{Nm}(Q)^e$ with $e = \sum_i z_i^2$ is a rational integer coprime to $\ell$ at every $\iota$, contributing $0$ to the height via `hθ.heightLe_of_eq_zpow_mul`.
   - Resulting bound (`heightLe_inv_heckeValueProd_div`): `HeightLe (10 * D.m * D.x₁ + D.k * (6 * D.denominatorBudget - D.directionAmount))` on `(heckeValueProd j ((θ ι).powTwist j) / heckeValueProd j 1)⁻¹`.
2. **Eigenvalue bounds on `liftCompression` (`LiftVolumeHeight.lean:648-716`):**
   - **Archimedean (`norm_map_det_liftCompression_le`):** For any eigenpair $(\alpha, z)$ of $\mathrm{pr}\,U_1$, false-lift orthogonality gives $\mathrm{normSqAt}(\mathrm{pr}\,U_1 z, \sigma) \le \mathrm{normSqAt}(U_1 z, \sigma) \le \exp(3A)^2\,\mathrm{normSqAt}(z, \sigma)$, forcing $\|\sigma\alpha\| \le \exp(3A)$. Multiplying across $d = \dim(D.\mathrm{goodForms}(W_j))$ eigenvalues gives $\|\sigma(\Delta(1))\| \le \exp(3 d A)$.
   - **$p$-adic (`norm_padic_det_liftCompression_le`):** Given `ClearsProjection` at $\beta$, an operator $\Lambda$ on $\mathrm{range}(\Theta_{j,\vartheta})$ with $|\iota(\det\Lambda)| \ge \exp(-d r \beta)$ maps integral forms orthogonal to false lifts into integral forms. Hence $\Lambda \circ (\mathrm{pr}\,U_\vartheta)$ preserves integrality, so its eigenvalues have $\iota$-norm $\le 1$, giving $\|\iota(\det(\Lambda\cdot \mathrm{pr}\,U_\vartheta))\| \le 1$ and thus $\|\iota(\Delta(\vartheta))\| \le \exp(d r \beta)$.
3. **Final assembly (`heightLe_liftDeterminant_ratio`, `LiftVolumeHeight.lean:726-783`):**
   Taking $\Delta(1)$ as the multiplier in `HeightLe.of_mul_le` yields `HeightLe (d * (3 * A) + d * (D.r * D.denominatorBudget))` on $\Delta(\xi_\iota^j) / \Delta(1)$. Multiplying by `(heightLe_inv_heckeValueProd_div hθ j).pow d` and applying `D.le_of_hasLogBound`:
   $$3A + r\,\mathrm{denominatorBudget} + 10 m x_1 + 6 k\,\mathrm{denominatorBudget} \le k \log b \cdot \mathrm{denominatorBudget}$$
   proves `HeightLe (D.targetDim * (D.k * (D.logB * D.denominatorBudget - D.directionAmount)))` on $Z_j(1)/Z_j(\theta(\iota)^j)$, where `D.targetDim_eq_finrank_goodForms_spaceW j` identifies $d = D.\mathrm{targetDim}$.

## Level 3 — Formal Lean Contract

All five anchors extract completely untruncated across `skeleton/06_one_lift/lift_volume_height.lean` and `skeleton/06_one_lift/twisted_value_heights.lean`:

- `06_one_lift/twisted_value_heights.lean#New.Data.exists_heckeValueProd_eq_mul_prod_ratio` (upstream `TwistedValueHeights.lean:1025-1062`) and `06_one_lift/twisted_value_heights.lean#New.Data.heightLe_inv_heckeValueProd_div` (upstream `TwistedValueHeights.lean:1064-1095`):
  ```lean
  theorem New.Data.heightLe_inv_heckeValueProd_div {θ : PrimePadicIso → D.MFoldTwist}
      (hθ : D.IsRegularFamily θ) (j : D.LiftIndex) :
      HeightLe (10 * D.m * D.x₁ + D.k * (6 * D.denominatorBudget - D.directionAmount))
        fun ι : PrimePadicIso ↦ (heckeValueProd j ((θ ι).powTwist j) / heckeValueProd j 1)⁻¹
  ```
- `06_one_lift/lift_volume_height.lean#New.Data.IsComparisonOperator.norm_padic_det_liftCompression_le` (upstream `LiftVolumeHeight.lean:648-691`) and `06_one_lift/lift_volume_height.lean#New.Data.IsComparisonOperator.norm_map_det_liftCompression_le` (upstream `LiftVolumeHeight.lean:693-716`).
- `06_one_lift/lift_volume_height.lean#New.Data.heightLe_liftDeterminant_ratio` (upstream `LiftVolumeHeight.lean:726-783`):
  ```lean
  lemma New.Data.heightLe_liftDeterminant_ratio {θ : PrimePadicIso → D.MFoldTwist} (hθ : D.IsRegularFamily θ)
      (j : D.LiftIndex) :
      HeightLe (D.targetDim * (D.k * (D.logB * D.denominatorBudget - D.directionAmount)))
        fun ι : PrimePadicIso ↦ D.liftDeterminant 1 j / D.liftDeterminant (θ ι) j
  ```

See the page's live `<VerificationBanner />` (`audit/verification-status.json`) for the pinned upstream commit and local Lean build status (`build_status: unverified-locally`).
