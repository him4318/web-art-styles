# 12 · Bauhaus poster (Bauhaus school, Weimar, c. 1923)

**Status:** Built ★★★
**Layer:** Material (works on any subject) — narrow use
**Web fit:** High — product UIs, dashboards, design tools, education, architecture and event pages. Its grid and primaries translate almost directly into components. Narrow in mood: it always reads as "modernist poster", so it's wrong when the brief wants softness or handmade warmth.
**Mockup:** `assets/mockups/12_bauhaus.png`

## Recognisable traits

- Pure geometry only: circles, squares, triangles, thick bars. No freehand curves, no outlined drawing.
- Flat primaries (red, yellow, blue) plus black on an off-white, slightly aged paper.
- Asymmetric, diagonal composition: a bold diagonal bar, a big red circle, type set vertically or at right angles.
- Heavy geometric sans lettering, often lowercase, rotated 90° and stretched to fill a measure.
- Grids of coloured cells (proto-Mondrian), Schlemmer-like figures built from discs and bars, chrome tube furniture.

## Palette

| Role | Hex | Note |
|---|---|---|
| Background (paper) | `#ebe3cf` | Source aged paper. |
| Surface | `#f2ecdf` | Source cream; cards. |
| Text | `#161514` | Source black. |
| Accent (red) | `#d3262b` | Big circles, primary buttons (white text). |
| Accent 2 (blue) | `#2a58b4` | Secondary buttons, links on paper. |
| Yellow | `#efc12f` | Highlights, triangles (black text). |
| Light blues | `#b9cee0` / `#a6c0da` | Soft cells, info backgrounds. |
| Grey | `#c6c3bb` | Dividers, disabled. |
| Orange | `#ee8a2a` / `#c25a14` | Optional secondary warm. |
| Chrome | `#c9c9c6` | Metal details. |

Relationship: three primaries plus black on paper, each used flat in big, distinct shapes; never mixed or gradiented. Grid colours exclude black (a black cell reads as a broken tile). Dark reading: black `#161514` ground, paper-coloured text, the same primaries; it stays very Bauhaus.

## Typography

Source used **Poppins 800** (210px vertical "bauhaus", 150px red word) and Poppins 600 at 31px for small lines. Good alternatives: **Josefin Sans**, **Outfit**, **Futura** where licensed (`"Poppins", "Futura", "Outfit", system-ui, sans-serif`). Use lowercase for display words, rotate one heading `-90deg` and scale it to exactly fill a measure. Body: the same family at 400–500, 16–17px. Labels: uppercase or lowercase, never mixed decoration.

## Surfaces, texture and edges

- **Paper:** `#ebe3cf` with a small fbm image (320×180) stretched at α0.5 for mild ageing, plus sparse dark print specks (density 0.02, α0.35). Bake as one background image.
- **Ink unevenness:** big shapes (the red circle) get a pale fbm blotch layer clipped inside them at α0.55, like screen-print or litho ink. In CSS: a noise PNG with `mix-blend-mode: soft-light` inside the shape, or `mask-image`.
- **Edges:** crisp and ruler-straight; no outlines on shapes (colour against colour). Thick black bars act as rules.
- **Chrome:** three-stop linear gradients on tube shapes; one light highlight line offset 2px.

## Shapes and layout

Radius is either 0 or 50%. Rules are thick (9.5–27px) black bars; shadows none. Layout on a strict grid with one strong diagonal (a long black bar crossing the composition), a dominant circle, and vertical type along one edge. Grid cells derive from the frame: compute cell size as (inner width − gaps) ÷ count; the source's first try overflowed by 16px. Asymmetry and white space are the point; avoid centring everything.

## Components

- **Buttons:** flat rectangles (radius 0) in red with white text or yellow with black text; secondary is a circle icon button in blue. Pressed: shift 2px, no shadow.
- **Cards:** cream rectangle; a single coloured shape (circle, triangle, quarter-circle) in a corner as the card's identity.
- **Navigation/sidebar:** a black bar with paper-coloured labels; active item marked by a red square, not a pill.
- **Inputs:** cream with a 3px black bottom bar; focus turns the bar blue.
- **Chat bubbles:** rectangles; user in blue with white text, replies in cream; a small triangle as the tail.
- **Tables:** thick black header bar, thin grey rules, numbers in tabular figures.
- **Badges:** a coloured circle, square or triangle with a label beside it.
- **Icons:** built from primitives only (circle, square, triangle, bar).
- **Loading:** a grid of cells swapping colours, or a ball rolling with a visible rotating diameter line.
- **Empty state:** a composition of a red circle, a yellow triangle and a black bar with one line of copy.

## Motion

- **Pointer bar:** a long black bar rotating about a hub like a gauge needle, eased out: `deg = 28 + 21.5·easeOut(clamp((t − 0.26)/0.22))`. Perfect for progress or a hero entrance.
- **Grid cell swaps:** a 4×4 grid where 6 cells change colour at 13fps steps, each cell with its own phase and changing only when `(step + 2·sw) % 9 < 6`, so the rhythm isn't mechanical. Swap palette: red, yellow, blue, cream, light blue, grey; never black.
- **Concentric arcs:** six arcs beside a circle pulsing outward like sound (`sin(9t − 0.7i)·0.05` phase, radius ±3px, arc length sweeping 55% → 100% in 0.4s). A good "speaking" or "listening" indicator.
- **Rolling ball:** rolls with rotation = distance ÷ radius, a white diameter line and dot showing the spin, two speed lines behind.
- **Steam / small lines:** a single 2.2px sine line with a travelling wave.
- Motion is mechanical and precise: stepped swaps, ease-out rotations, no wobble.
- **Reduced motion:** grid fixed, bar at its final angle, arcs static.

## Techniques

- Everything is SVG primitives; position from a grid, rotate with `transform-origin` at the hub.
- Grid swaps: CSS `steps()` animations on `background-color` per cell with different `animation-delay`s, or a tiny JS ticker at 13fps.
- Vertical title: `writing-mode: vertical-rl; transform: rotate(180deg)` (reads bottom to top), then scale with `transform: scaleY()` or tune font-size to fit the measure exactly.
- Making a mascot or illustration fit: build it from primitives: a circle head with a black vertical bar eye (blinks to a horizontal bar), red square cheek, limbs as thick round-ended bars with cream joint discs (Schlemmer's figures); a cat as a circle body plus a square head with triangle ears. Keep mouths short and inside the head (a long bar sticking out reads as a cigarette).

## Performance and accessibility

- Very cheap: flat SVG and CSS; the source's frame time was the lowest in its set. Only the baked paper texture costs anything.
- Contrast: black on paper ≈ 14:1; white on red `#d3262b` ≈ 5.1:1; white on blue `#2a58b4` ≈ 6.7:1; black on yellow ≈ 10.7:1. Red text on paper is only ≈ 4:1: use it for large display words, not body text. Never white on yellow.
- Rotated vertical type must be duplicated as accessible text or kept decorative.

## Pitfalls

- Grid cell size guessed → overflows the frame; derive from inner width minus gaps.
- Black in the colour-swap set → looks like a broken cell; exclude it.
- A long mouth bar on a face → reads as a cigarette; keep features inside the head.
- Curved, hand-drawn icons mixed in → breaks the style; primitives only.
- Everything centred and symmetric → generic flat design; add one diagonal and asymmetric weight.

## Nearest neighbours

- **22 · Russian constructivism:** also geometric with red and black, but halftone photos, Cyrillic slogans, aggressive diagonals and propaganda energy. Bauhaus is calmer, with all three primaries on cream paper.
- **30 · Matisse cut-outs:** also flat saturated shapes, but hand-cut organic edges with pinned shadows. Bauhaus is ruler-straight and compass-round.

## Source

Adapted from huashu-art-motion · `12_bauhaus.md` (built ★★★; noted shortcoming: the geometric cat's head sat off the shared skeleton position, which only mattered for video transitions).
