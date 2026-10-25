# 32 · Chiaroscuro (Rembrandt van Rijn, Dutch Golden Age, 1640s)

**Status:** Built ★ (the weakest built card in the catalogue: figures read flat — "most in need of a redo")
**Layer:** Material (the light-map method works on any subject)
**Web fit:** Medium — dark premium or editorial pages, museum, wine, whisky, legal, storytelling and "focus" modes where one thing should glow out of darkness. Low for bright, dense or playful product UI: most of the screen is deliberately dark.

## Recognisable traits

- Light is scarce and directional: a pool of warm light on the subject, everything else falling into deep brown darkness.
- Lost edges: on the shadow side, forms dissolve into the background; only the lit side has crisp detail.
- Separation light: the wall behind the subject's dark side is lit a little, so the figure still separates from the ground.
- Thick impasto on highlights (gold, linen, pearls, porcelain), thin brown glazes in shadow.
- Old-master surface: craquelure, a warm golden varnish, vignette.

## Palette

| Role | Hex | Note |
|---|---|---|
| Background (ambient dark) | `rgb(30,21,13)` = `#1e150d` | Source: light-map floor ≈ 13% ambient. |
| Surface (dark panel) | `#2b1f14` (suggested) | Barely lifted brown for cards. |
| Lit surface | `#e9d8b4` (suggested) | Card that sits "in the light pool". |
| Text on dark | `#efe3c8` (suggested) | Warm parchment. |
| Text on lit | `#2a1c10` (suggested) | Deep umber. |
| Accent (gold) | `#c8a050` (suggested) | Highlights, primary action. |
| Accent 2 (vermilion) | `#9e3a22` (suggested) | Rare warm red, as in a garment or rug. |
| Varnish tint | `rgba(200,140,60,.35)` | Source: soft-light golden glaze. |

Relationship: a narrow warm range from near-black umber to gold and cream; no cool colours except as dull greys. Value, not hue, does the work. Dark is the native reading; a "light" variant would lose the style — at most, a lit parchment page with a dark vignetted frame.

## Typography

Old-world but readable: **Cormorant Garamond** or **EB Garamond** for headings (regular, warm parchment colour), **Spectral** or **Source Serif 4** for body on lit cards; small caps with +0.08em tracking for labels. Avoid geometric sans. On dark grounds use weight 400+ and 17px+ body to survive the low light.

## Surfaces, texture and edges

- **Light map multiply (the core method):** paint the page or illustration fully lit, then multiply it by a light map — a dark ambient fill with soft radial pools added (`lighter`) where light falls. On the web: a full-viewport overlay `background: radial-gradient(...)` layers on `rgb(30,21,13)` with `mix-blend-mode: multiply`, or simply darken with layered radial gradients on the page background.
- **Source light pools (radius / alpha at 1080p):** face/focal pool r250 α0.95; chest r230 α0.55; secondary subject r240 α0.8; table r280 α0.6; window r330; separation light behind the subject's dark side r300 α0.35; a light shaft as a wedge polygon blurred 40px, α 0.8–0.95; candle r190 + 40·flicker, α 0.6 + 0.4·flicker.
- **Lost edges:** draw the subject once blurred (2.4px) at full opacity, then the sharp version at α 0.62 on top — the dark side melts, features stay legible. CSS: a blurred duplicate behind, or `filter: drop-shadow` in background colour.
- **Overlays (pre-rendered once):** craquelure at α ~0.08 (0.28×0.28), grain density 0.05 at α 0.12, vignette 0.75, golden soft-light glaze `rgba(200,140,60,.35)`.
- **Background brushwork:** big brown strokes (cell 18, length 54, width 16) then blurred 2.2px and laid at 75% — a "rubbed-in" ground.

## Shapes and layout

Composition is a spotlight: one focal element in the light pool, everything else recedes. Cards on dark have no borders — they are lighter areas with soft edges, or lit cream cards with a soft dark halo. No hard shadows. Use a single light direction (upper left, a window) consistently. Leave large dark areas; don't fill them with secondary content.

## Components

- **Buttons:** gold or cream on dark with soft inner light; hover raises the light (brighter pool) rather than changing hue.
- **Cards:** dark brown panels; the active or focal card sits in a light pool (cream surface, dark text).
- **Navigation:** parchment text on dark; active item brightened as if a candle moved near it.
- **Inputs:** dark field with a warm 1px lower edge; on focus the field "lights up" to cream.
- **Chat bubbles:** replies as lit parchment cards; user messages in dark panels with parchment text.
- **Tables:** on a lit card; dark rows recede, the hovered row lifts into light.
- **Loading:** a candle flame or a slowly brightening light pool.
- **Done moment:** a shaft of light falls on the finished item, with drifting dust inside it.
- **Empty state:** darkness with one small pool of light and a short line.
- **Illustration/mascot:** multiply it by the same light map as the page (lit side vs. shadow side), apply lost edges on its dark side, and add impasto touches only on its highlights.

## Motion

- **Candle flicker:** `flick(t) = 0.5·noise(2.2t) + 0.25·noise(6.5t)` drives radius and alpha of the candle pool; the pool centre drifts ±14px on `noise(0.9t)`.
- **Cloud shadow across window light:** `cloud = 0.5 + 0.5·sin(2.2t − 0.6)`; window light × (1 − 0.35·cloud), and a dark radial blot crosses the glass (source: 260px/s).
- **Dust in the light shaft:** ~70 motes visible only inside the shaft, twinkling.
- Movement is slow and atmospheric; the subject itself barely moves.
- `prefers-reduced-motion`: fixed light pools, no flicker, no dust.

## Techniques

- **Impasto with a bristle brush:** one stroke = round(w/1.3) parallel thin bristles, each ±17 brightness, ends randomly shortened 0–25%, width 1.4–2.6px; a faint shadow line below (α 0.28) and 2 + w/3 bright specks on top. Use only on lit highlights (hair, collars, porcelain, metal, pearls). Pre-render as SVG/PNG accents; don't generate live.
- **Layer order:** lit base → background strokes → props on their own layer after the strokes → subject (flat + volume gradient + fine strokes) → lost-edge pass → light map multiply → window screen-brighten → impasto → flame and dust → crack/grain/vignette/glaze.
- For a UI, the cheap equivalent: dark page, 2–3 radial light pools as background layers, a cached grain-and-crack tile at low alpha, a golden soft-light overlay on hero imagery only.

## Performance and accessibility

- Expensive: full-screen multiply with blurred pools re-rendered per frame (~47ms per frame in the source), animated blur. Cheap: static radial gradients, animate only the candle pool's `opacity`/`transform`, pre-render grain/cracks, cap DPR at 2.
- Contrast: the main risk. Parchment `#efe3c8` on `#1e150d` is ~14:1, but text placed in the dim zones between pools can fall below AA once multiplied — never multiply the text layer; keep text above the light map. Muted text must stay ≥ 4.5:1 against the actual (darkened) background.
- Motion: flicker must be gentle (no strobing); disable under reduced motion.

## Pitfalls

- Impasto drawn as three neat lines (shadow, stroke, highlight) → read as white stickers or plasters. Use the bristle brush.
- Copying a rectangular region of the original image back on top → brought back unbrushed background as a rectangular patch. Draw props as their own layer after the strokes; never copy regions.
- Grain density 0.12 / α 0.22 → dirty speckles on faces; 0.05 / 0.12 is right.
- Draw order matters (hand before cup, then thumb) → in general, overlapping illustration parts need a deliberate order.
- Flat-shaded figures on a painted ground → the source's main failure (review score 5/10). Give subjects real volume: gradient modelling plus the light map, not flat vector.

## Nearest neighbours

- **06 · Renaissance sfumato:** also old-master glazes and craquelure, but sfumato is evenly soft and fairly bright; Rembrandt is mostly darkness with a concentrated pool and strong value contrast.
- **24 · Hopper hard light:** also directional light as the subject, but Hopper is daylight with hard geometric slabs and cool shadows; Rembrandt is warm, soft-edged pools in near-black.

## Source

Adapted from huashu-art-motion · `32_rembrandt.md` (built ★; noted shortcoming: figures flat, review 5/10 — the card most in need of a redo).
