# Explainer Visual & Interactive Techniques

Every visual component serves one clear pedagogical job, runs purely client-side (zero backend), never loops, plays at most once on view or on reader action, and ships a complete still / reduced-motion / keyboard / narrow (`≤560px` & `390px`) form.

| Technique | Pedagogical Job | Implementation | Still / Reduced / Narrow Form | Status |
|---|---|---|---|---|
| Inline concept reveal | Introduce an unfamiliar math concept before naming it | Visually highlighted inline span (distinct background + start/end cues + revealed clickable term link) | Identical in static HTML | default |
| Concept hover/focus card | Recall an already-introduced term without leaving the sentence | Popover card on hover/focus showing the canonical inline glimpse + optional small SVG mini-visual + link to full Concept Page; Escape closes | Click-through link + page glossary fallback | default |
| Tabbed zoom levels | Switch smoothly between First-Principles, Undergrad, and Formal Lean views | Accessible segmented tabs (`←`/`→`, keyboard focus ring) keeping URL/state shareable | Stacked labeled sections in print/no-script HTML | default |
| Live verification banner | Surface ground-truth audit status on every page | Rendered at build time from `audit/verification-status.json` with exact commit, axiom audit, and statement-equivalence links | Static HTML banner | default |
| Scrubbable math parameter | Show how a bound or parameter (e.g., $X$, $m \approx \sqrt{X}$, $2/3$ barrier) shapes an inequality | Inline scrubbable number + reactive SVG/Canvas2D plot clearly marked as an illustrative model of the cited bound | Worked numeric table of 3 representative values + static plot | planned |
| Step-through proof mechanism | Walk a multi-step mathematical construction or contradiction in order | Back / Play-Pause / Next / Scrubber (`←`, `→`, `Space`, `Home`, `End`) + live state sentence; ≤12 steps | Numbered step list with final labeled SVG frame | planned |
| Stage dependency mini-map | Show how landmarks inside one stage connect | Clean inline SVG / Mermaid DAG with clickable nodes linking to landmark cards | Static SVG + ordered dependency list | default |
| Full proof DAG explorer | Navigate all 8 pillars and 41 extracted landmarks interactively | Client-side static graph reading `dag/dag.json`; search, stage filter, click to open card | Stage-grouped HTML index table | planned |
| Formal Lean contract block | Show exact anchored declaration beside plain-math translation | Syntax-highlighted Lean signature + provenance link to upstream file/lines + hypothesis/conclusion breakdown | Static code block + definition list | default |
