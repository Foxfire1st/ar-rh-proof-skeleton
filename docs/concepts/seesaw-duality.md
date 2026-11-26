---
slug: seesaw-duality
term: seesaw duality
aliases: ["seesaw dual pair", "Bessel period", "unfolded period"]
inline_glimpse: "A dual-pair adjunction identity that trades an iterated lower-chain theta lift of a Poincaré series for a nonzero twist-independent scalar times the restriction of an upper-chain composite theta lift."
visual_ref: assets/stage03-seesaw-double-chain.svg
where_analogy_breaks: "Picturing seesaw duality as a single one-step adjoint identity $\\langle \\theta(f), g \\rangle = \\langle f, \\theta^*(g) \\rangle$ captures why lifting then restricting equals restricting then lifting, but misses three layers: Stage 03 runs a double chain of $m = 2\\varrho + 2$ steps, unfolding one Bessel subgroup $R_k$ at a time (`integral_unfoldStageOn_mul_transposedLiftY`) and aggregating finite-place cut classes (`cutSum`) into a twisted tensor `seesawDatum ϑ`; the proportionality constant $c \\in \\mathbb{C}^\\times$ in `seesaw_identity` is twist-independent whereas the upper-chain datum $\\Psi$ genuinely depends on the twist $\\vartheta$; and crucially, `seesaw_identity` proves $c \\neq 0$ and does NOT by itself prove that either side of the identity is nonzero (non-vanishing belongs to Theorem B in Stage 04)."
sources:
  - title: "Seesaw dual reductive pairs"
    citation: "S. S. Kudla, Automorphic Forms of Several Variables, Progr. Math. 46 (1984)."
    kind: paper
  - title: "The Gross–Prasad conjecture and local theta correspondence"
    citation: "W. T. Gan & A. Ichino, Invent. Math. 206 (2016)."
    kind: paper
  - title: "Local theta lifting of generalized Whittaker models"
    citation: "R. Gomez & C.-B. Zhu, GAFA 24 (2014)."
    kind: paper
lean_anchors:
  - 03_upper_bound/seesaw_identity.lean#New.Data.unfoldedPeriod
  - 03_upper_bound/seesaw_identity.lean#New.Data.integral_unfoldStageOn_mul_transposedLiftY
  - 03_upper_bound/seesaw_identity.lean#New.Data.exists_lowerChainUpTo_poincareAt_eq
  - 03_upper_bound/seesaw_identity.lean#New.Data.seesaw_identity
child_concepts:
  - theta-correspondence
  - weyl-gauge
  - prime-number
  - arithmetic-height
---

# Seesaw Duality

## Level 1 — First-Principles

Imagine four unitary groups arranged at the four corners of a crossing diagram—a **seesaw dual pair**—where the left-hand column has a smaller group sitting inside a larger one ($G_1 \subset G_2$) and the right-hand column reverses the inclusion ($H_2 \subset H_1$). The diagonals $(G_1, H_1)$ and $(G_2, H_2)$ each communicate via the {{concept:theta-correspondence|integral transform carrying automorphic forms from one unitary group to another through a two-variable lattice-sum kernel}}. **Seesaw duality** is the structural identity saying that lifting up one diagonal and restricting to the smaller group produces the same pairing as restricting first and lifting along the opposite diagonal.

In Stage 03, this idea is scaled up from a single crossing to a full *double chain* of $m = 2\varrho + 2$ steps:

- Along the **lower chain** $Y_0 \to Y_1 \to \dots \to Y_m$, we start with a source form $\varphi$ on $X_0$, turn it into a Poincaré series $\mathcal{P}\varphi$ on $Y_0$, and send it through $m$ consecutive lifts. This lower chain is the object that later stages pair against twists to extract global lower-bound information.
- Along the **upper chain** $X_0 \to X_1 \to \dots \to X_m$, we lift $\varphi$ directly up the larger definite unitary groups and restrict to $Y_m$ only at the very end. Because the upper chain stays on definite groups, Stage 03 can control both its geometric growth and its divisibility at every small non-split {{concept:prime-number|indivisible whole number $p \in \{2, 3, 5, 7, \dots\}$}} via {{concept:weyl-gauge|moving-lattice $p$-adic coordinate norms and Galois-conjugate size bounds on algebraic modular forms}}, feeding {{concept:arithmetic-height|the global balance between ordinary magnitude and prime-by-prime divisibility}}.

How do we prove that the lower chain of a Poincaré series equals a scalar multiple of the restricted upper chain? We pair the lower chain with an arbitrary conjugate test form $z$ on $Y_m$ and unwind the $m$ lifts one step at a time using transpose/descent. At step $k$, integrating the descent against the partially unfolded function over a **Bessel period** subgroup $R_k$ replaces the global quotient integral by the next **unfolded period** $\Xi_{k+1}$ over $R_{k+1}(F)\backslash G_{k+1}(\mathbb{A})$. Repeating this recursion $m$ times and summing over finite-place cut representatives collects all finite-place data into a single twist-dependent chain datum $\Psi$ on the upper ladder.

One caveat is essential: the theorem `seesaw_identity` proves that the proportionality constant $c$ is nonzero ($c \in \mathbb{C}^\times$) and independent of the twist $\vartheta$ and of the ramified source form $\varphi$, **and does NOT by itself prove that either side of the identity is nonzero**. If $\operatorname{lowerChain}\ \vartheta\ (\mathcal{P}\varphi)$ happened to vanish identically, the identity $0 = c \cdot 0$ would still hold trivially with $c \neq 0$; establishing genuine non-vanishing belongs to Theorem B in Stage 04.

### Limits of the intuition

Picturing **seesaw duality** as a single one-step adjoint equation $\langle \theta(f), g\rangle = \langle f, \theta^*(g)\rangle$ captures why lifting-then-restricting matches restricting-then-lifting, but breaks down in three concrete ways. First, Stage 03 never applies a one-step seesaw directly between $X_0$ and $Y_m$: it unfolds $m = 2\varrho + 2$ consecutive **Bessel periods** through intermediate gradings (`integral_unfoldStageOn_mul_transposedLiftY`), keeping track of finite-place coset cuts (`cutPoints`, `cutSum`) and multiplicities at every rung of the ladder. Second, uniformity holds for the scalar $c \in \mathbb{C}^\times$ (independent of both the twist $\vartheta$ and the form $\varphi$), whereas the upper-chain datum $\Psi$ must vary with $\vartheta$ and satisfy admissibility bounds tied to the modulus of $\vartheta$. Third, `seesaw_identity` is strictly an equality of two linear maps up to a nonzero scalar $c \neq 0$: it guarantees that upper bounds on the composite lift transfer faithfully to the lower chain, but it does **not** prove non-vanishing of either side—that separation of concerns leaves non-vanishing entirely to Theorem B (Stage 04).

## Level 2 — Undergrad Bridge

In Kudla's classical framework (Kudla 1984), a **seesaw dual pair** consists of two dual reductive pairs $(G_1, H_1)$ and $(G_2, H_2)$ inside a common symplectic group with $G_1 \subset G_2$ and $H_2 \subset H_1$. Because both pairs share the restriction of the same Weil representation, Fubini's theorem equates two period integrals. In the unitary setting, pairing theta lifts with restriction periods produces **Bessel periods** of the type governed by the Gan–Ichino local theta correspondence (Gan & Ichino 2016) and unfolded via generalized Whittaker/Bessel models (Gomez & Zhu 2014).

Stage 03 executes this seesaw across a double chain of length $m = 2\varrho + 2$ connecting the definite Hermitian spaces $X_0, \dots, X_m$ to the lower-chain spaces $Y_0, \dots, Y_m$, where $Y_m$ embeds in $X_m$ as the codimension-one standard definite space (`lastGrading`):

1. **Unfolded periods (`unfoldedPeriod`):** Fix a tame character $\vartheta$, a linear functional $t$ on values of forms on $X_0$, a source form $\varphi \in D.\mathrm{SourceForm}$, and outer variables $g_i = (\xi_i, y_i) \in G_\infty(Y_i) \times G_f(Y_i)$. For $k \le 2\varrho + 1$, a grading $Y$ of $Y_{k+1}$ with Bessel group $R_{k+1}$, and a conjugate form $z \in D.\mathrm{ConjFormY}(k+1)$, the $(k+1)$-st **unfolded period** is the absolutely convergent integral (`integrable_unfoldStage_mul`)
   $$\Xi_{k+1} = \operatorname{unfoldedPeriod}(\vartheta, t, \varphi, \xi, y, k, Y, z) = \int_{R_{k+1}(F)\backslash G_{k+1}(\mathbb{A})} \mathcal{W}_{k+1}(h)\,z(h)\,dh,$$
   where $\mathcal{W}_{k+1} = \operatorname{unfoldStage}(\vartheta, t, \varphi, \xi, y, k, Y)$ is the function remaining on $G_{k+1} = U(Y_{k+1})$ after $k+1$ unfolding steps.

2. **Iterated Bessel unfolding (`integral_unfoldStageOn_mul_transposedLiftY`):** At step $k$, integrating the restricted function `unfoldStageOn` over $[R_k]$ against the transposed descent of $z$ shifts the integral one rung up the ladder:
   $$\int_{[R_k]} \operatorname{unfoldStageOn}(\vartheta, t, \varphi, \xi, y, k, r)\cdot (\operatorname{transposedLiftY}\ \vartheta\ k\ z)(r g_k)\,dr = [J^\circ : \operatorname{levelY}\ \vartheta\ k] \cdot \Xi_{k+1}.$$
   At $k = 0$, the Poincaré series $\mathcal{P}\varphi = D.\mathrm{poincareAt}\ g\ t\ \varphi$ unfolds into a sum over the finite cut points $\Omega_0 = \operatorname{cutPoints}\ \vartheta\ 0$ (`integral_mul_liftY_poincareAt`). Repeating $k = 0, \dots, 2\varrho + 1$ (`exists_integral_mul_lowerChainUpTo_eq_cutSum`) expresses the pairing of $z$ with `lowerChainUpTo ϑ (2ϱ + 2) (𝒫 φ)` as a twist-independent constant times `cutSum`, which aggregates the evaluated upper-chain tensors (`evaluatedTensor`) into the jointly level-invariant tensor `seesawDatum ϑ` (`isInvariant_seesawDatum`).

3. **Automorphic and algebraic seesaw (`exists_lowerChainUpTo_poincareAt_eq`, `seesaw_identity`):** At the final step $k = 2\varrho + 1$ ($M = m = 2\varrho + 2$), comparing against `exists_integral_eq_mul_inner_restrictToFormY` gives a twist-independent scalar $c_0 \in \mathbb{C}$ such that for every tame character $\vartheta$ and every ramified source form $\varphi$,
   $$\operatorname{lowerChainUpTo}\ \vartheta\ (2\varrho + 2)\ (\mathcal{P}\varphi) = c_0 \cdot \operatorname{restrictToFormY}\bigl(\operatorname{chainOfTensor}\ \vartheta\ \varphi\ (\operatorname{seesawDatum}\ \vartheta)\bigr).$$
   By `exists_isAdmissibleCombination_seesawDatum`, `seesawDatum ϑ` is an admissible linear combination of translated glued vacuum data scaled by a uniform constant $C$. Passing from automorphic to algebraic forms (`lowerChain_eq_of_lowerChainUpTo_eq`) yields the main **seesaw identity**: there exists a *nonzero* scalar $c \in \mathbb{C}^\times$ (independent of $\vartheta$ and $\varphi$) such that every twist $\vartheta$ admits a chain datum $\Psi$ (`IsChainDatum ϑ Ψ`) satisfying
   $$\operatorname{lowerChain}\ \vartheta\ (D.\mathrm{poincareAt}\ g\ t\ \varphi) = c \cdot \operatorname{compositeLift}\ \vartheta\ \Psi\ \varphi$$
   for all $\varphi \in D.\mathrm{ramifiedSourceForms}$. Note carefully the logical boundary: `seesaw_identity` guarantees $c \neq 0$, which makes the identity non-degenerate, **but does not prove that either $\operatorname{lowerChain}\ \vartheta\ (\mathcal{P}\varphi)$ or $\operatorname{compositeLift}\ \vartheta\ \Psi\ \varphi$ is nonzero**; non-vanishing is proved separately in Theorem B (Stage 04).

## Level 3 — Formal Lean Contract

Stage 03 extracts the entire seesaw unfolding chain and final identity into `skeleton/03_upper_bound/seesaw_identity.lean` (from upstream `src/New/UpperBound/Seesaw/SeesawIdentity.lean`):

- `03_upper_bound/seesaw_identity.lean#New.Data.unfoldedPeriod` (upstream `src/New/UpperBound/Seesaw/SeesawIdentity.lean:272-283`):
  ```lean
  def New.Data.unfoldedPeriod (ϑ : D.TameChar)
      (t : (WeylIndex (D.X 0) D.columnCount → ℂ) →ₗ[ℂ] ℂ) (φ : D.SourceForm)
      (ξ : ∀ i : ℕ, GInf D.E (D.signsY i)) (y : ∀ i : ℕ, Gf D.E (D.Y i))
      (k : ℕ) (hk : k ≤ 2 * D.ϱ + 1) {X : Type} {s : ℕ} [Fintype X] [DecidableEq X]
      (Y : Grading (D.spaceY (k + 1)) D.N X s) (z : D.ConjFormY (k + 1)) : ℂ
  ```
- `03_upper_bound/seesaw_identity.lean#New.Data.integral_unfoldStageOn_mul_transposedLiftY` (upstream `src/New/UpperBound/Seesaw/SeesawIdentity.lean:302-326`):
  ```lean
  lemma New.Data.integral_unfoldStageOn_mul_transposedLiftY (ϑ : D.TameChar)
      (t : (WeylIndex (D.X 0) D.columnCount → ℂ) →ₗ[ℂ] ℂ) (φ : D.SourceForm)
      (ξ : ∀ i : ℕ, GInf D.E (D.signsY i)) (y : ∀ i : ℕ, Gf D.E (D.Y i))
      {k : ℕ} (hk : k ≤ 2 * D.ϱ + 1) {X : Type} [Fintype X] [DecidableEq X]
      (Y : Grading (D.spaceY (k + 1)) D.N X (2 * D.ϱ + 1 - (k + 1))) (z : D.ConjFormY (k + 1)) :
      ∫ r : (gradingY k hk).besselGroup,
        unfoldStageOn ϑ t φ ξ y k hk r * transposedLiftY ϑ k z (r.val * (ξ k, y k))
          ∂(D.gradingY k hk).besselMeasure =
        stdLevelIndex (levelY ϑ k) * unfoldedPeriod ϑ t φ ξ y k hk Y z
  ```
- `03_upper_bound/seesaw_identity.lean#New.Data.exists_lowerChainUpTo_poincareAt_eq` (upstream `src/New/UpperBound/Seesaw/SeesawIdentity.lean:540-585`):
  ```lean
  lemma New.Data.exists_lowerChainUpTo_poincareAt_eq (t : (WeylIndex (D.X 0) D.columnCount → ℂ) →ₗ[ℂ] ℂ)
      (g : GInf D.E (D.signsY 0)) :
      ∃ c₀ : ℂ, ∀ (ϑ : D.TameChar) (φ : D.ramifiedSourceForms),
        lowerChainUpTo ϑ (2 * D.ϱ + 2) (D.poincareAt g t φ) =
          c₀ • restrictToFormY D.m_eq_two_mul_varrho_add_two (chainOfTensor ϑ φ (seesawDatum ϑ))
  ```
- `03_upper_bound/seesaw_identity.lean#New.Data.seesaw_identity` (upstream `src/New/UpperBound/Seesaw/SeesawIdentity.lean:617-665`):
  ```lean
  theorem New.Data.seesaw_identity {P : Parameters} {D : Data P}
      (t : (WeylIndex (D.X 0) D.columnCount → ℂ) →ₗ[ℂ] ℂ) (g : GInf D.E (D.signsY 0)) :
      ∃ c : ℂˣ, ∀ ϑ : D.Twist, ∃ Ψ : D.Translations →₀ ℂ, IsChainDatum ϑ Ψ ∧
        ∀ φ : D.ramifiedSourceForms,
          lowerChain ϑ (D.poincareAt g t φ) = (c : ℂ) • compositeLift ϑ Ψ φ
  ```

Observe the return type `∃ c : ℂˣ`: Lean certifies that $c \neq 0$ and that $c$ is independent of both `ϑ` and `φ`, while deliberately making no claim that either `lowerChain ϑ (D.poincareAt g t φ)` or `compositeLift ϑ Ψ φ` is nonzero (non-vanishing is established separately in Theorem B / Stage 04). See the page's live `<VerificationBanner />` (`audit/verification-status.json`) for the currently pinned upstream snapshot and local Lean build status (`build_status: unverified-locally`).
