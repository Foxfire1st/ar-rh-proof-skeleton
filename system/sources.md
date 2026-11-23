# Authoritative Sources & Provenance Policy

No mathematical statement or proof step may rest on "trust me bro." Every claim in the explainer is grounded in one or both of:

1. **Formal Lean 4 Proof (Upstream):**
   - Primary community-maintained Lean 4 proof repository: `https://github.com/leanprover-community/riemann-hypothesis` (`main`, local read-only checkout `/home/firefox/projects/riemann-hypothesis`).
   - Original 2026-11-20 announcement and 14-part archive: `https://alpo.ge/remark.pdf` and `https://alpo.ge/lean.tar.gz.part01` … `part14` (preserved verbatim at commit `40e56bab` in `leanprover-community/riemann-hypothesis`).
   - Independent replay archive and original 175,187-module manifest: `https://github.com/rohanarun/alpoge-rh-replay` (local read-only checkout `/home/firefox/projects/alpoge-rh-replay`).
   - Independent statement equivalence (`formal-conjectures`): `comparator/` inside `leanprover-community/riemann-hypothesis`.
2. **Canonical External Mathematical Literature (for Concept Cards):**
   - Standard peer-reviewed textbooks, papers, Mathlib documentation, or established expository references explaining each classical concept (e.g., Riemann zeta function, Dirichlet L-functions, analytic continuation, modular forms, CM curves, Galois cohomology).
   - Wherever a classical theorem is also formalized inside `leanprover-community/riemann-hypothesis`, Concept Cards cite **both** the external source and the internal Lean anchor.
