# site/docs/.vitepress/theme/components/ConceptHover.vue

## Governing Overview

[Repository overview](../../../../../overview.md)

## Purpose

Renders subsequent inline concept mentions as a script-off-safe Concept Page link paired with a hover, focus and click/Enter-pinnable recall card that closes on `Escape` without trapping focus.

## Code Commentary

Accepts required `slug`, `term`, `display`, `glimpse` and `href` props plus optional `visualSrc` and `visualAlt`. Generates collision-free popover IDs via Vue `useId()` and guards both `resolvedHref` and `resolvedVisualSrc` against double-prefixing `site.value.base` (`resolveSitePath`). Opens on hover (with a 120ms leave grace timer), focus-in or `?` button click/Enter pin; `updateDesktopPosition` right-aligns cards past the viewport midpoint and clamps within `16px` gutters via `--rh-card-shift` on `>640px` viewports while yielding to CSS fixed bottom-sheet layout at `<=640px`. `Escape` (`closeAll`) and the `×` button (`closeAndReturnFocus`) close the card, set `suppressFocusOpen` to prevent immediate reopen, and restore focus to the trigger when focus was inside the popover; outside clicks dismiss pinned cards. Without JavaScript, the term link stays clickable and full definitions remain in the page's static glossary fallback.

## Relationships

Registered globally in [theme/index.ts](../index.ts.md), styled by [tokens.css](../tokens.css.md), and emitted by `_render_concept_hover` in [markdown_transforms.py](../../../../../src/rh_pipeline/site/markdown_transforms.py.md) from `{{term:<slug>[|<display>]}}`.

## Evidence

- Defines `ConceptHover` props, `useId()` popover IDs and base-safe path resolution for links and optional mini SVG visuals. [1]
- Computes desktop viewport-clamped alignment (`--rh-card-shift`, `16px` gutter) on `>640px` viewports while delegating `<=640px` layout to CSS bottom-sheet rules. [2]
- Manages 120ms hover grace, focus tracking, pin toggle, outside-click dismissal and `Escape`/close focus return with `suppressFocusOpen` guard. [3]
- Renders a static concept link, ARIA-expanded `?` trigger and conditional `role="region"` recall card with close button, lazy visual and full Concept Page link. [4]
