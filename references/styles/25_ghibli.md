# 25 · Hand-painted watercolour animation (Studio Ghibli visual family, 1980s–2010s)

**Status:** Two parts. **Part A — watercolour background treatment: Built ★★.** **Part B — whole-studio visual language: Research recipe** (hypotheses, not verified across subjects; it does not inherit Part A's rating).
**Layer:** Material (the watercolour wash and two-tone cel shading work on any subject)
**Web fit:** High for warm consumer products, onboarding, empty states and marketing pages that want calm, lived-in charm. Medium for dense product UI (keep washes behind panels, not inside data). Never copy the studio's characters, creatures or logos; borrow the approach, not the cast.
**Mockup:** `assets/mockups/25_ghibli.png`

## Recognisable traits

- Bright, airy painted backgrounds: transparent watercolour/gouache washes layered over each other, with darker pigment pooling at the edges ("tide marks").
- Clean characters on top: simple two-tone cel shading (a base colour plus one crescent of shadow on the side away from light) and a soft warm-brown line, not black.
- Weather you can feel: wind moving curtains and grass in gusts, cumulus clouds, dappled sunlight through leaves (komorebi).
- Rooms and places that look lived in: specific objects, materials and wear, but never gloomy — interiors are light and warm.
- Environmental motion always has a cause (wind, gravity, contact), never random wobble.

## Palette

| Role | Hex | Note |
|---|---|---|
| Background (lit wall) | `#fbf3e0` | Source: wall near the window. |
| Background (corner) | `#dcc49a` / `#ecdcbc` | Source / mockup: the same wall in the corner. Radial gradient between the two. |
| Surface (paper) | `#fffcf3` at 80% (suggested) | Washed cards. |
| Text | `#3a2c22` (mockup) | Warm dark brown. |
| Line | `#4a3428` → softer `#6a4a38` | Source: cel outline; the softer value for illustrations over watercolour. |
| Sky | `#bcd9ea` / `#a9cfe6` (mockup) | Sky band, user bubbles. |
| Leaf | `#6f9a52` / `#4f7d3c` (mockup) | Success, nav numbers. |
| Sun | `#e7a63c` (mockup) | Primary action, running state. |
| Wood | `#b98a5c` (mockup) | Secondary text, borders. |

Relationship: warm cream ground, soft sky blue and leaf green as the cool partners, one sunny orange accent. Washes overlap transparently (multiply), so colours mix where they meet. Avoid neon and the blue-to-purple gradient. Dark reading (suggested): dusk or night scene — indigo `#1f2a3d` sky, lamp-lit warm cards `#f3e6c8` floating on it, fireflies instead of dapples.

## Typography

Friendly and readable: **Nunito** or **M PLUS Rounded 1c** for UI (`"Nunito", "M PLUS Rounded 1c", system-ui, sans-serif`), with a soft serif such as **Fraunces** (weight 400, optical size high) or **Lora** for headings; italics of that serif for small asides like loading text. Avoid hard geometric faces.

## Surfaces, texture and edges

- **Washed panels:** put the fill on a `::before` layer and run it through an SVG filter: `feTurbulence` (fractalNoise, baseFrequency ~0.035, 3 octaves) → `feDisplacementMap` (scale ~7) → `feGaussianBlur` (0.7). Add the tide-mark edge as `box-shadow: inset 0 0 0 1.6px <edge>` and the pooled pigment as `inset 0 0 14px <pool>`. The filter goes on the pseudo-element so text stays crisp.
- **Paper grain:** an SVG noise tile (`feTurbulence` baseFrequency ~0.75 → colour-matrix to warm brown) laid over the page with `mix-blend-mode: multiply` at ~0.3–0.35.
- **Colour blooms on the wall:** a handful of large blurred, displaced circles (warm yellow, pink, blue-grey, pale green) at very low alpha with `multiply`. Source used 28 blooms at alpha 0.045, 3 layers.
- **Clouds:** clusters of white circles (radial gradients) through the same watercolour filter, drifting slowly.

## Shapes and layout

Soft rounded corners (12–16px), but the displaced wash makes edges organic. No hard drop shadows; depth comes from overlapping washes and a pooled edge. Layout can include a sky band across the top and a strip of grass along the bottom, framing the content like a scene. Leave the centre bright and calm.

## Components

- **Buttons:** a sun-orange disc or pill with a soft inner shadow on the bottom edge (`inset 0 -3px 0 rgba(150,90,20,.35)`), brown text.
- **Cards and replies:** washed paper panels with tide-mark edges; serif heading in line brown.
- **Navigation/sidebar:** one large wash panel (wood brown to leaf green, low alpha); active item gets a sun wash.
- **Inputs:** a near-opaque paper wash; on focus, the edge darkens like wet pigment.
- **Chat bubbles:** user in sky-blue wash, replies in paper wash.
- **Tables:** dotted rules in line brown at ~30%.
- **Badges/status:** small coloured dots — leaf green for done, sun for running (pulsing).
- **Loading:** a blue water drop that spreads and fades ("wet paper blooming").
- **Done moment:** a green watercolour bloom spreading out from the checkbox.
- **Mascot/illustration:** two-tone cel shading plus a soft brown line, then a watercolour texture clipped to its silhouette (see Techniques) so it doesn't look like flat vector on painted paper.

## Motion

- **Curtain/fabric billow:** the lower edge bulges by `q²·(60 + 150·gust)` where `gust = 0.55 + 0.45·sin(5.4t + phase)`, plus a small ripple `sin(7t + 6q)`. Use fixed random seeds so the edge never "boils". Web: an SVG path or CSS skew on a curtain/banner element.
- **Grass in gusts:** a wave of wind travels sideways — `wind = 0.5 + 0.5·sin(3.1t − 0.018x)`. Web: each blade rotates from its base with `animation-delay` proportional to x (mockup: −3° → 14°, 3.2s).
- **Clouds** drift 22–30px/s in the source; on a page, 38–52s per crossing feels right.
- **Dappled light:** warm radial spots with `soft-light`/`screen`, positions drifting on noise, brightness `sin(5t + 1.7i)`.
- **Tree crown sway:** ±0.025rad around the trunk base.
- Easing is soft and natural (ease-in-out), never stepped.
- `prefers-reduced-motion`: keep the painted scene still — no drifting clouds, swaying grass, pulsing or dapples. Show the done bloom at its final state.

## Techniques

**Part A — watercolour background (Built ★★)**

- `deform(points)`: midpoint displacement. Each round pushes every edge midpoint along its normal by a near-Gaussian amount (sum of 3 uniforms) scaled by edge length (`min(1, len/80)`); amplitude × 0.6 per round.
- `wash(shape)`: deform 3 rounds for the base shape; each layer deforms 2 more rounds and is filled with `multiply` at low alpha, then stroked with a 1.6px line in the same colour at alpha = edge. Stacked strokes = the tide-mark edge.
- Typical parameters: large wall layers 5, alpha 0.4, amp 12; colour blooms layers 3, alpha 0.045, amp 40; wood planks layers 3, alpha 0.5, amp 3, edge 0.14; opaque whites (frames, cloth, clouds) `source-over` at alpha 0.9–0.95.
- For the web, run this once in a canvas at load (or offline) and use the result as a background image or tiles; the source's first frame took ~600ms to build the cache, so never do it per frame or per resize without debouncing.
- **Cel shading in one function:** fill the shape with the shadow colour, then inside a clip of the shape fill it again with the base colour shifted toward the light by (−dx, −dy): only a crescent of shadow remains on the far side. Stroke with 2.4px warm brown. In SVG: a shape, plus a translated copy of itself in `<clipPath>` of the original.
- **Watercolour on illustrations:** render the figure, build a texture layer (paper grain at 0.8 plus ~40 transparent blooms at alpha 0.1), keep it only inside the figure's silhouette (`destination-in`, or an SVG mask), and multiply it back on.

**Part B — whole-studio visual language (Research recipe)**

The source treats the studio as a family, not one recipe. Hypotheses to test, not rules:
1. Choose a scope that fits the content: the everyday-life films (*My Neighbor Totoro*, *Kiki's Delivery Service*, *Spirited Away*) differ from *The Tale of the Princess Kaguya*, whose line and empty-space organisation is distinct and should not be forced into the watercolour recipe.
2. Clear silhouettes and expressions in front, backgrounds with their own observed material detail behind.
3. Character and environment agree on perspective, light direction, contact shadows and colour; they need not share the same watercolour texture.
4. Everyday actions have weight, sequence and reaction; environmental motion has a cause.
5. Places are habitable and serve the story.

Content is variable: forests, country houses, girls, cats, brooms, food and giant clouds are not required. A city laundromat, a night-shift transit hub or a repair bay can carry the language. "Lived-in" does not mean always slow; tense moments need precise action. For UI: let micro-interactions have cause and consequence (a card settles with weight, a toggle has a small follow-through), and make illustrations specific to your product's world rather than generic pastoral scenery.

## Performance and accessibility

- Expensive: SVG displacement filters on many or large elements, and animating anything that sits under a filter (it re-rasterises each frame). Cheap route: filter only static pseudo-elements; animate the cloud/grass elements with transforms; pre-render the wall blooms to an image; cap DPR at 2; pause ambient loops when the tab is hidden.
- Contrast: brown `#3a2c22` on cream `#fbf3e0` is ~12:1. Keep washes behind text at high opacity (≥0.8 paper); sky-blue bubbles need the dark brown text, not white.
- Motion: gentle, but ambient loops still go under reduced motion.

## Pitfalls

- Blooms at alpha 0.07 × 4 layers plus heavy grain → the wall looked like mouldy parchment. Ghibli rooms are bright: blooms at 0.045 × 3, lighten the gradient a step.
- Two rectangular washes side by side → a hard seam. Dark areas must be large, strongly deformed polygons (amp ~60), never rectangles.
- Background painterly, character flat vector → "the background looks right, the character doesn't". Clip a watercolour texture into the figure and soften its line colour.
- Curtain that barely moves reads as frozen → raise the gust frequency (3.2 → 5.4) and amplitude.
- Random per-frame jitter on wash edges → edges "boil". Fix seeds.
- Filter on the element itself → blurry text and clipped focus rings. Put it on `::before`.

## Nearest neighbours

- **35 · Shinkai light:** also Japanese animation backgrounds, but Shinkai is photographic, saturated, with lens bloom, god rays and hard cel shadow bands; Ghibli is painterly, matte, with transparent washes and tide-mark edges.
- **17 · Chinese ink wash:** also water-based with pooling edges, but ink is monochrome with large empty paper and brush calligraphy; Ghibli is full colour, fills the frame with lived-in detail.

## Source

Adapted from huashu-art-motion · `25_ghibli.md` (Part A built ★★; noted shortcoming: characters weaker than backgrounds — consider hair highlight blocks and curtain shadows moving in floor light patches. Part B is research; a separate repair-shop study exists but is still "programmatic illustration", not studio-level recognisability).
