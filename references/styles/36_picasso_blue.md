# 36 · Picasso Blue Period (Pablo Picasso, 1901–1904)

**Status:** Built ★★
**Layer:** Period specimen (strongest as tribute or easter egg)
**Web fit:** Medium — editorial pages, essays, music or poetry sites, a "night mode" with a melancholy voice. Weak for product UI that needs colour-coded states: the style is strictly monochrome, so status must come from value, shape and icons.

## Recognisable traits

- Strict monochrome blue: every surface, skin and object is a value of one blue-grey family. No warm accents.
- Wide, long, dry, low-contrast brushstrokes, softened as if worked over and blurred — not short Impressionist dabs.
- Confident, heavy Prussian-blue contours that do not wobble.
- Elongated figures (taller, narrower) and slow, inward, melancholy poses.
- Faint canvas weave and a cold vignette darkening the edges.

## Palette

| Role | Hex | Note |
|---|---|---|
| Background (deep) | `#132240` | Source darkest value. |
| Surface / panel | `#1d3866` | Source. |
| Mid blue (accent fills, active) | `#2d5a94` | Source. |
| Muted / secondary | `#6b8fb6` | Source. ~4.7:1 on `#132240` — OK for large text and secondary UI, borderline for small body text. |
| Text / light surface | `#b6c8d8` | Source. ~6.8:1 on `#1d3866`. |
| Brightest (headline, "highlight") | `#cbd8e0` | Source ("blond" value). ~10.9:1 on `#132240`. |
| Cyan-grey (accent 2) | `#3b6878` | Source; a slightly greener blue for a second state. |
| Contour | `#0c1830` | Source Prussian blue. |

Relationship: one hue, many values; difference comes from lightness, never from a new hue. The source's lesson: a "blond" or "orange" thing is shown by being one step lighter or darker, not by adding ochre. Dark reading is native. Light reading: `#b6c8d8`/`#cbd8e0` paper with `#132240` text (~9:1) and `#0c1830` contours. Note: these blues sit around hue 210–220°, just below the catalogue's blue-purple "AI slop" zone; keep them flat and matte, never a glowing blue→violet gradient.

## Typography

A melancholy, literary serif: **Cormorant Garamond 500** or **EB Garamond** for display (`"Cormorant Garamond", "EB Garamond", Georgia, serif`), slightly condensed feel suits the elongation. Body: **Source Serif 4** or a neutral sans at 16–18px with generous line-height (1.6). Labels in small caps with +0.06em tracking, in `#6b8fb6`.

## Surfaces, texture and edges

- **Wide dry strokes:** pre-render a texture tile of long strokes (source: cell 34px, length 110px, width 34px, jitter 4, direction ±0.5·noise(0.003) around vertical for walls, horizontal for floors; colour mixed 80% from the base), then blur the layer 7px and lay it back at α 0.8. Use it as a background tile under panels, at low contrast.
- **Dry-brush hairs:** ~900 faint streaks (α 0.07, 30–90px long) in a second cached tile.
- **Contours:** panels and illustrations get a 5px (scale to 2–3px in UI) `#0c1830` outline — solid, never jittered or animated ("boiling").
- **Canvas weave + cold vignette:** a small SVG `feTurbulence` weave tile at very low opacity, and a radial vignette darkening corners toward `#0c1830`.

## Shapes and layout

Tall, narrow proportions: favour vertical cards, tall hero images, narrow text columns (55–65ch). Corners slightly soft (4–6px), as if painted. Heavy outlines rather than shadows; if a shadow is needed, a dark-blue flat offset. Generous empty dark space; one or two elements per view, slightly off-centre and drooping toward the bottom.

## Components

- **Buttons:** filled `#2d5a94` with `#cbd8e0` text and a dark contour; hover lifts one value step lighter; no glow.
- **Cards:** `#1d3866` with stroke texture and a solid dark contour.
- **Navigation/sidebar:** deep `#132240`; active item one value lighter with a thick left contour.
- **Inputs:** `#132240` fields with a `#6b8fb6` underline; focus = contour thickens to 3px in `#cbd8e0`.
- **Chat bubbles:** user `#2d5a94`, replies `#1d3866`; same contour on both.
- **Tables:** rules in `#6b8fb6` at 30%; zebra striping by value.
- **Badges/tags:** distinguish status by value + icon + text (light filled = active, outline = pending), since hue can't help.
- **Illustrations/mascot:** stretch them (source: scale ~0.84 × 1.12 around the feet) and recolour to values of the palette; a heavy steady contour. Never give them a warm accent.
- **Loading:** a slow "breathing" dot (see Motion).
- **Empty state:** a single elongated figure or object, small, low in a large dark field, one line of copy.

## Motion

- Everything slow. Source breathing: `sin(1.8t)·0.026` scale — about twice the amplitude and under half the frequency of a normal idle; lifts take 1.25s instead of 0.9s; steam rises at half speed.
- Background: horizontal bands (sea, mist) drift slowly, nearer bands faster (source: `20 + 0.25·(y − 400)` px/s); 14 moonlight glints flicker gently; one pale long cloud crosses.
- Hover: a value shift over 400–600ms, ease-in-out.
- Done moment: the item's fill rises one value step and its contour thickens briefly; no bounce, no confetti.
- `prefers-reduced-motion`: no breathing or drift; state changes are instant value shifts.

## Techniques

- **Entering the style (source transition):** drain colour to blue, then sweep a wide brush across. On the web: `filter: grayscale(1) sepia(1) hue-rotate(180deg) saturate(.6)` on an image, then a broad masked stroke reveal (`mask-image` animated left→right). Good for a one-off "mode switch".
- **Monochrome imagery:** duotone photos via an SVG `feColorMatrix` → `feComponentTransfer` mapping luminance to `#0c1830 … #cbd8e0`.
- **Elongation:** a single transform reused everywhere a figure is drawn (source used the same matrix for path, offscreen canvas and outline passes) — in SVG, wrap the illustration in one `<g transform>` so outlines and fills stay aligned.
- **Shawl/drapery folds:** fold lines `rgba(20,40,80,.6)` at 4px over `#6b8fb6` fabric.

## Performance and accessibility

- Cheap: cached stroke tiles, a static vignette, SVG duotone applied once. Expensive: re-blurring stroke layers live; animating filters.
- Contrast: body text in `#b6c8d8` or `#cbd8e0` on `#132240`/`#1d3866` (6.8–10.9:1). Don't set small body text in `#6b8fb6` on `#1d3866`. Because hue is unavailable, every status needs a text label or icon (WCAG 1.4.1).
- Motion: very slow by nature; still disable it under reduced motion.

## Pitfalls

- A touch of ochre on hair or a cat (after *The Old Guitarist*) broke the monochrome → stay all blue; separate by value.
- Short narrow strokes (cell 16, length 40, width 14) over a whole wall read as rain or a blanket, i.e. Impressionism → strokes must be wide, long, low contrast, then blurred.
- Round brush ends packed densely read as a crowd of heads → blur 7px to suppress, or use square/flat caps.
- Cutting a rectangular hole in the stroke layer for an object leaves a flat patch → draw the object on its own layer after the strokes.
- Jittery outlines → Blue Period contours are certain; keep them still.
- Figures weren't melancholy enough (noted shortcoming) → droop the pose and the layout, don't just tint blue.

## Nearest neighbours

- **32 · Rembrandt:** also mood through limited colour, but warm chiaroscuro with a light source; Picasso Blue is cold, evenly dim, outlined and flat-ish.
- **19 · Munch:** also expressive and melancholy, but long wavy flowing strokes and blood-red skies; Picasso Blue is calm, straight, wide strokes in one hue.

## Source

Adapted from huashu-art-motion · `36_picasso_blue.md` (built ★★; noted shortcoming: figures not melancholy enough in shape).
