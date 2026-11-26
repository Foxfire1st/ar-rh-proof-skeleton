---
slug: link-lattice
term: link lattice
aliases: ["common link", "pair discriminant", "frame-meeting norm", "weighted Schur test", "Siegel's lemma"]
inline_glimpse: "The Hermitian sublattice shared by two rational filling terms over one reduction block, whose discriminant and Bombieri–Vaaler Siegel's lemma counts crush cross-link and non-open terms via a weighted Schur test."
visual_ref: assets/stage04-sieve-vs-link-lattice-split.svg
where_analogy_breaks: "Picturing two rational terms γ, γ' with different links as almost orthogonal vectors whose cross-term cancels ignores that `RemainingTerms/Sum.lean` does NOT get cancellation in the cross-terms: it bounds every cross-link pair by absolute value (`termSize γ * termSize γ'`), winning purely from arithmetic sparsity—because forcing γ and γ' to share the same mod-M reduction `s` shrinks the common-link discriminant by `(2 / M)^{2if}`, which after raising to the `k`-th power beats the entire height zeta sum over sublattices of corank at most `n³`."
sources:
  - title: "On Siegel's lemma"
    citation: "E. Bombieri & J. D. Vaaler, Invent. Math. 73 (1983), 11–32."
    kind: paper
  - title: "Local spectral equidistribution for Siegel modular forms"
    citation: "E. Kowalski, A. Saha & J. Tsimerman, Compos. Math. 148 (2012), 335–384."
    kind: paper
  - title: "Density theorems for GL(n)"
    citation: "V. Blomer, Invent. Math. 232 (2023), 1–48."
    kind: paper
lean_anchors:
  - 04_spanning/remaining_terms_sum.lean#New.Filling.InSameLinkClass
  - 04_spanning/remaining_terms_sum.lean#New.Filling.sum_termSize_mul_not_inSameLinkClass_le
  - 04_spanning/remaining_terms_sum.lean#New.Filling.sum_termSize_mul_inSameLinkClass_le
  - 04_spanning/remaining_terms_sum.lean#New.Filling.secondMoment_remainingTerms_le
  - 04_spanning/remaining_terms_sum.lean#New.Filling.linkSecondMoment_le_of_le_meetNorm
child_concepts:
  - bessel-orbital-integral
  - kloosterman-sieve
  - gram-determinant
  - arithmetic-height
---

# Link Lattice

## Level 1 — First-Principles

The {{concept:kloosterman-sieve|one-sided Poisson summation sieve on the big Bruhat cell}} handles the main term of Stage 04 only one *link* at a time and only on generic open reductions. When we square the full sum of {{concept:bessel-orbital-integral|archimedean overlap amplitudes}} over all rational elements $\gamma$ outside the Bessel subgroup, two extra families of terms appear:

1. **Cross-terms and degenerate terms ($\neg\,\mathrm{InSameLinkClass}$):** Pairs $(\gamma, \gamma')$ whose links differ—or for which the term discriminant $\mathsf{g} = |\Delta|^2 - N^2 |\Delta'|^2$ vanishes—must be shown to add almost nothing when we expand $\|\sum_\gamma A_\gamma\|^2$.
2. **Non-open reductions ($\mathrm{meetNorm}\ s \ge z$):** Reductions $s \in \bar{G}$ where the frame span and its image under $s$ collide modulo some prime above the level $M$ (so $s$ fails to lie in the big cell), quantified by the **frame-meeting norm** $S.\mathrm{meetNorm}\ s$.

To crush both families, Stage 04 attaches to every rational element $\gamma$ its *link* $\Lambda_\gamma$: the linear graph of pairs $(t, \gamma^{-1} t)$ in the middle quotient $X^\perp / X$ for vectors $t$ orthogonal to both the isotropic span $X$ and $\gamma X$. Intersecting $\Lambda_\gamma$ with the integral adelic lattice produces a Hermitian **link lattice**, and for any pair $(\gamma, \gamma')$ sitting over the same block $(c, s, c')$, the intersection $\Lambda_\gamma \cap \Lambda_{\gamma'}$ is their **common link**, carrying a **pair discriminant** $d = S.\mathrm{pairDiscriminant}\ c\ c'\ \gamma\ \gamma'$ (measuring its {{concept:gram-determinant|squared fundamental volume}}).

Why does a pair of terms over the same block stay tiny? Two mechanisms multiply together:

- **Analytic bound (weighted Schur test):** Applying Cauchy–Schwarz with an asymmetric geometric weight that inverts when $\gamma$ and $\gamma^{-1}$ trade places—a **weighted Schur test**—bounds the amplitude of each orbit by an explicit `termSize` decaying like $G^{-k/2}$ in the level gauge $G$ of $\gamma$.
- **Arithmetic sparsity (Siegel's lemma):** Because $\gamma$ and $\gamma'$ reduce to the *same* matrix $s \pmod M$, their **common link** satisfies congruence constraints that compress its **pair discriminant** by $(2/M)^{2if}$ whenever the links intersect in codimension $i \ge 1$, or by the **frame-meeting norm** $z$ when both terms share a non-degenerate link over a meeting reduction $s$. Counting all Hermitian sublattices of bounded discriminant via **Siegel's lemma**—which cuts out every codimension-$s$ subspace by $s$ short integral vectors—forces the entire sum of `termSize γ * termSize γ'` over all cross-link pairs to fall below $b^{-kf} \mathcal{D}_{\mathrm{id}}$ and over all same-link meeting pairs to fall below $(b^{2f}/z)^k \mathcal{D}_{\mathrm{main}}$, controlled by {{concept:arithmetic-height|the global balance between discriminant size and congruence level}}.

### Limits of the intuition

Picturing pairs $(\gamma, \gamma')$ with distinct links as nearly orthogonal terms whose cross-products cancel by oscillation is misleading. `RemainingTerms/Sum.lean` obtains *zero* phase cancellation between distinct links: `enorm_amplitudeSum_mul_le` applies the triangle inequality and bounds each pair by the positive product `termSize γ * termSize γ'`. The huge saving $b^{-kf} \mathcal{D}_{\mathrm{id}}$ comes entirely from counting lattice pairs—specifically, the congruence saving $(2/M)^{2if}$ in `pairDiscriminant_sq_le` raised to the large power $k/2$, which overwhelms the height-weighted sum of Hermitian sublattices summed over all coranks $l \le n^3$.

## Level 2 — Undergrad Bridge

The remaining-term engine (`src/New/Spanning/RemainingTerms/`, 22 files ending in `Sum.lean`) adapts classical density/counting methods for automorphic periods (Kowalski, Saha & Tsimerman 2012; Blomer 2023) using an explicit Bombieri–Vaaler **Siegel's lemma** over CM number fields (Bombieri & Vaaler 1983, formalized in `RemainingTerms/SiegelLemma.lean`).

### 1. Links, near pairs, and the pair-discriminant gain (`Sum.lean`, lines 59–147)

Let $X = \langle e_1, \dots, e_\varrho \rangle$ be the standard isotropic span in $Y_0$. For $\gamma \in G(E)$, its link `link γ` is the $E$-subspace of $(X^\perp/X) \oplus (X^\perp/X)$ formed by pairs $([t], [\gamma^{-1} t])$ with $t \perp X, \gamma X$. Two terms $\gamma, \gamma'$ are in the same link class (`New.Filling.InSameLinkClass`) iff `S.termDisc γ ≠ 0`, `S.termDisc γ' ≠ 0`, and `link γ = link γ'`.

A **near pair over an agreeing block** (`IsNearPairOverAgreeingBlock c c' (γ, γ')`) consists of two chosen double-coset representatives over the same block $(c, s, c')$ whose archimedean cutoffs overlap (`boxedPoints`). For every such pair:
1. `one_le_pairDiscriminant`: $1 \le d := S.\mathrm{pairDiscriminant}\ c\ c'\ \gamma\ \gamma'$.
2. `pairDiscriminant_sq_le`: Letting $i = S.\mathrm{linkCorank}\ \gamma\ \gamma'$ be the codimension of `commonLink γ γ'` inside each link and $G(\gamma) = S.\mathrm{levelGauge}(\mathrm{cellRank}\,\gamma, c, c', \gamma)$,
   $$d^2 \le \Bigl(\frac{2}{M}\Bigr)^{2 i f}\, G(\gamma)\, G(\gamma').$$
3. `corank_le_and_pairDiscriminant_le`: The corank $l = k - \dim_E(\mathrm{commonLink}\ \gamma\ \gamma')$ satisfies $l \le n^3$, and $d \le S.\mathrm{discriminantBound} = b^{n^3 f}$.

### 2. Weighted Schur test and the unified pair-sum template (`SchurTest.lean`, `SiegelLemma.lean`, `Sum.lean:149–305`)

- **Orbit bound via weighted Schur test (`SchurTest.lean`, `Size.lean`):** For each term $\gamma$, `amplitudeMajorant_sq_le` applies Cauchy–Schwarz with the weight `schurWeight` carrying powers of `volumeRatio⁻¹` that invert under $\gamma \mapsto \gamma^{-1}$. Integrating over unipotent orbits yields `termSize_sq_le`:
  $$\mathrm{termSize}(c, c', \gamma)^2 \le \frac{\tau^4 D}{n_p\, G(\gamma)^k},$$
  where $\tau = S.\mathrm{tailScale}$, $n_p = S.\mathrm{pairCountScale}$, and $D \ge S.\mathrm{cellRankSecondMoment}(\mathrm{cellRank}\,\gamma)$.
- **Summing over sublattices via Siegel's lemma (`sum_termSize_mul_le`):** Whenever a set of near pairs satisfies $(d \eta)^2 \le G(\gamma) G(\gamma')$ and $\tau^{l+6} D \eta^{-k} \le B$, each pair contributes at most $B\, d^{-k} / (\tau^{l+2} n_p)$ (`ofReal_termSize_mul_le`). Summing $d^{-k}$ over all pairs of corank $l \le n^3$ uses `SiegelLemma.lean::tsum_sublatticeDiscriminant_inv_le` (cutting out each sublattice by short vectors and packing lattice points) and `cube_add_one_le_tailScale` ($n^3 + 1 \le \tau$) to conclude that the entire sum of `termSize γ * termSize γ'` over all classes $c, c'$ is at most $\tau B$.

### 3. Branching on `InSameLinkClass` and the two headline bounds (`Sum.lean:307–837`)

Splitting each block into pieces (`Piece`: one piece per link $\Lambda$ with $\mathsf{g} \neq 0$, plus one piece for $\mathsf{g} = 0$) separates the squared amplitude sum into diagonal same-link squares plus cross-link / $\mathsf{g} = 0$ pairs (`enorm_amplitudeSum_sq_le`):

1. **Not in the same link class (`sum_termSize_mul_not_inSameLinkClass_le`):** Either $i \ge 1$ (distinct links, gaining $\eta = (M/2)^{if}$ from `pairDiscriminant_sq_le` and crushed by `pow_mul_mainSecondMoment_div_le`) or $\mathsf{g} = 0$ (degenerate determinant, where the cell rank drops and `cellRankSecondMoment` is tiny). Summing via `sum_termSize_mul_le` bounds all such pairs by $b^{-kf} \mathcal{D}_{\mathrm{id}}$. Substituting into `enorm_amplitudeSum_sq_le` gives **`New.Filling.secondMoment_remainingTerms_le`**:
   $$\mathrm{linkSecondMoment}\ \{\gamma \mid \mathsf{g}(\gamma) \neq 0\}\ \Omega \le w\nu^2 \;\Longrightarrow\; \mathrm{secondMoment}\bigl(S.\mathrm{amplitudeSum}\ \mathrm{.cut}\ \Gamma^*\bigr)\ \Omega \le w + b^{-kf} \mathcal{D}_{\mathrm{id}}.$$
2. **Same link class with meeting reduction (`sum_termSize_mul_inSameLinkClass_le` $\Rightarrow$ `linkSecondMoment_le_of_le_meetNorm`):** When $\gamma, \gamma'$ share a non-degenerate link over a reduction $s \in \Omega$ whose frame span meets its $s$-image at places of norm product $S.\mathrm{meetNorm}\ s \ge z > 0$, the common-link discriminant gains a factor proportional to $z / b^{2f}$, yielding
   $$\mathrm{linkSecondMoment}\ \{\gamma \mid \mathsf{g}(\gamma) \neq 0\}\ \Omega \le \Bigl(\frac{b^{2f}}{z}\Bigr)^k \mathcal{D}_{\mathrm{main}}\, \nu^2.$$
   In `Moments.lean`, combining `secondMoment_remainingTerms_le`, `linkSecondMoment_le_of_le_meetNorm` (for non-open reductions where $z \ge p_0$ or $z \ge p_1$), and `EnergyEstimate.lean::linkSecondMoment_isOpenElement_le` (for open reductions) closes both the near-$\bar{R}_0$ and whole-group second-moment bounds.

## Level 3 — Formal Lean Contract

Stage 04 extracts the remaining-term link bounds into `skeleton/04_spanning/remaining_terms_sum.lean` (26 declarations, 0 stub truncations):

- `04_spanning/remaining_terms_sum.lean#New.Filling.InSameLinkClass` (upstream `src/New/Spanning/RemainingTerms/Sum.lean:307-311`):
  ```lean
  def New.Filling.InSameLinkClass {E : Type} [Field E] [CMType E] {𝔰 : Shape} (S : Filling E 𝔰)
      (γ γ' : S.G E) : Prop
  ```
- `04_spanning/remaining_terms_sum.lean#New.Filling.sum_termSize_mul_not_inSameLinkClass_le` (upstream `src/New/Spanning/RemainingTerms/Sum.lean:377-446`):
  ```lean
  theorem New.Filling.sum_termSize_mul_not_inSameLinkClass_le {E : Type} [Field E] [CMType E]
      {𝔰 : Shape} {S : Filling E 𝔰} [HasPrimitiveRoot5040 E] [S.IsAdmissible] :
      ∑ c : S.Classes, ∑ c' : S.Classes,
        ∑' q : {q : S.G E × S.G E |
          S.IsNearPairOverAgreeingBlock c c' q ∧ ¬S.InSameLinkClass q.1 q.2},
          ENNReal.ofReal
            (S.termSize c c' (q : S.G E × S.G E).1 * S.termSize c c' (q : S.G E × S.G E).2) ≤
        .ofReal (S.identitySecondMoment / S.b ^ (𝔰.k * numInfPlaces E))
  ```
- `04_spanning/remaining_terms_sum.lean#New.Filling.sum_termSize_mul_inSameLinkClass_le` (upstream `src/New/Spanning/RemainingTerms/Sum.lean:448-491`):
  ```lean
  theorem New.Filling.sum_termSize_mul_inSameLinkClass_le {E : Type} [Field E] [CMType E]
      {𝔰 : Shape} {S : Filling E 𝔰} [HasPrimitiveRoot5040 E] [S.IsAdmissible] {z : ℕ} (hz : z ≠ 0) :
      ∑ c : S.Classes, ∑ c' : S.Classes,
        ∑' q : {q : S.G E × S.G E | S.IsNearPairOverAgreeingBlock c c' q ∧
          S.InSameLinkClass q.1 q.2 ∧ ∃ s : S.ResidueGL, z ≤ S.meetNorm s ∧ q.1 ∈ S.termsOver c s c'},
          ENNReal.ofReal
            (S.termSize c c' (q : S.G E × S.G E).1 * S.termSize c c' (q : S.G E × S.G E).2) ≤
        .ofReal ((S.b ^ (2 * numInfPlaces E) / z) ^ 𝔰.k * S.mainSecondMoment)
  ```
- `04_spanning/remaining_terms_sum.lean#New.Filling.secondMoment_remainingTerms_le` (upstream `src/New/Spanning/RemainingTerms/Sum.lean:678-744`):
  ```lean
  theorem New.Filling.secondMoment_remainingTerms_le {E : Type} [Field E] [CMType E] {𝔰 : Shape} {S : Filling E 𝔰}
      [HasPrimitiveRoot5040 E] [S.IsAdmissible] {w : ℝ} {Ω : Set S.ResidueGL} (hw : 0 ≤ w)
      (h_linkMoment : linkSecondMoment {γ : S.G E | S.termDisc γ ≠ 0} Ω ≤
        ENNReal.ofReal (w * S.unipotentCovolume ^ 2)) :
      secondMoment (S.amplitudeSum .cut S.remainingTerms) Ω ≤
        w + S.identitySecondMoment / S.b ^ (𝔰.k * numInfPlaces E)
  ```
- `04_spanning/remaining_terms_sum.lean#New.Filling.linkSecondMoment_le_of_le_meetNorm` (upstream `src/New/Spanning/RemainingTerms/Sum.lean:746-837`):
  ```lean
  theorem New.Filling.linkSecondMoment_le_of_le_meetNorm {E : Type} [Field E] [CMType E]
      {𝔰 : Shape} {S : Filling E 𝔰} [HasPrimitiveRoot5040 E] [S.IsAdmissible]
      {Ω : Set S.ResidueGL} {z : ℕ} (hz : z ≠ 0)
      (h_meet : ∀ s ∈ Ω, z ≤ S.meetNorm s) :
      linkSecondMoment {γ : S.G E | S.termDisc γ ≠ 0} Ω ≤
        ENNReal.ofReal ((S.b ^ (2 * numInfPlaces E) / z) ^ 𝔰.k * S.mainSecondMoment *
          S.unipotentCovolume ^ 2)
  ```

See the page's live `<VerificationBanner />` (`audit/verification-status.json`) for the pinned upstream commit and local Lean build status (`build_status: unverified-locally`).
