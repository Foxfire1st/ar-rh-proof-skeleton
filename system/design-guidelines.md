# Design Process & Visual Guidelines

This repository builds a public, concept-first mathematical explainer on GitHub Pages (VitePress). Its visual identity belongs to this project—do not import Agents Remember's dark dashboard aesthetic.

## Companion Design Files (in `system/`)

- `TREATMENT.md` — 7-stage anti-one-shot authoring pipeline, voice, concept-first inline reveal rules, zoom levels, and subagent-per-visual craft.
- `TECHNIQUES.md` — Approved interactive and visual primitives with still, reduced-motion, keyboard, and narrow-width requirements.

## Authority Order

1. Developer decisions for the task.
2. Written requirement packet.
3. Approved page/component reference or mockup.
4. `design-guidelines.md`, `TREATMENT.md`, `TECHNIQUES.md`, and the site's CSS token file.
5. Existing site code.

## Core Design Principles

- **Typography & reading comfort first:** Generous line height, readable measure (~65–75ch for prose), clean math rendering (KaTeX), high contrast (≥7:1 body text, ≥4.5:1 labels).
- **Color never carries meaning alone:** Inline concept spans, verification status banners, and diagram states always pair color with clear labels, borders, or markers.
- **Full prose without script:** Every essay, inline explanation, *"Where this analogy breaks"* callout, and formal Lean contract stays readable in static HTML before JavaScript executes.
- **No decorative AI slop:** Every figure answers a specific mathematical question; zero gratuitous gradients, glow effects, stock imagery, or looping animations.

## Browser Verification: Look, Then Operate

For every UI component or explainer page:
1. **Look** at full desktop width, `560px`, and `390px`: check math rendering, inline concept highlights, hover cards, spacing, and wrapping.
2. **Operate** every control by pointer and by keyboard alone (`Tab`, `Enter`, `Escape`, arrow keys); verify focus never traps and layout does not jump.
3. **Emulate reduced motion** (`prefers-reduced-motion: reduce`): verify every interactive figure renders its clear final labeled still form.
4. **Check script-off readability:** confirm the complete explanation remains intact in HTML.
