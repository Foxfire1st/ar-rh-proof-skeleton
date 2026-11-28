---
slug: elementary-divisor
term: elementary divisor
aliases: ["Smith normal form", "colength", "near inverse"]
inline_glimpse: "The invariant prime-power factors diagonalizing a linear map between integral lattices—generalized over a non-noetherian valuation domain via determinant-divisibility colength and bounded for edge Hecke operators through a near inverse modulo a power of √p."
visual_ref: assets/stage03-four-case-divisibility-and-hecke-colength.svg
where_analogy_breaks: "Picturing elementary divisors as the diagonal entries of Smith normal form over $\\mathbb{Z}$ or a discrete valuation ring $\\mathbb{Z}_p$ captures how a matrix shrinks a lattice by a multiplicative index, but misses three layers of Stage 03: the coefficient ring $O$ models `embValRing` inside $\\mathbb{C}$ (the $p$-integral complex numbers under $\\iota_p : \\mathbb{C} \\hookrightarrow \\mathbb{C}_p$), which is a non-noetherian, non-discrete valuation domain containing `sqrtPrime` ($w^2 = p$) directly so ordinary finite-module length fails and is replaced by determinant-divisibility `New.ColengthDvd`; `New.FrobeniusCycle.elementary_divisors` does not bound every elementary divisor uniformly, allowing a small exceptional proportion $\\theta \\in (0, 1]$ truncated modulo $p^m$; and the proof constructs an approximate right inverse `New.nearInverse` satisfying $\\mathrm{Add}\\cdot \\mathrm{Inv} \\equiv p^{2 N c_0} I \\pmod{w^c}$ plus Nakayama's lemma rather than diagonalizing the global operator directly."
sources:
  - title: "Algebra, Chapter VII"
    citation: "N. Bourbaki, Springer."
    kind: book
  - title: "Tag 07ZA — Fitting ideals and lengths"
    citation: "The Stacks Project Authors, Stacks Project."
    kind: reference
  - title: "Polynomial Representations of GL_n"
    citation: "J. A. Green, Lecture Notes in Mathematics 830, Springer (1980)."
    kind: book
lean_anchors:
  - 03_upper_bound/elementary_divisor_theorem.lean#New.exists_elementary_divisors_mod_summand
  - 03_upper_bound/elementary_divisor_theorem.lean#New.colengthDvd_mul_mul_of_smul_top_le
  - 03_upper_bound/elementary_divisor_theorem.lean#New.FrobeniusCycle.elementary_divisors
  - 03_upper_bound/local_divisibility.lean#New.Data.exists_colengthDvd_gaugeLe_compositeLift
child_concepts:
  - prime-number
  - complex-plane
  - hecke-operator
  - frobenius-cycle
  - weyl-gauge
  - theta-correspondence
---

# Elementary Divisor

## Level 1 — First-Principles

Take an integer matrix $A$ acting on a grid of whole-number vectors $\mathbb{Z}^d$. Even when $A$ has a complicated pattern of entries, changing the integral coordinate axes on the input and output sides puts $A$ into **Smith normal form**: a diagonal matrix $\operatorname{diag}(d_1, d_2, \dots, d_d)$ where each diagonal entry divides the next ($d_1 \mid d_2 \mid \cdots \mid d_d$). Those invariant diagonal entries are the **elementary divisors** of $A$. Their product $d_1 d_2 \cdots d_d = |\det A|$ counts how many times the image grid $A(\mathbb{Z}^d)$ fits inside $\mathbb{Z}^d$—the finite index, or **colength**, of the image lattice.

Stage 03 needs this exact notion of **colength** to prove that an {{concept:hecke-operator|averaging operator summing form values over neighboring sublattices}} does not crush the space of integral source forms into a tiny corner. Yet a subtle ring-theoretic twist forces a more flexible definition. The automorphic forms in Stage 03 take values in the {{concept:complex-plane|two-dimensional number field $\mathbb{C}$ of real and imaginary parts}}, and at each {{concept:prime-number|indivisible whole number $p \in \{2, 3, 5, 7, \dots\}$}} we keep the scalars that remain $p$-integral under a fixed embedding $\iota_p : \mathbb{C} \hookrightarrow \mathbb{C}_p$. That subring—called `embValRing`—is a *valuation domain*: between any two elements, one always divides the other, so **Smith normal form** still works for finite matrices. Unlike $\mathbb{Z}_p$, however, `embValRing` is *neither noetherian nor discrete*: it contains arbitrary roots of $p$, including a square root $w = \operatorname{sqrtPrime}$ with $w^2 = p$ living directly inside $O$. Because ideals can descend infinitely without stabilizing, ordinary finite-module length breaks down. Stage 03 therefore replaces length by *determinant-divisibility* (`New.ColengthDvd M N z`): we say the **colength** of $N$ in $M$ divides $z$ if every finite tuple of vectors in $M$ can be linearly combined into $N$ by a square $O$-matrix whose determinant divides $z$.

How do you bound the **colength** of a complicated operator when you cannot compute its **Smith normal form** by hand? Stage 03 uses a three-step **near inverse** strategy:

1. **Build a near inverse modulo $w^c$:** Factor the operator through functions on edges as $M \subseteq M_{\mathrm{all}} \subseteq M_{\mathrm{edge}} \xrightarrow{\mathrm{Add}} \mathcal{N} \subseteq M^\circ$. Using a {{concept:frobenius-cycle|cyclic system of prime-power twisted embeddings with alternating actions}}, construct an approximate right inverse—a **near inverse** `Inv`—satisfying $\mathrm{Add}\cdot \mathrm{Inv} \equiv p^{2 N c_0} I \pmod{w^c}$ whose outputs obey a tight module of congruences $C$.
2. **Factor out a direct summand and apply Nakayama:** Apply **elementary divisors** relative to a split submodule (`exists_elementary_divisors_mod_summand`) together with Nakayama's lemma (`smul_top_le_of_le_sup_smul_top`). Because $C$ leaves room for only a small fraction $\theta$ of extra generators modulo the maximal ideal, $p^{2 N c_0} \mathcal{N}$ lies inside the span of $\mathrm{Add}(M_{\mathrm{all}})$ plus at most $\theta\cdot\operatorname{rank}$ extra vectors.
3. **Multiply colengths along the factorization:** Bound the determinant loss at the two outer inclusions ($M \subseteq M_{\mathrm{all}}$ and $\mathcal{N} \subseteq M^\circ$) and multiply the three **colength** factors (`colengthDvd_mul_mul_of_smul_top_le`). Truncating modulo $p^m$ produces `New.FrobeniusCycle.elementary_divisors`, which in turn builds the small-**colength** submodule $S$ on which the {{concept:theta-correspondence|integral-kernel transform pairing automorphic forms between two dual symmetry groups}} obeys its {{concept:weyl-gauge|non-archimedean local size bound tracking divisibility at every adelic point}} at every prime (`exists_colengthDvd_gaugeLe_compositeLift`).

### Limits of the intuition

Thinking of **elementary divisors** through classical **Smith normal form** over $\mathbb{Z}$ or $\mathbb{Z}_p$ explains why diagonal invariants control lattice index, but breaks down in three concrete ways. First, the scalar ring $O$ modelling `embValRing` inside $\mathbb{C}$ is non-noetherian and non-discrete and already contains `sqrtPrime` ($w^2 = p$), so quotients need not have finite composition length; `New.ColengthDvd` replaces length with determinant divisibility without even assuming $N \subseteq M$ or choosing a basis of $M$. Second, `New.FrobeniusCycle.elementary_divisors` does not show that every **elementary divisor** is small: a tiny exceptional fraction $\theta \in (0, 1]$ of directions can carry large prime powers, which is why the theorem truncates by adding $p^m L$ and incurs the term $\theta m$ in $(3 c_{\mathrm{div}} + \theta m)|X||I|$. Third, Stage 03 never explicitly diagonalizes the global operator; it bounds the **colength** indirectly via a modulo-$w^c$ **near inverse** and Nakayama's lemma.

## Level 2 — Undergrad Bridge

Let $O$ be a valuation domain (commutative domain in which for all $x, y \in O$, either $x \mid y$ or $y \mid x$; Bourbaki, *Algebra* Ch. VII). In Stage 03, $O$ is specialized to `embValRing = {z : ℂ | ‖ιₚ z‖ ≤ 1}` for a chosen place $\iota_p : \mathbb{C} \hookrightarrow \mathbb{C}_p$. Because $\mathbb{C}$ is algebraically closed, `embValRing` is non-noetherian and non-discrete, and `sqrtPrime` ($w^2 = p$) exists directly in $O$ (no ramified field extension is needed). Over a discrete valuation ring, the index of a sublattice is measured by module length and Fitting ideals (Stacks Project Tag 07ZA); over a general valuation domain, Stage 03 defines **colength** directly via determinants (`New.Reduction.Lattices.Colength`):
$$\operatorname{ColengthDvd}(M, N, z) \iff \forall n \in \mathbb{N},\; \forall \Phi : \operatorname{Fin} n \to V,\; (\forall k,\, \Phi_k \in M) \implies \exists m \in M_n(O),\; \det(m) \mid z \;\wedge\; \forall i,\, \sum_k m_{i,k}\Phi_k \in N.$$
This predicate requires neither bases nor the inclusion $N \subseteq M$, multiplies along chains (`ColengthDvd.trans`), pushes forward and pulls back along linear maps, and reduces to classical relative length when $O$ is a DVR (`ColengthDvd.relLength_le`). An endomorphism $y$ satisfies `IsOntoUpToColength p M n y c` if $y(M) \subseteq M$ and $\operatorname{ColengthDvd}(M, y(M) + p^n M, p^l)$ for some $l \in \mathbb{N}$ with $l \le c$.

1. **Elementary divisors modulo a direct summand (`New.exists_elementary_divisors_mod_summand`):** Over a valuation domain $O$, let $Q \in M_{K \times E}(O)$ admit a left inverse $R \in M_{E \times K}(O)$ ($R Q = I_E$), so $\operatorname{im}(Q)$ is a direct summand of $O^K$, and let $A \in M_{K \times E}(O)$. Applying Smith normal form to $(I_K - Q R) A$ yields $U \in \mathrm{GL}_K(O)$ and scalars $t : K \to O$ such that $R\, U_{\cdot j} = 0$ whenever $t_j \neq 0$, and
   $$\operatorname{im}(Q) + \operatorname{im}(A) = \operatorname{im}(Q) + \operatorname{span}_O\{t_j\, U_{\cdot j} : j \in K\}.$$

2. **Near inverse, Nakayama, and multiplicative colength (`New.colengthDvd_mul_mul_of_smul_top_le`):** Suppose $A B \equiv a I_E \pmod{u}$ where $a w \mid u$ for a non-unit $w \in \mathfrak{m}_O$, both $\operatorname{im}(Q)$ and $\operatorname{im}(B)$ take values in a congruence module $C \subseteq O^I$ at every $x \in X$, and $u h \in C \implies \bar{h} \in S \subseteq k_O^I$. Applying `exists_elementary_divisors_mod_summand` to $Q$ and $B$ counts the nonzero $t_j$ dividing $u$: by residue-field independence (`card_add_card_le_mul_finrank`), at most $|X|\dim_{k_O} S - |E|$ columns can survive modulo $u$. Nakayama's lemma (`smul_top_le_of_le_sup_smul_top`) removes the $u \cdot O^E$ remainder, producing a finite set $s \subseteq O^E$ with $|E| + |s| \le |X|\dim_{k_O} S$ and $a\cdot O^E \subseteq \operatorname{im}(A Q) + \operatorname{span}_O(s)$. Multiplying on the left and right by square matrices $d', d \in M_E(O)$ (representing the boundary inclusions $M \subseteq M_{\mathrm{all}}$ and $\mathcal{N} \subseteq M^\circ$) and adding $z \cdot O^E$ gives
   $$\operatorname{ColengthDvd}\!\Bigl(O^E,\; \operatorname{im}(d' A d) + z\cdot O^E,\; \det(d')\cdot a^{|E|}\cdot \det(d)\cdot z^{|s|}\Bigr).$$

3. **Elementary divisors of the edge Hecke operator (`New.FrobeniusCycle.elementary_divisors`):** In the polynomial-representation setting on symmetric powers of $\mathrm{GL}_k$ (Green 1980), let $T$ be a Frobenius cycle with $N$ letters, prime $p = w^2 \notin O^\times$, and residue field size $q = P^N$. Under `T.IsLargeDegree k n r θ` ($0 < r \le k/2$, $0 < \theta \le 1$, $n^4 \le q$, $(N k \log q / \theta)^5 P^{3r} \le n$), take $a = p^{2 N c_0}$ and $u = w^{c - 1}$ with $c = 5 N c_0$ ($c_0 = k^2 \log_p q$, so $a w = w^{4 N c_0 + 1} \mid w^{5 N c_0 - 1} = u$). Each boundary inclusion $d, d'$ contributes at most $c_{\mathrm{div}} = N n(P^{-2r} + r^2 / (k P))$ per dimension in $w$-valuation (so $\frac{1}{2} c_{\mathrm{div}}$ in $p$-valuation; together $c_{\mathrm{div}}$), while $a = p^{2 N c_0}$ contributes $2 N c_0 \le 2 c_{\mathrm{div}}$ (`mul_multinomialExponent_le_divisorConstant`). Thus any operator $Y$ intertwined with $T.\operatorname{edgeHeckeOperator}\ n\ J\ \mathcal{E}$ along $f : (X \to I \to O) \to_O M$ satisfies
   $$\operatorname{IsOntoUpToColength}\ p\ f.\operatorname{range}\ m\ Y\ \bigl((3 c_{\mathrm{div}} + \theta m)\cdot |X|\cdot |I|\bigr).$$

4. **Four-case local divisibility theorem (`New.Data.exists_colengthDvd_gaugeLe_compositeLift`):** At every prime $p$, Stage 03 synthesizes a submodule $S \le D.\operatorname{SourceForm}$ over `embValRing` and $l \le D.\operatorname{allowedLoss}(p)\cdot \dim_\mathbb{C} D.\operatorname{SourceForm}$ with $\operatorname{ColengthDvd}(D.\operatorname{integralSourceForms}, S, p^l)$ such that every ramified source form $\varphi \in S$ lifts under `compositeLift ϑ Ψ` (for $\gcd(p, \vartheta.\operatorname{normQ}) = 1$) to gauge $\le p^{D.\operatorname{allowedLoss}(p) - D.\operatorname{gain}(p)}$:
   - *Case 1 ($p \neq N$ and $p \notin \text{small nonsplit}$, i.e., split or $p \ge x_1$):* $S = M$ (`D.integralSourceForms`), $l = 0$, $\operatorname{allowedLoss}(p) = 0$.
   - *Case 2 (non-exceptional small nonsplit $p < x_1$, including ramified $p = d$):* $S = M$, $l = 0$, collisions already absorbed into $2\,\operatorname{loss}(p)$.
   - *Case 3 (exceptional inert $p < x_1$):* choose a full mark $\Lambda$, set $S = Y^m(M) + p^{j_p} M$, and combine `gaugeLe_compositeLift_of_isFullMark` with `isOntoUpToColength_prod_scaledHeckeOperator_pow` (proved via `elementary_divisors`), giving $l \le \operatorname{loss}(p)\dim \le 2\,\operatorname{loss}(p)\dim$.
   - *Case 4 (level prime $p = N$, where $x_1 \le N$ so $\operatorname{gain}(N) = 0$):* $S$ is the pullback of `D.upIntegralForms` ($\mathrm{Up}^{m/2}\varphi$ integral for $L_0$), with $l \le m f b \dim \le 2 m f b \dim$ (`exists_colengthDvd_upIntegralForms_pow`) and level-prime budget $\operatorname{allowedLoss}(N) = 2 m f b$.

## Level 3 — Formal Lean Contract

Stage 03 extracts the abstract elementary-divisor machinery into `skeleton/03_upper_bound/elementary_divisor_theorem.lean` (from upstream `src/New/UpperBound/ElementaryDivisors/ElementaryDivisorTheorem.lean`) and its local-divisibility consumer into `skeleton/03_upper_bound/local_divisibility.lean` (from upstream `src/New/UpperBound/Divisibility/LocalDivisibility.lean`):

- `03_upper_bound/elementary_divisor_theorem.lean#New.exists_elementary_divisors_mod_summand` (upstream `src/New/UpperBound/ElementaryDivisors/ElementaryDivisorTheorem.lean:63-102`):
  ```lean
  lemma New.exists_elementary_divisors_mod_summand {O E : Type*} [CommRing O] [IsDomain O]
      [ValuationRing O] [Fintype E] [DecidableEq E] {K : Type*} [Fintype K] [DecidableEq K]
      {Q : Matrix K E O} {R : Matrix E K O} (hR : R * Q = 1) (A : Matrix K E O) :
      ∃ (U : GL K O) (t : K → O),
        (∀ j : K, t j ≠ 0 → R *ᵥ U.val.col j = 0) ∧
        Q.mulVecLin.range ⊔ A.mulVecLin.range =
          Q.mulVecLin.range ⊔ span O (Set.range fun j : K ↦ t j • U.val.col j)
  ```
- `03_upper_bound/elementary_divisor_theorem.lean#New.colengthDvd_mul_mul_of_smul_top_le` (upstream `src/New/UpperBound/ElementaryDivisors/ElementaryDivisorTheorem.lean:233-253`):
  ```lean
  lemma New.colengthDvd_mul_mul_of_smul_top_le (d' A d : Matrix E E O) {a : O} {s : Finset (E → O)}
      (h_le : a • (⊤ : Submodule O (E → O)) ≤ A.mulVecLin.range ⊔ span O (s : Set (E → O))) (z : O) :
      ColengthDvd ⊤ ((d' * A * d).mulVecLin.range ⊔ z • ⊤)
        (d'.det * (a ^ Fintype.card E * (d.det * z ^ #s)))
  ```
- `03_upper_bound/elementary_divisor_theorem.lean#New.FrobeniusCycle.elementary_divisors` (upstream `src/New/UpperBound/ElementaryDivisors/ElementaryDivisorTheorem.lean:1159-1215`):
  ```lean
  theorem New.FrobeniusCycle.elementary_divisors {N k n : ℕ} {W O : Type} [NeZero N] [CommRing W] [IsLocalRing W]
      [CommRing O] [IsDomain O] [ValuationRing O] [CharZero O] (T : FrobeniusCycle N W O)
      {X : Type*} [Fintype X] (hp : ¬IsUnit (T.p : O)) {r : ℕ} {θ : ℝ}
      (h_large : T.IsLargeDegree k n r θ) {H : Type*} {I M : Type} [Fintype H] [DecidableEq H]
      [Fintype (ParahoricCosets W k r)] [Fintype I] [AddCommGroup M] [Module O M]
      (J : I ≃ Exponents N k n × H) (𝓔 : EdgeCoordinates W O k r X H) (f : (X → I → O) →ₗ[O] M)
      {Y : End O M} (hY : Y ∘ₗ f = f ∘ₗ T.edgeHeckeOperator n J 𝓔) (m : ℕ) :
      IsOntoUpToColength (T.p : O) f.range m Y
        ((3 * T.divisorConstant k n r + θ * m) * (Nat.card X * Nat.card I))
  ```
- `03_upper_bound/local_divisibility.lean#New.Data.exists_colengthDvd_gaugeLe_compositeLift` (upstream `src/New/UpperBound/Divisibility/LocalDivisibility.lean:404-481`):
  ```lean
  theorem New.Data.exists_colengthDvd_gaugeLe_compositeLift {P : Parameters} {D : Data P} {p : ℕ}
      [hp : Fact p.Prime] [PadicPlace p] :
      ∃ (l : ℕ) (S : Submodule embValRing D.SourceForm),
        l ≤ D.allowedLoss p * finrank ℂ D.SourceForm ∧
        ColengthDvd D.integralSourceForms S (p ^ l) ∧
        ∀ (ϑ : D.Twist) (Ψ : D.Translations →₀ ℂ), IsChainDatum ϑ Ψ → p.Coprime ϑ.normQ →
          ∀ φ ∈ S, φ ∈ D.ramifiedSourceForms →
            GaugeLe 1 (p ^ (D.allowedLoss p - D.gain p)) (compositeLift ϑ Ψ φ)
  ```

Notice how the valuation-domain design keeps `New.ColengthDvd` completely uniform across non-noetherian `embValRing` and discrete valuation rings alike, allowing `elementary_divisors` to plug straight into `exists_colengthDvd_gaugeLe_compositeLift`. See the page's live verification banner (`audit/verification-status.json`) for the currently pinned upstream snapshot and local Lean build status (`build_status: unverified-locally`).
