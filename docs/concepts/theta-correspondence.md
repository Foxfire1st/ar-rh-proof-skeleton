---
slug: theta-correspondence
term: theta correspondence
aliases: ["theta kernel", "iterated theta lift", "chain datum"]
inline_glimpse: "An integral transform built from a two-variable theta kernel that carries automorphic forms from one unitary group to another, iterated along a ladder of Hermitian spaces and packaged via admissible chain data."
visual_ref: assets/stage03-seesaw-double-chain.svg
where_analogy_breaks: "Thinking of a theta lift as an ordinary matrix-vector multiplication or integral operator captures how a two-variable kernel transports functions between spaces, but misses three layers: the theta kernel is built from the Weil representation of a dual reductive pair $U(X_j) \\times U(X_{j+1})$ twisted by a rank-dependent character (`rankTwist`), not an arbitrary scalar kernel; on definite unitary groups the algebraic lift acts simultaneously across all infinite places as a linear map (`jetMap`) between Weyl modules whose archimedean operator norm grows exponentially with the Young-diagram row sizes (`rowSizeSum j / 2`), and only the $m$-step iterated theta lift followed by restriction to $Y_m$ unlocks the level-$N$ saving $\\#\\Phi_a \\cdot a \\log N$; and a chain datum $\\Psi$ is not a single fixed test function, but a finitely supported combination of translated vacuum data whose coefficients depend on the twist $\\vartheta$, remain $p$-integral away from $\\vartheta.\\mathrm{normQ}$, and glue into a jointly level-invariant tensor."
sources:
  - title: "Transcending classical invariant theory"
    citation: "R. Howe, J. Amer. Math. Soc. 2 (1989)."
    kind: paper
  - title: "Langlands' functoriality and the Weil representation"
    citation: "S. Rallis, Amer. J. Math. 104 (1982)."
    kind: paper
  - title: "Algebraic modular forms"
    citation: "B. H. Gross, Israel J. Math. 113 (1999)."
    kind: paper
lean_anchors:
  - 03_upper_bound/seesaw_identity.lean#New.Data.transposedLiftY_apply
  - 03_upper_bound/seesaw_identity.lean#New.Data.integral_mul_liftY
  - 03_upper_bound/archimedean_size.lean#New.Data.sqrt_weylNormSq_jetMap_le
  - 03_upper_bound/archimedean_size.lean#New.Data.weylNormSq_compositeLift_le
child_concepts:
  - elliptic-theta-function
  - schur-jet
  - weyl-gauge
  - seesaw-duality
  - prime-number
  - arithmetic-height
---

# Theta Correspondence

## Level 1 — First-Principles

In analysis, an integral kernel $K(x, y)$ turns a function $f$ on one space into a new function $(Tf)(y) = \int K(x, y) f(x)\,dx$ on another space. The **theta correspondence** is the arithmetic analogue of that machine for symmetry-adapted automorphic forms on pairs of unitary groups $G_1 = U(V)$ and $G_2 = U(W)$. Its kernel—the **theta kernel**—is built from {{concept:elliptic-theta-function|quasi-periodic building-block functions attached to lattices}} on the space of linear maps $\operatorname{Hom}(V, W)$ between the two Hermitian spaces. Because the two unitary groups act simultaneously on the left and right of $\operatorname{Hom}(V, W)$ through a shared projective action (the Weil representation), integrating a form on $G_1$ against the **theta kernel** automatically outputs a form on $G_2$.

A basic symmetry of any integral kernel is transposition: integrating out the second variable instead of the first produces the adjoint (or descent) map running backward from $G_2$ to $G_1$. In Stage 03, each single-step lift along a ladder of Hermitian spaces $Y_j \to Y_{j+1}$ has an explicit transposed descent (`transposedLiftY`) given by the exact same **theta kernel** weighted by a finite level index, so pairing a conjugate form $z$ on $Y_{j+1}$ with the lift of $f$ equals pairing the descent of $z$ with $f$ (`integral_mul_liftY`).

Stage 03 does not stop at one step. Instead, it chains together $m = 2\varrho + 2$ consecutive lifts—an **iterated theta lift**—climbing a ladder of definite Hermitian spaces $X_0 \to X_1 \to \dots \to X_m$ of growing rank, and finally restricts the output from $X_m$ to a codimension-one subspace $Y_m$. Why climb a ladder of $m$ lifts instead of jumping once? Each step feeds the form through a **theta kernel** whose multivariate Taylor coefficients are {{concept:schur-jet|symmetry-adapted differential evaluations of theta expansions at the origin}}. Across all $m$ steps, those derivatives accumulate a huge prime-by-prime divisibility gain at every small non-split {{concept:prime-number|indivisible whole number $p \in \{2, 3, 5, 7, \dots\}$}}, while the final restriction at level $N$ cancels the archimedean factorial growth coming from the $m$ lifts. Together, that balance drives {{concept:arithmetic-height|the global tension between ordinary geometric magnitude and prime-by-prime divisibility}} in the upper bound.

To make the $m$-step composite lift completely explicit at finite places, Stage 03 packages the test functions of all $m$ kernels into a **chain datum** $\Psi$. Rather than picking one fixed Schwartz function per step, a **chain datum** is a finitely supported linear combination of translated vacuum functions across the $m$ transitions, constrained so that its coefficients stay $p$-adically integral away from the twist's modulus and its glued tensor stays invariant under the level subgroups of $X_0, \dots, X_m$. Measured with {{concept:weyl-gauge|moving-lattice $p$-adic coordinate norms and Galois-conjugate geometric bounds on algebraic modular forms}}, each single kernel expands norms by an explicit exponential of its Young-diagram row sizes, and the full composite lift absorbs the excess into the level-$N$ saving. Meanwhile, {{concept:seesaw-duality|a crossing dual-pair identity trading a lower chain of lifts for a restricted upper chain}} guarantees that this upper composite lift matches the lower chain needed in downstream stages.

### Limits of the intuition

Picturing the **theta correspondence** as an ordinary scalar integral operator captures how a two-variable kernel transfers functions between groups, but breaks down in three concrete ways. First, the **theta kernel** is not an arbitrary function of $(g, h)$: it comes from the Weil representation of the dual reductive pair $U(X_j) \times U(X_{j+1})$ twisted by a rank-dependent splitting character (`rankTwist`), which dictates how level subgroups and central characters transform across each step. Second, on definite unitary groups the algebraic lift acts simultaneously across all infinite places $\tau \in \Phi$ as a linear map (`jetMap`) between finite-dimensional Weyl modules, whose operator norm grows exponentially with half the sum of derivative sizes `rowSizeSum j / 2`; a single step looks too large on its own, and only composing all $m$ lifts and restricting to $Y_m$ cancels the excess against the level-$N$ saving $\#\Phi_a \cdot a \log N$. Third, a **chain datum** $\Psi$ is not a single twist-independent test function: its support and coefficients depend on the twisting character $\vartheta$, and admissibility enforces simultaneous finite-place integrality away from $\vartheta.\mathrm{normQ}$, an archimedean $\ell^1$ bound when the twist is unramified, and joint level invariance of the glued tensor.

## Level 2 — Undergrad Bridge

Let $E/F$ be the CM field with $f = |\Phi|$ infinite places, and let $V, W$ be Hermitian spaces over $E$ with unitary groups $G_1 = U(V)$ and $G_2 = U(W)$. Together $(U(V), U(W))$ form a reductive dual pair inside the symplectic group $\operatorname{Sp}(\operatorname{Res}_{E/\mathbb{Q}}\operatorname{Hom}_E(V, W))$ (Howe 1989; Rallis 1982). Fixing a splitting character $\chi$ (in Stage 03, `rankTwist (D.chainChar ϑ (j + 1))`, built from $\vartheta^{j+1}$ times $\upsilon^{\lfloor n/2 \rfloor}$) defines the twisted Weil representation $\omega_\chi$ of $G_1(\mathbb{A}) \times G_2(\mathbb{A})$ on Schwartz functions $\phi \in \mathcal{S}(\operatorname{Hom}(V, W)(\mathbb{A}))$, and the global **theta kernel**
$$\Theta_\phi(g, h) = \theta\bigl(\omega_\chi(g, h)\,\phi\bigr) = \sum_{x \in \operatorname{Hom}_E(V, W)} \bigl(\omega_\chi(g, h)\,\phi\bigr)(x).$$

1. **Lift and transposed descent (`transposedLiftY_apply`, `integral_mul_liftY`):** For a tame character $\vartheta$, step $j < m$, level subgroup $J_j = \operatorname{levelY}\ \vartheta\ j \le \operatorname{stdLevel}$, and Schwartz datum $\phi_j = \operatorname{datumY}\ \vartheta\ j$ on $\operatorname{Hom}(Y_j, Y_{j+1})$, the transposed lift (descent) sends a conjugate form $z \in D.\mathrm{ConjFormY}(j+1)$ to a conjugate form on $Y_j$:
   $$(\operatorname{transposedLiftY}\ \vartheta\ j\ z)(g) = [J^\circ : J_j]\int_{[U(Y_{j+1})]} \theta\bigl(\omega_{\chi_{j+1}}(g, h)\,\phi_j\bigr)\,z(h)\,dh.$$
   By Fubini's theorem on the compact/rapidly decreasing quotient integrals (`isRapid_conjFormY`), lift and descent are exact formal transposes:
   $$\int_{[U(Y_{j+1})]} z(h)\,(\operatorname{liftY}\ \vartheta\ j\ f)(h)\,dh = \int_{[U(Y_j)]} (\operatorname{transposedLiftY}\ \vartheta\ j\ z)(g)\,f(g)\,dg.$$

2. **Algebraic lifts on definite groups and one-kernel size (`sqrt_weylNormSq_jetMap_le`):** Along the upper chain $X_0, \dots, X_m$, every $X_j$ is definite, so automorphic forms become algebraic modular forms (Gross 1999)—functions on the finite adelic unitary group $G_f(X_j)$ taking values in the Weyl module indexed by `WeylIndex (D.X j) D.columnCount`. For the trivial twist (`TameChar.one`), a translation tuple $t \in D.\mathrm{Translations}$ shifts the normalized vacuum function at step $j$ to `D.translateDatum t j`, and the **theta kernel** induces a linear map `(D.liftKernel TameChar.one j).jetMap` between the Weyl modules of $X_j$ and $X_{j+1}$. For every automorphism $\sigma \in \operatorname{Aut}(\mathbb{C})$, $j < m$, $g \in G_f(X_j)$, $g' \in G_f(X_{j+1})$, and $\xi \in \operatorname{WeylIndex}(X_j, \kappa) \to \mathbb{C}$,
   $$\sqrt{\operatorname{weylNormSq}\!\left(\sigma \circ K_j.\operatorname{jetMap}\bigl(K_j.\omega(g, g')(\operatorname{translateDatum}\ t\ j)\bigr)\,\xi\right)} \le \exp\!\left(x^{50} + \frac{\operatorname{rowSizeSum}(j)}{2}\right)\sqrt{\operatorname{weylNormSq}(\sigma \circ \xi)},$$
   where $\operatorname{rowSizeSum}(j)$ sums $s_\infty(\lambda) = \log \lambda! + \lambda \log(M/\Omega)$ (`D.jetSize`) over the row lengths $\lambda$ of the Young diagrams at all infinite places $\tau \in \Phi$ for step $j$.

3. **Chain data and composite-lift archimedean bound (`weylNormSq_compositeLift_le`):** A **chain datum** $\Psi : D.\mathrm{Translations} \to_0 \mathbb{C}$ satisfying `D.IsChainDatum ϑ Ψ` specifies a finitely supported linear combination of translated glued vacuum tensors whose support lies in the admissible level translations, whose coefficients are integral away from $\vartheta.\mathrm{normQ}$ (and satisfy an $\ell^1$ conjugate bound when $\vartheta.Q = \emptyset$), and whose glued tensor is jointly invariant under the levels `D.levelX j`. The **iterated theta lift** `compositeLift ϑ Ψ φ` passes a source form $\varphi$ through all $m$ lifts $X_0 \to \dots \to X_m$ using this glued tensor and restricts the final form to $Y_m$.

   Summing the one-kernel bounds over $j < m$ gives $\sum_{j < m} \operatorname{rowSizeSum}(j) \le \operatorname{chainSize} + \operatorname{errorUnit}/2$ (`sum_rowSizeSum_le`), where $\operatorname{chainSize} = m\bigl(\#\Phi_a\,s_\infty(a) + k\sum_\tau s_\infty(\kappa(\tau))\bigr)$ and $\operatorname{errorUnit} = f a (\log b)^3$. Restricting to $Y_m$ contributes the level-$N$ saving $-\#\Phi_a\,a \log N$, and `chainSize_sub_le` converts the remainder into prime sums:
   $$\operatorname{chainSize} - \#\Phi_a\,a \log N \le h_{\mathrm{main}} - m\,\mathrm{rem} + 2\sum_{p \in \mathcal{P}_{\mathrm{ns}}} \operatorname{gain}_p \log p + 32 f a \log b.$$
   Consequently, if every value of $\sigma \circ \varphi$ has squared Weyl norm at most $\exp(B)$, then for every $y \in G_f(Y_m)$,
   $$\operatorname{weylNormSq}\bigl(\sigma \circ (\operatorname{compositeLift}\ 1\ \Psi\ \varphi)(y)\bigr) \le \exp\!\left(B + h_{\mathrm{main}} - m\,\mathrm{rem} + 2\sum_{p \in \mathcal{P}_{\mathrm{ns}}} \operatorname{gain}_p \log p + f a (\log b)^3\right).$$

## Level 3 — Formal Lean Contract

Stage 03 extracts the transpose/descent kernel identities into `skeleton/03_upper_bound/seesaw_identity.lean` (from upstream `src/New/UpperBound/Seesaw/SeesawIdentity.lean`) and the one-kernel and composite-lift archimedean bounds into `skeleton/03_upper_bound/archimedean_size.lean` (from upstream `src/New/UpperBound/Size/ArchimedeanSize.lean`):

- `03_upper_bound/seesaw_identity.lean#New.Data.transposedLiftY_apply` (upstream `src/New/UpperBound/Seesaw/SeesawIdentity.lean:183-200`):
  ```lean
  lemma New.Data.transposedLiftY_apply (ϑ : D.TameChar) {j : ℕ} (hj : j < D.m) (z : D.ConjFormY (j + 1))
      (g : New.G D.E (D.signsY j)) :
      transposedLiftY ϑ j z g = stdLevelIndex (levelY ϑ j) *
        ∫ h : New.G D.E (D.signsY (j + 1)),
          θ ((D.spaceY j).hom (D.spaceY (j + 1)))
            (twistedPairWeil (rankTwist (D.chainChar ϑ (j + 1))) (g, h) (datumY ϑ j)) * z h
          ∂(D.spaceY (j + 1)).quotMeasure
  ```
- `03_upper_bound/seesaw_identity.lean#New.Data.integral_mul_liftY` (upstream `src/New/UpperBound/Seesaw/SeesawIdentity.lean:202-228`):
  ```lean
  lemma New.Data.integral_mul_liftY (ϑ : D.TameChar) {j : ℕ} (hj : j < D.m) (z : D.ConjFormY (j + 1))
      (f : D.FormY j) :
      ∫ h : New.G D.E (D.signsY (j + 1)), z h * liftY ϑ j f h ∂(D.spaceY (j + 1)).quotMeasure =
        ∫ g : New.G D.E (D.signsY j), transposedLiftY ϑ j z g * f g ∂(D.spaceY j).quotMeasure
  ```
- `03_upper_bound/archimedean_size.lean#New.Data.sqrt_weylNormSq_jetMap_le` (upstream `src/New/UpperBound/Size/ArchimedeanSize.lean:221-262`):
  ```lean
  lemma New.Data.sqrt_weylNormSq_jetMap_le {P : Parameters} (D : Data P) (σ : ℂ ≃+* ℂ) (t : D.Translations)
      {j : ℕ} (hj : j < D.m) (g : Gf D.E (D.X j)) (g' : Gf D.E (D.X (j + 1)))
      (ξ : WeylIndex (D.X j) D.columnCount → ℂ) :
      √(weylNormSq (σ ∘ (D.liftKernel TameChar.one j).jetMap
          ((D.liftKernel TameChar.one j).ω (g, g') (D.translateDatum t j)) ξ)) ≤
        exp (D.x ^ 50 + D.rowSizeSum j / 2) * √(weylNormSq (σ ∘ ξ))
  ```
- `03_upper_bound/archimedean_size.lean#New.Data.weylNormSq_compositeLift_le` (upstream `src/New/UpperBound/Size/ArchimedeanSize.lean:430-451`):
  ```lean
  theorem New.Data.weylNormSq_compositeLift_le {P : Parameters} {D : Data P}
      {Ψ : D.Translations →₀ ℂ} (hΨ : D.IsChainDatum 1 Ψ) {σ : ℂ ≃+* ℂ} {φ : D.SourceForm} {B : ℝ}
      (hB : ∀ g : Gf D.E (D.X 0), weylNormSq (σ ∘ (φ : D.FormX 0).val g) ≤ exp B)
      (y : Gf D.E (D.Y D.m)) :
      weylNormSq (σ ∘ (compositeLift TameChar.one Ψ φ).val y) ≤
        exp (B + (D.mainAmount - D.m * D.totalRemainder +
          2 * ∑ p ∈ D.smallNonsplitPrimes, D.gain p * log p + D.errorUnit))
  ```

Together, `transposedLiftY_apply` and `integral_mul_liftY` supply the adjoint step needed to unfold the lower chain, while `sqrt_weylNormSq_jetMap_le` feeds directly into `weylNormSq_compositeLift_le` to bound the archimedean size of the upper composite lift. See the page's live `<VerificationBanner />` (`audit/verification-status.json`) for the currently pinned upstream snapshot and local Lean build status (`build_status: unverified-locally`).
