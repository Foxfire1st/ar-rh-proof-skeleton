---
slug: elliptic-theta-function
term: elliptic theta function
aliases: ["theta product"]
inline_glimpse: "A quasi-periodic entire function on a CM lattice normalized at the origin, whose finite products along tight-frame projections build integral multi-variable theta functions and collapse p-adically into exponential normal forms."
visual_ref: assets/stage02-tight-frame-to-integral-vacuum.svg
where_analogy_breaks: "Thinking of an elliptic theta function as a doubly periodic trigonometric wave like $\\cos(x)\\cos(y)$ misses Liouville's obstruction: no non-constant entire function can be strictly doubly periodic, so $\\theta(z)$ must absorb a Gaussian multiplier on every lattice shift, and a multi-variable theta product only satisfies the right transformation law when the pullback of those quadratic multipliers along a tight frame sums exactly to the standard Hermitian form."
sources:
  - title: "Tata Lectures on Theta I"
    citation: "D. Mumford, Progress in Mathematics 28, Birkhäuser (1983)."
    kind: book
  - title: "On the equations defining abelian varieties I"
    citation: "D. Mumford, Inventiones Mathematicae 1 (1966), pp. 287–354."
    kind: paper
  - title: "Divisibility of anticyclotomic L-functions and theta functions with complex multiplication"
    citation: "T. Finis, Annals of Mathematics 163 (2006), pp. 767–807."
    kind: paper
lean_anchors:
  - 02_derivatives/tight_frames.lean#New.Parameters.IsResolutionProduct
  - 02_derivatives/integral_vacuum.lean#New.Parameters.IsThetaProduct
  - 02_derivatives/theta_products.lean#New.CMLattice.rescale_thetaProd_eq_mul_expSubst
child_concepts:
  - complex-plane
  - complex-multiplication
  - tight-frame
  - theta-normal-form
  - arithmetic-height
---

# Elliptic Theta Function

## Level 1 — First-Principles

On the real line, sine and cosine repeat identically whenever you step forward by $2\pi$. Now try the same trick in two dimensions on {{concept:complex-plane|the two-dimensional plane of real and imaginary coordinates}}: ask for a smooth, pole-free (entire) function that repeats identically along every vector of a two-dimensional grid $\Lambda$. A classical theorem of Liouville says this is impossible unless the function is flat constant—an entire function bounded on one repeating cell would be bounded everywhere, hence constant. An **elliptic theta function** $\theta(z)$ breaks this deadlock by allowing a predictable multiplier whenever you translate by a grid vector $l \in \Lambda$: shifting $z \mapsto z + l$ multiplies $\theta(z)$ by an explicit exponential factor of $z$ and $l$, while keeping $\theta$ completely pole-free and normalized to $\theta(0) = 1$.

Stage 02 needs to control high-dimensional theta functions on a many-variable space attached to a field $E$, both in ordinary magnitude and prime by prime in {{concept:arithmetic-height|the global balance between ordinary size and divisibility at every prime}}. Analyzing a genuinely multi-variable function all at once is daunting, so the proof builds it from one-variable **elliptic theta functions** attached to a {{concept:complex-multiplication|grid with extra scaling-and-rotation symmetries by integers of $K = \mathbb{Q}(\sqrt{-d})$}}. Take a {{concept:tight-frame|balanced collection of projection directions whose weighted outer products reproduce the identity}}, project a high-dimensional vector $z$ onto each direction, evaluate a one-variable **elliptic theta function** (or a higher-degree monomial built from it) on every projection, and multiply the results together. The resulting multi-variable function is a **theta product**.

Factorizing a high-dimensional function into a **theta product** pays off twice across Stage 02. First, because the directions form a {{term:tight-frame}}, the individual one-variable exponential multipliers add up to the exact global transformation law required on the big space; averaging finite translates of a single **theta product** then recovers an integral multiple of the vacuum vector with controlled $p$-adic values and bounded Galois conjugates. Second, when we zoom into the Taylor expansion $p$-adically at a small rescaling factor $\varpi$, each one-variable factor splits into an integral power series times an exponential; multiplying those factors turns the product of exponentials into a *single* exponential of a sum over the frame directions. That sum is precisely what collapses modulo a prime uniformizer into the {{concept:theta-normal-form|canonical $p$-adic factorization pairing coordinates shifted by powers of $p$}}.

### Limits of the intuition

Thinking of an **elliptic theta function** as a two-dimensional cousin of cosine, and of a **theta product** as a simple separable product $f(x_1)\cdots f(x_n)$ along orthogonal axes, captures why one-variable estimates multiply cleanly. Both pictures miss essential twists in Stage 02. First, the quasi-periodicity multiplier of an **elliptic theta function** is genuinely quadratic in the shift $l$, which is why the linear projections must come from an overcomplete {{term:tight-frame}} rather than an orthogonal basis, and why the lattice area must satisfy an exact compatibility equation with the frame bound. Second, a **theta product** in the proof is not merely a classical function of {{term:complex-plane|complex numbers}} on $\mathbb{C}^N$: it simultaneously lives as a Schwartz vector in the Weil representation, so translating by rational points mixes finite-adelic shifts with archimedean twisted translations of each one-variable factor.

## Level 2 — Undergrad Bridge

Let $C$ be a CM lattice in $\mathbb{C}$ with period parallelogram area $A = C.\mathrm{area}$ and a chosen $2$-torsion point $t$. Its **elliptic theta function** $\theta = C.\theta$ is the Weierstrass sigma function $\sigma(z)$ translated by $t$ and normalized so that $\theta(0) = 1$ (Mumford 1983; Mumford 1966). For degree $m \in \mathbb{N}$ and shift $l \in \mathbb{C}$, the degree-$m$ twisted translation operator (`C.twistedTranslate m l`) is
$$(T_{m,l} g)(z) = \exp\!\left(-\frac{m\pi}{A}\bigl(z\bar{l} + \tfrac{1}{2}|l|^2\bigr)\right) g(z + l).$$
An entire function $f : \mathbb{C} \to \mathbb{C}$ is a theta section of degree $m$ for $C$ if $T_{m,l} f = \chi(l)^m f$ for all $l \in C.\Lambda$, where $\chi(l) = C.\mathrm{multiplier}(l)$ is the lattice multiplier. Besides $\theta$ (degree $1$), the monomial sections $C.\mathrm{monomialSection}(r, t, u) = \theta^r (\sigma^2)^t G^u$—where $G = C.\mathrm{translatedY} \cdot \theta^3$ is entire—provide integral sections of any degree $m = r + 2t + 3u$.

Stage 02 deploys products of one-variable theta sections along two complementary pipelines:

1. **Resolution products and the integral vacuum (`tight_frames.lean`, `integral_vacuum.lean`).** Given a tight frame $w : I \to E$ of length $M$ with multiplicities $m : I \to \mathbb{N}$ and a CM lattice $C$ of area $|b|^2 M \sqrt{d}/2$, define the linear projections $A_{j,k}(z) = \tfrac{b}{\sqrt{2\pi/\sqrt{d}}} \sum_{\tau \in \Phi} \overline{\tau(w_j)} z_{\tau,k}$. Because $w$ is a tight frame, pulling back the weighted Hermitian forms $m_j \cdot \tfrac{\pi}{A} \bar{u} v$ along $A_{j,k}$ sums identically to the standard Hermitian form $\sum_{\tau, k} \bar{u}_{\tau,k} v_{\tau,k}$ (`sum_pullForm_resolutionLinearForm_apply`). Hence $\prod_{j,k} f_{j,k}(A_{j,k}(z))$ obeys the multi-variable theta functional equation (`normalizedShift_prod_comp_of_mem_lattice`), yielding a Schwartz vector $F \in V.\mathrm{finitePart}$ whose rational translates factor both at values and at Taylor jet series (`IsResolutionProduct`, `exists_isResolutionProduct_of_isTightFrame`). Specializing to the root-of-unity frame $w_k = \zeta^k / q_1$ with uniform multiplicity $m_k = q_1$ and $f = \theta^{q_1}$ produces a **theta product** (`IsThetaProduct`) of level $e$ with $V.\theta(F) = 1$ and $p$-adically integral finite values (`exists_isThetaProduct_cmCurve`). Averaging $F$ over $(q_1^2)^{fn}$ finite Heisenberg translates isolates a nonzero multiple of the vacuum (`IsThetaProduct.exists_smul_vacElem_eq_sum`), establishing the integral vacuum `intVacuum` and Galois jet conjugacy (`exists_isJetConjugate_translates`).

2. **$p$-adic factorization into theta normal form (`theta_products.lean`).** At a prime $p$ not split in $K$ ($\chi_K(p) \neq 1$), formal-group analysis (cf. Finis 2006) factors the $p$-adically read one-variable theta series at scale $\varpi$ (with $v_p(\varpi) = \rho_p$) as
   $$\theta(\varpi z) = W(z)\,\exp\!\bigl(\kappa\,\Xi(z)\bigr), \qquad W, \Xi \in \mathcal{O}_{\mathbb{C}_p}[[z]],\ \Xi(0) = 0.$$
   Forming the multi-variable theta product $C.\mathrm{thetaProd}(\mu, m) = \prod_i \theta(\mu_i(z))^{m_i}$ along integral linear forms $\mu_i$ and rescaling all variables by $\varpi$ turns products into sums inside the exponential (`rescale_thetaProd_eq_mul_expSubst`):
   $$\bigl(C.\mathrm{thetaProd}(\mu, m)\bigr)(\varpi z) = \left(\prod_i W(\mu_i(z))^{m_i}\right) \cdot \exp\!\left(\kappa \sum_i m_i\,\Xi(\mu_i(z))\right).$$
   When the forms $\mu_i$ come from a resolution of the identity, the weighted sum $\sum_i m_i\,\Xi(\mu_i(z))$ collapses modulo $\pi_p$ into the Frobenius coupling sum (`dvd_sum_subst_sub_couplingSum`), establishing `HasNormalForm` (`exists_hasNormalForm`).

## Level 3 — Formal Lean Contract

Stage 02 formalizes resolution products, specialized theta products, and the $p$-adic theta-product exponential factorization across three extracted modules:

- `02_derivatives/tight_frames.lean#New.Parameters.IsResolutionProduct` (upstream `src/New/Derivatives/Vacuum/TightFrames.lean:674-689`):
  ```lean
  structure New.Parameters.IsResolutionProduct [Fintype I] (V : Space (P.E s) n ε) (C : CMLattice P.d) (b : ℂ)
      (w : I → P.E s) (m : I → ℕ) (f : I × n → ℂ → ℂ) (F : 𝒮 V) : Prop where
    mem_finitePart : F ∈ V.finitePart
    differentiable (i : I × n) : Differentiable ℂ (f i)
    entireTheta_translate (γ : n → P.E s) (z : InfinitePlace (P.E s) × n → ℂ) :
      entireTheta (V.ϱ (finVec (finiteImage γ)) F) z =
        ∏ i : I × n, C.twistedTranslate (m i.1) (resolutionShift b w γ i) (f i)
          (resolutionLinearForm (b / P.areaScale) w i z)
  ```
- `02_derivatives/integral_vacuum.lean#New.Parameters.IsThetaProduct` (upstream `src/New/Derivatives/Vacuum/IntegralVacuum.lean:134-160`):
  ```lean
  structure New.Parameters.IsThetaProduct [DecidableEq n] (V : Space (P.E s) n ε) (C : CMLattice P.d) (Ω : ℂˣ)
      (e : ℕ+) (F : 𝒮 V) : Prop where
    mem_finitePart : F ∈ V.finitePart
    theta_translate (γ : n → P.E s) : V.θ (V.ϱ (finVec (finiteImage γ)) F) =
      ∏ k : Fin (P.q₁ s), ∏ j : n, zetaCoordThetaTranslate C Ω k (γ j) 0 ^ P.q₁ s
    jetSeries_translate (γ : n → P.E s) (c : ℂ) :
      V.jetSeries c (V.ϱ (finVec (finiteImage γ)) F) = ∏ k : Fin (P.q₁ s), ∏ j : n,
        (taylorAtZero (zetaCoordThetaTranslate C Ω k (γ j))).subst (zetaLinearSeries Ω c k j) ^ P.q₁ s
    norm_le_one (p : ℕ) [Fact p.Prime] [PadicPlace p] (x : n → 𝔸f (P.E s)) : ‖ιₚ (F (finVec x))‖ ≤ 1
    isThetaFunction :
      IsThetaFunction (star (P.q₁ s * e : P.E s) • idealLattice (P.E s) n) (entireTheta F)
  ```
- `02_derivatives/theta_products.lean#New.CMLattice.rescale_thetaProd_eq_mul_expSubst` (upstream `src/New/Derivatives/FormalGroup/ThetaProducts.lean:404-433`):
  ```lean
  lemma New.CMLattice.rescale_thetaProd_eq_mul_expSubst {κ : ℂ_[p]} {W Ξ : PowerSeries (valRingCp p)}
      (hΞ : PowerSeries.constantCoeff Ξ = 0)
      (h_theta : (taylorVia ιₚ C.θ).rescale ϖ = seriesToCp W * expSubst (κ • seriesToCp Ξ))
      (m : I → ℕ) :
      (C.thetaProd (fun (i : I) (x : X) ↦ (μ i x : ℂ_[p])) m).rescale (fun _ : X ↦ ϖ) =
        seriesToCp (∏ i : I, W.subst (linearSeries (μ i)) ^ m i) *
          expSubst (κ • seriesToCp (∑ i : I, m i • Ξ.subst (linearSeries (μ i))))
  ```

Notice how `IsResolutionProduct` abstracts the general tight-frame product of arbitrary entire sections `f i`, `IsThetaProduct` packages the specialized root-of-unity product with its jet factorization, $p$-adic integrality, and level-$q_1 e$ theta-function property needed to extract `intVacuum`, and `rescale_thetaProd_eq_mul_expSubst` performs the formal-group algebraic bridge that feeds `New.Parameters.exists_hasNormalForm` (`ThetaProducts.lean:439-477`). See the page's live verification banner (`audit/verification-status.json`) for the currently pinned upstream snapshot and local Lean build status (`build_status: unverified-locally`).
