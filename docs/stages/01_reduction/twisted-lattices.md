# Twisted Lattices & A Twist for Each Prime

This page explains the `New.TwistedLattice` abstraction (`src/New/Reduction/Lattices/TwistedLattice.lean`), the height calculus (`src/New/Reduction/Lattices/Heights.lean`), and the headline product-formula inequality `New.TwistedLattice.covolume_add_index_nonneg` ("a twist for each prime") that underpins Theorem C in the reduction. Return to the [Stage 01 Overview](./overview.md) or review how Theorem C balances against Theorems A and B in [The Main Inequality & Theorems A and B](./main-inequality.md).

## Level 1 — Abitur / First-Principles

Take any nonzero fraction $q = a/b$ written in lowest terms. If you measure its ordinary real size $|q|$ alongside its divisibility by every {{concept:prime-number|indivisible whole number greater than $1$}}, those measurements obey a strict conservation law: the ordinary size and all prime-scale sizes multiply to $1$, so their logarithms sum to $0$. In particular, any nonzero integer has ordinary absolute value at least $1$, and any basis of an ordinary integer grid encloses a squared volume of at least $1$—you cannot compress independent integer vectors arbitrarily close to zero without forcing a denominator to appear at some prime.

In this proof, however, no single classical integer grid works at every prime simultaneously. The automorphic lifts attached to a fixed level can misbehave at bad primes unless you switch to an auxiliary version of the space whose level avoids that prime. To handle this, the proof replaces a single integer grid with a {{concept:twisted-lattice|structure that measures lengths in one fixed standard space while checking divisibility across a family of prime-dependent spaces}}:
- **At infinity (ordinary size):** lengths and volumes are measured once and for all in the single standard space (the "trivial twist" $1$), using a rational inner-product matrix so that every symmetry of {{concept:complex-plane|the two-dimensional number system of real and imaginary parts}} preserves the ruler cleanly. From this standard space we measure {{concept:gram-determinant|the squared $n$-dimensional volume of a list of vectors, computed as the determinant of their pairwise inner products}}.
- **At each finite prime $\ell$:** divisibility is checked across every auxiliary space ("twist") allowed at $\ell$. A vector is considered integral at a prime place if its image is integral in *all* twists allowed at that prime.

Now suppose you have $n$ short, nearly integral vectors $y_1, \dots, y_n$—the vectors produced by Theorems A and B—and you complete them to a full basis of the target space by adjoining $n'$ remaining vectors $z_1, \dots, z_{n'}$. How do you force a lower bound on the {{term:gram-determinant|covolume}} of $y$ when every prime wants a *different* twist $\xi_\iota$?

You perform a three-way currency conversion on the basis's {{term:gram-determinant}} at the trivial twist, $\mathrm{Gram}(1)$:
1. Switch from the trivial twist $1$ to a prime-specific allowed twist $\xi_\iota$, paying the exact geometric exchange rate $Z(1) / Z(\xi_\iota)$ between the two spaces.
2. Clear the denominators of the $n'$ completion vectors $z_i$ using a scalar $\gamma(\xi_\iota)$, and record the cost of putting that scalar back in.
3. Clear the near-integrality error ("index") of the $n$ vectors $y_i$ using a single prime-level clearing matrix that works *simultaneously* for every allowed twist at $\ell$.

Because inner products pair vectors with their conjugates, the clearing matrix enters twice—once for the embedding and once for its conjugate—giving a factor of $2\delta$ for the index. Applying {{concept:arithmetic-height|the global accounting rule balancing a number's ordinary size against its divisibility at every prime}} to all three factors together proves the headline inequality: the ordinary size bound plus twice the index plus the completion and exchange-rate costs must be at least $0$.

![Three-factor decomposition linking the trivial-twist volume determinant to prime-by-prime twisted determinants through the global conservation law](assets/stage01-twisted-lattice-product-formula.svg)

### Where this analogy breaks

Describing the exchange rate $Z(1)/Z(\xi_\iota)$ as a simple prime-by-prime multiplication hides the core technical subtlety of `Heights.lean`. The twists $\xi_\iota$ and scalars $\gamma(\xi_\iota)$ *change from prime to prime*, so each individual factor is a family of {{term:complex-plane|complex numbers}} indexed by prime places rather than a single global algebraic number. You cannot apply the {{term:arithmetic-height|product formula}} directly to a prime-varying family. Instead, {{term:arithmetic-height}} wraps each family in a *single* nonzero global multiplier $H$ whose ordinary size is bounded uniformly across all symmetries of the {{term:complex-plane|complex numbers}}, while $H$ times the local family is bounded at each prime by a local {{term:arithmetic-height|p-adic valuation}} cap. Multiplying the three families cancels the prime-varying pieces back into the single constant number $\mathrm{Gram}(1)$, and only then does the {{term:arithmetic-height|product formula}} apply to the rational norm of the combined global multiplier $H$.

## Level 2 — Undergrad Bridge

### 1. Why a single $\mathbb{Z}$-lattice fails, and how `New.TwistedLattice` splits the roles

In a classical geometry-of-numbers argument, a single lattice $\Lambda \subset V$ supplies both an archimedean inner product (for Hadamard's upper bound on the Gram determinant) and integrality at every prime $\ell$ (for the lower bound $\det \ge 1$). In the automorphic setting of Stages 02–07, however, the theta-lift maps $\Theta_\xi$ depend on a twist $\xi \in T$ whose conductor must be coprime to $\ell$ to control $\ell$-adic valuations cleanly, while at $\infty$ we only have sharp size control at the trivial twist $\xi = 1$.

`New.TwistedLattice V T` (`src/New/Reduction/Lattices/TwistedLattice.lean:113-138`) axiomatizes exactly this separation:
- **Twisted inner product spaces and lifts:** for each $\xi \in T$, a complex inner product space `Space ξ` and a $\mathbb{C}$-linear map $\Theta_\xi = L.\mathrm{lift}\ \xi : V \to \mathrm{Space}\ \xi$, together with a predicate `IsAllowed l ξ` specifying which twists are permitted at a prime $l$.
- **$p$-adic lattices and conjugate pairing:** for each `PrimePadicIso` $\iota = (\ell, \iota.\mathrm{iso} : \mathbb{C} \simeq+* \overline{\mathbb{Q}}_\ell)$—inducing the valuation subring $\mathcal{O}_\iota = \iota.\mathrm{integers} = \{x \in \mathbb{C} : \|\iota(x)\| \le 1\}$ and conjugate isomorphism $\bar{\iota}(z) = \iota(\bar{z})$ (`PrimePadicIso.conj`)—each `Space ξ` carries an $\mathcal{O}_\iota$-submodule `L.lattice ξ ι`. Whenever `IsAllowed ι.prime ξ` holds, the pairing axiom `norm_inner_le_one` requires
  $$\forall \varphi \in L.\mathrm{lattice}\ \xi\ \bar{\iota},\ \forall \varphi' \in L.\mathrm{lattice}\ \xi\ \iota,\qquad \|\iota(\langle \varphi, \varphi' \rangle_{\mathbb{C}})\| \le 1.$$
  A vector $y \in V$ belongs to `L.integralVectors ι` if and only if $\Theta_\xi y \in L.\mathrm{lattice}\ \xi\ \iota$ for *every* twist $\xi$ allowed at $\ell = \iota.\mathrm{prime}$ (`mem_integralVectors`).
- **Rational coordinates at the trivial twist $\xi = 1$:** `Space 1` has finite coordinates `coord : Space 1 →ₗ[ℂ] Coord → ℂ` and a *rational* matrix `gramMatrix : Matrix Coord Coord ℚ` whose complexification is positive-definite (`posDef_gramMatrix`) and represents the inner product (`inner_eq`):
  $$\langle \varphi, \varphi' \rangle_{\mathbb{C}} = (\mathrm{coord}\ \varphi)^* \, G \, (\mathrm{coord}\ \varphi'), \qquad G = L.\mathrm{gramMatrix.map}(\uparrow).$$

### 2. Index (`IndexLe`), covolume (`CovolumeLe`), and volume (`Z(ξ)`)

**Index (`IndexLe`) and why it enters with a factor of $2\delta$.**
A square matrix $u \in M_S(\mathcal{O}_\iota)$ *carries* a family $y : S \to W$ into an $\mathcal{O}_\iota$-submodule $M$ with loss $a$ (`PrimePadicIso.Carries u y M a`) if $\|\iota(\det u)\| \ge \ell^{-a}$ and $\sum_k u_{ik} y_k \in M$ for all $i$. A family $y : S \to V$ satisfies `L.IndexLe y δ` (`TwistedLattice.lean:163-168`) if there exist $e(\ell) \ge 0$ with $\sum_{\ell \in U} e(\ell)\log \ell \le \delta$ for every finite set $U$ of primes, and for every $\iota \mid \ell$ a matrix $u \in M_S(\mathcal{O}_\iota)$ satisfying `ι.Carries u y (L.integralVectors ι) (e ι.prime)`. Crucially, **$u$ depends on $\iota$ but is completely independent of the twist $\xi$**, because its target is `L.integralVectors ι` (the intersection over all twists allowed at $\ell$). In practice, `IndexLe` is established from submodule colength bounds (`ColengthDvd M (L.integralVectors ι) (l^j)`) via `indexLe_of_colengthDvd` (which uses `inv_pow_le_norm_of_dvd`) and assembled across unions by block-diagonal matrices (`Carries.fromBlocks`, `IndexLe.sumElim`, `indexLe_card_mul`).

To see why $\delta$ doubles to $2\delta$, consider any twist $\xi$ allowed at $\ell = \iota.\mathrm{prime}$ and any family $w$ with clearing matrices $u$ at $\iota$ and $u'$ at $\bar{\iota}$. Because the Gram matrix entries $\langle \Theta_\xi w_i, \Theta_\xi w_k \rangle$ are conjugate-linear in the first slot and linear in the second, `norm_inner_le_one` pairs `lattice ξ ι.conj` with `lattice ξ ι`, yielding `norm_det_mul_det_gram_le_one`:
$$\|\bar{\iota}(\det u')\| \cdot \|\iota(\mathrm{Gram}(\xi))\| \cdot \|\iota(\det u)\| \le 1, \qquad \mathrm{Gram}(\xi) := \det\bigl(\langle \Theta_\xi w_i, \Theta_\xi w_k \rangle\bigr).$$
When $w = \mathrm{Sum.elim}\ y\ z$ adjoins $n'$ vectors $z_i$ with $\gamma \Theta_\xi z_i$ integral at both $\iota$ and $\bar{\iota}$, `exists_matrix_elim_mem_lattice` forms block-diagonal clearing matrices $v = \operatorname{diag}(u, \gamma I_{n'})$ and $v' = \operatorname{diag}(u', \gamma I_{n'})$ with $\det v = (\det u)\gamma^{n'}$ and $\det v' = (\det u')\gamma^{n'}$. Since both $u$ at $\iota$ and $u'$ at $\bar{\iota}$ satisfy the determinant lower bound $\ge \ell^{-a} = \exp(-a\log \ell)$ (recall $\iota$ and $\bar{\iota}$ lie over the same prime $\ell$), `norm_pow_mul_det_gram_le` (`TwistedLattice.lean:389-419`) rearranges the product into
$$\bigl\|\iota\bigl((|\gamma|^2)^{n'} \mathrm{Gram}(\xi)\bigr)\bigr\| = \|\bar{\iota}(\gamma)\|^{n'} \|\iota(\mathrm{Gram}(\xi))\| \|\iota(\gamma)\|^{n'} \le \exp(2 a \log \ell).$$
Both clearing determinants contribute $a \log \ell$, doubling $\delta$ to $2\delta$.

**Covolume (`CovolumeLe`), conjugates, Hadamard, and Fischer.**
For any $\sigma \in \operatorname{Aut}(\mathbb{C})$, write `L.conjCoord σ y := σ ∘ L.coord (L.lift 1 y)` and `L.normSqAt y σ := Re((conjCoord σ y)* G (conjCoord σ y))`. A family $y : S \to V$ satisfies `L.CovolumeLe y c` if $\|\det \mathrm{formGram}(G, \mathrm{conjCoord}\ \sigma \circ y)\| \le \exp c$ for every $\sigma \in \operatorname{Aut}(\mathbb{C})$. Three lemmas make `CovolumeLe` immediately usable:
1. **Galois conjugates (`CovolumeLe.norm_map_det_gram_le`):** Since $G$ has rational entries, applying $\sigma$ to $\langle \Theta_1 v, \Theta_1 w \rangle$ yields `map_inner_lift_one`:
   $$\sigma\langle \Theta_1 v, \Theta_1 w \rangle = (\mathrm{conjCoord}\ (\mathrm{conjAut}\ \sigma)\ v)^* \, G \, (\mathrm{conjCoord}\ \sigma\ w), \qquad (\mathrm{conjAut}\ \sigma)(z) := \overline{\sigma(\bar{z})}.$$
   Cauchy–Schwarz on the exterior power (`norm_det_pairing_sq_le`) gives $|\sigma(\mathrm{Gram}(1))|^2 \le |\det\mathrm{formGram}(G, \mathrm{conjCoord}(\mathrm{conjAut}\ \sigma) \circ y)| \cdot |\det\mathrm{formGram}(G, \mathrm{conjCoord}\ \sigma \circ y)| \le (\exp c)^2$, hence $\|\sigma(\mathrm{Gram}(1))\| \le \exp c$ for every $\sigma \in \operatorname{Aut}(\mathbb{C})$.
2. **Hadamard's inequality (`covolumeLe_of_normSqAt_le`):** If `L.normSqAt (z i) σ ≤ exp h` for all $i \in \mathrm{Fin}\ n$ and all $\sigma$, then `L.CovolumeLe z (n * h)`.
3. **Fischer's inequality (`CovolumeLe.sumElim`):** If `L.CovolumeLe y c` and `L.CovolumeLe z c'`, then the block Gram matrix of `Sum.elim y z` satisfies $|\det M| \le |\det M_{11}| \cdot |\det M_{22}| \le \exp(c + c')$, giving `L.CovolumeLe (Sum.elim y z) (c + c')`.

**Volume (`L.volume ξ`).**
Assuming $V$ is a finite-dimensional complex inner product space, `L.volume ξ := (L.lift ξ).normDet ^ 2` (`TwistedLattice.lean:336`) represents $Z(\xi) = \det(\Theta_\xi^* \Theta_\xi)$, the Gram determinant of the $\Theta_\xi$-images of an orthonormal basis of $V$. It is nonzero whenever $\Theta_\xi$ is injective (`volume_ne_zero`), and for *any* basis $w$ of $V$ (`finrank ℂ V = n + n'`), `det_gram_comp` gives the basis-factorization identity
$$\mathrm{Gram}(\xi) = Z(\xi) \cdot \det \mathrm{gram}_{\mathbb{C}}(w).$$

### 3. Heights with a single global multiplier (`Heights.lean`) and "A Twist for Each Prime"

For a family $y : \mathrm{PrimePadicIso} \to \mathbb{C}$, `New.HeightLe c y` (`src/New/Reduction/Lattices/Heights.lean:45-52`) asserts the existence of a **single nonzero global multiplier $H \in \mathbb{C}^\times$** (independent of $\iota$), an archimedean bound $\alpha \in \mathbb{R}$, and prime bounds $h(\ell) \ge 0$ such that:
1. $\|\sigma H\| \le \exp \alpha$ for all $\sigma \in \operatorname{Aut}(\mathbb{C})$,
2. $\|\iota(H \cdot y_\iota)\| \le \ell^{h(\ell)}$ for all $\iota \mid \ell$, and
3. $\alpha + \sum_{\ell \in T} h(\ell)\log \ell \le c$ for every finite set $T$ of primes.

Because $H$ is global while $y_\iota$ may vary with $\iota$, heights compose multiplicatively (`HeightLe.mul`, `HeightLe.pow`, `HeightLe.prod`) even when each prime uses a different twist $\theta(\iota)$. Applying `HeightLe.of_le` (taking $H = 1$, $\alpha = 0$) to `norm_pow_mul_det_gram_le` gives `heightLe_pow_mul_det_gram`: the family $\iota \mapsto (|\gamma_\iota|^2)^{n'} \mathrm{Gram}(\theta(\iota))$ has height at most $2\delta$.

This prepares the headline theorem **`New.TwistedLattice.covolume_add_index_nonneg`** (`TwistedLattice.lean:454-485`): given injective lifts $\Theta_\xi$, a basis $w = \mathrm{Sum.elim}\ y\ z$ of $V$ with $n + n' = \dim V$, allowed twists $\theta(\iota)$ at every $\iota$, covolume bound $c$ and index bound $\delta$ for $y$, nonzero scalars $\gamma(\xi)$ making $\gamma(\xi)\Theta_\xi z_i$ integral at every allowed $\iota$, archimedean length bound $b_{\mathrm{short}}$ for $z$, and height bounds
$$\mathrm{HeightLe}\ b_{\mathrm{denom}}\ \bigl(\iota \mapsto |\gamma(\theta\iota)|^{-2}\bigr), \qquad \mathrm{HeightLe}\ b_{\mathrm{volume}}\ \left(\iota \mapsto \frac{Z(1)}{Z(\theta\iota)}\right),$$
we conclude
$$0 \le c + 2\delta + n'(b_{\mathrm{short}} + b_{\mathrm{denom}}) + b_{\mathrm{volume}}.$$

The proof is a 20-line chain of three steps:
1. **Exact three-factor decomposition of $\mathrm{Gram}(1)$ (`h_factor`).** Since $\mathrm{Gram}(\xi) = Z(\xi) \cdot \det\mathrm{gram}_{\mathbb{C}}(w)$ with $Z(\theta\iota) \neq 0$ and $|\gamma(\theta\iota)| \neq 0$, the basis determinant cancels identically at every $\iota$:
   $$\mathrm{Gram}(1) = \Bigl((|\gamma(\theta\iota)|^2)^{n'} \mathrm{Gram}(\theta\iota)\Bigr) \cdot \Bigl((|\gamma(\theta\iota)|^2)^{-1}\Bigr)^{n'} \cdot \frac{Z(1)}{Z(\theta\iota)}.$$
   By `heightLe_pow_mul_det_gram`, `hγ.pow n'`, `hZ`, and `HeightLe.mul`, the constant family $\iota \mapsto \mathrm{Gram}(1)$ has height at most $2\delta + n' b_{\mathrm{denom}} + b_{\mathrm{volume}}$.
2. **Uniform conjugate bound on $\mathrm{Gram}(1)$.** Combining `hc` with Hadamard (`covolumeLe_of_normSqAt_le hz_short`) via Fischer (`CovolumeLe.sumElim`) and `CovolumeLe.norm_map_det_gram_le` yields $\|\sigma(\mathrm{Gram}(1))\| \le \exp(c + n' b_{\mathrm{short}})$ for every $\sigma \in \operatorname{Aut}(\mathbb{C})$, while linear independence of $w$ and injectivity of $\Theta_1$ give $\mathrm{Gram}(1) \neq 0$.
3. **Product formula via `HeightLe.neg_le_of_norm_map_le`.** Multiplying the constant family $\mathrm{Gram}(1)$ by the constant family $\mathrm{Gram}(1)^{-1}$ (which has height $\le c + n' b_{\mathrm{short}}$ by `HeightLe.inv_const`) produces `HeightLe (c + n' b_short + 2δ + n' b_denom + b_volume) 1`. Inside `HeightLe.nonneg_of_one` (`Heights.lean:195-225`), uniform $2$-adic boundedness of the global multiplier $H$ across all $\iota : \mathbb{C} \simeq+* \overline{\mathbb{Q}}_2$ forces $H$ to be algebraic over $\mathbb{Q}$ (`isAlgebraic_of_norm_complexToCp_le`). Setting $q := \operatorname{Norm}_{\mathbb{Q}(H)/\mathbb{Q}}(H) \in \mathbb{Q}^\times$ and $d := [\mathbb{Q}(H):\mathbb{Q}] > 0$, `norm_algebraMap_norm_gen_le` bounds $\log|q| \le d\alpha$ and $\log|q|_\ell \le d\,e(\ell)\log \ell$. Applying the rational `log_product_formula` ($\log|q| + \sum_{\ell < N} \log|q|_\ell = 0$) gives
   $$0 \le d\bigl(c + 2\delta + n'(b_{\mathrm{short}} + b_{\mathrm{denom}}) + b_{\mathrm{volume}}\bigr),$$
   and dividing by $d > 0$ completes the proof.

## Level 3 — Formal Lean Contract

The extracted Stage 01 skeleton below reproduces the complete public API of `src/New/Reduction/Lattices/TwistedLattice.lean` (lines `55–485`) at pinned upstream commit `41c2716658c42ba08eb0648994a2efe83082a95c` (short `41c2716658c4`), preserving the exact `PrimePadicIso.Carries` and `New.TwistedLattice` structure definitions while replacing proof bodies with deterministic SHA-256 extraction stubs. Together with `New.HeightLe` and `New.log_product_formula` in `src/New/Reduction/Lattices/Heights.lean`, `New.TwistedLattice.covolume_add_index_nonneg` is the exact interface invoked by Stages 05–07 (`New.Determinant.Twist.LongForms`) to deduce Theorem C (`D.integral_forms_long`) for [The Main Inequality & Theorems A and B](./main-inequality.md).

**Verification status (`audit/verification-status.json`):** Local Lean build verification is **`unverified-locally`** (`scan_scope: entry`, pinned commit `41c2716658c42ba08eb0648994a2efe83082a95c`, `0` forbidden tokens in `src/Main.lean`, `5` expected `sorry` tokens in root `Challenge.lean`).

<!-- rh:lean-node:01_reduction/twisted_lattice -->
