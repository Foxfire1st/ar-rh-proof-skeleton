---
slug: galois-main-summand
term: Galois main summand
aliases: ["main summand", "lift trace decomposition", "inverse-cyclotomic string", "Hodge-Tate jump window", "Hodge–Tate jump window", "lift setting"]
inline_glimpse: "The fixed, twist-independent rank-r (or rank-1) l-adic Galois representation R extracted from a Hecke eigensystem whose trace, after twisting by theta^(-1) and adding the inverse-cyclotomic string sum_{e < t} chi_cyc^(-e), matches every twisted theta lift."
visual_ref: assets/stage07-main-summand-twisting-pipeline.svg
where_analogy_breaks: "Picturing a Galois main summand as a single fixed direct summand inside a fixed matrix representation oversimplifies two subtleties: the decomposition is certified first at the level of Frobenius traces along a dense filter (mostFrobenii) up to normalizingAvatar V, and the full lift representation splits block-diagonally over an integral lattice only after Ribet's method and non-degeneracy of the trace form assemble a common lattice."
sources:
  - title: "Langlands' functoriality and the Weil representation"
    citation: "S. Rallis, Amer. J. Math. 104 (1982), 469–515."
    kind: paper
  - title: "Functoriality and the inverse Galois problem"
    citation: "C. Khare, M. Larsen & G. Savin, Compos. Math. 144 (2008), 1197–1217."
    kind: paper
  - title: "Compatible systems of symplectic Galois representations and the inverse Galois problem III"
    citation: "S. Arias-de-Reyna, L. Dieulefait, S. W. Shin & G. Wiese, Math. Ann. 361 (2015), 909–925."
    kind: paper
lean_anchors:
  - 07_congruences/rows_from_eigensystems.lean#New.Selmer.isMainSummandWeights_descentPair
  - 07_congruences/rows_from_eigensystems.lean#New.Selmer.isMainSummandWeights_mainPair
  - 07_congruences/rows_from_eigensystems.lean#New.Selmer.eventually_trace_frob_eq_of_hasLiftHeckePolynomial
  - 07_congruences/rows_from_eigensystems.lean#New.Selmer.exists_isMainSummand_of_isRepOfType
  - 07_congruences/rows_from_eigensystems.lean#New.Selmer.exists_isMainSummand_descentPair
  - 07_congruences/fit_twists_exist.lean#New.Selmer.exists_isMainSummand_mainPair
child_concepts:
  - hecke-operator
  - central-idempotent
  - theta-correspondence
  - regular-twist-family
  - prime-number
---

# Galois Main Summand

## Level 1 — First-Principles

When Stage 07 studies congruences between lifted modular forms and non-lifted modular forms on the target unitary group $U(V_j)$, it translates each modular form's {{concept:hecke-operator|split-place averaging eigenvalues}} into a finite-dimensional matrix symmetry of the absolute Galois group $\operatorname{Gal}(\overline{\mathbb{Q}}/E)$ over a {{concept:prime-number|prime}} $\ell$. If every choice of {{concept:regular-twist-family|character twist}} $\vartheta$ produced an unrelated Galois representation, there would be infinitely many unrelated congruence problems to control at once.

Instead, {{concept:theta-correspondence|the unramified dual-pair theta lift}} from a smaller unitary space $W_j$ (rank $r$) to $V_j$ (rank $r + t$, with $t = 5r$) leaves an unmistakable fingerprint on Hecke polynomials: at every split place $w$ off the level, the lifted Hecke polynomial factors as a fixed $q$-Pochhammer polynomial $\prod_{e=0}^{t-1}(1 - q^e \alpha X)$ times the original source Hecke polynomial rescaled by $\vartheta^{-1}$. Passing to Frobenius traces splits the normalized $\ell$-adic Galois representation attached to any lifted eigenform into two pieces—its **lift trace decomposition**:

$$\operatorname{tr}\bigl(\rho_{\mathrm{lift}}^{\mathrm{norm}}(g)\bigr) \;=\; \psi_\vartheta(g)\,\operatorname{tr}\bigl(T(g)\bigr) \;+\; \sum_{e=0}^{t-1} \chi_{\mathrm{cyc}}^{-e}(g),$$

where $\psi_\vartheta$ is the $\ell$-adic Galois avatar of $\vartheta^{-1}$ and $\sum_{e=0}^{t-1}\chi_{\mathrm{cyc}}^{-e}$ is the **inverse-cyclotomic string** of length $t$. The fixed rank-$r$ representation $T$—often called a *row* in upstream `New/Congruences/`—is the **Galois main summand** (`New.Selmer.IsMainSummand`). Crucially, $T$ does not depend on the twist $\vartheta$ at all: the finite-dimensional space of good forms on $W_m$ (cut out by the {{concept:central-idempotent|good finite-field projection}} $e_0$) yields only *finitely many* rank-$r$ main summands $T$, and the finitely many rank-one {{concept:hecke-l-value-ratio|critical character-cutoff pairs}} contribute finitely many rank-$1$ main summands.

To keep local $p$-adic lifting losses small later, Stage 07 also checks that every **Galois main summand** satisfies two structural constraints:
1. **Admissible local labels (`IsLiftSetting`):** At the chosen places above the two level primes $p_0, p_1$, the local polynomial labels $c_i$ form a **lift setting**—degree-$r$ squarefree polynomials with at least one primitive label above each level prime and no twisted label vanishing at $1$.
2. **Separated Hodge–Tate jump windows (`IsMainSummandWeights`):** At every complex embedding $\sigma$ of $E$ (with CM-type sign $\varepsilon = \pm 1$), each Hodge–Tate jump $u$ falls either in a narrow low band $\Delta/4 \le |u| \le 2m\Delta$ or inside a high **Hodge–Tate jump window** within distance $t$ of $-\varepsilon w_j$, with at most $rm/k$ small jumps and at most $r$ negative jumps across any conjugate pair $\{\sigma, \bar\sigma\}$.

### Limits of the intuition

Treating the **lift trace decomposition** as a ready-made direct-sum decomposition of matrices over the $\ell$-adic integers skips an important step. `RowsFromEigensystems.lean` establishes the identity first for Frobenius traces along the dense filter `mostFrobenii D.E` after scaling by `normalizingAvatar V`, and verifies that $T$ satisfies `IsMainSummandLike` (conjugate self-duality $T^c \cong T^\vee(1 - t)$, jump windows, large image, and controlled tame inertia above $p_0, p_1$). Separating the main block $T$ from the **inverse-cyclotomic string** on an integral lattice happens downstream in `FirstOrder/TraceRelations.lean` (`exists_blockDiagonal_lattice`).

## Level 2 — Undergrad Bridge

In `src/New/Congruences/GaloisRepresentations/RowsFromEigensystems.lean` (15 declarations, 0 stub truncations) and `src/New/Congruences/FitTwistsExist.lean`, Stage 07 builds both rank-$r$ and rank-$1$ **Galois main summands** in four steps:

1. **Linear-term extraction (`apply_one_eq_of_heckePolyEval_eq`, `neg_coeff_eq_of_hasLiftHeckePolynomial`):**
   If $H_w(X)$ satisfies `HasLiftHeckePolynomial` for a descent pair $V$ of ranks $0 < r < n$ ($t = n - r$), comparing linear coefficients in `heckePolyEval n w s X = qPochhammer w.q t (α X) * heckePolyEval r w z (β X)` gives
   $$-H_w.\mathrm{coeff}(n - 1) \;=\; q^t \frac{\nu}{\nu'}\, b(w, 1) \;+\; \Bigl(\sum_{i=0}^{t-1} q^i\Bigr)\nu,$$
   where $\nu = V.\mathrm{charW}.\mathrm{frobValue}\ r\ w$ and $\nu' = V.\mathrm{charV}.\mathrm{frobValue}\ n\ w$.
2. **Frobenius trace identity (`eventually_trace_frob_eq_of_hasLiftHeckePolynomial`):**
   Multiplying by `normalizingAvatar V x.frob` (the $\ell$-adic avatar of $(V.\mathrm{charW})^{-1}$ of weight $-r$) and transferring complex numbers via `ι.ιℓ : ℂ ≃+* PadicAlgCl ℓ` yields, along `mostFrobenii D.E`,
   $$-\mathrm{normalizingAvatar}(V, \varphi)\,\iota_\ell\bigl(H_w.\mathrm{coeff}(n - 1)\bigr) \;=\; \theta_0(\varphi)\,\theta(\varphi)\,\iota_\ell(b(w, 1)) \;+\; \sum_{e=0}^{t-1} \chi_{\mathrm{cyc}}^{-e}(\varphi),$$
   where $\theta_0$ is the avatar of $\vartheta^{-1}(x/\bar x)$ and $\theta$ is the avatar of $\mu^{-1}(x/\bar x)|x|^a$ ($r - t = 2a$).
3. **Jump-window verification (`isMainSummandWeights_descentPair`, `isMainSummandWeights_mainPair`):**
   - For the $j$-th descent pair ($1 \le j \le m$), the $r$ jumps $u_q = \lfloor t/2 \rfloor - \varepsilon(\lambda_q + w_j)$ split into $k - 1$ high jumps ($q + 1 < k$, with $|\varepsilon u_q + w_j| \le t$) and exactly $m$ low jumps ($k \le q + 1 \le r$, with $\Delta/4 \le |u_q| \le 2m\Delta$), satisfying `IsMainSummandWeights D D.r D.t` (`countP_low_weights_descentPair`).
   - For a main pair $p = (\nu, z)$ ($m < z \le r$, $w_j + r - t \le c \le w_j$), the single jump $z - 1 - \varepsilon c$ satisfies $|\varepsilon u + w_j| \le t$ and obeys `IsMainSummandWeights D 1 (2 * p.z - 1)`.
4. **Existence for descent pairs and main pairs (`exists_isMainSummand_of_isRepOfType`, `exists_isMainSummand_descentPair`, `exists_isMainSummand_mainPair`):**
   Twisting an attached representation $R$ (`IsRepOfType O r J c ∅ R`) by `O.hasWeight.inv.toAlgebraic 0 a` shifts its jumps into `IsMainSummandWeights` (`isMainSummandLike_twistByChar`) and matches lifted Frobenius traces (`exists_isMainSummand_of_isRepOfType`). Applying this to every eigensystem occurring in `D.goodForms (D.spaceW D.m)` gives `exists_isMainSummand_descentPair`, while applying it to the trivial rank-$1$ representation (`isRepOfType_one`) gives `exists_isMainSummand_mainPair` in `FitTwistsExist.lean`.

## Level 3 — Formal Lean Contract

All six anchored declarations extract completely untruncated across `skeleton/07_congruences/rows_from_eigensystems.lean` (15 declarations, 0 truncations) and `skeleton/07_congruences/fit_twists_exist.lean`:

- `07_congruences/rows_from_eigensystems.lean#New.Selmer.isMainSummandWeights_descentPair` (upstream `RowsFromEigensystems.lean:146-174`) and `07_congruences/rows_from_eigensystems.lean#New.Selmer.isMainSummandWeights_mainPair` (upstream `RowsFromEigensystems.lean:196-226`).
- `07_congruences/rows_from_eigensystems.lean#New.Selmer.eventually_trace_frob_eq_of_hasLiftHeckePolynomial` (upstream `RowsFromEigensystems.lean:329-381`):
  ```lean
  lemma New.Selmer.eventually_trace_frob_eq_of_hasLiftHeckePolynomial (ι : PadicIso ℓ) [DecidableEq n]
      {c : (i : O.ChosenIndex) → (ResField (O.chosenPlace i))[X]} (hO : IsLiftSetting O c 1) {a : ℤ}
      (ha : (Fintype.card r - O.rankDiff : ℤ) = 2 * a) {ϑ : D.TameChar} {V : DescentPair D.E r n}
      (hμ : V.u1Char = O.u1Char * ϑ.toAutChar) {y : V.FormV} {b : FinPlace D.E → ℕ → ℂ}
      (hy_lifts : V.HasLiftHeckePolynomial y b) {H : FinPlace D.E → ℂ[X]}
      (hy_eig : IsEigenform (y : Gf D.E n → WeylIndex n V.columnCount → ℂ) H) :
      ∀ᶠ x : FrobeniusDatum D.E in mostFrobenii D.E,
        -((normalizingAvatar V x.frob).val * ι.ιℓ ((H x.place).coeff (Fintype.card n - 1))) =
          ((ϑ.hasWeight.inv.toAlgebraic 0 0).avatar x.frob).val *
              (((O.hasWeight.inv.toAlgebraic 0 a).avatar x.frob).val * ι.ιℓ (b x.place 1)) +
            ∑ e ∈ Finset.range V.rankDiff, cyclotomicInv ℓ D.E (PadicAlgCl ℓ) x.frob ^ e
  ```
- `07_congruences/rows_from_eigensystems.lean#New.Selmer.exists_isMainSummand_of_isRepOfType` (upstream `RowsFromEigensystems.lean:383-436`), `07_congruences/rows_from_eigensystems.lean#New.Selmer.exists_isMainSummand_descentPair` (upstream `RowsFromEigensystems.lean:476-561`), and `07_congruences/fit_twists_exist.lean#New.Selmer.exists_isMainSummand_mainPair` (upstream `FitTwistsExist.lean:293-318`).

See the page's live `<VerificationBanner />` (`audit/verification-status.json`) for the pinned upstream commit and local Lean build status (`build_status: unverified-locally`).
