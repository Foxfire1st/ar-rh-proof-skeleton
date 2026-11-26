---
slug: tight-frame
term: tight frame
aliases: ["Parseval frame", "eutactic star", "resolution of the identity"]
inline_glimpse: "A balanced, possibly overcomplete family of vectors with integer multiplicities whose projections reconstruct the exact standard inner product—allowing integral root-of-unity spokes to pull back one-dimensional elliptic theta sections into high-dimensional integral theta functions."
visual_ref: assets/stage02-tight-frame-to-integral-vacuum.svg
where_analogy_breaks: "Picturing a tight frame as equally spaced real spokes in $\\mathbb{R}^2$ or $\\mathbb{R}^3$ captures overcompleteness and isotropic length reconstruction, but misses two arithmetic layers: the spokes live in a CM field $E$ satisfying a Hermitian orthogonality identity across all embeddings of the CM type while remaining adelically integral ($\\alpha^{-1} w_j \\in \\widehat{\\mathcal{O}}_E$), and their integer multiplicities $m_j$ prescribe the exact degrees of one-dimensional theta sections pulled back into the adelic Heisenberg model."
sources:
  - title: "Low-dimensional lattices V: Integral coordinates for integral lattices"
    citation: "J. H. Conway & N. J. A. Sloane, Proc. R. Soc. Lond. A 426 (1989)."
    kind: paper
  - title: "Finite normalized tight frames"
    citation: "J. J. Benedetto & M. Fickus, Adv. Comput. Math. 18 (2003)."
    kind: paper
lean_anchors:
  - 02_derivatives/tight_frames.lean#New.Parameters.isTightFrame_pow
  - 02_derivatives/tight_frames.lean#New.Parameters.exists_isResolutionProduct_of_isTightFrame
  - 02_derivatives/theta_products.lean#New.Parameters.piPrimeInt_dvd_sum_linearSeries_sub_coupling
child_concepts:
  - prime-number
  - complex-plane
  - complex-multiplication
  - elliptic-theta-function
  - theta-normal-form
---

# Tight Frame

## Level 1 — First-Principles

In ordinary geometry, an orthonormal basis gives the cleanest possible coordinate system: project any vector onto each perpendicular axis, square the lengths of those shadows, and their sum recovers the squared length of the vector itself. Sometimes, however, insisting on perpendicular axes is too rigid—for example, when you want every axis vector to have clean algebraic coordinates without messy square roots. You can trade minimality for symmetry by using *more* vectors than dimensions. Picture three equally spaced spokes of a wheel in a flat plane, or the vertices of a regular $q$-gon around a circle: even though the spokes are redundant (linearly dependent), summing the squared projections along all spokes still equals a fixed constant multiple $M$ of the squared length of every vector. Such a balanced, overcomplete family of directions is a **tight frame** (known as a **Parseval frame** when normalized to $M = 1$, a **eutactic star** in lattice geometry, or a **resolution of the identity** once rescaled so that weighted projections reconstruct every vector verbatim).

In Stage 02 of the proof, tight frames solve a delicate geometric-versus-arithmetic dilemma. We work in a high-dimensional space over {{concept:complex-plane|the two-dimensional plane of real and imaginary coordinates}}, whose coordinates correspond to the distinct embeddings of a cyclotomic field built over a quadratic field with {{concept:complex-multiplication|an elliptic curve equipped with extra geometric symmetries beyond whole-number scaling}}. Our goal is to assemble high-dimensional theta functions by multiplying together one-dimensional {{concept:elliptic-theta-function|quasi-periodic building-block functions attached to a two-dimensional lattice}}, each evaluated along a linear slice. For the resulting product to transform like a genuine theta function and stay integral on finite-adelic lattices at every {{concept:prime-number|indivisible whole number $p \in \{2, 3, 5, 7, \dots\}$}}, the slice directions must satisfy two competing demands at once:

1. **Exact geometric balance:** the weighted sum of their pulled-back Hermitian forms must reproduce the exact standard inner product, with no skew or leftover cross-terms.
2. **Exact algebraic integrality:** every slice vector must lie in the integral adelic lattice of the field, free of prime denominators.

An orthonormal basis in general fails integrality, whereas the powers $1, \zeta, \zeta^2, \dots, \zeta^{q_1 - 1}$ of a primitive $q_1$-th root of unity automatically satisfy both: every power is an algebraic integer, and character cancellation forces all off-diagonal sums between distinct embeddings to vanish identically, giving a tight frame of length $M = q_1$.

The same resolution-of-the-identity property pays a second dividend when we reduce modulo a prime $p$. Taking a weighted sum of $(u_j^\dagger z)^{p^e}(u_j^\dagger z)$ across all spokes of an integral resolution of the identity collapses the individual frame vectors completely: every trace of which spokes were chosen cancels out, leaving a single universal polynomial coupling paired variables. That cancellation is the algebraic hinge that puts every frame-built product of one-dimensional theta sections into {{concept:theta-normal-form|canonical exponential factorization of a rescaled multivariable power expansion modulo a prime}}.

### Limits of the intuition

Picturing a tight frame as equally spaced real spokes on a circle captures overcompleteness and isotropic length recovery, but breaks down in two crucial ways here. First, the frame vectors $w_j$ are not mere real arrows in $\mathbb{R}^d$: they are elements of a number field $E$ viewed simultaneously across all infinite embeddings of a CM type over {{term:complex-plane|complex numbers}}, so the frame condition is a Hermitian orthogonality law $\sum_j m_j \Phi_\sigma(w_j)\overline{\Phi_\tau(w_j)} = M \delta_{\sigma\tau}$ paired with an adelic integrality requirement ($\alpha^{-1} w_j \in \widehat{\mathcal{O}}_E$). Second, the weights $m_j \in \mathbb{N}$ are not arbitrary positive scalars: each $m_j$ is a whole-number multiplicity fixing the exact degree $r + 2t + 3u = m_j$ of the monomial sections of the one-dimensional {{term:elliptic-theta-function}} pulled back along spoke $w_j$ on a {{term:complex-multiplication|CM lattice}} of matched area, so that the resulting vector in the adelic Heisenberg model is simultaneously integral at every prime and periodic under signed lattice shifts.

## Level 2 — Undergrad Bridge

In a finite-dimensional complex inner product space $H$, a finite family of vectors $(v_j)_{j \in I}$ with nonnegative weights $(m_j)_{j \in I}$ is a **tight frame** with frame bound $M > 0$ if
$$\sum_{j \in I} m_j\, |\langle z, v_j \rangle|^2 = M\,\|z\|^2 \qquad \text{for all } z \in H,$$
or equivalently, as an operator identity on $H$,
$$\sum_{j \in I} m_j\, v_j v_j^* = M\,\mathrm{Id}_H.$$
When $M = 1$, this is a **Parseval frame** (or **resolution of the identity** $z = \sum_j m_j \langle z, v_j \rangle v_j$). In Euclidean lattice theory, Conway and Sloane (1989) study rational/integral tight frames under the name *eutactic stars* to embed lattices with integral coordinates; Benedetto and Fickus (2003) develop the general theory of finite normalized tight frames.

Stage 02 instantiates this structure arithmetically over the tower of CM fields $E = P.E(s) = K(\zeta_{q_1})$, where $K = \mathbb{Q}(\sqrt{-d})$ ($d \equiv 7 \pmod 8$ prime), $\Phi = P.\Phi(s)$ is the chosen CM type of $E$, and $\alpha = P.\alpha(s)$ is the finite idele of the Weil data cutting out the fractional ideal $\mathfrak{a}_0 = E \cap \alpha \widehat{\mathcal{O}}_E$. A family $w : I \to E$ with integer multiplicities $m : I \to \mathbb{N}$ satisfies `IsTightFrame w m M` when:

1. **Adelic integrality:** $\alpha^{-1} w_j \in \widehat{\mathcal{O}}_E$ for every $j \in I$ (i.e., $w_j \in \mathfrak{a}_0$).
2. **CM-type orthogonality:** for all infinite places $\sigma, \tau \in \Phi$,
   $$\sum_{j \in I} m_j\, \Phi_\sigma(w_j)\,\overline{\Phi_\tau(w_j)} = M\,\delta_{\sigma\tau}.$$

Three structural consequences drive the Stage 02 pipeline:

- **Existence from roots of unity (`isTightFrame_pow`):** For any primitive $q_1$-th root of unity $z \in E$, distinct embeddings $\Phi_\sigma, \Phi_\tau$ take $z$ to distinct $q_1$-th roots of unity in $\mathbb{C}$. Hence $\Phi_\sigma(z)\overline{\Phi_\tau(z)} = \Phi_\sigma(z)/\Phi_\tau(z)$ is a nontrivial $q_1$-th root of unity for $\sigma \neq \tau$ and $1$ for $\sigma = \tau$, so geometric-series cancellation gives
  $$\sum_{i=0}^{q_1 - 1} \Phi_\sigma(z^i)\,\overline{\Phi_\tau(z^i)} = q_1\,\delta_{\sigma\tau}.$$
  Since every $z^i \in \mathcal{O}_E \subseteq \mathfrak{a}_0$, the powers $(z^i)_{0 \le i < q_1}$ with $m_i = 1$ form a tight frame of length $q_1$; repeating each spoke $k$ times (`isTightFrame_zeta_pow`) scales the frame bound to $k q_1$.
- **Pullback of Hermitian forms and resolution products (`exists_isResolutionProduct_of_isTightFrame`):** Fix an integer $\delta \in \mathcal{O}_K$ with $\delta\bar{\delta} = N \neq 0$, a CM lattice $C$ containing $a \in C.\Lambda$ of area
  $$\operatorname{area}(C) = \frac{|a|^2}{N}\, M\, \frac{\sqrt{d}}{2},$$
  and $b = a / \bar{\delta}$. For each $i = (j, k) \in I \times n$, define the linear form
  $$L_{j,k}(z) = \frac{b}{\sqrt{2\pi/\sqrt{d}}} \sum_{\tau \in \Phi} \overline{\Phi_\tau(w_j)}\, z_{\tau, k}.$$
  Pulling back the scaled Hermitian form $m_j H_C(u, v) = m_j \frac{\pi}{\operatorname{area}(C)} \bar{u} v$ along $L_{j,k}$ and summing over $I \times n$ cancels $\operatorname{area}(C)$ against $|b|^2 M \sqrt{d}/2$ via the tight-frame identity, recovering the exact standard Hermitian form $\sum_{\tau, k} \overline{u_{\tau,k}} v_{\tau,k}$ (`sum_pullForm_resolutionLinearForm_apply`). Consequently, for any choice of monomial sections $f_{j,k} = \theta^r (\sigma^2)^t G^u$ of degree $r + 2t + 3u = m_j$, there exists a vector $F \in \mathcal{S}(V)$ in the finite part of the adelic Heisenberg model (`IsResolutionProduct`) such that:
  1. Every rational translate $\varrho(\operatorname{finVec}(\gamma)) F$ has entire theta function equal to the product of twisted translates of $f_{j,k}$ along $L_{j,k}$;
  2. All finite-adelic evaluations $F(\operatorname{finVec} y)$ satisfy $\|\iota_p(F(\operatorname{finVec} y))\|_p \le 1$ at every prime $p$ and place $\iota_p$;
  3. $\operatorname{entireTheta} F$ is a theta function for the lattice $\bar{\delta}\mathfrak{a}_0^n$.
- **Collapse to Frobenius couplings modulo $\pi_p$ (`piPrimeInt_dvd_sum_linearSeries_sub_coupling`):** Passing to vectors $u_j \in E^n$ integral above $p$ that form a resolution of the identity $z = \sum_j m_j (u_j^\dagger z) u_j$ (so $\sum_j m_j \Phi_{\tau'}(u_{j,c})\overline{\Phi_\sigma(u_{j,c'})} = \delta_{\tau'\sigma}\delta_{cc'}$), the Frobenius congruence $\overline{\Phi_\tau(u_{j,c})}^{p^e} \equiv \Phi_{\tau'}(u_{j,c}) \pmod{\pi_p}$ for $e \in I_p^\circ$ and $\tau' = -p^e \tau$ implies
  $$\sum_{j \in J} m_j\, (u_j^\dagger z)^{p^e} (u_j^\dagger z) \equiv \sum_{j \in J} m_j \Bigl(\sum_{\tau, c} \Phi_{\tau'}(u_{j,c})\, z_{\tau,c}^{p^e}\Bigr)\Bigl(\sum_{\sigma, c'} \overline{\Phi_\sigma(u_{j,c'})}\, z_{\sigma,c'}\Bigr) = \sum_{\tau, c} z_{\tau,c}^{p^e} z_{\tau',c} = \Gamma_e(z) \pmod{\pi_p}.$$
  Every dependence on the specific frame $(u_j, m_j)$ disappears modulo $\pi_p$, feeding directly into `exists_hasNormalForm`.

## Level 3 — Formal Lean Contract

Stage 02 extracts the tight-frame theorems into `skeleton/02_derivatives/tight_frames.lean` (from upstream `src/New/Derivatives/Vacuum/TightFrames.lean`) and the modulo-$\pi_p$ coupling collapse into `skeleton/02_derivatives/theta_products.lean` (from upstream `src/New/Derivatives/FormalGroup/ThetaProducts.lean`):

- `02_derivatives/tight_frames.lean#New.Parameters.isTightFrame_pow` (upstream `src/New/Derivatives/Vacuum/TightFrames.lean:389-405`):
  ```lean
  lemma New.Parameters.isTightFrame_pow {z : P.E s} (hz : IsPrimitiveRoot z (P.q₁ s)) :
      IsTightFrame (fun i : Fin (P.q₁ s) ↦ z ^ i.val) 1 (P.q₁ s)
  ```
- `02_derivatives/tight_frames.lean#New.Parameters.exists_isResolutionProduct_of_isTightFrame` (upstream `src/New/Derivatives/Vacuum/TightFrames.lean:828-911`):
  ```lean
  lemma New.Parameters.exists_isResolutionProduct_of_isTightFrame (V : Space (P.E s) n ε) [hV : V.IsStd] {M N : ℕ}
      {I : Type} [Fintype I] {w : I → P.E s} {m : I → ℕ} (δ : 𝓞 P.K) (hN_ne : N ≠ 0)
      (hN : δ * conj (δ : ℂ) = N) (hw : IsTightFrame w m M) (x : I × n → ℕ × ℕ × ℕ)
      (hx : ∀ i : I × n, CMLattice.monomialDeg (x i) = m i.1)
      ⦃C : CMLattice P.d⦄ ⦃a : ℂ⦄ (ha : a ∈ C.Λ) (h_area : C.area = ‖a‖ ^ 2 / N * M * √P.d / 2) :
      ∃ F : 𝒮 V,
        IsResolutionProduct V C (a / conj (δ : ℂ)) w m (fun i : I × n ↦ C.monomialSection (x i)) F ∧
        (∀ (p : ℕ) [Fact p.Prime] [PadicPlace p] (y : n → 𝔸f (P.E s)), ‖ιₚ (F (finVec y))‖ ≤ 1) ∧
        IsThetaFunction (star (algebraMap P.K (P.E s) δ) • idealLattice (P.E s) n)
          (entireTheta F)
  ```
- `02_derivatives/theta_products.lean#New.Parameters.piPrimeInt_dvd_sum_linearSeries_sub_coupling` (upstream `src/New/Derivatives/FormalGroup/ThetaProducts.lean:243-276`):
  ```lean
  lemma New.Parameters.piPrimeInt_dvd_sum_linearSeries_sub_coupling (P : Parameters) (s : ℕ) {p : ℕ} [Fact p.Prime]
      [PadicPlace p]
      {n J : Type} [Fintype n] [Fintype J] {u : J → n → P.E s} {m : J → ℕ} [DecidableEq n]
      {e : ℕ} (he : e ∈ partnerExponents P.d p)
      (h_res : IsResolution u m) (hu : ∀ (j : J) (i : n), IsIntegralAbove p (u j i))
      {μ : J → P.Φ s × n → valRingCp p}
      (hμ : ∀ (j : J) (x : P.Φ s × n), (μ j x : ℂ_[p]) = daggerForms u j x) :
      C (piPrimeInt P.d) ∣
        ∑ j : J, m j * (linearSeries (μ j) ^ p ^ e * linearSeries (μ j)) -
          coupling (P.partner s p) e
  ```

Notice the clean structural separation across the two files: `New.Parameters.sum_pullForm_resolutionLinearForm_apply` turns `IsTightFrame` into the Hermitian form identity behind `New.Parameters.IsResolutionProduct` (producing integral, periodic vectors in `𝒮 V`), while `piPrimeInt_dvd_sum_linearSeries_sub_coupling` converts the unscaled resolution property `IsResolution u m` into the prime-level coupling congruence feeding `exists_hasNormalForm`. See the page's live verification banner (`audit/verification-status.json`) for the currently pinned upstream snapshot and local Lean build status (`build_status: unverified-locally`).
