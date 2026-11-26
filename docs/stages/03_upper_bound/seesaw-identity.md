# The Iterated Seesaw Identity

This page explains the seesaw landmark of Stage 03 — `src/New/UpperBound/Seesaw/SeesawIdentity.lean` (`03_upper_bound/seesaw_identity`, 23 declarations, 0 stub truncations) — together with its upstream setup in `src/New/UpperBound/Objects/{Columns,Rows,RowChain,PoincareMap}.lean`. Return to the [Stage 03 Overview](./overview.md), or continue to [Short Columns & Archimedean Size](./short-columns-and-archimedean-size.md) and [Local Divisibility & Elementary Divisors](./local-divisibility-and-elementary-divisors.md).

## Level 1 — First-Principles

To bound the target vectors of [Stage 01](../01_reduction/overview.md), the proof needs to know both their Euclidean size and how divisible their entries are at every prime. The trouble is that those target vectors live on an *indefinite* unitary group $U(Y_0)$, where modular forms are continuous functions on a non-compact space rather than finite tables of algebraic numbers. On a *definite* unitary group $U(X_0)$, by contrast, a modular form is a finite array of algebraic vectors, and [Stage 02](../02_derivatives/overview.md)'s theta derivatives give direct size and divisibility bounds.

The seesaw identity resolves this mismatch by connecting two paths from the definite base space $X_0$ to a common definite destination $Y_m$. Each step along either path applies a {{concept:theta-correspondence|linear map transferring modular forms from one unitary group to another by integrating against a joint theta function}}:

- **The lower path** sends a source form $\varphi$ on $X_0$ to a form $\mathcal{P}\varphi$ on the indefinite space $Y_0$ via the Poincaré map $\mathcal{P}$, and then pushes $\mathcal{P}\varphi$ through $m$ lifts $Y_0 \to Y_1 \to \cdots \to Y_m$, where the final space $Y_m$ is definite.
- **The upper path** keeps everything inside definite spaces, pushing $\varphi$ through $m$ definite lifts $X_0 \to X_1 \to \cdots \to X_m$ and then restricting the output from $X_m$ to the codimension-$1$ subspace $Y_m \subset X_m$.

![Two parallel chains of unitary spaces linked at the bottom by the Poincaré map from X_0 to Y_0 and at the top by restriction from the definite space X_m to Y_m](assets/stage03-seesaw-double-chain.svg)

Why do these two completely different journeys land on proportional forms on $Y_m$? The proof uses {{concept:seesaw-duality|a pairing symmetry between two dual chains of spaces that exchanges a lift along one chain for a restriction along the other}} in four concrete steps:

1. **Test against an arbitrary form on $Y_m$:** Two vectors in an inner-product space are equal if and only if they have the same inner product with every test vector $z$. Pairing the output of the lower chain on $Y_m$ with a test form $z$ turns the problem into a single global integral over $U(Y_m)$.
2. **Walk $z$ down the lower chain by transposition:** Every {{term:theta-correspondence}} has an adjoint (its transposed lift, or descent) that moves the integral from $Y_{j+1}$ back down to $Y_j$. Applying transposition $m$ times walks the test form $z$ all the way down to $Y_0$, where it meets $\mathcal{P}\varphi$.
3. **Unfold the Poincaré map into finite coset sums:** By construction, pairing a form on $Y_0$ with $\mathcal{P}\varphi$ evaluates the {{term:seesaw-duality|Bessel period}} of $\varphi$. Moving that period back up the chain one lift at a time unfolds each global integral into an archimedean factor times a finite weighted sum over congruence cosets (`cutSum`). The denominators introduced by these coset weights only involve primes dividing the twist modulus $\vartheta.Q$, so away from the twist modulus no new denominators appear.
4. **Recognize the top integral as a definite restriction:** After $m$ unfolding steps, the remaining integral sits on the definite space $Y_m \subset X_m$. Evaluating the finite coset sum on the vacuum data produces a single {{term:theta-correspondence|chain datum}} $\Psi$ for the upper definite chain, and the top integral collapses to a nonzero scalar $c$ times the inner product of $z$ with the restricted upper chain of $\varphi$.

This yields the headline theorem `New.Data.seesaw_identity`: there exists a single nonzero scalar $c$ (independent of both the twist $\vartheta$ and the source form $\varphi$) such that for every twist $\vartheta$, there is a {{term:theta-correspondence|chain datum}} $\Psi$ satisfying
$$\mathrm{lowerChain}_\vartheta(\mathcal{P}\varphi) = c \cdot \mathrm{compositeLift}_\vartheta(\Psi, \varphi)$$
for every source form $\varphi$ on $X_0$ belonging to the subspace of {{concept:weyl-gauge|source forms whose averages over central scalings at primes dividing the level $\pi_N$ vanish}} (the {{term:weyl-gauge|ramified source forms}}).

**What this theorem does and does not claim:** `New.Data.seesaw_identity` proves that the proportionality constant $c$ is nonzero and does not depend on the twist $\vartheta$. It does **not** prove that either side of the equation is nonzero. Both sides could vanish for a given $\varphi$ without contradicting the seesaw identity; proving that the Poincaré images actually survive and span a large subspace of target forms is the separate work of Theorem B in Stage 04 (`04_spanning`).

### Where this analogy breaks

Thinking of the seesaw as a simple commutative square $X_0 \to Y_0 \to Y_m$ versus $X_0 \to X_m \to Y_m$ hides two structural complications in the real proof. First, the spaces $Y_0, \dots, Y_{m-1}$ are *indefinite*, so the lower-chain lifts do not factor step-by-step into definite finite-sum maps; instead, the upper chain groups the $m = 2\varrho + 2$ definite lifts into $\varrho + 1$ **double lifts** whose finite data are glued above the level prime $N$ (`evaluatedTensor`, `seesawDatum`). Second, the {{term:theta-correspondence|chain datum}} $\Psi$ on the right-hand side *depends on the twist $\vartheta$* — only the overall scalar $c \in \mathbb{C}^\times$ is twist-independent — and $\Psi$'s archimedean coefficient bound holds only for the trivial twist ($\vartheta.Q = \emptyset$), which is all Stage 01 needs because the {{concept:gram-determinant|squared fundamental volume of the target lattice}} ({{term:gram-determinant|covolume}}) is measured at the trivial twist while index is measured prime-by-prime away from $\vartheta.Q$.

## Level 2 — Undergrad Bridge

### 1. The two chains of unitary spaces and the Poincaré map (`Objects/{Columns,Rows,RowChain,PoincareMap}.lean`)

Fix admissible data $D : \mathrm{Data}\ P$ over the cyclotomic CM field $E = D.E = K(\zeta_{q_1})$, with number of lifts $m = 2\varrho + 2$, ranks $r$, $k = r + 1 - m$, $t = 5r$, and prime level $N = \pi_N\bar{\pi}_N$.

| Chain | Spaces and Ranks | Signatures | Maps and Levels |
| --- | --- | --- | --- |
| **Upper chain** (`Columns.lean`) | $X_0 \to X_1 \to \cdots \to X_m$, with $\operatorname{rank}(X_j) = n_j = k + j(t + 1)$, so $n_m = r + mt + 1$ | Every $X_j$ is **definite** | Definite theta lifts glued in $\varrho + 1$ double lifts, followed by `D.restrictToY` from $X_m$ to $Y_m$ |
| **Lower chain** (`Rows.lean`, `RowChain.lean`) | $Y_0 \to Y_1 \to \cdots \to Y_m$, with $\operatorname{rank}(Y_j) = r + jt$, so $\operatorname{rank}(Y_m) = r + mt = n_m - 1$ | $Y_0 = X_0 \perp \mathbb{H}^\varrho \perp \langle -N \rangle$ is **indefinite**; signatures shift along the chain until $Y_m \subset X_m$ is **definite** | Adelic theta lifts `liftY ϑ j`, composed into `lowerChainUpTo ϑ j` (automorphic) and `lowerChain ϑ` (algebraic on $Y_m$) |

At the base of the two chains sits the **Poincaré map** $\mathcal{P} = D.\mathrm{poincareAt}\ g\ t : D.\mathrm{SourceForm} \to D.\mathrm{principalForms}(D.\mathrm{spaceY}\ 0)$ (`PoincareMap.lean`), built for a fixed archimedean point $g \in G_\infty(Y_0)$ and linear functional $t$ on the Weyl module of values on $X_0$. By `inner_poincareAt`, $\mathcal{P}\varphi$ is the Riesz representative of the Bessel period on the Bessel group $R_0$ of the standard grading `D.gradingY 0`:
$$\langle y, \mathcal{P}\varphi \rangle = \int_{[R_0]} \mathcal{W}_\varphi(r)\,\overline{y(r g)}\,dr.$$
For any conjugate form $z$ of level $J$ on $Y_0$, `integral_mul_poincareAt` unfolds the global pairing over $[G_0]$ into a finite sum over cosets of $J_M / (J \cap J_M)$ (where $J_M = D.\mathrm{chosenLevel}$):
$$[J_M : J \cap J_M] \int_{[G_0]} z(x)\,(\mathcal{P}\varphi)(x)\,dx = \sum_{q \in J_M / (J \cap J_M)} \int_{[R_0]} \mathcal{W}_\varphi(r)\,z(r\cdot (g, q.\mathrm{out}))\,dr.$$

### 2. Transposed lifts and iterated Bessel unfolding (`SeesawIdentity.lean`, lines 172–490)

For each $j < m$, forms of the conjugate discrete-series type on $Y_j$ form `ConjFormY D j`. The **transposed lift** (descent) `transposedLiftY ϑ j : ConjFormY (j + 1) →ₗ[ℂ] ConjFormY j` scales the swapped theta lift by the level index $[J^\circ : J_\xi(Y_j)] = \mathrm{stdLevelIndex}(\mathrm{levelY}\ \vartheta\ j)$. The first block of lemmas establishes its basic properties:

- **`transposedLiftY_apply`**: expresses `(transposedLiftY ϑ j z) g` explicitly as `[J° : J_ξ(Y_j)]` times the theta integral of `θ(ω(g, h) datumY ϑ j) * z h` over $[G_{j+1}]$.
- **`integral_mul_liftY`**: proves exact adjointness between lift and descent:
  $$\int_{[G_{j+1}]} z(h)\,(\mathrm{liftY}\ \vartheta\ j\ f)(h)\,dh = \int_{[G_j]} (\mathrm{transposedLiftY}\ \vartheta\ j\ z)(g)\,f(g)\,dg.$$
- **`twistedPairWeil_datumY_of_mem_levelY`**, **`hasLevel_transposedLiftY`**, and **`isRapid_conjFormY`**: verify that `datumY ϑ j` is fixed by `(1, u)` for $u \in \mathrm{levelY}\ \vartheta\ j$, hence `transposedLiftY ϑ j z` inherits right invariance under `levelY ϑ j`, and every conjugate form on $Y_{j+1}$ is rapidly decreasing.

Next, `unfoldedPeriod` defines the stage-$(k+1)$ integral $\Xi_{k+1}$ of `unfoldStage ϑ t φ ξ y k hk Y * z` over $R_{k+1}(F) \backslash G_{k+1}(\mathbb{A})$, where `(ξ i, y i)` are the archimedean and finite parts of the outer variables at stages $0, \dots, k$:

1. **`integrable_unfoldStage_mul`** establishes absolute convergence of $\Xi_{k+1}$ for $k \le 2\varrho + 1$.
2. **`integral_unfoldStageOn_mul_transposedLiftY`** gives the exact unfolding recursion: integrating `unfoldStageOn` against `transposedLiftY ϑ k z` at $r \cdot (\xi_k, y_k)$ over $[R_k]$ equals $\mathrm{stdLevelIndex}(\mathrm{levelY}\ \vartheta\ k) \cdot \Xi_{k+1}$.
3. **`sum_quotient_chosenLevel_eq_sum_cutPoints`** and **`integral_mul_liftY_poincareAt`** handle the base case $k = 0$: combining `integral_mul_liftY`, `integral_mul_poincareAt`, and the level index identity $[J_M : J][J^\circ : J_M] = [J^\circ : J]$ rewrites $\int_{[G_1]} z \cdot \mathrm{liftY}\ \vartheta\ 0\ (\mathcal{P}\varphi)$ as $[J^\circ : J_M] \sum_{c \in \Omega_0} \Xi_1$, where $\Omega_0 = \mathrm{cutPoints}\ \vartheta\ 0$.
4. **`exists_integral_mul_lowerChainUpTo_eq_cutSum`** runs the induction over $k = 0, \dots, 2\varrho + 1$: assuming $\Xi_{k+1}$ factors into an archimedean function $a(\xi)$ times a finite-coset function $S([y])$ whenever $y_0 \in J_M$ and $y_m = 1$, each inductive step descends $z$ via `integral_mul_liftY` and converts the outer integral over $R_{k+1}(F)\backslash G_{k+1}(\mathbb{A})$ into a weighted sum over the finite cut $\Omega_{k+1} = \mathrm{cutPoints}\ \vartheta\ (k+1)$ (dividing by `cutMultiplicity`, whose support is controlled by lattice bounds from `hasLatticeBounds_of_unfoldStage_ne_zero` on ramified source forms $\varphi$). The result is a single constant $c \in \mathbb{C}$ independent of $\vartheta, \varphi, z, S$ such that
   $$\int_{[G_{k+1}]} z(h)\,\mathrm{lowerChainUpTo}\ \vartheta\ (k+1)\ (\mathcal{P}\varphi)(h)\,dh = c \cdot \mathrm{cutSum}\ \vartheta\ (k+1)\ S.$$

### 3. Top-stage restriction, `seesawDatum`, and `seesaw_identity` (`SeesawIdentity.lean`, lines 69–155 and 494–665)

At the top stage $M = m = 2\varrho + 2$, the space $Y_m$ is standard definite, its last grading `D.lastGrading hM` has no remaining isotropic vectors, and `D.restrictToY` maps forms on $X_m$ to algebraic forms on $Y_m$ (read in automorphic forms as `restrictToFormY`, bridged back by `lowerChain_eq_of_lowerChainUpTo_eq`).

- **`exists_integral_eq_mul_inner_restrictToFormY`** proves that for any linear form $t$ on Weyl values of $X_m$, there is a constant $C \in \mathbb{C}$ such that integrating a matrix coefficient $F(\mathrm{inclX}(k)) = \mathrm{algFormCoeff}\ 1\ t\ \psi(\mathrm{diag}(k, 1))$ against $\overline{x}$ over $[G_m]$ equals $C \cdot \langle x, \mathrm{restrictToFormY}\ hM\ \psi \rangle$.
- **`evaluatedTensor`** and **`evaluatedTensor_mk`** form the pure tensor of the $\varrho + 1$ evaluated double-lift data translated by coset representatives `c i`.
- **`seesawDatum ϑ`** is defined as `cutSum ϑ (2 * D.ϱ + 2) le_rfl (evaluatedTensor ϑ)`.
- **`isInvariant_seesawDatum`** proves `(seesawDatum ϑ).IsInvariant ϑ`: translating cut variables by a tuple $b$ in the upper levels `D.levelX j` (with $b_m = 1$) leaves `cutSum` unchanged and matches the joint level action on each double lift.
- **`exists_lowerChainUpTo_poincareAt_eq`** combines `exists_integral_mul_lowerChainUpTo_eq_cutSum` at $k = 2\varrho + 1$ with `exists_integral_eq_mul_inner_restrictToFormY` and Riesz uniqueness (`ext_inner_left`) to obtain a single $c_0 \in \mathbb{C}$ such that for all tame characters $\vartheta$ and ramified source forms $\varphi$,
  $$\mathrm{lowerChainUpTo}\ \vartheta\ m\ (\mathcal{P}\varphi) = c_0 \cdot \mathrm{restrictToFormY}\ (\mathrm{chainOfTensor}\ \vartheta\ \varphi\ (\mathrm{seesawDatum}\ \vartheta)).$$
- **`exists_isAdmissibleCombination_seesawDatum`** uses `exists_isAdmissibleCombination_gluedVacuum` and `isAdmissibleCombination_cutSum` to express `seesawDatum ϑ` as a finite sum $\sum r_x \cdot C \cdot D.\mathrm{gluedVacuum}\ x$ over admissible translations with denominator dividing $B^{2m}$ away from $\vartheta.\mathrm{normQ}$ (where $B = D.\mathrm{residueMatrixCount} = M^{f n_m^2}$) and total conjugate $\ell^1$-norm $\le B^{2\varrho + 2}$ when $\vartheta.Q = \emptyset$.

Finally, **`New.Data.seesaw_identity`** clears the scalar $c_0 C / B^{2m}$ using a unit $u := \mathrm{normUnit}(c_0 C / B^{2m}) \in \mathbb{C}^\times$ and sets $c := u^{-1} \in \mathbb{C}^\times$:
- Rescaling `seesawDatum ϑ` by $u \cdot c_0$ produces an admissible combination of translated glued vacuums supported on `supportedTranslations (D.M * ϑ.normQ)` with total size bound $B^{2m} \cdot B^{2\varrho + 2} = M^{3 f m n_m^2}$ when $\vartheta.Q = \emptyset$, yielding a chain datum $\Psi$ with `IsChainDatum ϑ Ψ` and `gluedTensor Ψ = (u : ℂ) • c₀ • seesawDatum ϑ`.
- **Honesty check on $c \neq 0$ versus non-vanishing:** Notice how `normUnit` works in Lean (`SeesawIdentity.lean:636–640`): if $c_0 C \neq 0$, then $u = (c_0 C / B^{2m})^{-1}$; if $c_0 C = 0$, then `normUnit 0 = 1`, so $u = 1$, $c = 1 \in \mathbb{C}^\times$, and `u • c₀ • seesawDatum ϑ = 0` gives $\Psi = 0$, making both sides identically $0$. Thus `seesaw_identity` guarantees $c \in \mathbb{C}^\times$ (nonzero and twist-independent), and makes **no** claim that either side is nonzero. Non-vanishing and spanning of the Poincaré images are proved independently in Stage 04 (`04_spanning`).

## Level 3 — Formal Lean Contract

The extracted skeleton `skeleton/03_upper_bound/seesaw_identity.lean` mirrors `src/New/UpperBound/Seesaw/SeesawIdentity.lean` at pinned upstream commit `41c2716658c42ba08eb0648994a2efe83082a95c` (`41c2716658c4`, `leanprover/lean4:v4.37.0-rc1`, Mathlib rev `31ce06eaadf4a200c0e1407c5f10953cf606b8f8`), containing **23 declarations and 0 stub truncations** (part of Stage 03's 173 declarations and 0 stub truncations across 5 skeletons).

### Headline Theorem Signature

From `src/New/UpperBound/Seesaw/SeesawIdentity.lean:629–633` (matching `skeleton/03_upper_bound/seesaw_identity.lean:271–275`):

```lean
theorem New.Data.seesaw_identity {P : Parameters} {D : Data P}
    (t : (WeylIndex (D.X 0) D.columnCount → ℂ) →ₗ[ℂ] ℂ) (g : GInf D.E (D.signsY 0)) :
    ∃ c : ℂˣ, ∀ ϑ : D.Twist, ∃ Ψ : D.Translations →₀ ℂ, IsChainDatum ϑ Ψ ∧
      ∀ φ : D.ramifiedSourceForms,
        lowerChain ϑ (D.poincareAt g t φ) = (c : ℂ) • compositeLift ϑ Ψ φ
```

- **Hypotheses:** Admissible data `D : Data P`, a linear functional `t` on the Weyl module of values on `X_0`, and an archimedean point `g` of `U(Y_0)`.
- **Conclusion:** There exists a single unit `c : ℂˣ` (hence `c ≠ 0`, independent of `ϑ` and `φ`) such that every twist `ϑ : D.Twist` admits a finitely supported `Ψ : D.Translations →₀ ℂ` satisfying `IsChainDatum ϑ Ψ` (admissible support at the level, values integral away from `ϑ.normQ`, $\ell^1$ conjugate bound when `ϑ.Q = ∅`, and level-invariance of `gluedTensor Ψ`) and matching `lowerChain ϑ (D.poincareAt g t φ) = (c : ℂ) • compositeLift ϑ Ψ φ` on all `φ : D.ramifiedSourceForms`.
- **Non-vanishing boundary:** As recorded in upstream `GUIDE.md` and visible directly in the `normUnit` step (`SeesawIdentity.lean:636–640`), the theorem asserts `c : ℂˣ` (non-zero constant) and does **not** assert that either side of the equality is nonzero; non-vanishing and spanning of the Poincaré images are established in Stage 04 (`04_spanning`).

### Direct Proof Chain in `seesaw_identity.lean`

| Declaration | Upstream Lines | Role in the Proof |
| --- | :---: | --- |
| `restrictToFormY`, `lowerChain_eq_of_lowerChainUpTo_eq` | 69–85 | Bridge `D.restrictToY` and `lowerChain` between automorphic forms and algebraic forms at $M = m$. |
| `exists_integral_eq_mul_inner_restrictToFormY` | 87–154 | Pair a top-stage block-embedded matrix coefficient on $X_m$ against a form on $Y_m$ to recover $C \cdot \langle x, \mathrm{restrictToFormY}\ \psi \rangle$. |
| `ConjFormY`, `transposedLiftY`, `transposedLiftY_apply`, `integral_mul_liftY` | 172–228 | Define conjugate forms on $Y_j$ and the transposed lift (descent), proving exact adjointness with `liftY`. |
| `twistedPairWeil_datumY_of_mem_levelY`, `hasLevel_transposedLiftY`, `isRapid_conjFormY` | 230–270 | Level invariance of the lift datum and descent, plus rapid decrease of conjugate forms. |
| `unfoldedPeriod`, `integrable_unfoldStage_mul`, `integral_unfoldStageOn_mul_transposedLiftY` | 272–326 | Define the unfolded period $\Xi_{k+1}$, prove integrability, and establish the one-step unfolding recursion. |
| `sum_quotient_chosenLevel_eq_sum_cutPoints`, `integral_mul_liftY_poincareAt`, `exists_integral_mul_lowerChainUpTo_eq_cutSum` | 328–490 | Base case $k = 0$ via `D.integral_mul_poincareAt` and induction over $k \le 2\varrho + 1$ reducing to `cutSum`. |
| `evaluatedTensor`, `evaluatedTensor_mk`, `seesawDatum`, `isInvariant_seesawDatum` | 494–538 | Assemble the upper-chain finite data tensor `seesawDatum ϑ` from the cuts and prove joint level invariance. |
| `exists_lowerChainUpTo_poincareAt_eq`, `exists_isAdmissibleCombination_seesawDatum`, `seesaw_identity` | 540–665 | Match the lower and upper chains via Riesz uniqueness, expand `seesawDatum ϑ` in glued vacuums, and rescale by `normUnit` to produce `c : ℂˣ` and `Ψ`. |

**Verification status (`audit/verification-status.json`):** Local Lean compilation and kernel replay remain **`build_status: unverified-locally`** at pinned upstream commit `41c2716658c42ba08eb0648994a2efe83082a95c` (`41c2716658c4`, `leanprover/lean4:v4.37.0-rc1`, Mathlib rev `31ce06eaadf4a200c0e1407c5f10953cf606b8f8`). Static token scanning currently has **`scan_scope: entry`**, covering only `Challenge.lean`, `src/Main.lean`, and `src/Statements.lean` (`0` forbidden tokens in solution files, `5` expected `sorry` tokens in `Challenge.lean`). `SeesawIdentity.lean` has not been locally compiled, kernel-checked, or statically scanned here.

<!-- rh:lean-node:03_upper_bound/seesaw_identity -->
