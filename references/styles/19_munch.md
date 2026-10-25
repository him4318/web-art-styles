# 19 · Expressionism, *The Scream* (Edvard Munch, 1893)

**Status:** Built ★★
**Layer:** Period specimen (strongest as tribute or easter egg)
**Web fit:** Low–Medium. Fits editorial features on anxiety or art, Halloween or error-state easter eggs, poster-like campaign pages. Poor for product UI: the whole idea is a world that won't stay still.

## Recognisable traits

- Long, curving, thinly painted strokes that flow in bands, following the shape of sky, water and land (no short dabs).
- A blood-orange sky in wavy horizontal bands of red, orange and yellow.
- Everything sways: the landscape, the figure and the furniture bend together as if the air itself were distorted.
- Dark, blue-black fjord curves against the burning sky; a straight diagonal railing cutting through.
- A skull-like figure with hands on cheeks, made of a few curves, in sickly yellow-green.

## Palette

| Role | Hex | Note |
|---|---|---|
| Background (warm wall) | `#a8401e` → `#5a1a14` | Source flat underlayer showing between strokes. |
| Surface | `#f4e6c8` (suggested pale) or `#5a1a14` (deep) | Text panels. |
| Text | `#1e0f0a` (suggested) on pale/yellow; `#f4e6c8` on deep red | |
| Sky bands | `#e2541c` `#f08a24` `#f4c03a` `#c8301e` `#e8743a` `#f6a63a` `#b82a1e` | Source 7-colour cycle. |
| Fjord / shadow | deep blue-black (suggested `#1d2a3a`) | Source names the fjord region but gives no hex. |
| Sickly face shadow | `rgba(120,150,70,.4)` | Source yellow-green shadow on faces. |

Relationship: hot orange-red bands against cold dark blue; the underlayer is the same hue family, so gaps between strokes show colour, never black. Primary reading is dark-warm (deep red ground); a light reading on pale ochre is possible for text-heavy pages.

## Typography

Avoid comic handwriting (the source found Kalam pulled viewers out). Use an old, slightly irregular serif: **IM Fell English** (source choice) for headings (`"IM Fell English", Georgia, serif`). Body: a plain serif or sans (suggested **Source Serif 4**) that stays still. The source warps caption text row by row; on the web, warp only a display heading (SVG `feDisplacementMap` or per-letter transforms), never body text.

## Surfaces, texture and edges

- **Flow-line backgrounds:** pre-render (canvas, once or as a short loop video/WebP) about 3,600 seeds on a 24px jittered grid, each integrated along a direction field for 26–42 steps of 6–8px (strokes 200–300px long), width 9–13px, α .5–.78, no outlines. Strokes stop at region edges, so regions stay clean.
- Source region parameters `[steps, step, width, α]`: sky [32, 7, 10, .78], fjord [26, 6, 9, .78], wall [42, 7, 13, .5], floor [32, 8, 12, .55], each plus 0–8 random steps.
- **Direction fields:** sky `0.55·sin(0.011x + 0.018y − 2.4t)`; fjord tangential around a centre; wall vertical with ±0.5 sine wobble; floor pointing to a vanishing point.
- **Sky bands:** colour chosen by a wavy y: `yy = y + 22·sin(0.014x − 2.2t)`, band height 28px, cycling the seven colours.
- **Underlayer first:** paint a flat same-hue gradient beneath strokes so gaps read as colour.
- **CSS-only approximation:** stacked `repeating-linear-gradient` bands in the sky colours, passed through an SVG `feTurbulence` + `feDisplacementMap` (low frequency, scale 20–40) to wave them. Static, as an image.

## Shapes and layout

Curving, flowing forms; few straight lines except one strong diagonal (the railing) that can become a diagonal divider or a slanted header edge. No boxes with crisp shadows: panels are plain flat blocks of pale or deep colour sitting on the flowing ground, perhaps with slightly wavy edges (an SVG path, not a jittered polygon). Composition: a diagonal leading from a corner toward the focal content.

## Components

- **Buttons:** flat deep-red or pale ochre blocks with slightly wavy outlines; hover makes the edge ripple once.
- **Cards:** pale panels with a stroke-textured border band.
- **Navigation:** a diagonal band (the railing) carrying the items.
- **Inputs:** plain, still, high-contrast fields; this style must not touch them.
- **Loading:** a short band of flowing strokes, or the sky bands drifting.
- **Error / 404 state:** this is the natural use: the screaming figure (two hands to cheeks, mouth `7 + 2·sin(9t)` opening, body swaying `sin(3.2t)·0.12rad`).
- **Illustrations:** draw them with the same long strokes and let them warp with the background; outline-free.

## Motion

- **The whole world flows:** displace the entire composition row by row `9·sin(0.012y + 4.2t) + 4·sin(0.031y − 6t)` and column by column `6·sin(0.009x − 3.6t)`, in 4px bands. Far cheaper than animating every line. On the web, do this to a decorative background layer only (pre-rendered frames, or an SVG displacement filter animated via `seed`/`baseFrequency` swaps in a short loop), never to UI or text.
- **Sky flowing:** the band boundaries scroll and wave over time.
- **Hair and fabric:** travelling waves `sin(9q − 7t)` along strands.
- **Hover:** a single ripple along a wavy edge, 300–500ms.
- Easing: continuous sines; it never settles. Under `prefers-reduced-motion`: one static frame. This style's motion is a strong vestibular trigger, so consider making it opt-in even without the media query.

## Techniques

- Pre-render rather than integrate thousands of strokes per frame (the source's frames cost 90–140ms of JS each, far too slow for a page).
- Row/column warps expose empty edges: stretch each row by 2× margin, with margin ≥ the maximum displacement, or oversize the layer and clip it.
- Overlapping limbs drawn in line mode let a torso line cut through a raised arm; redraw the near arm on top.

## Performance and accessibility

- Expensive: live flow-field integration, per-frame full-screen warps, animated displacement filters. Cheap: a pre-rendered looping WebP/video of the background, paused when hidden, capped at DPR 2.
- Contrast: pale text on deep red 10.7:1; pale on the mid red `#a8401e` 5.0:1; dark text on the yellow band 11.0:1 and on orange 7.4:1. But banded backgrounds vary under the text: always set copy on a solid panel.
- Motion sickness: whole-screen undulation is among the worst triggers; keep it out of reading areas, slow, and off under reduced motion.

## Pitfalls

- Short strokes with dark outlines → reads as Van Gogh at first glance; remove outlines and lengthen strokes (10 → 26+ steps), widen and lower opacity.
- Second attempt still too short [26, 6, 7, .85] → still "Van Gogh"; Munch needs 200–300px strokes.
- Warping exposes dark gaps at the edges → oversize the warped layer.
- Comic handwriting for captions → breaks the mood; use an old serif.
- Warping text or controls → unreadable and unusable; warp backgrounds only.

## Nearest neighbours

- **09 · Van Gogh (post-impressionism):** also directional strokes and swirling skies, but short, thick, outlined dabs and swirls in blues and yellows. Munch uses long, thin, outline-free bands in red-orange, and the whole world warps together.
- **36 · Picasso Blue Period:** also emotional and expressive, but a near-monochrome blue with broad strokes and stillness. Munch is hot, banded and moving.

## Source

Adapted from huashu-art-motion · `19_munch.md` (built ★★, period specimen; noted shortcoming: the wall was once read as Van Gogh).
