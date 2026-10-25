# 47 · Sisley (Alfred Sisley, Impressionism, 1870s–1890s)

**Status:** Research recipe — one redone single-scene study (a flooded village landing with a ferry), self-scored ~80, independently reviewed ~68. The lower score counts; below 80, so techniques are hypotheses.
**Layer:** Material (works on any subject) — atmosphere, weather-tinted light and long horizontal strokes transfer well; the source warns that close-up figures and interiors are untested.
**Web fit:** Medium-high for calm editorial and marketing pages: travel, weather, climate, outdoor brands, real estate, quiet "about" pages. Large calm skies make good backgrounds for headlines. Weak for dense product UI.

## Recognisable traits

- The sky and the weather govern everything: ground, buildings and water all take their colour from the sky. Not always sunny blue-green; snow, floods and overcast are core.
- Long, light, broken horizontal strokes, especially in sky and water; near strokes wide and horizontal, far strokes narrow and fragmented.
- Continuous atmospheric depth: a horizon, a road or a riverbank structures the view — the landscape is a coherent space, not a backdrop.
- Small figures and buildings kept at landscape scale.
- Water carries reflections of sky, houses and trees, darkened and broken by ripples.

## Palette

Hexes from the source study's reference notes (*Snow at Louveciennes*, *Flood at Port-Marly*, boats on the Seine) — observed, not official.

| Role | Hex | Note |
|---|---|---|
| Background (horizon sky) | `#ebdfc2` | Warm pale horizon; page ground. |
| Sky | `#6a98c8` → `#a8c0d8` → `#ebdfc2` | Top-to-horizon gradient for hero and headers. |
| Cloud | `#f2ecde` body, `#aab0c4` shade, `#f8e4b8` lit edge | Cards as "clouds": cream with lilac-grey shadow side. |
| Text | `#2a3340` | Ferryman's coat blue-black; ~9.6:1 on the horizon ground. |
| Muted text | `#4e5766` | Slate roof shadow. |
| Water | `#c4cbc0` → `#9fb0aa` → `#788a82` | Footer, lower sections. |
| Accent (ochre wall) | `#d6bd86` / `#d8bf88` | Highlights, primary button fill (with dark text). |
| Accent 2 (slate roof) | `#6a7482` | Secondary buttons, icons (3.5:1 on cream — large or decorative only). |

Relationship: lights lean warm yellow-pink, shadows cool lilac — the sky's colour is in everything. Avoid saturated greens and blues; this is a soft, weathered, silvery palette. Dark reading (suggested): a winter dusk — `#2a3340` ground, `#ebdfc2` text, `#f8e4b8` lit-cloud accents.

## Typography

A light, open serif for headings: **Spectral 300–500** or **EB Garamond** — `"Spectral", "EB Garamond", Georgia, serif`, generous size and leading, sentence case. Body in a calm sans (**Inter** or **Source Sans 3**, 16–18px). Labels in tracked small caps, slate grey. Nothing heavy: weight would break the airiness.

## Surfaces, texture and edges

- **Sky panels:** pre-render a wide sky (the study used 2600px wide) with clouds (warm-white body, lilac-grey shade, warm-yellow lit edge) and re-paint with long horizontal strokes: `cell 10, len 54, wid 10, angle −0.06 ± 0.05`. Ship as WebP; use as hero background with `background-position` drift.
- **Water band:** a horizontal-stroke texture (`cell 11, len 70, wid 8, angle 0`) with reflected shapes darkened; used for footers or the lower third of a hero.
- **Canvas grain:** cached tile `#e8e0cc`, `multiply` ~0.22.
- **Card edges:** clean shapes with very soft feathering on the horizontal edges only (top/bottom), as if fading into haze; text area stays crisp.

## Shapes and layout

Low, wide proportions; strong horizontals; a visible horizon line around 40–55% of the hero height. Radii small (2–4px); no outlines; shadows almost absent — separate surfaces by value (cloud cream vs sky blue) instead. Lots of sky: let headlines sit in it with ample air. Spacing generous and even; don't crowd the upper half.

## Components

- **Buttons:** ochre `#d6bd86` with dark slate text; secondary = transparent with 1px slate hairline. Hover: a warm lit edge appears along the top (`box-shadow: inset 0 2px 0 #f8e4b8`).
- **Cards:** cloud cream on sky, lilac-grey shadow side as a 1–2px inset on one edge.
- **Navigation:** sits in the sky band, plain text; active item gets a short horizontal stroke underline.
- **Inputs:** cream field with slate hairline; focus = 2px `#2a3340` outline.
- **Tables:** horizontal rules only, slate at 20%.
- **Loading:** a slow ripple line (horizontal displacement of a reflected stroke strip).
- **Empty state:** an open snowy field under a wide sky; one line of copy.
- **Weather/status widgets:** a natural fit — vary the sky tint by state.

## Motion

- **Cloud drift (source motif):** the pre-painted sky strip translates slowly horizontally.
- **Water shimmer:** offset each 6px row of the water image by `dx = sin(1.3t + 0.09y)·(1.5 + 9k) + sin(2.7t + 0.21y)·1.5k`, where `k` goes 0 → 1 from the waterline to the bottom (nearer = larger).
- **Boat bob:** an element riding the water rises/falls `sin(1.5t)·3px` with tilt `sin(1.5t + 1)·0.025rad`.
- Calm, smooth, never stepped. Under `prefers-reduced-motion`, stop the clouds and water; keep the still painting.

## Techniques

```js
// Water shimmer: draw a cached stroke-painted water image in 6px strips, offset per row
function drawWater(ctx, water, t, waterLine, H, W) {
  for (let y = waterLine; y < H; y += 6) {
    const k = (y - waterLine) / (H - waterLine);
    const dx = Math.sin(t * 1.3 + y * 0.09) * (1.5 + k * 9) + Math.sin(t * 2.7 + y * 0.21) * 1.5 * k;
    ctx.drawImage(water, 0, y, W, 6, dx - 24, y, W + 48, 6); // overdraw 24px each side: no gaps at edges
  }
}
```

- Re-paint layers separately (sky / water / land) with their own stroke directions, then composite; land uses `cell 8, len 26, wid 8` with near-vertical strokes for poplars and posts and near-horizontal for ground.
- Complementary jitter for water: cool `[96,120,156]`, warm `[236,222,176]`.
- Blur and over-draw the base 14px past edges before stroking (fixes a row of notch dots on the left edge, found in the study).

## Performance and accessibility

- Cheap: one cached sky image translated with CSS, a water canvas updated at 30fps in a small area. Expensive: full-screen per-row redraw at high DPR — cap DPR at 2, limit the water band height, pause when hidden or off-screen (`IntersectionObserver`).
- Contrast: slate `#2a3340` on horizon cream ~9.6:1; slate-grey `#6d7684` only 3.5:1 (large text only). Text over painted sky must sit on a cream "cloud" panel or be large (≥ 24px) and dark.
- Moving water near text is distracting: keep it below the reading area.

## Pitfalls

- Roofs read as flat slabs, windows as black boxes (source shortcoming) → give roofs a lit and a shade plane and windows a sky reflection (`rgba(210,225,235,.55)`).
- Poplars look like seaweed (source shortcoming) → straighter trunks, tapered columns of short vertical strokes.
- Notch dots along the image edge → overdraw the base past the edges before stroking.
- Everything sunny blue-green → Sisley is about weather; try snow, flood, overcast palettes.
- Enlarging a tiny painted figure → re-draw it at the new size with its own stroke density.

## Nearest neighbours

- **46 · Pissarro:** shorter, multi-directional strokes and warm flecks in greens, with people doing visible work. Sisley's strokes are long and horizontal and the sky leads; people are tiny.
- **28 · Monet (built, water lilies):** a close-up field of colour on water with no horizon. Sisley keeps a horizon and deep, continuous space.
- **42 · Guardi:** looser, flickering urban outlines; Sisley's outlines are calmer and landscapes more ordered.

## Source

Adapted from huashu-art-motion · `47_sisley.md` (research recipe; self ~80, independent ~68; known shortcomings: flat roofs and black windows, seaweed-like poplars, dark boat reflection, figures only silhouettes).
