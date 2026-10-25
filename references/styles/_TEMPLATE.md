# NN · Style name (artist / tradition, date or period)

<!--
Card template for web-art-styles. One card per style, English, written for designing websites and app UIs.
Keep every heading. Write "—" under a heading only if the source genuinely has nothing for it.
Facts about the artist or tradition stay modest and checkable; never invent "the official hex".
Numbers (hex, px, ms, angles) come from the source card where it has them; otherwise say "suggested".
-->

**Status:** Built ★★★ | Built ★★ | Built ★ | Research recipe — *(from the source catalogue; research recipes are unverified ideas, not tested treatments)*
**Layer:** Material (works on any subject) | Period specimen (strongest as tribute or easter egg)
**Web fit:** High / Medium / Low — one line on where it fits (product UI, marketing page, editorial, easter egg) and where it doesn't.
**Mockup:** `assets/mockups/<file>.png` *(only if one exists; otherwise omit this line)*

## Recognisable traits

3–5 bullets: the stable visual features that make people name the style at a glance (material, mark-making, colour relationship, light, form, space). Each trait must survive a change of subject.

## Palette

A table with roles for a UI: background, surface/panel, text, muted text, accent, accent 2, plus any style-specific colour (gold leaf, ochre…). Hex values from the source when present, else "suggested". One line on the colour *relationship* (what goes with what, what never appears). Note a light and a dark reading if the style supports both.

## Typography

Display face and body face (prefer free Google Fonts, give a fallback stack), sizes/weights/tracking for labels. Keep body text readable; style fonts go on headings and labels.

## Surfaces, texture and edges

How panels, cards, backgrounds and edges are made in the style, and how to build that for the web (CSS gradients, SVG filters, a pre-rendered texture tile, clip-path, canvas drawn once). Be concrete.

## Shapes and layout

Corner treatment, line weight, borders/outlines, shadows (offset? blurred? none?), spacing rhythm, composition tendencies (asymmetry, empty space, grids, framing) and how they map onto page layout.

## Components

Short notes for: buttons, cards, navigation/sidebar, inputs, chat or message bubbles, tables, badges/tags, icons, loading state, empty state. Skip ones the style has nothing to say about.

## Motion

What moves in this style and how (the source's "motif animations"), translated to UI: idle ambience, hover, loading/thinking, success/"done" moment, page entrance. Timing and easing (stepped vs smooth). Always include the `prefers-reduced-motion` fallback.

## Techniques

The source's rendering methods and key parameters, translated into web terms. Short code (CSS/SVG/JS) only where it saves real explanation. Keep numbers from the source.

## Performance and accessibility

What is expensive (full-screen filters, per-frame canvas, big blurs) and the cheap route (draw once, tile, CSS transforms, cap DPR at 2, pause when the tab is hidden). Contrast risks and how to keep body text at WCAG AA. Motion sensitivity.

## Pitfalls

Mistakes recorded in the source ("坑") and the obvious UI ones, as short "Problem → fix" bullets.

## Nearest neighbours

How to tell it apart from the 1–2 closest styles in the catalogue (two concrete differences each).

## Source

One line: `Adapted from huashu-art-motion · <original card file name>` plus the original card's quality note / known shortcoming if any.
