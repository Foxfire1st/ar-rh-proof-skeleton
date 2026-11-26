---
slug: central-idempotent
term: central idempotent
aliases: ["cuspidal type", "good idempotent", "primitive polynomial label", "Plancherel share"]
inline_glimpse: "A class-function element of a finite group algebra satisfying z² = z that acts as a G-equivariant orthogonal projection, splitting good cuspidal types from rare bad types across two level primes."
visual_ref: assets/stage04-second-moment-spreading-funnel.svg
where_analogy_breaks: "Picturing a central idempotent as a clean band-pass filter that keeps 99.99% of frequencies and throws away a tiny bad corner captures why `1 - e_0` has small Plancherel share (`Re((1 - e_{p_i})(1)) ≤ b⁻⁸ / 2`), but hides why Stage 04 MUST split the level across TWO distinct primes `p_0` and `p_1`: bounding `‖(1 - e_{p_0} ⊗ e_{p_1}) P‖²` works one prime at a time by keeping the other prime fixed at `1`, which forces Bessel-subgroup terms to become purely unipotent (`mem_besselU_of_reduction_eq_one`) so the trace collapses to the reduced unipotent character."
sources:
  - title: "The characters of the finite general linear groups"
    citation: "J. A. Green, Trans. Amer. Math. Soc. 80 (1955), 402–447."
    kind: paper
  - title: "Finite Fields"
    citation: "R. Lidl & H. Niederreiter, Cambridge Univ. Press (1983), ch. 3."
    kind: book
  - title: "Finite Groups of Lie Type: Conjugacy Classes and Complex Characters"
    citation: "R. W. Carter, Wiley (1985)."
    kind: book
lean_anchors:
  - 04_spanning/spanning_theorem.lean#New.Data.goodIdempotentAbove
  - 04_spanning/spanning_theorem.lean#New.Data.bad_types_rare
  - 04_spanning/moments.lean#New.IsCentralProj.isSymmetricProjection
  - 04_spanning/moments.lean#New.Filling.HasAmplitudeBlocks.hilbertSchmidtNorm_one_sub_piTensorElem_comp_sq_le
child_concepts:
  - prime-number
  - trace-comparison
  - hilbert-schmidt-norm
  - bessel-orbital-integral
---

# Central Idempotent

## Level 1 — First-Principles

Whenever a finite symmetry group $G$ acts by rotations (unitary maps) on a complex vector space $V$, that space breaks apart into mutually perpendicular irreducible blocks. A **central idempotent** is a weighted average $z = \sum_{g \in G} z(g)\, g$ in the group algebra $\mathbb{C}[G]$ that satisfies two rules: its weights depend only on conjugacy classes (it commutes with every group element—hence *central*), and applying it twice is the same as applying it once ($z^2 = z$—hence *idempotent*). In any unitary representation of $G$, a **central idempotent** automatically acts as an orthogonal projection onto a $G$-stable subspace (`IsCentralProj.isSymmetricProjection`). Its coefficient at the identity, $\operatorname{Re} z(1) = \sum_{g \in G} |z(g)|^2$, is its **Plancherel share**: the fraction of the regular representation that $z$ retains.

In Stage 04, the finite group is $\bar{G} = G_{p_0} \times G_{p_1}$, a product of matrix groups $\mathrm{GL}_r(\mathbb{F}_w)$ over residue fields above two distinct level {{concept:prime-number|indivisible whole numbers $p_0, p_1$}}. Stage 04 needs to filter the principal-level automorphic forms down to a clean "target" subspace of **good forms** that simultaneously supports two demands:

1. **Cuspidality for matching traces:** At least one place above each level prime carries a **primitive polynomial label**—an irreducible monic degree-$r$ polynomial over the residue field whose roots generate the full multiplicative group of the degree-$r$ extension—inducing a **cuspidal type** that unlocks {{concept:trace-comparison|matching spherical Hecke traces between indefinite and definite unitary spaces}}.
2. **Negligible energy loss on the Poincaré map:** The complement $1 - e_0$ (the "bad types" lacking an allowed primitive label) must occupy such a tiny **Plancherel share** that projecting the scaled Poincaré map $P$ onto the good forms loses almost none of its {{concept:hilbert-schmidt-norm|root-sum-of-squares operator energy}}.

Stage 04 builds one **good idempotent** $e_{p_0} \in \mathbb{C}[G_{p_0}]$ above $p_0$ and a second **good idempotent** $e_{p_1} \in \mathbb{C}[G_{p_1}]$ above $p_1$, and takes their tensor product $e_0 = e_{p_0} \otimes e_{p_1}$. Classical counting of monic irreducible and primitive polynomials over finite fields proves `bad_types_rare`: each complement $1 - e_{p_i}$ has identity coefficient $\operatorname{Re}((1 - e_{p_i})(1)) \le b^{-8} / 2$. Because $e_0$ factors across two primes, Stage 04 splits
$$1 - e_{p_0} \otimes e_{p_1} = (1 - e_{p_0}) \otimes 1 + e_{p_0} \otimes (1 - e_{p_1})$$
and bounds each prime's loss separately while holding the other prime fixed at $1$, proving $\|(1 - e_0) P\|_{\mathrm{HS}}^2 \le 4 b^{-4} D_X$ via {{concept:bessel-orbital-integral|unfolded Petersson amplitude blocks and the induced unipotent character mean}}.

### Limits of the intuition

Picturing a **central idempotent** as a clean band-pass filter that passes nearly $100\%$ of modes and rejects a tiny bad band captures the small **Plancherel share** $\operatorname{Re}((1 - e_{p_i})(1)) \le b^{-8}/2$ and the orthogonal projection property, but hides why *two* distinct level primes $p_0, p_1$ are algebraically indispensable. If the level used only a single prime $p$, bounding $\|(1 - e_p) P\|_{\mathrm{HS}}^2$ would sum over all $s \in G_p$, mixing elements of the full Bessel subgroup $\bar{R}_0 = \bar{U}_0 \bar{H}_0$ rather than just its unipotent radical $\bar{U}_0$. With two primes, holding $s(i+1) = 1$ in `Moments.lean::mem_besselU_of_reduction_eq_one` kills the $U(X_0)$ factor completely and forces every contributing Bessel element to be purely unipotent, reducing $\operatorname{tr}(r_\psi(s))$ to $D_X \psi_U(s)$.

## Level 2 — Undergrad Bridge

In Green's parametrization of complex characters of $\mathrm{GL}_r(\mathbb{F}_q)$ (Green 1955; Carter 1985), irreducible cuspidal representations correspond to Frobenius orbits of regular characters of $\mathbb{F}_{q^r}^\times$, which can be indexed by monic irreducible polynomials of degree $r$ over $\mathbb{F}_q$. Counting primitive polynomials of degree $r$ over $\mathbb{F}_q$ via Möbius inversion (Lidl & Niederreiter 1983, ch. 3) shows that for large $q$, almost all monic degree-$r$ polynomials (and almost all Plancherel mass of $\mathrm{GL}_r(\mathbb{F}_q)$) fall into allowed cuspidal types containing a primitive label.

### 1. Construction of $e_{p_i}$ and rarity of bad types (`SpanningTheorem.lean`, `BadTypes.lean`)

For each level prime index $i \in \{0, 1\}$, `New.Data.goodIdempotentAbove i` defines $e_{p_i} \in \mathbb{C}[G_{p_i}]$ (where $G_{p_i} = \prod_{w \in \text{chosenPlaces } i} \mathrm{GL}_r(\mathbb{F}_w)$) as the sum of tensor products $\bigotimes_w e_{c_w}$ over tuples $c$ of allowed degree-$r$ polynomials above $p_i$ containing at least one primitive polynomial:
- **`isCentralProj_goodIdempotentAbove`:** Each $e_{p_i}$ satisfies `IsCentralProj`—it is idempotent, central (a class function), and hence self-adjoint in any unitary representation (`IsCentralProj.isSymmetricProjection`).
- **`levelRepresentation_goodIdempotentAbove_eq`:** Their tensor product `piTensorElem D.goodIdempotentAbove` $= e_{p_0} \otimes e_{p_1}$ acts through `D.levelRepresentation` identically to the target projector `D.e₀`.
- **`re_coeff_one_sub_goodIdempotentAbove_le`:** Combining `MainTerm/BadTypes.lean` with Green's degree formula yields the Plancherel-share bound
  $$\operatorname{Re}\bigl((1 - e_{p_i})(1)\bigr) \le \frac{b^{-8}}{2} \qquad (i \in \{0, 1\}),$$
  packaged together as **`New.Data.bad_types_rare`**.

### 2. Why two primes turn $\operatorname{Re}(x(1)) \le b^{-8}/2$ into $\|(1 - e_0) P\|_{\mathrm{HS}}^2 \le 4 b^{-4} D_X$ (`Moments.lean`)

Let $x = 1 - e_{p_i} \in \mathbb{C}[G_{p_i}]$ be a central idempotent at one prime with $\operatorname{Re}(x(1)) \le b^{-8}/2$. By `sum_norm_sq_coeff_of_isCentralProj`, $\sum_{t \in G_{p_i}} |x(t)|^2 = \operatorname{Re}(x(1)) \le b^{-8}/2$. Embedding $G_{p_i} \hookrightarrow \bar{G}$ via `Pi.mulSingle i` (fixing the *other* prime $p_{i+1}$ at $1$):
1. **Unfolding to trace + remainder:** Since $x$ acts as an orthogonal projection, $\|x P\|_{\mathrm{HS}}^2 = \operatorname{tr}(P^* x P) = \operatorname{tr}(r_\psi(x)) + \operatorname{tr}\bigl(\sum_t x(t)(P_t - r_\psi(t))\bigr)$.
2. **Trace term ($\le b^{-4} D_X$):** Because the other prime component is $1$, `mem_besselU_of_reduction_eq_one` forces every contributing rational Bessel element into the unipotent subgroup `besselU`, so `trace_besselTranslate_eq_mul_reducedChar` evaluates $\operatorname{tr}(r_\psi(t)) = D_X \psi_U(t)$. Averaging over conjugates replaces $\psi_U$ by the class function $\psi_U^\natural = \mathrm{reducedCharMean}$; Cauchy–Schwarz against `sum_norm_reducedCharMean_sq_le` ($\sum |\psi_U^\natural|^2 \le 2$) gives
   $$\operatorname{Re}\,\operatorname{tr}(r_\psi(x)) \le D_X \sqrt{\sum |x(t)|^2} \sqrt{\sum |\psi_U^\natural(t)|^2} \le D_X \sqrt{b^{-8}/2} \sqrt{2} = b^{-4} D_X$$
   (`re_sum_coeff_mul_trace_besselTranslate_le`).
3. **Remainder term ($\le b^{-6} D_X \le b^{-4} D_X$):** `hilbertSchmidtNorm_sum_smul_sub_besselTranslate_sq_le` bounds $\|\sum_t x(t)(P_t - r_\psi(t))\|_{\mathrm{HS}}^2 \le b^{-12} D_X$ using the second moment near $\bar{R}_0$, so Cauchy–Schwarz against $\mathrm{Id}$ adds at most $b^{-6} D_X$.
4. **Combining the two primes:** Each prime contributes $\|x_i P\|_{\mathrm{HS}}^2 \le 2 b^{-4} D_X$ (`hilbertSchmidtNorm_asAlgebraHom_comp_sq_le`). Applying `hilbertSchmidtNorm_one_sub_mul_comp_sq_le` to $1 - e_{p_0} \otimes e_{p_1}$ sums the two prime losses to
   $$\|(1 - e_0) P\|_{\mathrm{HS}}^2 \le 4 b^{-4} D_X$$
   (`hilbertSchmidtNorm_one_sub_piTensorElem_comp_sq_le`), which combines with $\|P^* P - 1\|_{\mathrm{HS}}^2 \le b^{-12} D_X$ to yield the near-isometry error $5 b^{-4}$ in `isNearIsometry_piTensorElem_comp`.

## Level 3 — Formal Lean Contract

Stage 04 extracts the good-idempotent definitions, rarity bound, and projection loss bounds into `skeleton/04_spanning/spanning_theorem.lean` and `skeleton/04_spanning/moments.lean`:

- `04_spanning/spanning_theorem.lean#New.Data.goodIdempotentAbove` (upstream `src/New/Spanning/SecondMoment/SpanningTheorem.lean:176-182`):
  ```lean
  noncomputable def New.Data.goodIdempotentAbove (i : Fin 2) :
      MonoidAlgebra ℂ (∀ w : D.filling.chosenPlaces i, GL D.RowIndex (ResField w.val))
  ```
- `04_spanning/spanning_theorem.lean#New.Data.bad_types_rare` (upstream `src/New/Spanning/SecondMoment/SpanningTheorem.lean:234-245`):
  ```lean
  theorem New.Data.bad_types_rare :
      (∀ i : Fin 2, IsCentralProj (D.goodIdempotentAbove i)) ∧
      D.levelRepresentation.asAlgebraHom (piTensorElem D.goodIdempotentAbove) =
        (D.principalRep (D.spaceY 0)).asAlgebraHom D.e₀ ∧
      ∀ i : Fin 2, ((1 - D.goodIdempotentAbove i).coeff 1).re ≤ (D.b ^ 8)⁻¹ / 2
  ```
- `04_spanning/moments.lean#New.IsCentralProj.isSymmetricProjection` (upstream `src/New/Spanning/Unfolding/Moments.lean:75-82`):
  ```lean
  lemma New.IsCentralProj.isSymmetricProjection {H : Type} [Group H] [Fintype H]
      {ρ : Representation ℂ H V} (hρ : ∀ (s : H) (x y : V), ⟪ρ s x, ρ s y⟫_ℂ = ⟪x, y⟫_ℂ)
      {z : MonoidAlgebra ℂ H} (hz : IsCentralProj z) :
      (ρ.asAlgebraHom z).IsSymmetricProjection
  ```
- `04_spanning/moments.lean#New.Filling.HasAmplitudeBlocks.hilbertSchmidtNorm_one_sub_piTensorElem_comp_sq_le` (upstream `src/New/Spanning/Unfolding/Moments.lean:1177-1217`):
  ```lean
  theorem New.Filling.HasAmplitudeBlocks.hilbertSchmidtNorm_one_sub_piTensorElem_comp_sq_le [WeilData E]
      (hP : S.HasAmplitudeBlocks ρ P) (hρ : ∀ (s : S.ResidueGL) (x y : V), ⟪ρ s x, ρ s y⟫_ℂ = ⟪x,
        y⟫_ℂ)
      (hg : ∀ i : Fin 2, IsCentralProj (g i))
      (hg_small : ∀ i : Fin 2, ((1 - g i).coeff 1).re ≤ (S.b ^ 8)⁻¹ / 2) :
      hilbertSchmidtNorm ((1 - ρ.asAlgebraHom (piTensorElem g)) ∘ₗ P) ^ 2 ≤
        4 * (S.b ^ 4)⁻¹ * S.sourceDim
  ```

See the page's live `<VerificationBanner />` (`audit/verification-status.json`) for the pinned upstream commit and local Lean build status (`build_status: unverified-locally`).
