# 51 · Persian miniature (late Timurid Herat and early Safavid Tabriz, c. 1480–1550)

**Status:** Research recipe — one redone single-scene study (a courtyard water wheel feeding an octagonal pool, two readers under a pavilion), self-scored ~80 then ~72 after a figure redo, independently reviewed ~63. Below 80; techniques are hypotheses.
**Layer:** Period specimen with a transferable core — the layered, "tapestry" space that lets many events be read at once transfers to any multi-step subject; the specific ornament is period-bound.
**Web fit:** Medium — editorial features, museum and cultural sites, illustrated process explainers, storybook pages, ornate landing pages. Low for everyday product UI: full-coverage pattern fights content.

## Recognisable traits

- A raised viewpoint with no vanishing point: the ground stands up like a hung carpet; distance is shown by stacking higher, not by shrinking.
- Architecture cut open so you see inside rooms; several events visible at once along clear paths.
- Every surface patterned: geometric tiles, brick, floral dots — but the pattern follows each surface and stays put.
- Saturated mineral colours (lapis blue, vermilion, peacock green) controlled by fine pattern, with gold reserved for frames and precious materials.
- Text blocks set into the picture and borders (gold rule + lapis band) framing the page.
- Small figures, side-on, tall turbans, slender bodies; fine dark-brown outlines on everything.

Scope note from the source: this is not one artist or era. Choose a range (here Herat/Tabriz) before working, and don't mix Persian, Mughal and other "fine oriental painting" into one look.

## Palette

From the study's anchors (Herat *Yusuf and Zulaikha*, a garden bathing page, a *Shahnama* of Shah Tahmasp page) — observed, not official.

| Role | Hex | Note |
|---|---|---|
| Background (paper) | `#dcc38c` / `#d9bf88` | Burnished paper; page ground. |
| Surface (cream) | `#efe0b4` / `#efe3bd` | Text blocks and cards. |
| Text / outline | `#2b2118` | Dark brown-black ink; ~9.2:1 on paper. |
| Accent (lapis) | `#1f4f8f` / lighter `#2f6fb0` | Borders, primary actions; cream on lapis ~6.2:1. |
| Accent 2 (vermilion) | `#b7372b` | Highlights, alerts; ~4.4:1 on cream (bold/large). |
| Peacock green | `#2a8a7a` / pool `#2c7e8a` | Water, success. |
| Gold | `#d6a93c` | Frames, rules, borders only. |
| Rose | `#d98a7a` | Blossoms, tags. |
| Brick / greens | `#c0643f`, `#6f9a4a`, `#3f6e3a` | Architecture and garden. |

Relationship: a warm paper ground carrying many saturated mineral colours at once, each held by brown outlines and gold rules. Gold marks material (frames, metal), never fills large areas. Dark reading (suggested): lapis ground, cream text panels inset with gold rules.

## Typography

Latin headings: **Cormorant Garamond 600** or **Cinzel** for a manuscript feel — `"Cormorant Garamond", Georgia, serif`. Body: **Source Serif 4** or **Literata**. If the page has Persian text, use a real typeface with a real text: **Vazirmatn** (UI) or **Noto Nastaliq Urdu** for display Nastaliq, `dir="rtl"`. The source is firm: never fake calligraphy or use pseudo-Arabic squiggles as decoration (the study's own text block was fake script and is listed as a shortcoming).

## Surfaces, texture and edges

- **Page frame:** nested borders — outer gold rule, a lapis band, an inner gold hairline. In CSS: `border: 3px solid #d6a93c; box-shadow: 0 0 0 10px #1f4f8f, 0 0 0 12px #d6a93c;` (suggested).
- **Tile fields:** 8-pointed stars on a repeating grid (alternating radius 0.38/0.2 of the tile size, rotated π/8), two colours plus cream ground. Build as an SVG `<pattern>`; size 36px in the study.
- **Channels/inlays:** stacked strokes for inlaid borders — ink 78px, gold 72px, teal 58px on the same path — a neat SVG trick for any banded divider or progress track.
- **Paper fibre:** a light paper texture in `#d6bf8a`, `multiply` at ~0.28.
- **Garden ground:** dense scatter of small ellipses (greens, cream, rose), ~1,400 per screen, pre-rendered as a tile.

## Shapes and layout

Flat colour, no shadows, no gradients. Every shape has a fine brown outline (2–3px). Layout is the transferable lesson: arrange a process or story as stacked horizontal registers seen from above, each with its own small scene, connected by a clear path (a water channel, a road) that runs through them. Cut boxes open: a card can show its "inside" (a revealed sub-panel) rather than hiding it behind a click. Use a framed text block inside the composition for headlines.

## Components

- **Buttons:** lapis with cream text and a gold hairline inset; hover brightens to `#2f6fb0`.
- **Cards:** cream panels framed with gold rule + lapis band; header cell patterned with tiles.
- **Navigation:** a lapis band across the top with gold separators.
- **Inputs:** cream field, brown 1.5px outline; focus = gold 2px outer ring plus lapis inner line.
- **Tables:** cream cells with brown hairlines; header row in lapis; never tile-pattern the cells.
- **Badges:** small outlined blobs in vermilion, green, rose.
- **Loading:** a water wheel turning or gold ripples running along a channel.
- **Empty state:** an empty octagonal pool with a single fish circling.
- **Process/flow diagrams:** the strongest fit — a layered courtyard where each step is a small scene on a shared path.

## Motion

- **Gold wave along a channel (source motif):** a gold highlight travels along the channel path (SVG `stroke-dasharray` with animated `stroke-dashoffset`).
- **Water wheel:** steady rotation of a 12-spoke wheel with gold buckets.
- **Pool rings:** six rings expand from a point, `q = (0.35t + i/6) mod 1`, radius `20 + 150q` by `8 + 50q`, alpha `0.8·(1 − q)` in pale cream.
- **Fish:** three fish orbit the pool on an ellipse (140 × 70px), alternating direction, tails flicking `sin(8t)·4px`.
- **Petals:** ~22 petals fall slowly with sideways sway `sin(1.4t + i)·24px`.
- Figures barely move (a breath of 1.5–2px). Pattern must never drift relative to its surface. Under `prefers-reduced-motion`, freeze wheel and fish, show rings as static.

## Techniques

- Tile star in SVG: a polygon with 16 points, odd points at `0.2·size`, even at `0.38·size`, rotated π/8; tile with `<pattern patternUnits="userSpaceOnUse" width="36" height="36">`.
- Octagonal pool: an 8-gon squashed vertically (×0.74), outlined in gold; ripple and fish elements clipped to it with `clip-path: polygon(...)`.
- Keep subject layers and pattern layers separate so patterns can be clipped to each surface and stay fixed when things move.

## Performance and accessibility

- Cheap: SVG patterns, a few CSS-transform animations. Expensive: thousands of individual SVG nodes for scatter — bake those to a tile image.
- Contrast: brown on paper ~9.2:1; cream on lapis ~6.2:1; vermilion on cream ~4.4:1 (large/bold only). Never put text on tile patterns.
- Dense pattern is tiring: restrict it to frames, headers and illustrations, and keep reading areas plain cream.
- Cultural care: use real script with correct shaping, and choose one historical range; avoid generic "Oriental" pastiche.

## Pitfalls

- Fake connected script in text blocks (source shortcoming) → use real text and a proper font, or no script at all.
- Single-point perspective on a courtyard → breaks the style; stack and tilt the ground up.
- Patterns that slide when objects move → bind patterns to surfaces.
- Figures too small and expressionless (source shortcoming) → give key figures more size, gesture and gaze.
- Ground lacking layered stacking (source shortcoming) → add rising registers.
- Mixing Persian, Mughal and Ottoman motifs as "authenticity" → pick one range.

## Nearest neighbours

- **43 · Lorenzetti:** also readable social space with many actors, but in continuous solid urban space with depth. The miniature has no vanishing point, stacked registers and full-coverage pattern.
- **05 · Gothic illuminated manuscript (built):** gold leaf with brushed highlights, vines and margins around text. Persian miniature uses gold for frames only, with lapis bands and geometric tile fields inside the picture.
- **55 · Dunhuang (Northern dynasties):** dry, cracked mineral murals with loose flying lines; the miniature is pristine, precise and gilded.

## Source

Adapted from huashu-art-motion · `51_persian_miniature.md` (research recipe; self ~80 → ~72 after figure redo, independent ~63; known shortcomings: figures small and simple, faces without expression, fake script in the text block, ground lacks layering).
