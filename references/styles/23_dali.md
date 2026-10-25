# 23 · Surrealism, *The Persistence of Memory* (Salvador Dalí, 1931)

**Status:** Built ★★
**Layer:** Period specimen (strongest as tribute or easter egg)
**Web fit:** Medium for editorial features, art and culture sites, campaign pages about time or dreams, and easter eggs (a melting clock as a "session expired" or timeout page); Low for product UI, where deliberate unreality hurts trust.

## Recognisable traits

- Melting objects: rigid things (watches) draped over edges like soft cloth, hanging and dripping.
- An immense, clear, empty landscape: a deep blue sky fading to pale yellow at the horizon, a calm sea, golden cliffs.
- Long, raking shadows from a low sun, stretched far across the ground.
- Academic, smooth rendering: volume from gradients and soft inner shadow, no outlines, high finish.
- Dream motifs: long-legged elephants on stilts, ants swarming a watch, bare dead branches.

## Palette

| Role | Hex | Note |
|---|---|---|
| Background (sky) | `#22407c` → `#5f88bd` → `#e9dcae` | Source sky gradient, top to horizon. |
| Surface (wall) | `#f4ead2` → `#cdbf9f` | Source wall; panels. |
| Text | `#2b1d10` (suggested) on wall/sand; `#f6efd6` on the deep sky | The source switched its counter to cream text with a dark shadow on the blue sky. |
| Sea | `#8aa6b8` → `#4f6f86` | |
| Rock | `#e8c27a` / `#9a6a34` | |
| Ground | `#d2ad70` → `#7a5129` | |
| Accent (clock gold) | `#f0d27c` / `#9a7426` | Dial `#f6efd6`. |

Relationship: a cool blue sky over warm sand and gold, joined by a pale yellow horizon; colours are clear and luminous, never muddy. Light reading: sand and wall panels. Dark reading: the deep sky as the page with cream text and gold accents.

## Typography

Headings in an elegant high-contrast serif, suggested **Playfair Display** or **Bodoni Moda** (`"Playfair Display", "Bodoni Moda", Georgia, serif`), sometimes italic. A display word can "melt" (its lower part warped downward with an SVG displacement); body copy never does. Body: a quiet serif (suggested **Lora** 400) on wall or sand panels.

## Surfaces, texture and edges

- **Volume fill (`vol`):** a linear gradient across a shape plus an inner shadow: in CSS, `background: linear-gradient(...)` with `box-shadow: inset 0 0 Npx rgba(…)`. Everything is rounded and smooth; no outlines.
- **Landscape background:** sky gradient, sea band, cliffs and ground as layered SVG shapes; cache as one image.
- **Long shadows (shear projection):** project each object's silhouette onto the ground with a shear: `setTransform(1, 0, −kx, ky, kx·yg, yg·(1 − ky))`, where `yg` is that object's ground line. Draw all silhouettes into one offscreen layer in solid black, then composite the layer once at α .55 with 1.5px blur, clipped to the ground. In CSS: a duplicate silhouette with `transform: skewX(…) scaleY(…)`, `transform-origin` at its base, `filter: brightness(0) blur(1.5px)`, opacity .55; group shadows in one container so overlaps don't darken.
- **Shadow direction:** the sun sits low at front-left, shadows fall back and to the right. Never toward the viewer.
- **Layering:** shadows lie on the ground, so they go above the background but below walls, tables and figures; split the background into layers.

## Shapes and layout

Smooth, rounded, soft-edged forms; corners can sag (a card whose bottom edge droops is the signature move, used once). Composition: a low horizon with a lot of sky, a few isolated objects far apart on an empty plain, each with a long shadow. For pages: wide hero landscapes, content on stone-like wall panels placed like objects on the plain, generous emptiness.

## Components

- **Buttons:** smooth sand or gold pills with a gradient and inner shadow, dark text; on hover, the shadow beneath stretches.
- **Cards:** wall-coloured panels with a long, soft, sheared shadow cast to the right.
- **Navigation:** objects on a horizon line, or a simple wall panel at the top.
- **Inputs:** smooth recessed wells on wall colour.
- **Clock / time displays:** the soft watch: a timer or countdown whose face sags more as time passes.
- **Loading:** a soft watch slowly drooping, hands turning, or ants circling a watch.
- **Error / timeout / 404:** a melting clock on a branch; perfect for "your session melted away".
- **Illustrations:** smooth gradient volumes, no outlines, fine dark-brown lines for facial features, soft blurred stripes (blur 2px) for markings, radial-gradient eyes.

## Motion

- **Melting:** a soft-watch mapping in local disc coordinates `(u, v)`: the part above a fold line lies flat (vertical scale 0.28–0.32); below it the disc hangs: `X = cx + u·(1 − flare·q)`, `Y = … + d·(0.62 + 0.3·sag) + a central Gaussian sag + small sine ripples`. Rim, dial, 12 ticks, both hands and the highlight all go through the same mapping. `flare = −0.12` makes the hanging part spread like cloth with a rounded lower edge; drip 0.18–0.4; sag from 0.1 to 1.4 over the animation (smaller ranges don't read as melting).
- **Shadows lengthening:** `kx = 0.85 + 0.6·easeInOut(progress)`; tie it to scroll or a timeout countdown.
- **Long-legged elephant:** four thin legs stepping with π/2 phase offsets, knees bending at 0.52 of the leg, the body bobbing slightly, crossing slowly (70px/s at 1920 wide) on the horizon.
- **Ants:** nine ants circling a watch on an ellipse.
- **Clock hands:** turning at different speeds per watch (4.5–6.5rad/s in the source; slower for UI).
- Easing: slow, viscous ease-in-out; nothing bounces. Under `prefers-reduced-motion`: show the clock already melted and shadows at their final length, no walking or crawling.

## Techniques

- Implement the soft watch as an SVG whose path points are computed from the mapping (a few frames pre-computed, or per frame for a single hero; canvas at 47ms per frame in the source is too slow for many). A cheaper web route: an SVG `feDisplacementMap` with a vertical gradient as the map, intensity animated by swapping a few pre-made maps.
- Melting card edge: a `clip-path: path()` whose bottom edge has a soft drip, or an SVG mask, with a gold rim.
- Keep one light direction for all highlights and shadows on the page.

## Performance and accessibility

- Expensive: per-frame mapping of many soft objects, live blurred shadows per object. Cheap: pre-rendered landscape, one shadow layer, a handful of melting states.
- Contrast: dark text on the wall 13.6:1, on the pale horizon 11.9:1, on sand 7.7:1, on clock gold 11.1:1; cream on the deep sky 8.7:1; cream on the sea's dark end 4.6:1 (large text only). Text over the sky gradient changes contrast top to bottom: keep copy on panels or in the deep-sky region.
- Slow, viscous motion is gentle, but warping UI is disorienting: melt decoration, never controls or body text.

## Pitfalls

- Shadows cast toward the viewer → cut off by the frame bottom; light from front-left, shadows to back-right.
- One cached background for everything → shadows can't sit between ground and objects; split into layers.
- Positive flare with more drip → the watch turns into an ice-cream cone; use negative flare for cloth.
- Sag range too small (0.35 → 1.0) → "the clock isn't visibly melting"; go wider.
- Dark text on the deep blue sky → invisible; cream text with a dark shadow.
- Figures in flat vector style → too illustrative (the source's shortcoming); add finer highlights and reflected light in shadows.

## Nearest neighbours

- **24 · Hopper:** also long shadows and empty, clear light, but realistic everyday interiors and hard-edged light patches. Dalí uses the same light on an impossible dream plain with melting objects.
- **06 · Renaissance sfumato:** also smooth academic volume and soft modelling, but warm indoor tones, varnish and craquelure, with no surreal deformation. Dalí is crisp, luminous and impossible.

## Source

Adapted from huashu-art-motion · `23_dali.md` (built ★★, period specimen; noted shortcoming: figures leaned toward vector illustration; the source suggests the reclining soft "self-portrait" creature as a fourth motif).
