# 04 · Roman mosaic (Pompeii, before 79 CE)

**Status:** Built ★★★
**Layer:** Material (works on any subject) — narrow use
**Web fit:** Medium — a memorable hero, illustration treatment, loading screen or section divider; a mosaic-rendered logo or avatar. Too heavy to apply to every surface of a working UI: text and controls should sit on plain plaster panels next to the mosaic, not inside it.

## Recognisable traits

- Everything is made of small stone tesserae with visible grout between them; the stones themselves never move.
- *Andamento*: rows of stones follow the contours of each shape, with a pale halo of stones running around every figure, then the background fills in regular offset rows.
- Stones are irregular, chiselled squares cut by their neighbours, each with a slight bevel (lit top left) and small colour variation.
- A heavy dark outline around shapes, built from a row of dark stones.
- Classical borders: a running wave (Vitruvian scroll) in black and white, red frames with gold lines, Latin capitals like "CAVE CANEM".

## Palette

| Role | Hex | Note |
|---|---|---|
| Background (wall stones) | `#e4ddcd` | ±8% per-stone variation. |
| Halo / surface | `#f5f1e8` | Pale ring around figures; plain panels for UI text. |
| Outline / text | `#46322a` | Dark stone row. |
| Grout | `rgb(112,103,92)` ±8 | Only between stones. |
| Accent (red frame) | `#a8322a` / dark `#7c221d` | Frames, primary buttons. |
| Gold line | `#d0a23c` | Thin frame lines, highlights. |
| Blues (picture) | `#7c84b8` / `#4e5796` / `#2f3672` | Three-tone faces of a form. |
| Greens / earth | `#6f9341`, `#92b254`, `#cba344` | Landscape stones. |
| Hot accent | `#d64a22` / `#f2952c` | Lava, alerts. |

Relationship: natural stone colours (cream, terracotta, ochre, slate blue, olive) with a few accent stones; 9% of stones lean warm, 7% lean cool. Dark reading: a dark slate wall (`#2f3672`-ish or deep brown) with cream text panels works, but the classic look is light.

## Typography

Mosaic letters must be **monoline, heavy-stroke capitals with each stroke about two stones wide** (source: 14px strokes, 60px cap height, 16px spacing). Thin serifs break up: an E loses its arms and becomes F, an A loses its bar. Draw headline letters as custom stroked paths, then tessellate. Outside the mosaic, use a classical caps face for headings, e.g. **Cinzel 600** (`"Cinzel", "Trajan Pro", Georgia, serif`), and a plain sans for body.

## Surfaces, texture and edges

- **Tessellation:** render once (or at build time) to a canvas or image: compute a stone layout, then fill each pixel from the stone it belongs to. Store a per-pixel stone ID and bevel shade; recolouring later is a cheap loop.
- **Grout:** a pixel is grout if it's outside every stone or close to the bisector between its two nearest stones (`(D2 − D1)·h/2 < g`, g = clamp(0.1s, 0.75, 1.25)px), giving 1.5–2.5px joints with irregular, chiselled stone edges.
- **Bevel:** within 2.2px of a stone's edge, lighten or darken ±16% by the dot of the edge normal with light (−0.6, −0.8); add a slight dome term (−5%·D1²) and ±3% pixel grain.
- **For simple UI surfaces:** a pre-rendered seamless mosaic tile (e.g. 512px) as `background-image` for a header band or border, with plaster-coloured panels on top for content.

## Shapes and layout

Square-ish everything; corners square or barely rounded. Outlines are 8–10px "rows" in the mosaic, but UI panels can use a 2px dark line. No shadows except the stone bevel. Layout is framed: a red frame with a gold inner line around a central *emblema* (picture panel), borders of running wave on the edges. Grid layouts suit it naturally; give each section a framed panel.

## Components

- **Buttons:** a red tile with cream text and a dark 2px outline; hover lifts the bevel highlight.
- **Cards:** plaster panel with a mosaic border strip (a few rows of small stones) along the top.
- **Navigation:** a running-wave border band as the top bar; active item gets a gold stone underline.
- **Inputs:** plain plaster with a dark stone-row bottom border.
- **Tables:** plaster with dark rules; mosaic only in the header band.
- **Badges:** a small square of 3×3 stones in a status colour.
- **Avatars/logos:** render through the tessellator with small stones (5–6px) so faces read.
- **Loading:** the wave border flowing clockwise around a frame.
- **Empty state:** a small *emblema* (a cat, an amphora) framed in red and gold.

## Motion

- **Colour flows through fixed stones:** the classic mosaic animation keeps stone positions fixed and moves the colour pattern across them (resample the source art each frame at stone centres). It only reads when the pattern moves much farther than one stone per frame.
- **Conveyor border:** for slow scrolling borders, bake a long strip whose stone layout repeats with the pattern period (source: 81px period, 11 rows, widths normalised to exactly 81px per period, 26 periods long) and translate the whole strip; stones travel with the pattern. Source pace: ~15px/s drift plus an occasional one-period ease-in-out jump.
- **Emblema sprites:** small moving props (under ~3 stones wide in the main layout) can't be read inside a fixed layout; render them as their own small-stone sprite (≈4.4px stones) and move the sprite over the mosaic.
- **Smoke and sparks:** drifting clouds and 10px square "spark stones" on parabolic paths for a lively hero.
- **Reduced motion:** a static mosaic; borders stop.

## Techniques

- Layout (simplified from Hausner 2001, *Simulating Decorative Mosaics*): edge map from the flat artwork (neighbour RGB difference > 34, plus region boundaries) → exact Euclidean distance transform → walk the iso-bands `|d − (k + ½)s| < 1.5`, placing a stone every ≥ s px oriented along the contour tangent → fill the rest with offset rows (row jitter ±5%, column ±9%), using only a minimum spacing check (0.8–0.86 × stone size) so narrow shapes still get stones.
- Make stones 8–28% *larger* than half the spacing and let the bisector cut them; undersized stones leave ~40% grout and look sparse.
- Stone sizes by role (source): wall 12px, halo 11px, figures 10px, faces 5–6px, hair 8px, borders 7.6px, letters 7px.
- Making a figure read: face stones ~5px; eyes as white plus ~3 dark stones; mouth as a red oval about 2 stones; heavier dark outline around the head.
- Store layout typed arrays (`Int32Array` IDs, `Uint8Array` shade); use `getContext('2d', {willReadFrequently: true})`. Write `putImageData` to an offscreen canvas and `drawImage` it, so CSS transforms and clipping still apply.

## Performance and accessibility

- Expensive: computing the layout (~200ms at 1080p) and per-pixel recolouring each frame. Cheap route for the web: precompute at build time and ship an image; animate only a baked border strip with `transform`; limit live recolouring to a small region, DPR ≤ 2, paused when hidden.
- Contrast: dark stone `#46322a` on wall `#e4ddcd` ≈ 8.9:1; cream on red `#a8322a` ≈ 5.9:1. Never put body text on the tessellated texture; grout lines cut through letterforms.
- Screen readers: mosaic lettering is an image; provide real text.

## Pitfalls

- Recolouring only for a slow pattern → movement smaller than a stone gets quantised away; move the stones with the pattern, or move far more than a stone.
- A region with smaller stones for detail → looks like a patch when still; keep one stone size per area.
- Thin-serif fonts tessellated → broken letters; heavy monoline strokes.
- `putImageData` straight to the visible canvas → ignores transforms and clips; go through an offscreen canvas.
- Placing grid stones only far from contours → narrow shapes (a 26px tail) get no stones; rely on spacing checks.
- Dynamic region too small → stones crossing its edge show half new, half old colour; pad it generously.

## Nearest neighbours

- **29 · Seurat pointillism:** also built from small units, but round dots of pure colour mixed optically with no grout. Mosaic uses square stones following contours, set in grout.
- **14 · 8-bit pixel art:** also a grid of square units, but a strict uniform grid with a tiny palette and no grout. Mosaic stones are irregular, bevelled and follow the shapes.

## Source

Adapted from huashu-art-motion · `04_roman.md` (built ★★★; noted shortcoming: faces lose detail at small stone sizes).
