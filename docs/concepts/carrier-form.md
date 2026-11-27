---
slug: carrier-form
term: carrier form
aliases: ["exchange pair", "Hecke value product", "three-exchange reduction"]
inline_glimpse: "The fixed, nonzero, full-level holomorphic theta lift of 1 from U(0, m) to U(2m, m) onto which the first two Siegel–Weil exchanges collapse a twisted class sum, letting a third exchange turn the entire twist dependence into a scalar c(theta) controlled by the raw Hecke value product Gamma_j(theta)."
visual_ref: assets/stage06-three-exchanges-and-gluing.svg
where_analogy_breaks: "Thinking of the carrier form as a single reusable template that simply factors a twist out of an integral hides two technical constraints: first, the Hecke value product Gamma_j(theta) is the raw unnormalized product of lValueProd over the three exchange pairs (not yet divided by Gamma_j(1)); second, the three exchanges leave the scalar c(theta) with squared modulus C / Gamma_j(theta) where C could still be zero until GluingMaps proves periodLift_injective and invokes Stage 04 finrank_targetForms_ne_zero."
sources:
  - title: "Non-vanishing theorems for the cohomology of certain arithmetic quotients"
    citation: "J.-S. Li, J. Reine Angew. Math. 428 (1992), 177–217."
    kind: paper
  - title: "The regularized Siegel–Weil formula (the second term identity) and the Rallis inner product formula"
    citation: "W. T. Gan, Y. Qiu & S. Takeda, Invent. Math. 198 (2014), 739–831."
    kind: paper
  - title: "Theta liftings and Hecke L-functions"
    citation: "T. Yang, J. Reine Angew. Math. 485 (1997), 25–53."
    kind: paper
lean_anchors:
  - 06_one_lift/three_exchanges.lean#New.Rows.exists_mul_stdClassSum_eq_carrierForm
  - 06_one_lift/three_exchanges.lean#New.Rows.carrierForm_mem_rapidOfLevel_and_isAdelicHol
  - 06_one_lift/three_exchanges.lean#New.Rows.carrierForm_ne_zero
  - 06_one_lift/three_exchanges.lean#New.Data.exists_mul_thirdExchangeLeftSide_eq_choiceFn
  - 06_one_lift/three_exchanges.lean#New.Data.exchangePair
  - 06_one_lift/three_exchanges.lean#New.Data.heckeValueProd
child_concepts:
  - theta-correspondence
  - siegel-weil-exchange
  - hecke-l-value-ratio
---

# Carrier Form

## Level 1 — First-Principles

In Stage 06, we want to compare a twisted theta lift against an untwisted one without losing track of where the twist character $\vartheta$ sits. Directly pairing two twisted forms spreads $\vartheta$ across matrices, level subgroups, and finite-prime sums. **Three-exchange reduction** strips $\vartheta$ out of all those moving parts in three consecutive applications of {{concept:siegel-weil-exchange|the complementary-rank swap that equates two theta integrals up to a scalar controlled by partial L-values}}, until nothing remains of $\vartheta$ except a single scalar multiplier $c(\vartheta)$.

The bridge in the middle of that reduction is the **carrier form** $\varphi^\circ = \mathrm{carrierForm}\ E\ m$. Inside {{concept:theta-correspondence|the unitary dual-pair lifting mechanism}}, $\varphi^\circ$ is the theta lift of the constant function $1$ from the compact definite group $U(0, m)$ to the indefinite unitary group $U(2m, m)$. It has three decisive properties:
1. **Twist-free and full level:** $\varphi^\circ$ depends only on the CM field $E$ and the even integer $m$, never on the twist $\vartheta$. It is rapidly decreasing and invariant under the full congruence structure at every level (`carrierForm_mem_rapidOfLevel_and_isAdelicHol`).
2. **Holomorphic and nonzero:** $\varphi^\circ$ is adelically holomorphic for a positive-first basis order, and it is strictly nonzero (`carrierForm_ne_zero`).
3. **Target of the first two exchanges:** Evaluating a twisted class sum of {{term:theta-correspondence|theta kernels}} on $U(2m, m)$ against an explicit coefficient function produces a scalar multiple of $\varphi^\circ$ (`exists_mul_stdClassSum_eq_carrierForm`).

Once the twist has been pushed into a scalar in front of $\varphi^\circ$, the third exchange integrates $\varphi^\circ$ against a theta kernel on the enlarged space $W' = W_j \oplus (E^j)^- \oplus E^{3m-j} \oplus W^\flat$ to produce twist-free functions $\mathsf{F}_i = \mathrm{choiceFn}\ j\ i$ on $U(W_j)$. Across all three exchanges, the combined scalar $c(\vartheta)$ obeys
$$\|c(\vartheta)\|^2 = \frac{C}{\Gamma_j(\vartheta)},$$
where $\Gamma_j(\vartheta) = \mathrm{heckeValueProd}\ j\ \vartheta$ is the **Hecke value product** over three **exchange pairs** (`New.Data.exchangePair j i` for $i \in \{\mathrm{some}\,(-1), \mathrm{some}\,1, \mathrm{none}\}$):
- **First exchange ($i = \mathrm{some}\,(-1)$):** a flat pair $\nu_{-1} = \upsilon^{-m/2}\chi^{-j}$ evaluated up to $z_{-1} = m/2$ with twist sign $-1$.
- **Second exchange ($i = \mathrm{some}\,1$):** a flat pair $\nu_{+1} = \upsilon^{-m}\chi^{j}$ evaluated up to $z_{+1} = m$ with twist sign $+1$.
- **Third exchange ($i = \mathrm{none}$):** the main-chain character $\nu_j = \mu_j^0 \upsilon^n$ (with $n = j + (t - 11m - 1)/2$) evaluated up to $z_* = 7(m/2) + k/2$ with twist sign $+1$.

Each factor in $\Gamma_j(\vartheta)$ is an unnormalized product of squared {{concept:hecke-l-value-ratio|partial Hecke L-values}} and conductor-norm powers at the chosen places and the twist places $Q$. Dividing $\Gamma_j(\vartheta)$ by $\Gamma_j(1)$ happens later, once the comparison operator compares the twisted lift against the untwisted lift.

### Limits of the intuition

Calling the **carrier form** a "twist-free receptacle" makes the three exchanges look like an automatic algebraic factorization. Two caveats matter in the formal proof. First, `heckeValueProd j ϑ` is the *raw* product $\Gamma_j(\vartheta)$, not a normalized ratio—dividing by $\Gamma_j(1)$ occurs only when `TwistedValueHeights` and `LiftVolumeHeight` form $\rho_\vartheta = \Gamma_j(\vartheta)/\Gamma_j(1)$. Second, `exists_mul_thirdExchangeLeftSide_eq_choiceFn` produces a constant $C \in \mathbb{C}$ with $\|c(\vartheta)\|^2 = C / \Gamma_j(\vartheta)$ without yet ruling out $C = 0$; non-vanishing of the scalar (and hence of $C$) is proved only downstream in `GluingMaps.lean` (`twistFactor_ne_zero`).

## Level 2 — Undergrad Bridge

In Li (1992), Yang (1997), and Gan–Qiu–Takeda (2014), stable-range theta lifts of characters from definite unitary groups are holomorphic, rapidly decreasing, and nonzero, while equal-rank Siegel–Weil exchanges convert definite class sums with determinant data into special values of Hecke $L$-functions. `src/New/OneLift/ComparisonOperator/ThreeExchanges.lean` executes this pipeline in four steps:

1. **First exchange on $(I_m, I_m)$ (`exists_mul_stdClassSum_modKernelStdSpace_eq_one`, `exists_stdClassSum_modKernelStdSpace_neg_eq_one`):**
   For even $m = 2e > 0$ and a tame character $\alpha$ of weight $-k$ on $(\mathfrak{m}, \chi)$, the class sum over $U(I_m)$ of the determinant-weighted theta kernel equals $\gamma^{-1}$ (constant function $1$), with $\|\gamma\|^2 = C / L_*(\alpha^{-1}\upsilon^e, e)$. Conjugating and passing to the negated space $I_m^-$ handles the companion sign.
2. **Second exchange to the carrier form (`exists_mul_stdClassSum_eq_carrierForm`, `carrierForm_mem_rapidOfLevel_and_isAdelicHol`, `carrierForm_ne_zero`):**
   Composing the first two exchanges on $S$ of signature $(2m, m)$ paired with $I_{3m}$ evaluates
   $$\gamma \sum_h \mathrm{carrierCoeff}\ m\ c\ h \cdot \theta_{\upsilon^m, c}[\chi \circ \det, 1](x, h) = \varphi^\circ(x)$$
   with $\|\gamma\|^2 = C / \bigl(L_*(c^{-1}\upsilon^m, m) \cdot L_*(c\upsilon^{m/2}, m/2)\bigr)$. Fock-space invariance (`fockInner_realPairWeil_variableMinor_pow_eq_self`) ensures $\varphi^\circ \neq 0$, while rapid decay and holomorphy hold at every level.
3. **Third exchange to twist-free choice functions (`exists_mul_thirdExchangeLeftSide_eq_choiceFn`, `integral_carrierForm_mul_eq_card_mul_choiceFn`):**
   Pairing $W = E^{3m} \oplus W'$ against $V''$ (rank $r + 9m + 1$) with invariant trace datum $\psi_\Lambda^{\mathrm{inv}}$ modulo $\varpi^2 Q$ equates $c(\vartheta)\cdot \mathrm{thirdExchangeLeftSide}\ \vartheta\ j\ i\ g = \mathrm{choiceFn}\ j\ i\ g$, where $\mathrm{choiceFn}\ j\ i$ is the integral of $\varphi^\circ$ against the theta kernel on $(S, W')$ and $\|c(\vartheta)\|^2 = C / \Gamma_j(\vartheta)$.
4. **Rewriting as a single theta series (`thirdExchangeLeftSide_eq_kernelClassSum`, `isAdelicHol_choiceFn`):**
   Using row-stabilizer level `rowFixingLevel` and product datum $\Psi^\times$ whenever $u w = \Lambda$, the double class sum collapses into `kernelClassSum` on $U(W_j)$ built from `leftSideSeries ϑ j i`, while `choiceFn j i` inherits holomorphy of weight $u'' = u - u^\circ$ (`isAdelicHol_choiceFn`).

## Level 3 — Formal Lean Contract

All six anchors live in `skeleton/06_one_lift/three_exchanges.lean` (43 declarations; 1 unrelated intermediate lemma `fockInner_realPairWeil_variableMinor_pow_eq_self` is truncated at `let v :=`, while all 6 anchors below extract untruncated):

- `06_one_lift/three_exchanges.lean#New.Data.exchangePair` (upstream `ThreeExchanges.lean:138-177`) and `06_one_lift/three_exchanges.lean#New.Data.heckeValueProd` (upstream `ThreeExchanges.lean:179-187`).
- `06_one_lift/three_exchanges.lean#New.Rows.exists_mul_stdClassSum_eq_carrierForm` (upstream `ThreeExchanges.lean:282-404`):
  ```lean
  lemma New.Rows.exists_mul_stdClassSum_eq_carrierForm {m : ℕ} [NeZero m] (hm_even : Even m) :
      ∃ C : ℝ, ∀ {𝔪 : Modulus E} {c : AutChar E} {χ : MulChar 𝔪.ResidueRing ℂ},
        IsTameFor 𝔪 c 0 χ → ∃ γ : ℂ,
          ‖γ‖ ^ 2 = C / (lValueProd 𝔪.places (c⁻¹ * upsilonU1 ^ m) m *
            lValueProd 𝔪.places (c * upsilonU1 ^ (m / 2)) (m / 2)) ∧
          ∀ x : G E (1 ⊕ᵥ -1), γ * stdClassSum (stdLevel E (Fin (2 * m) ⊕ Fin m))
            (fun h : Gf E (Fin (2 * m) ⊕ Fin m) ↦ carrierCoeff m c h *
              modKernelStdSpace (signedSpace E (1 ⊕ᵥ -1)) (upsilonU1 ^ m) c 𝔪
                (fun y : Matrix (Fin (2 * m) ⊕ Fin m) (Fin (2 * m) ⊕ Fin m) 𝔪.ResidueRing ↦ χ y.det)
                1 x h) = carrierForm E m x
  ```
- `06_one_lift/three_exchanges.lean#New.Rows.carrierForm_mem_rapidOfLevel_and_isAdelicHol` (upstream `ThreeExchanges.lean:417-485`) and `06_one_lift/three_exchanges.lean#New.Rows.carrierForm_ne_zero` (upstream `ThreeExchanges.lean:530-558`).
- `06_one_lift/three_exchanges.lean#New.Data.exists_mul_thirdExchangeLeftSide_eq_choiceFn` (upstream `ThreeExchanges.lean:811-860`):
  ```lean
  lemma New.Data.exists_mul_thirdExchangeLeftSide_eq_choiceFn : ∃ (C : ℂ) (c : D.Twist → ℂ), ∀ ϑ : D.Twist,
      ‖c ϑ‖ ^ 2 = C / heckeValueProd j ϑ ∧
      ∀ (i : ChoiceIndex j) (g : New.G D.E (D.signsW j)),
        c ϑ * thirdExchangeLeftSide ϑ j i g = choiceFn j i g
  ```

See the page's live `<VerificationBanner />` (`audit/verification-status.json`) for the pinned upstream commit and local Lean build status (`build_status: unverified-locally`).
