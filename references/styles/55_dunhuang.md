# 55 · Dunhuang murals, Northern dynasties (Mogao caves, c. 5th–6th century)

**Status:** Research recipe — v1 single-scene study (an apsara flying above, a white deer running below, two painted registers), self-scored ~72, not independently reviewed. Below 80; techniques are hypotheses.
**Layer:** Period specimen with a transferable material — the dry, cracked mineral-pigment wall works on any subject; flying figures and deer are period motifs.
**Web fit:** Medium — cultural, museum, heritage and travel pages, festival and editorial features, a tribute theme. Low for product UI: cracks, flaking and red grounds fight data and form fields.

**Not the same as 20 · Dunhuang.** Card 20 is an older built recipe (★★) of the **High Tang** murals: a brighter earth-red ground (`#a5452f`), lush lapis and malachite with gold, Tang costume, lotus-tile floors, and peeling limited to the background. This card is the **Northern dynasties** branch: two-register walls, a deeper red (`#92342d`) or a cream ground, white animals, saw-tooth mountains, horizontally flying apsaras, and an aged wall surface whose cracks run *through* the figures. Pick one period; never mix them into a generic "Dunhuang look".

## Recognisable traits

- A dry, old, cracked wall: mineral pigments on clay plaster with crackle, flaking that shows the layer underneath, salt bloom and grain — cracks cross the painted figures too.
- Ground colours: a whole wall of earth red (as in Cave 257's Deer King story) or a cream ground (as in Cave 285), often stacked in two registers separated by a multicolour band with pearl roundels.
- Flat white animals with one outline and almost no internal lines; mountains as rows of narrow, tall saw-tooth triangles, each in one colour with one darker side, red ground showing between them.
- Apsaras flying horizontally, head first, scarves streaming back, the lower body turning into one long skirt trailing behind; small faces, long fine eyes, a red flush on the cheeks; red-brown "iron-wire" lines.
- Comma-shaped blue-green "wind leaves" tumbling backward in flocks.
- A lozenge band at the top and a triangular valance band at the bottom.

## Palette

From the study's reference viewing (Caves 257, 285, 428; colours sampled from photos of the murals, not official).

| Role | Hex | Note |
|---|---|---|
| Background (cream register) | `#e6d9bf` / `#e8dcc5` | Page ground and reading surface. |
| Background (red register) | `#92342d` / dark `#7c2a24` | Hero and section bands; cream text on it ~6.1:1. |
| Surface (lead white) | `#efe6d2` | Cards; the white of animals. |
| Text (deep brown line) | `#3a1812` | ~11.4:1 on cream. |
| Line / muted text | `#6b2a1c` | Red-brown iron-wire line; ~7.6:1 on cream. |
| Accent (azurite-teal) | `#3e919c` / dark `#1f5f6a` | Accents; teal on cream only 2.6:1 — use the dark variant for text. |
| Accent 2 (malachite) | `#577f7a` | Wind leaves, secondary. |
| Ochre | `#c58a3e` / `#b89d7d` | Scarves, dotted spine lines. |
| Flush | `#df8d74` | Cheeks, small highlights. |
| Lapis (later caves) | `#35597c` | Rare accent. |
| Dark | `#241512` | Band edges; dark mode ground. |

Relationship: earth red and cream grounds, blue-green and white on top, outlined in red-brown — everything matte, chalky and slightly darkened by age. Note: aged lead white darkens and some skin turns grey-blue (Cave 428); the study didn't do this yet. Dark reading (suggested): smoke-darkened `#241512` ground, cream text, teal and ochre accents.

## Typography

Headings: **Noto Serif SC** (Chinese) and for Latin a calm inscriptional serif such as **Cormorant SC** or **Cinzel** — `"Cormorant SC", "Noto Serif SC", Georgia, serif`. Body: **Source Serif 4** or **Noto Sans SC** at 16–18px on cream. Don't use brush-calligraphy or "Chinese-style" novelty fonts; murals of this period have no such display lettering.

## Surfaces, texture and edges

- **Clay wall:** a noise texture (`#d9c6a6` with dark `#9a7a54`, coarse grain) multiplied over the whole page at ~0.42, pre-rendered as a tile.
- **Red ground:** its own mottled texture (`#92342d` with dark `#5a1d1a`), rough grain.
- **Ageing overlay (pre-rendered transparent PNG):** cracks as random walks of 10–32 segments, each 12–32px, turning ±0.45 rad, with a 12% chance of branching; flaking patches as irregular polygons revealing a lighter under-layer with a faint 1.4px dark edge; patches of salt bloom. Lay it over images and decorative bands — **not** over body text.
- **Soot:** a smoke-darkened gradient along the top of each register (`rgba(60,34,20,.26)` → 0 at 22%).
- **Edges:** shapes keep crisp painted outlines; the wall, not the shape, supplies the roughness. An SVG `feDisplacementMap` (scale ~6) plus a speckle mask can mimic pigment soaked into a rough wall.

## Shapes and layout

Horizontal registers are the layout: stack page sections as painted bands (cream above, red below) separated by a multicolour divider. The study's divider, top to bottom: dark 8px, white 14px, teal 20px, ochre 14px, white 12px, dark 8px — a ready-made section separator. Top of page: lozenge band; footer: triangular valance. No shadows, no radii; flat painted shapes with red-brown outlines (2–3px). Figures and motifs move horizontally across bands, giving pages a left-to-right flow.

## Components

- **Buttons:** cream lead-white with a red-brown outline and deep-brown text; on red grounds, teal-dark fill with cream text.
- **Cards:** cream panels with a thin red-brown outline and a narrow multicolour band on top; no cracks over the text.
- **Navigation:** a lozenge band strip with plain labels; active item marked by a pearl roundel.
- **Inputs:** cream field, red-brown 1.5px outline; focus = dark teal 2px ring.
- **Section dividers:** the six-stripe band with pearl dots.
- **Badges:** small comma-leaf shapes in teal, malachite, ochre.
- **Loading:** wind leaves tumbling left, or a scarf ribbon waving.
- **Empty state:** an empty red register with a few saw-tooth mountains and a small white deer.

## Motion

- **Wind leaves (source motif):** comma-shaped leaves drift left at 70–190px/s in speed layers (smaller, paler ones in front), rotating slightly; ~62% teal, the rest malachite or ochre.
- **Apsara float:** the figure bobs `sin(1.15t + φ)·12px` and rocks `−0.07 + sin(0.9t + φ)·0.03` rad; its scarves and skirt are ribbons whose points follow a travelling wave `sin(2.6t − 0.55i + φ)` with amplitude growing toward the tail.
- **Deer gallop:** two beats — stretch in the air, gather under — legs paired and offset; hoof dust puffs fade over a 1/1.6s loop.
- **Falling flowers and dust motes:** slow, sparse.
- Pre-render the wall and bands; only small motif elements move. Under `prefers-reduced-motion`, show motifs in a single pose and drop leaves and dust.

## Techniques

- Ribbon helper: a polyline of 9–13 points with a width that tapers from base to tip, filled, outlined in red-brown — reusable for scarves, skirts and loading indicators (animate the points with the travelling wave above).
- Saw-tooth mountains: rows of triangles 130–280px wide, each filled from a small palette (teal, white, dark, green), with one side darkened, overlapping by row; cache them.
- Ageing overlay is the last layer over imagery, so cracks run across figures as on the real walls; keep a cleaner version for text-bearing areas.

## Performance and accessibility

- Cheap: tiled textures, a pre-rendered ageing PNG, a few transformed motif elements. Expensive: generating cracks or noise live; full-screen multiply blends on large animated layers. Pause motion when hidden.
- Contrast: deep brown on cream ~11.4:1; cream on earth red ~6.1:1; azurite teal on cream ~2.6:1 (decorative only — use `#1f5f6a` for text). No text over crack and flake overlays.
- Red grounds are heavy; keep them to bands, not full reading areas.
- Cultural care: these are Buddhist cave paintings; use motifs respectfully and avoid trivialising sacred figures in jokey UI.

## Pitfalls

- Mixing Northern-dynasty and High-Tang features → choose one period (this card, or 20).
- Glossy gradients and cloud swirls → that's modern "guofeng" illustration, not a mural.
- Mountains as plain geometric triangles without overlapping rhythm (source shortcoming) → stack rows with overlap, as in Cave 257.
- Flat-bodied apsaras (source shortcoming) → give the torso a half-turned twist.
- Pristine bright colours → add the aged, darkened lead white and chalky matte finish.
- Ageing overlay across body text → keep a clean reading surface.

## Nearest neighbours

- **20 · Dunhuang, High Tang (built ★★):** gold, lush lapis/malachite, plump figures, Tang costume, peeling only in backgrounds. 55 is earlier, sparer, two-register, with white animals, saw-tooth mountains and cracks through everything.
- **51 · Persian miniature:** full-coverage pattern, precise lines, gold frames, pristine surfaces. Dunhuang is dry, old and cracked, with loose flying lines and mineral, not gilded, colour.
- **38 · Giotto:** fresco with lime-white bloom and solid figures in architectural space; Dunhuang is dry-wall mineral paint with flat figures in horizontal friezes.

## Source

Adapted from huashu-art-motion · `55_dunhuang.md` (research recipe v1, self ~72, not independently reviewed; known gaps: mountains too geometric, flat apsara torsos, no lead-white darkening, High Tang branch not done; untested across subjects and in portrait layouts).
