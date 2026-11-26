---
slug: weyl-gauge
term: Weyl gauge
aliases: ["p-adic gauge", "B-short", "ramified source form"]
inline_glimpse: "The p-adic coordinate norm of an algebraic modular form measured in the g-twisted Weyl lattice, paired with Galois-conjugate archimedean bounds to define B-short spanning sets and ramified source forms."
visual_ref: assets/stage03-size-level-cancellation.svg
where_analogy_breaks: "Thinking of the Weyl gauge as an ordinary entrywise $p$-adic max-norm of a vector misses three structural features: at each finite adelic point $g \\in G_f$, the gauge twists the value $\\varphi(g)$ by the inverse Weyl action $g^{-1}$ before taking coordinates, so integrality is measured relative to the moving lattice at $g$ rather than a fixed global basis; being B-short enforces a simultaneous global constraint—Weyl gauge $\\le 1$ at every prime $p$ and every embedding $\\iota_p : \\mathbb{C} \\hookrightarrow \\mathbb{C}_p$, together with squared Weyl norm $\\le B$ across every automorphism $\\sigma \\in \\operatorname{Aut}(\\mathbb{C})$; and projecting onto ramified source forms multiplies the shortness bound by $4^f$ while trimming dimension by at most $f/(N-1) \\le b^{-8}$, and rescaling via `GaugeLe.zpow_neg_smul` converts a real-valued gauge bound $p^r$ into an integral form using an integer exponent $\\beta \\le -r$."
sources:
  - title: "Algebraic modular forms"
    citation: "B. H. Gross, Israel J. Math. 113 (1999)."
    kind: paper
  - title: "Lattice methods for algebraic modular forms on classical groups"
    citation: "M. Greenberg & J. Voight, Contemp. Math. 624 (2014)."
    kind: paper
  - title: "Representations of Algebraic Groups"
    citation: "J. C. Jantzen, 2nd ed., AMS (2003)."
    kind: book
lean_anchors:
  - 03_upper_bound/short_columns.lean#New.IsShort
  - 03_upper_bound/short_columns.lean#New.span_isShort_eq_top
  - 03_upper_bound/short_columns.lean#New.Data.ramifiedSourceForms
  - 03_upper_bound/short_columns.lean#New.Data.span_isShort_ramifiedSourceForms_eq_top
  - 03_upper_bound/local_divisibility.lean#New.GaugeLe.zpow_neg_smul
child_concepts:
  - prime-number
  - arithmetic-height
  - theta-correspondence
  - schur-jet
  - hecke-operator
  - elementary-divisor
---

# Weyl Gauge

## Level 1 — First-Principles

An algebraic modular form $\varphi$ on a definite unitary group $G_f$ assigns to every finite adelic point $g$ a vector $\varphi(g)$ inside a finite-dimensional representation space—a Weyl module indexed by Young-diagram fillings across all infinite places. To feed such forms into {{concept:arithmetic-height|the global balance between ordinary geometric size and prime-by-prime divisibility}}, we need two compatible rulers: one measuring ordinary Euclidean size, and another measuring divisibility at each {{concept:prime-number|indivisible whole number $p \in \{2, 3, 5, 7, \dots\}$}}.

The subtlety is that different points $g \in G_f$ correspond to different lattices inside the same Hermitian space, so checking whether $\varphi(g)$ is "integral" in a single fixed coordinate system would break symmetry. Instead, we first transport $\varphi(g)$ back to the reference lattice by acting with $g^{-1}$ in the Weyl representation, and then take the largest $p$-adic absolute value among its coordinates. That moving-lattice coordinate size is the **Weyl gauge** (also called the **$p$-adic gauge**). A form is $p$-integral precisely when its **Weyl gauge** is at most $1$ at every point $g$, and it is divisible by $p^k$ precisely when its **Weyl gauge** is at most $p^{-k}$.

With both rulers in hand, we can ask whether a space of modular forms has a well-conditioned spanning set rather than an arbitrarily skewed basis. A form $\varphi$ is called **B-short** if its **Weyl gauge** is at most $1$ at *every* prime and *every* $p$-adic embedding, while every Galois conjugate $\sigma(\varphi(g))$ has squared Weyl norm at most $B$. Stage 03 proves (`span_isShort_eq_top`) that for any free level subgroup $K$, the **B-short** forms span the entire level space with an explicit bound $B$ controlled by the field discriminant, rank, and Young-diagram box count.

Stage 03's upper chain of {{concept:theta-correspondence|integral transforms carrying automorphic forms along a ladder of unitary groups}} cannot act on arbitrary source forms: its seesaw match and {{concept:schur-jet|symmetry-adapted multivariate derivative divisibility bounds}} require forms that are genuinely ramified at every prime $w$ dividing the auxiliary level integer $\pi_N$ (where $\pi_N \bar{\pi}_N = N$). A **ramified source form** is a source form whose translates by the powers $z_w^i$ ($0 \le i < N - 1$) of a local unit generator $z_w$ sum to zero at each $w \mid \pi_N$. Applying the product of difference operators $\prod_{w \mid \pi_N}(1 - z_w)$ maps the full source space onto the **ramified source forms**, multiplies shortness by at most $4$ per prime ($4^f$ in total), and loses at most a $b^{-8}$ fraction of the dimension. That delivers a **B-short** spanning set of **ramified source forms** with $B = \exp(f a (\log b)^3)$ (`span_isShort_ramifiedSourceForms_eq_top`).

Finally, the **$p$-adic gauge** turns local divisibility directly into global size savings. Whenever the composite lift—precomposed at exceptional primes with {{concept:hecke-operator|averaging operators over local lattice neighbors}} on a submodule of controlled {{concept:elementary-divisor|index exponent measuring how far a sublattice sits inside the full integral lattice}}—shrinks the **Weyl gauge** of a form to at most $p^r$ (with $r < 0$), rescaling by $p^{-\beta}$ for any integer $\beta \le -r$ brings the **Weyl gauge** back up to at most $1$ (`GaugeLe.zpow_neg_smul`). Performing this rescaling across all small non-split primes cancels their prime-power weights from the archimedean bound without ever breaking integrality.

### Limits of the intuition

Thinking of the **Weyl gauge** as a simple entrywise $p$-adic max-norm captures how divisibility shrinks coordinate norms, but breaks down in three concrete ways. First, at each point $g \in G_f$ the gauge applies the inverse Weyl representation `weylRep κ e (g⁻¹)` before reading coordinates, so integrality tracks the $g$-shifted lattice rather than a single global basis, and shifted gauges `GaugeLe d c φ` evaluate the lattice at $g \cdot d$. Second, **B-short** is a simultaneous global condition over all primes $p$, all embeddings $\iota_p : \mathbb{C} \hookrightarrow \mathbb{C}_p$, and all field automorphisms $\sigma \in \operatorname{Aut}(\mathbb{C})$, not just a bound at one place or one archimedean embedding. Third, restricting to **ramified source forms** costs a factor of $4^f$ in the shortness bound and trims the dimension by at most $f/(N - 1) \le b^{-8}$, and when `GaugeLe.zpow_neg_smul` restores integrality from a real-valued bound $p^r$, it necessarily quantizes the gain to an integer exponent $\beta \le -r$.

## Level 2 — Undergrad Bridge

Let $E$ be the CM field with $f$ infinite places, and let $G_f = \operatorname{Gf}(E, n)$ be the finite adelic unitary group of the standard definite Hermitian form $\sum x_i \bar{x}_i$ of rank $n$. Following Gross (1999) and Greenberg & Voight (2014), an algebraic modular form $\varphi \in \operatorname{AlgForm}(n, \kappa, e)$ maps $G_f$ into the Weyl module $\mathbb{C}^{\operatorname{WeylIndex}(n, \kappa)}$ (Jantzen 2003), where a tuple $h = (h_\tau)_{\tau \in \Phi} \in \prod_\tau \operatorname{GL}_n$ acts in weight $(\kappa, e)$ by
$$\operatorname{weylRep}(\kappa, e)(h) = \left(\prod_{\tau \in \Phi} (\det h_\tau)^{e_\tau}\right)\operatorname{schur}(\kappa, h).$$

1. **Weyl gauge and B-short forms (`IsShort`):** Fix a prime $p$ and an embedding $\iota_p : \mathbb{C} \hookrightarrow \mathbb{C}_p$ (`PadicPlace p`). At $g \in G_f$, the **Weyl gauge** of a coordinate vector $v : \operatorname{WeylIndex}(n, \kappa) \to \mathbb{C}$ is
   $$\operatorname{weylGauge}_p(\kappa, e, g, v) = \bigl\|\operatorname{weylRep}(\kappa, e)\bigl(\operatorname{adeleExtension}(\iota_p)(g^{-1})\bigr)\,(\iota_p \circ v)\bigr\|_p.$$
   For a fixed base shift $d \in G_f$ and real bound $c$, `GaugeLe d c φ` asserts $\operatorname{weylGauge}_p(\kappa, e, g d, \varphi(g)) \le c$ for all $g \in G_f$. A form $\varphi$ is **B-short** (`IsShort B φ`) if:
   - for every prime $p$ and every `PadicPlace p`, `GaugeLe 1 1 φ` holds (integrality in Weyl's lattice at every $g$); and
   - for every $\sigma \in \operatorname{Aut}(\mathbb{C})$ and every $g \in G_f$, $\operatorname{weylNormSq}(\sigma \circ \varphi(g)) \le B$.

2. **Spanning by B-short delta forms (`span_isShort_eq_top`):** Suppose $K \le G_f$ is a *free* level subgroup ($\Gamma \cap g K g^{-1} = \{1\}$ for all $g \in G_f$). At each double coset $\Gamma x K$, choosing invertible matrices $Y_0, Y_\tau \in \operatorname{GL}_n(E)$ with $x^{-1} Y_0$ and $x^{-e_\tau} Y_\tau$ integral over $\widehat{\mathcal{O}}_E$ and entries bounded by $R = C = 6 N_E (f n)^2$ (`shortBasisConst E n N_E`, where $N_E \in \mathfrak{D}_{E/\mathbb{Q}} \setminus \{0\}$) yields coordinate vectors
   $$v = \left(\prod_{\tau \in \Phi} \det Y_\tau\right)\operatorname{weylRep}(\kappa, 0)(Y_0)\,\operatorname{weylEval}(y)$$
   satisfying $\operatorname{weylGauge}_p(\kappa, e, x, v) \le 1$ (`norm_weylRep_mulVec_det_smul_weylEval_le_one`) and $\operatorname{weylNormSq}(\sigma \circ v) \le R^{6M}$ (`weylNormSq_det_smul_weylRep_mulVec_weylEval_le`) for $M \ge n f + \operatorname{boxes}(\kappa, n)$. Turning these into delta forms (`isShort_deltaForm`) spans the entire level space by **B-short** forms for every $B \ge C^{6M}$.

3. **Ramified source forms (`ramifiedSourceForms`, `span_isShort_ramifiedSourceForms_eq_top`):** For admissible data $D$ with prime level $N = \pi_N \bar{\pi}_N$ ($\pi_N \in \mathcal{O}_E$), each place $w \mid \pi_N$ has residue field $\mathbb{F}_N$ and a chosen norm-one finite idele $z_w = D.\mathrm{scalingGenerator}\ w \in U(1)(\mathbb{A}_f)$ that is integral, equal to $1$ off $\{w, \bar{w}\}$, and has order $N - 1$ modulo $w$. Acting by scalar matrices gives `D.centralRep z_w` with $(D.\mathrm{centralRep}\ z_w)^{N-1} = 1$. The submodule of **ramified source forms** is
   $$D.\mathrm{ramifiedSourceForms} = \bigcap_{w \mid \pi_N} \ker\!\left(\sum_{i=0}^{N-2} (D.\mathrm{centralRep}\ z_w)^i\right) = \operatorname{range}\!\left(\prod_{w \mid \pi_N} \bigl(1 - D.\mathrm{centralRep}\ z_w\bigr)\right)$$
   (`ramifiedSourceForms_eq_range`). Because each kernel $\ker(1 - z_w)$ has dimension at most $(N - 1)^{-1} \dim(D.\mathrm{SourceForm})$ (`finrank_ker_one_sub_scalingGenerator_le`) and $|\{w \mid \pi_N\}| \le f$,
   $$\dim(D.\mathrm{ramifiedSourceForms}) \ge \left(1 - \frac{f}{N - 1}\right)\dim(D.\mathrm{SourceForm}) \ge \bigl(1 - b^{-8}\bigr)\dim(D.\mathrm{SourceForm}).$$
   Since each difference operator $1 - z_w$ multiplies shortness by $4$ (`IsShort.sub_translate`, `isShort_ramifyingOp`), applying `D.ramifyingOp univ` to the $\exp(E_{\mathrm{err}})/4^f$-short spanning set (`New.Data.span_isShort_eq_top`, where $E_{\mathrm{err}} = f a (\log b)^3$) spans `D.ramifiedSourceForms` by $\exp(E_{\mathrm{err}})$-short forms.

4. **Ultrametricity and gauge rescaling (`GaugeLe.zpow_neg_smul`):** Because the **$p$-adic gauge** is ultrametric (`GaugeLe.add`), monotone in the exponent (`GaugeLe.rpow_mono`), and scales by $\|\iota_p(a)\|_p$ under $a \bullet \varphi$ (`GaugeLe.smul`), any form satisfying `GaugeLe d (p ^ r) φ` and any integer $\beta \le -r$ obey
   $$\|\iota_p(p^{-\beta})\|_p \cdot p^r = p^{\beta + r} \le 1 \implies \operatorname{GaugeLe}\ d\ 1\ \bigl((p : \mathbb{C})^{-\beta} \bullet \varphi\bigr).$$
   Applied to the local divisibility bound `GaugeLe 1 (p ^ (allowedLoss p - gain p)) (compositeLift ϑ Ψ φ)`, this converts real-valued divisibility exponents into exact integer prime powers that divide out of the lifted form while preserving global integrality.

## Level 3 — Formal Lean Contract

Stage 03 extracts `IsShort`, `span_isShort_eq_top`, `ramifiedSourceForms`, and `span_isShort_ramifiedSourceForms_eq_top` into `skeleton/03_upper_bound/short_columns.lean` (from upstream `src/New/UpperBound/Size/ShortColumns.lean`), and the gauge rescaling calculus into `skeleton/03_upper_bound/local_divisibility.lean` (from upstream `src/New/UpperBound/Divisibility/LocalDivisibility.lean`):

- `03_upper_bound/short_columns.lean#New.IsShort` (upstream `src/New/UpperBound/Size/ShortColumns.lean:157-161`):
  ```lean
  def New.IsShort (B : ℝ) (φ : AlgForm n κ e) : Prop
  ```
- `03_upper_bound/short_columns.lean#New.span_isShort_eq_top` (upstream `src/New/UpperBound/Size/ShortColumns.lean:366-433`):
  ```lean
  lemma New.span_isShort_eq_top {K : Subgroup (Gf E n)} [IsLevel K] [Nonempty n]
      (free : ∀ g : Gf E n, classAut stdRatPoints K g = ⊥)
      {N M : ℕ} [NeZero N] (hN_diff : ↑N ∈ differentIdeal ℤ (𝓞 E))
      (hM : Fintype.card n * Fintype.card (InfinitePlace E) + boxes κ (Fintype.card n) ≤ M) {B : ℝ}
      (hB : (shortBasisConst E n N : ℝ) ^ (6 * M) ≤ B) :
      span ℂ {φ : levelSpace (algFormRep κ e) K | IsShort B φ.val} = ⊤
  ```
- `03_upper_bound/short_columns.lean#New.Data.ramifiedSourceForms` (upstream `src/New/UpperBound/Size/ShortColumns.lean:743-746`):
  ```lean
  def New.Data.ramifiedSourceForms {P : Parameters} (D : Data P) : Submodule ℂ D.SourceForm
  ```
- `03_upper_bound/short_columns.lean#New.Data.span_isShort_ramifiedSourceForms_eq_top` (upstream `src/New/UpperBound/Size/ShortColumns.lean:1008-1020`):
  ```lean
  lemma New.Data.span_isShort_ramifiedSourceForms_eq_top {P : Parameters} (D : Data P) :
      span ℂ {φ : D.ramifiedSourceForms | IsShort D.errorUnit.exp (φ : D.SourceForm).val} = ⊤
  ```
- `03_upper_bound/local_divisibility.lean#New.GaugeLe.zpow_neg_smul` (upstream `src/New/UpperBound/Divisibility/LocalDivisibility.lean:115-125`):
  ```lean
  lemma New.GaugeLe.zpow_neg_smul {r : ℝ} {β : ℤ} {φ : AlgForm (E := E) n κ e} (hφ : GaugeLe d (p ^ r) φ)
      (hβ : β ≤ -r) : GaugeLe d 1 ((p : ℂ) ^ (-β) • φ)
  ```

Notice how `span_isShort_eq_top` feeds `New.Data.span_isShort_eq_top` and `span_isShort_ramifiedSourceForms_eq_top` to produce short integral spanning columns for the source space, while `GaugeLe.zpow_neg_smul` supplies the rescaling step that turns local **$p$-adic gauge** gains into integral rescaled outputs. See the page's live `<VerificationBanner />` (`audit/verification-status.json`) for the currently pinned upstream snapshot and local Lean build status (`build_status: unverified-locally`).
