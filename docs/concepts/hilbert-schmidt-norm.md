---
slug: hilbert-schmidt-norm
term: Hilbert–Schmidt norm
aliases: ["Hilbert-Schmidt norm", "near isometry", "Welch bound", "spreading property", "greedy covering"]
inline_glimpse: "The Frobenius-style root-sum-of-squares norm on linear maps whose second moment over group translates, paired with near-isometry on the good subspace, proves Welch-style spreading and Wolsey greedy covering for Theorem B."
visual_ref: assets/stage04-second-moment-spreading-funnel.svg
where_analogy_breaks: "Picturing the Hilbert–Schmidt second moment as scattering unit vectors uniformly on a sphere so no hyperplane traps too many of them captures the exact Cauchy–Schwarz tension behind the Welch bound, but hides two structural details: Stage 04 spreads subspaces rather than single vectors—tracking the operator norm of `P* s P` across the finite product group `Ḡ = G_{p_0} × G_{p_1}`—and the greedy covering step (`IsSpread.exists_few_le_finrank_iSup_range`) lives in `FewTranslates.lean` and is consumed in Stage 01 (`ShortAndEnough.lean::enough_forms`), where restricting from `SourceForm` to `ramifiedSourceForms` costs an extra `2 / b^2` share on top of the `16 ε₀ = 2 / b^2` greedy miss."
sources:
  - title: "Lower bounds on the maximum cross correlation of signals"
    citation: "L. R. Welch, IEEE Trans. Inform. Theory 20 (1974), 397–399."
    kind: paper
  - title: "An analysis of the greedy algorithm for the submodular set covering problem"
    citation: "L. A. Wolsey, Combinatorica 2 (1982), 385–393."
    kind: paper
  - title: "Finite normalized tight frames"
    citation: "J. J. Benedetto & M. Fickus, Adv. Comput. Math. 18 (2003), 357–385."
    kind: paper
lean_anchors:
  - 04_spanning/spanning_theorem.lean#New.Data.isNearIsometry_e₀_comp_scaledPoincare
  - 04_spanning/spanning_theorem.lean#New.Data.sum_hilbertSchmidtNorm_scaledPoincare_sq_le
  - 04_spanning/spanning_theorem.lean#New.Data.isSpread_goodPoincare
  - 04_spanning/moments.lean#New.Filling.HasAmplitudeBlocks.hilbertSchmidtNorm_sub_one_sq_le
child_concepts:
  - tight-frame
  - gram-determinant
  - central-idempotent
  - trace-comparison
  - bessel-orbital-integral
---

# Hilbert–Schmidt Norm

## Level 1 — First-Principles

Given an $m \times n$ matrix $A$ representing a linear map between two inner-product spaces, the simplest way to measure its total size is to square the magnitude of every entry, add them all up, and take the square root: $\|A\|_{\mathrm{HS}} = \sqrt{\sum_{i,j} |A_{ij}|^2} = \sqrt{\operatorname{tr}(A^* A)}$. This root-sum-of-squares size is the **Hilbert–Schmidt norm** (often called the Frobenius norm for matrices). Unlike the ordinary operator norm, it is invariant under rotating either the domain or the codomain, turns linear maps themselves into vectors in a larger inner-product space via $\langle A, B \rangle_{\mathrm{HS}} = \operatorname{tr}(A^* B)$, and measures the dimension of any orthogonal projection directly: $\|\mathrm{pr}_U\|_{\mathrm{HS}}^2 = \dim U$.

In Stage 04, this norm powers a three-step geometric funnel proving that a small batch of group translates of the Poincaré map $P$ cannot collapse into a thin corner of the target space:

1. **Near isometry (`IsNearIsometry`):** If $P^* P$ stays close to the identity matrix in squared **Hilbert–Schmidt norm** ($\|P^* P - 1\|_{\mathrm{HS}}^2 \le b^{-12} D_X$) and cutting down to good target forms via a {{concept:central-idempotent|orthogonal projection selecting well-behaved cuspidal types from a finite group algebra}} loses at most $4 b^{-4} D_X$, then the projected map $e_0 P$ preserves the squared energy of every subspace $U$ up to a tiny error: $\|e_0 P \mathrm{pr}_U\|_{\mathrm{HS}}^2 \ge \dim U - 5 b^{-4} D_X$. This is the **near isometry** condition—it guarantees that $e_0 P$ never collapses large subspaces to zero.
2. **Welch-type second moment and spreading (`IsSpread`):** Suppose a finite group $\bar{G}$ moves the image of $P$ around unitarily. By Cauchy–Schwarz applied in the space of Hilbert–Schmidt operators, the sum $\sum_{s \in \bar{G}} \|P^* s P\|_{\mathrm{HS}}^2$ has a universal floor—the matrix analogue of the **Welch bound** from signal processing and {{concept:tight-frame|balanced overcomplete systems of vectors whose pairwise inner products minimize total squared overlap}} theory—equal to $\#\bar{G}\, D_X^2 / d_T$, where $d_T$ is the target dimension bounded by {{concept:trace-comparison|matching traces across spaces with different archimedean signatures}}. Stage 04 proves via {{concept:bessel-orbital-integral|unfolded Petersson block amplitudes over double cosets}} that the actual second moment overshoots that theoretical floor by at most a factor $(1 + 9 b^{-4})$. That near-tightness forces the **spreading property**: against *any* already-covered subspace $F$ occupying a fraction $\alpha$ of the target space, at least one translate $s e_0 P$ has preimage of $F$ of dimension at most $\frac{1 + \alpha + 3\varepsilon_0}{2} D_X$, meaning it adds at least roughly $\frac{1 - \alpha}{2} D_X$ brand-new dimensions outside $F$.
3. **Greedy covering (`FewTranslates` $\to$ Theorem B):** Once every partial subspace $F$ can be enlarged by a fixed fraction of its remaining codimension using a single translate, picking translates one by one—**greedy covering**—shrinks the missing dimension exponentially fast. Only about $\frac{5}{2}(1 + \log(1/(16\varepsilon_0)))\, d_T / D_X \le 5 \log(1 + 2b^2)\, d_T / D_X$ translates suffice to span all but a tiny $4/b^2$ fraction of the target space, feeding directly into Stage 01's {{concept:gram-determinant|squared parallelotope volume controlling the covolume of the target lattice}} argument.

### Limits of the intuition

Picturing the **Hilbert–Schmidt norm** second moment as spreading unit vectors evenly on a sphere so no hyperplane traps too many of them captures the exact Cauchy–Schwarz tension behind the **Welch bound**, but misses two architectural details of the Lean formalization. First, Stage 04 never spreads individual vectors: it spreads whole $D_X$-dimensional images of $e_0 s P$ across the finite product group $\bar{G} = G_{p_0} \times G_{p_1}$, measuring pairwise overlap through the trace of $(s P P^* s^{-1})^*(t P P^* t^{-1}) = \|P^* s^{-1} t P\|_{\mathrm{HS}}^2$. Second, Stage 04 stops at the one-translate **spreading property** `New.Data.isSpread_goodPoincare`; the **greedy covering** loop `New.IsSpread.exists_few_le_finrank_iSup_range` lives in upstream `FewTranslates.lean` and is invoked in Stage 01 (`ShortAndEnough.lean::New.Data.enough_forms`), where restricting from `SourceForm` to `ramifiedSourceForms` adds an extra $2/b^2$ share of codimension loss on top of the $16\varepsilon_0 = 2/b^2$ greedy remainder to produce the final $(1 - 4/b^2) d_T$ bound.

## Level 2 — Undergrad Bridge

For finite-dimensional complex inner-product spaces $S, T$, `New.hilbertSchmidtVector A` embeds a linear map $A : S \to_{\mathbb{C}} T$ into $\ell^2(\{1,\dots,\dim S\}, T)$ by evaluating $A$ on an orthonormal basis $(b_i)$ of $S$, giving the inner product $\langle A, B \rangle_{\mathrm{HS}} = \operatorname{tr}_S(A^* B)$ and norm
$$\|A\|_{\mathrm{HS}}^2 = \sum_{i} \|A b_i\|^2 = \operatorname{tr}_S(A^* A).$$
For any orthogonal projection $\mathrm{pr}_U$ onto a subspace $U \subseteq S$, $\|\mathrm{pr}_U\|_{\mathrm{HS}}^2 = \dim U$.

### 1. From Gram defect to near isometry (`FewTranslates.lean`, `Moments.lean`)

A map $A : S \to T$ satisfies `IsNearIsometry A ε` if for every subspace $U \subseteq S$,
$$\|A\,\mathrm{pr}_U\|_{\mathrm{HS}}^2 \ge \dim U - \varepsilon \dim S.$$
If $\|A^* A - 1\|_{\mathrm{HS}}^2 \le \varepsilon^2 \dim S$, Cauchy–Schwarz on $\operatorname{Re}\langle \mathrm{pr}_U, A^* A - 1 \rangle_{\mathrm{HS}}$ yields `IsNearIsometry A ε` (`isNearIsometry_of_hilbertSchmidtNorm_le`). In Stage 04:
- `Moments.lean::hilbertSchmidtNorm_sub_one_sq_le` bounds $\|P^* P - 1\|_{\mathrm{HS}}^2 \le b^{-12} D_X$, giving `IsNearIsometry P b⁻⁶` (a fortiori `b⁻⁴`);
- `Moments.lean::hilbertSchmidtNorm_one_sub_piTensorElem_comp_sq_le` bounds $\|(1 - e_0) P\|_{\mathrm{HS}}^2 \le 4 b^{-4} D_X$;
- `IsNearIsometry.proj_comp` adds the two errors to give `New.Data.isNearIsometry_e₀_comp_scaledPoincare`: `IsNearIsometry (e₀ ∘ₗ P) (5 * b⁻⁴)`.

### 2. Welch-type Cauchy–Schwarz and one good translate (`isSpread_of_second_moment`)

Let $\rho : \bar{G} \to U(T)$ be a unitary representation commuting with an orthogonal projection $\mathrm{pr}_0$ onto $T_0 \subseteq T$ of dimension $d_T = \dim T_0$, and set $S_B = \sum_{s \in \bar{G}} \rho(s) B B^* \rho(s)^{-1}$. Because $\langle \rho(s) B B^* \rho(s)^{-1}, \rho(t) B B^* \rho(t)^{-1}\rangle_{\mathrm{HS}} = \|B^* \rho(s^{-1} t) B\|_{\mathrm{HS}}^2$ (`inner_hilbertSchmidtVector_translate`), the squared Hilbert–Schmidt norm of $S_B$ is exactly $\#\bar{G} \sum_{s \in \bar{G}} \|B^* \rho(s) B\|_{\mathrm{HS}}^2$. For any subspace $W \subseteq T_0$ of share $\alpha = \dim W / d_T$, Cauchy–Schwarz against $\mathrm{pr}_W$ gives (`sq_sum_hilbertSchmidtNorm_orthoProj_comp_le`):
$$\Bigl(\sum_{s \in \bar{G}} \|\mathrm{pr}_W \rho(s) B\|_{\mathrm{HS}}^2\Bigr)^2 \le \dim W \cdot \#\bar{G} \sum_{s \in \bar{G}} \|B^* \rho(s) B\|_{\mathrm{HS}}^2.$$
Note the Welch-type floor (Welch 1974; Benedetto & Fickus 2003): taking $W = T_0$ shows $\sum_{s} \|B^* \rho(s) B\|_{\mathrm{HS}}^2 \ge \frac{(\sum_s \|\mathrm{pr}_0 \rho(s) B\|_{\mathrm{HS}}^2)^2}{d_T \#\bar{G}} \approx \frac{\#\bar{G}\, D_X^2}{d_T}$. Conversely, if $d_T \sum_{s \in \bar{G}} \|B^* \rho(s) B\|_{\mathrm{HS}}^2 \le \#\bar{G} (1 + \varepsilon) D_X^2$, AM–GM gives $\sqrt{\alpha(1+\varepsilon)} \le \frac{\alpha + 1 + \varepsilon}{2}$, so some $s \in \bar{G}$ satisfies
$$\|\mathrm{pr}_W \rho(s) B\|_{\mathrm{HS}}^2 \le \frac{\alpha + 1 + \varepsilon}{2} D_X.$$
Letting $W' = (\mathrm{pr}_0 \rho(s) B)^{-1}(W) \subseteq S$, we have $\rho(s)\mathrm{pr}_0 B|_{W'} = \mathrm{pr}_W \rho(s) B|_{W'}$, so combining with `IsNearIsometry (pr₀ ∘ₗ B) ε` gives
$$\dim W' - \varepsilon D_X \le \|\mathrm{pr}_0 B \mathrm{pr}_{W'}\|_{\mathrm{HS}}^2 \le \|\mathrm{pr}_W \rho(s) B\|_{\mathrm{HS}}^2 \le \frac{\alpha + 1 + \varepsilon}{2} D_X \implies \dim W' \le \Bigl(\frac{1 + \alpha}{2} + \frac{3}{2}\varepsilon\Bigr) D_X.$$
Setting $\varepsilon_0 = (8 b^2)^{-1}$ (which dominates both $5 b^{-4}$ and $9 b^{-4}$) yields `New.Data.isSpread_goodPoincare` with spreading error $\frac{3}{2}\varepsilon_0 = \frac{3}{2}(8 b^2)^{-1}$.

### 3. Wolsey greedy covering (`FewTranslates.lean` $\to$ `ShortAndEnough.lean`)

By `finrank_sup_range_add_finrank_comap`, adding the image of $B_s$ to $F$ increases dimension by $D_X - \dim B_s^{-1}(F)$. Whenever $1 - \alpha > 16\varepsilon_0$, the spreading bound leaves at least $\bigl(\frac{1}{2} - \frac{3}{32}\bigr)(1 - \alpha) D_X \ge \frac{2}{5}(1 - \alpha) D_X$ new dimensions, multiplying the codimension by at most $1 - r \le e^{-r}$ with $r = \frac{2 D_X}{5 d_T}$ (Wolsey 1982). After $l = \lceil \log(1/(16\varepsilon_0)) / r \rceil$ greedy steps, at most a share $16\varepsilon_0 = 2/b^2$ of $d_T$ remains uncovered, using $\#t \cdot D_X \le \frac{5}{2}(1 + \log(1/(16\varepsilon_0))) d_T \le 5 d_T \log(1 + 2 b^2)$.

## Level 3 — Formal Lean Contract

Stage 04 extracts the near-isometry, second-moment, and spreading theorems into `skeleton/04_spanning/moments.lean` and `skeleton/04_spanning/spanning_theorem.lean`:

- `04_spanning/moments.lean#New.Filling.HasAmplitudeBlocks.hilbertSchmidtNorm_sub_one_sq_le` (upstream `src/New/Spanning/Unfolding/Moments.lean:473-512`):
  ```lean
  theorem New.Filling.HasAmplitudeBlocks.hilbertSchmidtNorm_sub_one_sq_le [HasPrimitiveRoot5040 E]
      (hP : S.HasAmplitudeBlocks ρ P) :
      hilbertSchmidtNorm (P.adjoint ∘ₗ P - 1) ^ 2 ≤ S.sourceDim / S.b ^ 12
  ```
- `04_spanning/spanning_theorem.lean#New.Data.isNearIsometry_e₀_comp_scaledPoincare` (upstream `src/New/Spanning/SecondMoment/SpanningTheorem.lean:247-259`):
  ```lean
  theorem New.Data.isNearIsometry_e₀_comp_scaledPoincare :
      IsNearIsometry ((D.principalRep (D.spaceY 0)).asAlgebraHom D.e₀ ∘ₗ D.scaledPoincare)
        (5 * (D.b ^ 4)⁻¹)
  ```
- `04_spanning/spanning_theorem.lean#New.Data.sum_hilbertSchmidtNorm_scaledPoincare_sq_le` (upstream `src/New/Spanning/SecondMoment/SpanningTheorem.lean:261-276`):
  ```lean
  theorem New.Data.sum_hilbertSchmidtNorm_scaledPoincare_sq_le [Fintype D.chosenGL] :
      ∑ s : D.chosenGL,
          hilbertSchmidtNorm (D.scaledPoincare.adjoint ∘ₗ D.principalRep (D.spaceY 0) s ∘ₗ
            D.scaledPoincare) ^ 2 ≤
        (1 + 9 / D.b ^ 4) * D.filling.mainSecondMoment
  ```
- `04_spanning/spanning_theorem.lean#New.Data.isSpread_goodPoincare` (upstream `src/New/Spanning/SecondMoment/SpanningTheorem.lean:327-359`):
  ```lean
  theorem New.Data.isSpread_goodPoincare : IsSpread D.goodPoincare (3 / 2 * (8 * D.b ^ 2)⁻¹)
  ```

See the page's live `<VerificationBanner />` (`audit/verification-status.json`) for the pinned upstream commit and local Lean build status (`build_status: unverified-locally`).
