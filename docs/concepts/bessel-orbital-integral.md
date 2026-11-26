---
slug: bessel-orbital-integral
term: Bessel orbital integral
aliases: ["amplitude block", "Bessel translate", "cut Bessel function", "unipotent covolume", "induced unipotent character"]
inline_glimpse: "An archimedean correlation integral ∫ β_c(y) β_{c'}(γ⁻¹ y) W(y) ⊗ W(γ⁻¹ y)* dy unfolding Petersson inner products P* ρ(s) P into class-indexed amplitude blocks."
visual_ref: assets/stage04-unfolding-block-anatomy.svg
where_analogy_breaks: "Picturing P_s = P* ρ(s) P as a block matrix whose entries are ordinary overlap integrals of translated wave packets captures why the identity regime looks diagonal and why distant elements γ oscillate away, but omits the two-step truncation at infinity: the actual Poincaré map unfolds to the full discrete-series Bessel vector (`.full`), whereas both the main-term sieve and remaining-term bounds require compact support modulo the Bessel subgroup via the cut Bessel function (`.cut`), linked back to `.full` through `CutTailBound.lean` and `SchurBound.lean`."
sources:
  - title: "Über die Entwicklungskoeffizienten der automorphen Formen"
    citation: "H. Petersson, Acta Math. 58 (1932), 169–215."
    kind: paper
  - title: "Relative trace formulae toward Bessel and Fourier–Jacobi periods on unitary groups"
    citation: "Y. Liu, Manuscripta Math. 145 (2014), 1–69."
    kind: paper
  - title: "The orbit method and analysis of spherical functions"
    citation: "P. Nelson & A. Venkatesh, Acta Math. 226 (2021), 1–209."
    kind: paper
lean_anchors:
  - 04_spanning/spanning_theorem.lean#New.Data.hasAmplitudeBlocks_scaledPoincare
  - 04_spanning/moments.lean#New.Filling.HasAmplitudeBlocks
  - 04_spanning/moments.lean#New.Filling.besselTranslate
  - 04_spanning/moments.lean#New.Filling.sum_norm_reducedCharMean_sq_le
  - 04_spanning/moments.lean#New.Filling.HasAmplitudeBlocks.sum_hilbertSchmidtNorm_sq_le
child_concepts:
  - seesaw-duality
  - hilbert-schmidt-norm
  - central-idempotent
  - kloosterman-sieve
  - link-lattice
---

# Bessel Orbital Integral

## Level 1 — First-Principles

In Stage 03, {{concept:seesaw-duality|dual-pair adjunction between lifts and restrictions}} showed that the Poincaré map $\mathcal{P}$ converts a source form on the definite space $X_0$ into an automorphic form on the indefinite space $Y_0$ representing the Bessel period. To prove Theorem B in Stage 04, we need to measure how much two shifted copies $P$ and $\rho(s) P$ of the scaled Poincaré map overlap—that is, we need the entries and {{concept:hilbert-schmidt-norm|root-sum-of-squares Frobenius size}} of the operator $P_s = P^* \rho(s) P$ on the source space.

Because $P$ is built by averaging a local Bessel test function $W$ over rational group elements, pairing $\rho(s) P$ against $P$ unfolds by Petersson's formula into a sum over rational elements $\gamma \in G(E)$ whose finite-prime reduction matches $s$. Each individual $\gamma$ contributes an archimedean correlation integral
$$A_\gamma = \int \beta_c(y)\,\beta_{c'}(\gamma^{-1} y)\, W(y) \otimes W(\gamma^{-1} y)^*\, dy,$$
called a **Bessel orbital integral**. Grouping the source space by double-coset classes $c, c'$ turns the operator $P_s$ into a grid of matrices whose $(c, c')$ **amplitude block** is the complex conjugate of $\nu^{-1} \sum_{\gamma} A_\gamma$, where $\nu$ is the **unipotent covolume** of the class cutoff (`HasAmplitudeBlocks`).

Once $P_s$ is unfolded into **amplitude blocks**, every rational element $\gamma$ falls into one of three cleanly separated regimes:

1. **The Bessel subgroup $\bar{R}_0$ (`besselTranslate`):** Terms $\gamma$ lying inside the Bessel subgroup itself do not mix across distant orbits; their normalized block sum forms an explicit unitary **Bessel translate** operator $r_\psi(s)$ on the source space with $r_\psi(1) = 1$. Whenever $s$ matches $\bar{R}_0$ at one of the two level primes, all remaining terms outside the Bessel subgroup contribute at most $b^{-12} \mathcal{D}_{\mathrm{id}}$, forcing $P^* P \approx 1$ (`hilbertSchmidtNorm_sub_one_sq_le`) and letting Stage 04 bound the {{concept:central-idempotent|bad-type projection loss}} via the **induced unipotent character** mean $\psi_U^\natural$ (`sum_norm_reducedCharMean_sq_le`).
2. **Open big-cell terms:** Terms lying in the generic cell where all leading frame minors are nonzero feed into the {{concept:kloosterman-sieve|one-sided Poisson summation sieve pairing archimedean band-limited kernels with finite Kloosterman densities}}, contributing the main second moment $(1 + b^{-4})\mathcal{D}_{\mathrm{main}}$.
3. **Remaining degenerate and cross-link terms:** Non-generic terms and pairs of orbits whose {{concept:link-lattice|common-link sublattices}} differ or meet modulo the level are suppressed far below the main term using Siegel's lemma and a weighted Schur test.

To make the second and third estimates work with compactly supported integrands, Stage 04 also replaces the full discrete-series Bessel vector $W$ (`.full`) by a **cut Bessel function** $W^\flat$ (`.cut`) supported on bounded Bessel length $b^{6n+2}$, controlling the truncation tail explicitly in $L^2$.

### Limits of the intuition

Picturing $P_s = P^* \rho(s) P$ as a block matrix of overlap integrals of shifted wave packets captures why the identity regime collapses to $r_\psi(s)$ and why off-diagonal blocks decay, but omits the two-layer archimedean machinery sitting behind `HasAmplitudeBlocks`. First, the Poincaré map unfolds verbatim to the *full* discrete-series Bessel vector (`amplitude .full`), whereas both `EnergyEstimate.lean` and `RemainingTerms/Sum.lean` operate on the compactly supported **cut Bessel function** (`amplitude .cut`), bridged back to `.full` by the explicit $L^2$ tail and derivative bounds in upstream `Bessel/{LargeFunction,SchurBound,CutTailBound}.lean` and the Laplace-method period lower bound in `src/New/Appendix/DiscreteSeries/Periods.lean`. Second, `HasAmplitudeBlocks` encodes an exact complex-conjugate block convention (`star (A(c, s, c') / ν)`) whose equivariance laws `P_{st} = P_s r_ψ(t)` and `P_{ts} = r_ψ(t) P_s` are essential to collapse the near-$\bar{R}_0$ sum into a bound on $P^* P - 1$.

## Level 2 — Undergrad Bridge

The unfolding identity generalizes Petersson's inner-product formula for Poincaré series (Petersson 1932) into the unitary Bessel relative trace formula framework (Liu 2014), with archimedean period lower bounds and localization informed by the orbit-method philosophy of Nelson–Venkatesh (2021).

### 1. Amplitude blocks and scaling (`SpanningTheorem.lean`, `Moments.lean`)

Let $S = D.\mathrm{filling}$ be admissible filling data for $G = U(Y_0)$ over the CM field $E$, with finite residue group $\bar{G} = S.\mathrm{ResidueGL} = G_{p_0} \times G_{p_1}$, double-coset classes `S.Classes` of the definite subgroup $U(X_0)$, and Bessel value coordinates `S.BesselCoord`. The source space is `EuclideanSpace ℂ (S.Classes × S.BesselCoord)` of dimension $D_X = S.\mathrm{sourceDim}$. Rescaling the unscaled Poincaré map by `D.poincareScale` $= (\operatorname{vol}(J)\cdot \nu)^{-1/2}$ produces `D.scaledPoincare`, which satisfies **`hasAmplitudeBlocks_scaledPoincare`** (`HasAmplitudeBlocks`): for every $s \in \bar{G}$ and basis vectors $e_{(c,i)}, e_{(c',j)}$,
$$\langle \rho(s) P e_{(c', j)},\, P e_{(c, i)} \rangle = \nu^{-1} \sum_{\gamma \in S.\mathrm{termsOver}\ c\ s\ c'} A_\gamma^{\mathrm{full}}(i, j),$$
where $\gamma \in S.\mathrm{termsOver}\ c\ s\ c'$ means $h_c^{-1} \gamma h_{c'}$ is integral with reduction $s \in \bar{G}$, $\nu = S.\mathrm{unipotentCovolume}$ is the uniform integral of `S.classCutoff c` over the real unipotent group, and $A_\gamma^{\mathrm{full}} = \int \beta_c(y)\beta_{c'}(\gamma^{-1} y) W(y) \otimes W(\gamma^{-1} y)^* dy$. Consequently (`hilbertSchmidtNorm_sq_eq_secondMoment`),
$$\|P^* \rho(s) P\|_{\mathrm{HS}}^2 = \nu^{-2} \sum_{c, c'} \bigl\|A^{\mathrm{full}}(c, s, c')\bigr\|^2 = \mathrm{secondMoment}\bigl(S.\mathrm{amplitudeSum}\ \mathrm{.full}\ \mathrm{.univ}\bigr)\ \{s\}.$$

### 2. The Bessel translate $r_\psi(s)$ and near-isometry (`Moments.lean`, lines 157–512)

Restricting the sum inside each block to rational elements $\gamma \in \mathrm{besselS}$ (block unitriangular matrices times $U(X_0)$) defines `S.besselTranslate s` ($r_\psi(s)$):
- $r_\psi(s) = 0$ for $s \notin \bar{R}_0 = S.\mathrm{reducedImage}\ \mathrm{besselS}$ (`besselTranslate_eq_zero`);
- on $\bar{R}_0$, $r_\psi$ is a unitary representation: $r_\psi(1) = 1$, $r_\psi(st) = r_\psi(s) r_\psi(t)$, and $r_\psi(s)^* = r_\psi(s^{-1})$;
- $P$ intertwines $\rho|_{\bar{R}_0}$ and $r_\psi$: $\rho(t) P = P r_\psi(t)$ (`rep_comp_eq_comp_besselTranslate`), giving two-sided equivariance $P_{st} = P_s r_\psi(t)$ and $P_{ts} = r_\psi(t) P_s$.

Subtracting the Bessel-subgroup terms leaves only $\gamma \in \mathrm{besselS}^c$ (`hilbertSchmidtNorm_sub_besselTranslate_sq`). Summing over all $s \in \bar{G}$ that agree with $\bar{R}_0$ at one prime $p_{i+1}$ gives `sum_hilbertSchmidtNorm_sub_besselTranslate_sq_le`:
$$\sum_{\substack{s \in \bar{G} \\ \exists t \in \bar{R}_0,\, s_{i+1} = t_{i+1}}} \|P_s - r_\psi(s)\|_{\mathrm{HS}}^2 \le b^{-12} \mathcal{D}_{\mathrm{id}}, \qquad \mathcal{D}_{\mathrm{id}} = \#\bar{R}_0 \cdot D_X.$$
Because $\|P_t - r_\psi(t)\|_{\mathrm{HS}}^2 = \|P^* P - 1\|_{\mathrm{HS}}^2$ is constant across all $t \in \bar{R}_0$ by unitary equivariance, restricting the sum to $t \in \bar{R}_0$ and dividing by $\#\bar{R}_0$ (`le_sourceDim_div_of_card_nsmul_le`) immediately gives
$$\|P^* P - 1\|_{\mathrm{HS}}^2 \le b^{-12} D_X \qquad (\texttt{hilbertSchmidtNorm\_sub\_one\_sq\_le}).$$

### 3. Whole-group second moment and the induced unipotent character mean (`Moments.lean`, lines 554–1029)

- **Whole-group second moment (`sum_hilbertSchmidtNorm_sq_le`):** Combining the Bessel-subgroup contribution, the `.full` vs. `.cut` tail estimate, the open big-cell Kloosterman sieve (`EnergyEstimate.lean`), and the remaining-term link bounds (`RemainingTerms/Sum.lean`) bounds the entire group sum:
  $$\sum_{s \in \bar{G}} \|P^* \rho(s) P\|_{\mathrm{HS}}^2 \le \bigl(1 + 9 b^{-4}\bigr)\,\mathcal{D}_{\mathrm{main}}, \qquad \mathcal{D}_{\mathrm{main}} = \frac{D_X^2}{\mathrm{deg}\cdot \mathrm{mass}_r}.$$
- **Induced unipotent character mean (`sum_norm_reducedCharMean_sq_le`):** Let $\psi_U = S.\mathrm{reducedChar}$ be the additive character $u \mapsto \psi_f(-\sigma(u)/(M\varpi))$ of integral unipotent reductions extended by $0$ to $\bar{G}$, and let $\psi_U^\natural(s) = S.\mathrm{reducedCharMean}\ s$ be its average over all conjugates of $s$ in $\bar{G}$. Rewriting $\psi_U^\natural(s)$ as a $K$-orbital integral $\operatorname{vol}(K)^{-1}\int_K \chi_M(y l y^{-1})\,dy$ (`integral_extendedBesselChar_conj_eq`) yields the $L^2$ bound
  $$\sum_{s \in \bar{G}} \bigl|\psi_U^\natural(s)\bigr|^2 \le 2,$$
  which drives the one-prime bad-type trace bound $\operatorname{Re}\,\operatorname{tr}(r_\psi(x)) \le b^{-4} D_X$.

## Level 3 — Formal Lean Contract

Stage 04 extracts the unfolding and second-moment theorems into `skeleton/04_spanning/spanning_theorem.lean` and `skeleton/04_spanning/moments.lean`:

- `04_spanning/spanning_theorem.lean#New.Data.hasAmplitudeBlocks_scaledPoincare` (upstream `src/New/Spanning/SecondMoment/SpanningTheorem.lean:153-173`):
  ```lean
  theorem New.Data.hasAmplitudeBlocks_scaledPoincare :
      D.filling.HasAmplitudeBlocks D.levelRepresentation D.scaledPoincare
  ```
- `04_spanning/moments.lean#New.Filling.HasAmplitudeBlocks` (upstream `src/New/Spanning/Unfolding/Moments.lean:86-100`):
  ```lean
  def New.Filling.HasAmplitudeBlocks (ρ : Representation ℂ S.ResidueGL V)
      (P : EuclideanSpace ℂ (S.Classes × S.BesselCoord) →ₗ[ℂ] V) : Prop
  ```
- `04_spanning/moments.lean#New.Filling.besselTranslate` (upstream `src/New/Spanning/Unfolding/Moments.lean:157-167`):
  ```lean
  noncomputable def New.Filling.besselTranslate (s : S.ResidueGL) :
      EuclideanSpace ℂ (S.Classes × S.BesselCoord) →ₗ[ℂ]
        EuclideanSpace ℂ (S.Classes × S.BesselCoord)
  ```
- `04_spanning/moments.lean#New.Filling.sum_norm_reducedCharMean_sq_le` (upstream `src/New/Spanning/Unfolding/Moments.lean:991-1029`):
  ```lean
  theorem New.Filling.sum_norm_reducedCharMean_sq_le [ok : S.IsAdmissible] :
      ∑ s : S.ResidueGL, ‖S.reducedCharMean s‖ ^ 2 ≤ 2
  ```
- `04_spanning/moments.lean#New.Filling.HasAmplitudeBlocks.sum_hilbertSchmidtNorm_sq_le` (upstream `src/New/Spanning/Unfolding/Moments.lean:554-583`):
  ```lean
  theorem New.Filling.HasAmplitudeBlocks.sum_hilbertSchmidtNorm_sq_le [WeilData E] (hP : S.HasAmplitudeBlocks ρ P)
      (hρ : ∀ (s : S.ResidueGL) (x y : V), ⟪ρ s x, ρ s y⟫_ℂ = ⟪x, y⟫_ℂ) :
      ∑ s : S.ResidueGL, hilbertSchmidtNorm (P.adjoint ∘ₗ ρ s ∘ₗ P) ^ 2 ≤
        (1 + 9 / S.b ^ 4) * S.mainSecondMoment
  ```

See the page's live `<VerificationBanner />` (`audit/verification-status.json`) for the pinned upstream commit and local Lean build status (`build_status: unverified-locally`).
