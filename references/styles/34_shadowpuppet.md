# 34 · Shadow puppetry (Chinese piying, leather puppet theatre)

**Status:** Built ★★★
**Layer:** Period specimen (strongest as tribute, festival page, storytelling or cultural content); the carved-and-backlit treatment transfers to illustrations and hero art.
**Web fit:** Medium — cultural, museum, festival (e.g. Lunar New Year), children's storytelling and narrative landing pages; a striking loading or intro screen. Low for dense product UI: the warm glowing screen and multiplied overlaps darken quickly.

## Recognisable traits

- A glowing cloth screen lit from behind by an oil lamp: warm radial light, brightest in the middle, burning out to brown at the edges.
- Figures are translucent dyed leather: colour shows only where light passes through, and overlapping pieces get darker (colours multiply).
- Pierced and carved decoration inside every piece: fish-roe dots, flowers, cloud spirals, lattices, incised lines — always inside a solid leather rim.
- Jointed figures: separate pieces pinned with rivets, moved by thin rods running down off-stage; joints are stiff, wrists dangle.
- Faces in profile with an "empty face" for gentle roles — the face is carved away, leaving only its outline plus phoenix eye, willow-leaf brow and a dot of lip.

## Palette

| Role | Hex | Note |
|---|---|---|
| Background (screen centre → edge) | `#fff6d6` → `#f2d7a0` → `#cf9a58` → `#7a4a20` | Source: lamp-lit cloth, radial. |
| Surface | `#fbeccb` (suggested) | Cards as patches of brighter screen. |
| Text / outline | `#3a160a` | Source: dark-brown leather edge; use for text. |
| Muted text | `#6b3d22` (suggested) | |
| Accent (red leather) | `#c0352a` | Source. Primary action. |
| Accent 2 (green leather) | `#2f7d4c` | Source. Success. |
| Amber / yellow | `#d99a2e` / `#e2bd4a` | Source. Highlights, badges. |
| Blue | `#2c5a8a` | Source. Links, info. |
| Orange | `#d9782a` | Source. |
| Frame wood | `#4a2412` (suggested) | Stage frame around the screen. |

Relationship: saturated dyed colours seen *through* warm light — every colour is multiplied by the screen, so the palette always reads warm; red × green overlap goes nearly black. No pure white (white areas are carved holes showing the screen) and no cool background. Dark reading: the stage at night — a dark wooden frame `#1c0f08` around a smaller glowing screen; text inside the screen stays dark brown.

## Typography

- Display: a brush or woodblock-flavoured face — **Ma Shan Zheng** or **ZCOOL XiaoWei** for Chinese titles; for Latin, a calligraphic serif such as **Cormorant SC** or **Cinzel** at moderate weight.
- Body: **Noto Serif SC** / **Noto Serif** (`"Noto Serif SC", "Noto Serif", serif`) in dark brown, normal weight.
- Labels: carved-feeling small caps, or vertical text (`writing-mode: vertical-rl`) for a single Chinese label on a side rail.

## Surfaces, texture and edges

- **Screen:** a radial gradient through the four source stops, plus a 3px cloth weave tile (cached), plus a `screen`-blended hot spot at the lamp (radius 260 + 40·flicker at 1080p).
- **Leather piece = fill + carve + rim + outline:** fill with the dye colour; cut the carve pattern out (`destination-out`, or an SVG `mask`); then, still clipped to the piece, stroke a rim of 2×edge in the same colour so carvings never cut through the outline; finally stroke 2.6px dark-brown `#3a160a`.
- **Carve library (source sizes):** fish-roe dots (spacing 14–22, r 3–3.5); five-petal flowers (spacing 36–46); cloud spirals (small Archimedean spirals, line 3.4, spacing 24–40); lattice (26–32); incised lines (3–7px).
- **Stamp onto the screen:** each piece is first printed as a soft offset shadow (multiply α 0.28, blur 6px, offset (7, 5)) — the puppet not quite touching the cloth — then the piece itself multiplied on. CSS: `filter: drop-shadow(7px 5px 6px rgba(58,22,10,.28))` plus `mix-blend-mode: multiply`.
- **Rivets:** small dark dots with a lighter centre at each joint. **Rods:** 5px dark-brown lines, blur 1.2px, multiply, running from the joints down off the bottom edge.

## Shapes and layout

The page is a stage: a wooden frame around a glowing screen, content on the screen. Shapes are profile silhouettes with ornamental, scalloped or cloud-curl edges; corners can be cut or cloud-shaped rather than plain radii. Composition is a horizontal strip like a puppet stage: elements arranged along a baseline, mostly in profile, overlapping as little as possible at the focal point. Shadows are the soft offset "not touching the screen" shadow only.

## Components

- **Buttons:** a leather tab in red or green with a carved dot border and dark-brown outline; on press it presses flat against the screen (offset shadow shrinks to zero).
- **Cards:** brighter patches of screen, or a pale leather plaque with a carved lattice border and a solid rim.
- **Navigation:** a row of carved leather tabs along the stage's top or side; active tab lit from behind (brighter, warmer).
- **Inputs:** plain screen-cream field with a dark-brown rim; focus = the lamp brightens behind it.
- **Chat bubbles:** leather plaques — replies in amber, user in blue — with carved edging and the text on a solid uncarved centre.
- **Tables:** on a solid uncarved plaque; no carving under text.
- **Badges:** small round carved medallions (flower or cloud).
- **Loading:** a butterfly on a rod flapping by flipping `scaleX` at 12fps, or the lamp flickering.
- **Done moment:** a puppet piece swings on its rivet and settles; carved flowers light up.
- **Empty state:** a single carved figure or object on the glowing screen.
- **Illustration/mascot:** pieces jointed at rivets, white areas carved out (see Techniques), rods attached at neck or wrist.

## Motion

- **Lamp flicker:** `fl = 0.5·noise(3t) + 0.3·noise(9t)`, stepped at 15fps; the light centre drifts ±26/±18px and the hot spot breathes with it.
- **Rod puppetry:** whole figures sway around a pivot ±0.012rad; heads turn on the neck rivet ±0.06rad; motion stepped at 10fps.
- **Loose wrist:** `dangle = 0.35·sin(11t)·(1 − 0.6·effort)` — the hand swings when the arm is idle and steadies when it is doing something.
- **Chained tails/ribbons:** 4 hinged segments, each rotating `0.3·sin(7t − 0.9i) + 0.38` relative to the previous, so the tip lags segment by segment.
- **Nodding** `0.1·sin(6t) + 0.05`; bead hair ornaments swinging.
- `prefers-reduced-motion`: steady lamp, puppets at rest, no flapping.

## Techniques

- **Carving in SVG:** each piece is a `<g>` with a fill shape masked by a `<mask>` (white piece, black carve pattern from a `<pattern>`), then a `<path>` with the same outline drawn as `stroke` only — clipped to the piece — for the rim, then the dark outline. Keep pieces as separate `<g>` elements with `transform-origin` at their rivet.
- **White parts are holes:** where an illustration would have white (a white chest, white paws, white collar), carve the area out entirely and keep only the leather rim — the screen shows through. This rule works for every silhouette or cut-out style.
- **Hinged chains:** nest `<g>` transforms (each segment inside the previous one) so rotations accumulate.
- **Overlap = darker:** with `mix-blend-mode: multiply` on each piece, overlaps darken naturally.

## Performance and accessibility

- Expensive: every piece on its own offscreen layer with multiply and blur (the source took ~121ms per frame — "a bit slow"). Cheap: static SVG puppets with CSS transforms on rivet pivots, one `drop-shadow` per figure group rather than per piece, a cached cloth tile, lamp flicker via `opacity` on one radial layer. Cap DPR at 2; pause when hidden.
- Contrast: `#3a160a` on the bright centre `#fff6d6` is ~15:1, but the edges `#cf9a58`/`#7a4a20` are too dark for dark text — keep text in the bright centre or on solid plaques. Never put text over carved patterns.
- Motion: flicker is low-amplitude warmth, but still stop it under reduced motion.

## Pitfalls

- A table leg over a red skirt → multiplied red × green became a black trapezoid. Overlap darkening is authentic, but keep overlaps away from the focal point.
- A key area drawn as one solid piece covered a white detail (the cat's white chest disappeared) → carve white parts out and keep only the rim.
- Rods crossing other figures look chaotic → run rods down and outward from each piece, never across another subject or across text.
- Carving that cuts through the outline → always restroke the rim inside the clip after carving.
- Text on carved leather → unreadable; use an uncarved centre.

## Nearest neighbours

- **30 · Matisse cut-outs:** also cut shapes with a soft offset shadow, but Matisse is opaque gouache paper on a flat wall in pure complementary colours; shadow puppets are translucent, backlit, carved with patterns and jointed.
- **03 · Greek black-figure:** also profile silhouettes with incised lines, but black-figure is opaque black on terracotta with no light behind it; piying is coloured light through leather on a glowing screen.

## Source

Adapted from huashu-art-motion · `34_shadowpuppet.md` (built ★★★; noted shortcomings: multiplied overlaps go black; ~121ms per frame was slow).
