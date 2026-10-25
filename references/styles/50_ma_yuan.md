# 50 · Ma Yuan (Southern Song, c. 1160–1225)

**Status:** Research recipe — one redone single-scene study (a scholar under a willow on a riverbank corner), self-scored ~78 in its second round, independently reviewed ~74 in its first. Closest of the research recipes to 80, but still unverified.
**Layer:** Material (works on any subject) — off-centre weight, meaningful empty space and disciplined ink transfer directly to page layout. The source warns that "one corner" is a tendency, not a fixed bottom-left template.
**Web fit:** High for calm product UI, writing and reading tools, portfolios, editorial and luxury pages — the empty-space discipline is a layout system in itself. Medium for dense dashboards (keep the asymmetry in chrome, not in tables).
**Mockup:** see `assets/mockups/17_ink.png` (the "Ink & seal" direction uses Ma Yuan's composition).

## Recognisable traits

- Off-centre weight ("Ma One-Corner"): solid forms gather in one corner or edge; the rest is open air.
- Empty space is part of the picture — it creates scale, direction and situation (mist, water, distance), never "background waiting to be filled".
- Disciplined brushwork with bone: firm contours, short broad "axe-cut" strokes (*fupi cun*) on rocks, crisp near forms, far mountains as pale washes without outlines.
- Things reaching into the void: a branch, a cliff edge, a figure looking out into empty space.
- Ink working with restrained colour on gold-brown silk — not a desaturated, blurred or rice-paper-textured image.

## Palette

Two sources: the study's silk and ink (anchored on *Walking on a Mountain Path in Spring*, *Viewing Plum Blossoms by Moonlight*, *Banquet by Lantern Light*), and the "Ink & seal" UI panel, which is the tested web reading.

| Role | Hex | Note |
|---|---|---|
| Background (silk) | `#bfa46c` / `#c9a65a` | Gold-brown silk; for a full-bleed hero or a tribute page. |
| Background (UI paper) | `#efe7d4` | From the Ink & seal panel; the everyday UI ground. |
| Text (ink) | `#1d1814` (UI) / `#161816` (study) | Burnt-black ink; ~14.3:1 on the paper. |
| Wash (muted) | `#8b8172` | Pale ink wash for distant shapes and fills; only 3.1:1 on paper — decorative or ≥ 24px text. |
| Muted text | `#6b6152` (suggested) | Darkened wash for secondary text (~4.9:1). |
| Mountain wash | `rgba(60,62,56,.16–.26)` | Far ranges, fading to 0 downward. |
| Mist | `rgba(226,210,160,.55)` | Radial band that hides mountain feet. |
| Seal red | `#b3261e` | Seal stamp, counts, "fix" labels; ~5.3:1 on paper. |

Relationship: ink in five tones (burnt, thick, heavy, light, clear) on warm silk or paper; one small red seal is the only colour accent. No blue-grey "Chinese style" gradients. Dark reading (suggested): ink-black `#1d1814` ground, silk `#efe7d4` text, wash at 25%, seal red unchanged.

## Typography

Headings in a refined, high-contrast serif: **Instrument Serif** (used in the panel) or **Cormorant Garamond** — `"Instrument Serif", "Cormorant Garamond", Georgia, serif`, regular weight, often italic for asides. Body in **Geist** or **Inter** 15–16px. For Chinese text, **Noto Serif SC** or **LXGW WenKai**. Seal text in the serif at 13px, tracking +0.04em. No brush-calligraphy fonts for UI text.

## Surfaces, texture and edges

- **Silk/paper ground:** the panel's ground is cream plus an SVG noise tile stretched horizontally (`feTurbulence baseFrequency=".012 .25"`, 3 octaves, colour-matrixed to brown at ~50%) and a soft vignette (`radial-gradient(... transparent 55%, rgba(80,60,30,.18))`). The study's silk adds warp lines every 3px (alpha 0.02–0.065) and weft lines (0.015–0.055), ~2,400 flecks, and a vignette of 0.62.
- **Ink-wash shapes (bleed filter):** soft fills behind user messages, active dots and checkboxes use `feTurbulence (.04, 3 octaves) → feDisplacementMap scale 9 → feGaussianBlur 1.2`, with irregular radii like `40% 55% 45% 50%`.
- **Dry-brush rules (dry filter):** dividers and the composer's top line use `feTurbulence (.9 .06) → feDisplacementMap scale 3`, masked by a second noise so the line breaks up like a dry brush.
- **Far mountain:** an SVG path filled with the wash colour at 22% opacity with the bleed filter, placed in the empty top-right corner.

## Shapes and layout

Asymmetry is the system: weight one side (a solid sidebar or a dense content block at the bottom-left), leave the opposite side open, and let one element "reach" into the void (a heading, a pull-quote, a single image). No boxes around content: separate with a dry-brush vertical rule (3px) and air. Corners: none visible; ink shapes have irregular radii. No drop shadows. Generous right padding in the main column (the panel used 70px) so the seal has somewhere to land.

## Components

(From the Ink & seal panel.)
- **Navigation/sidebar:** plain text, no box; a dry-brush vertical rule separates it. Active item marked by a small ink dot (7px, bleed filter). Counts in seal red.
- **Chat bubbles:** user message in italic serif on an ink-wash blob (wash at 28%); replies on a fainter wash (16%) with radii `18px 26px 22px 30px`.
- **Tool/status lines:** completed steps prefixed with an ink dash "—"; the running step with a pulsing red dot.
- **Checkboxes:** 1.5px ink circles with the bleed filter (ensō-like), not squares.
- **Done state:** a red square seal reading "DONE", rotated −8°, with an inner paper ring (`inset 0 0 0 3px seal, inset 0 0 0 4.5px paper`) and a speckled stamp filter.
- **Tags:** wash text with a 1px ink hairline at 20%.
- **Composer:** no box; a dry-brush top rule only.
- **Loading:** a brush stroke writing itself across 120×26px, with an italic wash caption.
- **Mascot/brand:** an ensō circle drawn once (dash offset), two ink dots and a small red centre.
- **Empty state:** almost nothing — one wash mountain in a corner and a single line of text placed off-centre.

## Motion

- **Brush writing (loader):** `stroke-dasharray: 160; stroke-dashoffset: 160 → 0` over 2.2s, `cubic-bezier(.6,.1,.3,1)`, holding from 70%, stroke 8px, round cap, dry filter.
- **Ensō:** draws once in 3s ease-out after 0.2s.
- **Seal stamp:** from `rotate(-8deg) scale(1.9)`, opacity 0 → 1 by 60%, over 0.45s with `cubic-bezier(.2,1.5,.4,1)` (a press with a slight bounce), delayed 0.9s.
- **Willow sway (source motif, for illustrations):** strands in clusters, uneven lengths; each joint `k` offsets `sin(1.5t + φ + 1.1k)·(10 + 14k)` px plus a faster `sin(3.1t + 2φ)·3k`.
- **Small life in the void:** a bird crossing, a boat drifting a few hundred px over the session — never particles to "fill" the space (source rule).
- Under `prefers-reduced-motion`, show strokes, ensō and seal fully drawn (`stroke-dashoffset: 0`), no stamp bounce.

## Techniques

```html
<svg width="0" height="0" style="position:absolute" aria-hidden="true"><defs>
  <filter id="bleed" x="-20%" y="-20%" width="140%" height="140%">
    <feTurbulence type="fractalNoise" baseFrequency=".04" numOctaves="3" seed="5" result="n"/>
    <feDisplacementMap in="SourceGraphic" in2="n" scale="9"/><feGaussianBlur stdDeviation="1.2"/></filter>
  <filter id="dry" x="-10%" y="-40%" width="120%" height="180%">
    <feTurbulence type="fractalNoise" baseFrequency=".9 .06" numOctaves="2" seed="11" result="n"/>
    <feDisplacementMap in="SourceGraphic" in2="n" scale="3" result="d"/>
    <feTurbulence type="fractalNoise" baseFrequency=".5 .03" seed="3" result="m"/>
    <feColorMatrix in="m" values="0 0 0 0 0  0 0 0 0 0  0 0 0 0 0  0 0 0 -3 2.1" result="mask"/>
    <feComposite in="d" in2="mask" operator="in"/></filter>
  <filter id="stamp" x="0" y="0" width="100%" height="100%">
    <feTurbulence type="fractalNoise" baseFrequency=".7" seed="8" result="n"/>
    <feColorMatrix in="n" values="0 0 0 0 0  0 0 0 0 0  0 0 0 0 0  0 0 0 -2.4 2.2" result="m"/>
    <feComposite in="SourceGraphic" in2="m" operator="in"/></filter>
</defs></svg>
```

- Axe-cut rocks (illustrations): clip to the rock polygon, lay a top-to-bottom wash, then ~90 short broad strokes (40–130px, width 7–17px, dry 0.85) along the rock's slope ± 0.17 rad, then thin hard strokes along the same angle, then a broken contour only on lit edges and the waterline (skip ~25% of segments).
- Mist: a radial gradient squashed vertically (`scale(1, .16)`); a linear gradient leaves visible seams at the edges.

## Performance and accessibility

- SVG filters are cheap on small elements (bubbles, dots, rules) but expensive on large areas; never apply `bleed` to a full panel or animate filter parameters. Cache the background noise as a data-URI tile.
- Contrast: ink on paper ~14.3:1; seal red on paper ~5.3:1; wash `#8b8172` on paper only ~3.1:1 — use `#6b6152` for secondary text. Filters must not apply to text glyphs (displacement makes small text illegible).
- Completed tasks shown only by a seal must also carry text/`aria-label` ("Done").

## Pitfalls

- Treating empty space as background to decorate → every void must suggest distance, mist or direction.
- Equally spaced willow strands → read as a fence; cluster them, vary length.
- Segmented tapering trunks → read as bamboo; polygon axe-cuts → read as arrowheads (both tried and dropped). Use one continuous thick line and short broad brush strokes.
- Rectangular mist gradients → visible seams; use radial.
- Axe-cut strokes still too dense (source shortcoming) → fewer, broader strokes.
- Seal without writing (source shortcoming) → if you add an inscription, use real text in a real font, never fake calligraphy.

## Nearest neighbours

- **17 · Chinese ink (built):** expressive *xieyi* — flying-white strokes and spreading ink bleed are its signature. Ma Yuan is the disciplined academy style: firm contours, axe-cut texture, one-corner composition; ink bleed is not his main identifier.
- **Fan Kuan-style monumental landscape (not in the catalogue):** a towering central mountain with dense dotted texture — the opposite of Ma Yuan's off-centre corner and empty air.
- **25 · Ghibli:** also uses calm air, but with watercolour washes and full colour; Ma Yuan is ink with a single red seal.

## Source

Adapted from huashu-art-motion · `50_ma_yuan.md` (research recipe; self ~78 in round two, independent ~74 in round one; known shortcomings: axe-cut strokes too dense, blunt trunk ends, seal without inscription). Web techniques mined from the "Ink & seal" style-direction panel.
