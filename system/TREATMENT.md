# Explainer Treatment & 7-Stage Authoring Pipeline

One-shot "read → dump page" authoring produces shallow summaries and skips source verification. Every stage guide, landmark card, and Concept Page follows these seven explicit stages:

## The 7 Authoring Stages

1. **Read with provenance:** Inspect the exact Lean signatures (in `rh-proof-skeleton/skeleton/` and upstream `riemann-hypothesis`), DAG neighborhood, `audit/verification-status.json`, and canonical external math sources. Record exact paths, commits, and citations.
2. **Select landmarks & scope:** Name what carries the mathematical argument in this stage and explicitly state what was left out and why.
3. **Inventory declarations & jargon:** List every Lean declaration anchored and every mathematical term that appears. No technical term may enter Level-1 prose unexplained.
4. **Write self-contained explanations & Concept Cards:** Draft each landmark explanation and create/update any missing canonical Concept Card (inline glimpse, full page, external sources, internal Lean anchors, *"Where this analogy breaks"*).
5. **Finish the complete essay first:** Stitch the explanations into one coherent narrative *before designing any visual*. The prose carries the complete logical spine; visuals deepen and illuminate it.
6. **Craft visuals as dedicated mini-projects (subagent fan-out):** Once the essay is complete, decide every visual that will genuinely unlock understanding—and **treat each visual as its own mini-project**, spawning one dedicated subagent per visual so each gets full context and craftsmanship rather than being glossed over in a crowded main turn. Every visual ships with still, reduced-motion, keyboard, and narrow-screen forms.
7. **Build, gate & browser check:** Run all deterministic gates (anchor resolution, concept slugs, source citations, jargon-order lint, static build), then look and operate in Playwright.

## Concept-First, Name-Second ("Matryoshka") Rule

- **First mention on a page:** Explain the idea in plain, concrete terms inside a visually highlighted inline span (clear start and end), then reveal its mathematical name as a clickable link.
- **Later mentions:** Show the linked term; hover or keyboard focus opens the same concise inline explanation plus mini visual.
- **Click-through:** Opens the dedicated Concept Page, where any new concept receives the same inline-first + hover + drill-down treatment.

## Three Simultaneous Zoom Levels

1. **Level 1 — Abitur / First-Principles:** High-school calculus, vectors/matrices, determinants, basic complex numbers, induction; deep prerequisites framed as honestly typed input/output black boxes. Always includes **"Where this analogy breaks."**
2. **Level 2 — Undergrad Bridge:** Concrete toy analogies and actual spaces, norms, sums, or identities bridging intuition to the real proof mechanism.
3. **Level 3 — Formal Lean Contract:** Exact anchored Lean declaration signature, plain-math translation of hypotheses and conclusion, upstream file/line provenance, and verification status inherited from `audit/verification-status.json`.

## Voice & Honesty

- Clear, direct prose; present tense, active voice; no hype, exclamation marks, superlatives, or handwaving.
- Never present an analogy as a proof or an unverified build claim as verified.
- Explicitly name what is not covered, what is a black box, and where an analogy breaks.
