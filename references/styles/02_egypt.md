# 02 · Egyptian tomb painting (New Kingdom, c. 1350 BCE)

**Status:** Built ★★★
**Layer:** Period specimen (strongest as tribute or easter egg)
**Web fit:** Medium — museum, travel, education and game pages, or a themed skin for a product with lots of structured lists (the register-and-column system maps well onto UI). Too ornamental for a daily-driver productivity tool unless the borders stay thin.

## Recognisable traits

- No perspective, no shading: flat opaque colour inside clean, even dark-brown outlines (2.2–2.8px), never boiling or sketchy.
- The canon: heads in profile with a frontal almond eye and kohl tail, shoulders shown frontally. Period feeling comes from these conventions, not from brushwork.
- Everything sits in registers: horizontal bands, ground lines, and vertical columns of hieroglyphs separated by red rules.
- Dense block borders in four colours (red, blue, yellow, green) separated by cream gaps and black seams.
- A pale plaster wall with soft mottling and fine hairline cracks running across everything.

## Palette

| Role | Hex | Note |
|---|---|---|
| Background (plaster) | `#eadcb8` | Mottled darker patches `#d8c597`. |
| Surface | `#f2ead6` | Source cream between border blocks; cards. |
| Text / outline | `#2a1810` | Source outline brown. |
| Muted text | `#6b5440` (suggested) | Brown, not grey. |
| Accent (red) | `#b8322a` | Column rules `#9a2b22`; sun disc `#c8372b`. |
| Accent 2 (blue) | `#2a58a8` | Lapis `#2c56b0`, turquoise `#3fa6a0`. |
| Yellow / green | `#e2b13a` / `#2e8a55` | Border blocks, status colours. |
| Gold | `#e0ab38` / dark `#a8741c` / light `#f3d27a` | Jewellery, highlights, "done". |
| Deep band | `#1c2236` | A dark register stripe; usable as a dark header. |
| Skin (ochre) | `#e6b282` | For illustrated figures. |

Relationship: saturated mineral colours used flat and side by side, always held apart by a cream gap or a dark seam; never blended, never gradiented. Dark reading: use the deep band `#1c2236` as ground with cream text and the same block colours, gold for accents.

## Typography

The style has no Latin lettering, so keep Latin type calm and let ornament carry the period. Headings: a wide, even-stroked face such as **Marcellus SC** or **Josefin Sans 600** (`"Marcellus SC", "Josefin Sans", Georgia, serif`), uppercase labels at +0.08em. Body: a neutral sans. Hieroglyphs as ornament only: **Noto Sans Egyptian Hieroglyphs** or custom SVG glyphs. Real hieroglyphs carry meaning; don't fake "translations" of your UI copy.

## Surfaces, texture and edges

- **Plaster:** a pre-rendered mottling tile multiplied over the whole page (`mix-blend-mode: multiply`), plus a fine grain tile and 4–6 random-walk hairline cracks baked into the same overlay, so they cross panels and figures like real wall ageing.
- **Block borders:** a repeating linear gradient or a small SVG pattern: blocks of red/blue/yellow/green, each separated by a cream gap and a 1–2px black seam. Use as `border-image` on the page frame and section headers.
- **Registers:** sections separated by a ground line (2–3px brown) and a thin red rule; columns separated by vertical red rules.
- **Edges:** crisp. No displacement, no roughness; the age lives in the plaster overlay, not the shapes.

## Shapes and layout

Straight edges and small or zero radii; outlines 2–3px. No drop shadows at all (there's no light source). Layout is a strict grid of horizontal registers; content reads left to right along ground lines, with vertical glyph columns as side rails. Figures and illustrations face the direction of reading. Leave the plaster visible between blocks; density comes from pattern, not from crowding.

## Components

- **Buttons:** flat blocks in red or blue with cream text and a brown 2px outline; pressed state darkens one step (no shadow).
- **Cards:** cream panel with a brown outline and a block-border top strip.
- **Navigation/sidebar:** a vertical column like a hieroglyph register: items separated by red rules; the active item gets a gold block.
- **Inputs:** cream with a brown outline; focus adds a 3px lapis outline.
- **Chat bubbles:** flat panels on ground lines; speaker identified by a small glyph (ankh, eye) rather than an avatar photo.
- **Tables:** registers: header row in the deep band colour, rows on ground lines, red vertical rules between columns.
- **Badges/icons:** single glyph forms (ankh, eye of Horus, scarab, lotus, reed) drawn in 10–20 strokes inside a ~50px box, round line caps, ~3px stroke.
- **Loading:** a scarab pushing a sun disc across a ground line.
- **Empty state:** a lotus or a single glyph column with one line of copy.

## Motion

- **Scarab rolls the sun:** constant speed across the line (source ≈ 10.7px per frame at 60fps, wrapping); ball rotation = distance ÷ radius; legs swap between two poses every 4 frames (`steps(1)`). A natural progress bar.
- **Aten rays with tiny hands:** rays from a red sun disc whose hand tips open and close (`0.5 + 0.5·sin(2π·t/0.4 + 0.9k)`), each ray slightly out of phase, tips wobbling ±3px. A good "thinking" indicator.
- **Water glyphs:** zigzags stepped at 12fps, 4 steps per loop, so they look hand-redrawn.
- **Glyphs written in:** characters appear stroke by stroke using `stroke-dasharray`/`stroke-dashoffset` (each subpath draws from its own start, which reads as real writing), ink starting bright gold `#f6c846` and settling to its colour with a short glow, ~0.36s each. Great for page entrance or a "done" moment.
- **Aten pulse:** rays lengthen ±9% in a wave across the fan, the disc gets an outer glow ring; never tint the disc itself.
- **Reduced motion:** static sun and scarab, glyphs shown fully drawn, rays fixed open.

## Techniques

- Glyph sprite: draw each glyph as an SVG symbol with round caps and its own default colour; place them in columns with a seeded layout. Rules from the source: no glyph twice in a row in one column, the same glyph at most 3 times per column, down-weight busy ones (scarab, water) by ~60%. Without these rules the columns read as a sticker sheet.
- Scale glyphs to column width ÷ 78 × (1.0–1.14), gaps 6–11px, for a hand-set rhythm.
- Pleated linen for illustrations: fill skin first, then white at α0.9 so a trace of skin shows through; fine fan-shaped pleat lines at α0.5.
- Making a mascot or illustration fit: profile head, frontal almond eye with kohl tail, both shoulders visible; limbs wrapped inside the silhouette rather than as separate tubes; patterned collars as concentric bands. Braided hair as long strips with alternating diagonal ticks (herringbone), not beads.

## Performance and accessibility

- Cheap: everything static is SVG or a baked image; motion is a few small SVG groups. The plaster multiply overlay is a single fixed layer; don't animate it.
- Contrast: brown `#2a1810` on plaster `#eadcb8` ≈ 12.5:1. Cream on blue `#2a58a8` ≈ 5.7:1 and on red `#b8322a` ≈ 5:1 (OK for buttons). Gold on plaster fails; use gold only as fill or on the deep band (`#e0ab38` on `#1c2236` ≈ 7.5:1).
- Busy borders compete with content; keep them to the page frame and section heads.

## Pitfalls

- Glyph columns with no repetition rules → wall of scarab stickers; limit repeats per column.
- Thin wavy motifs (ray wiggles) as the only motion → nobody notices; give one medium-sized element real movement (a glyph being written, the scarab).
- Translucent glow laid over the red sun → it turns orange; glow outside the disc, then redraw the disc.
- Hands on rays clipped by their container → clamp the tip positions inside the frame.
- Beaded hair or tube legs on illustrations → caviar and bowling pins; use strips and enclosed silhouettes.
- Line "boil" or hand jitter borrowed from other styles → it stops looking Egyptian; outlines stay clean.

## Nearest neighbours

- **03 · Greek black-figure:** also profile figures and borders, but black silhouettes with incised lines on terracotta and almost no colour. Egypt is full flat colour with outlines on pale plaster.
- **05 · Gothic manuscript:** also ornate frames and script, but gold leaf, vellum and pointed arches with vines. Egypt is matte mineral colour in strict registers.

## Source

Adapted from huashu-art-motion · `02_egypt.md` (built ★★★; noted shortcoming: the cup-holding elbow of the figure was stiff).
