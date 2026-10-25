# 03 · Greek black-figure pottery (Attic, c. 530 BCE)

**Status:** Built ★★★
**Layer:** Period specimen (strongest as tribute or easter egg)
**Web fit:** Medium — strong for editorial, museum, philosophy/education, food and wine brands, or a bold two-colour marketing page. The limited palette makes it easier to use for product UI than most period styles, but terracotta grounds are loud for long reading.

## Recognisable traits

- Three things, all required: **black-glaze silhouettes** (no outlines), **incised lines** scratched through the black to show the clay (folds, features, overlaps), and **added colour** (purple-red bands and small white dots).
- Terracotta ground with faint horizontal throwing marks from the potter's wheel.
- Bands of running ornament: meander (Greek key) on top, lotus-bud chains and ray patterns below, all rhythmically repeating.
- Profile figures, almond eyes drawn frontally, stylised curls as spirals.
- Hand-painted inscriptions with uneven letter heights and a small "o".

## Palette

| Role | Hex | Note |
|---|---|---|
| Background (clay) | `#dc6a2f` | Source terracotta. |
| Surface (vase panel) | `#e8803f` | Lighter reserved panel for content. |
| Surface (cream, suggested) | `#f3cfa4` | Source "reserved clay" white area; use for long text panels. |
| Text / silhouette | `#1b120d` | Warm near-black glaze. |
| Incised line | `#e8904c` | Light clay; text or rules on black. |
| Accent (added red) | `#8a2a37` | Purple-red bands, active states. |
| Added white | `#f6ead6` | Dots (r≈2, spaced 11–17px), tiny highlights. |
| Gold (hair) | `#f2bd48` | Optional, sparing. |

Relationship: black on orange, with red and white added only as small bands and dots. Never fill large areas with the added colours. Dark reading: invert to a black-glaze ground with incised-clay text `#e8904c` (≈7.5:1), which is effectively red-figure; nice for a dark mode.

## Typography

Inscriptions are monoline capitals with uneven heights (×0.9–1.06 per letter), a slightly wobbling baseline (±6%) and a small "o" at a quarter of the cap height. For headings: hand-set SVG letters or a monoline caps face such as **Marcellus SC** (`"Marcellus SC", "Cinzel", Georgia, serif`), letter-spacing +0.06em, with per-letter `translateY` jitter from a seed. Body: a neutral sans or a calm serif. Avoid faux-Greek "Lithos" style lettering for body copy.

## Surfaces, texture and edges

- **Clay ground:** a low-resolution noise tile (mottling ±30 levels) plus a horizontal band noise (high y-frequency) for throwing marks, upscaled; add a few hundred faint horizontal hairlines (150–1050px long, α0.03–0.08) and sparse dark/light specks. Bake once.
- **Cracks:** a handful of jittered lines (9px segments, ±2px) with a 1px offset pale edge beneath, which instantly gives the crack thickness.
- **Vignette:** radial, transparent centre to `rgba(30,6,0,.62)` at the edges.
- **Black panels:** solid `#1b120d`, no texture; detail is incised as clay-coloured 1.5–2px lines.
- **Frames:** 18px black border with a string of white dots (≈11.6px spacing) and a fine inner line.

## Shapes and layout

Silhouettes with crisp edges; no shadows, no gradients except an optional faint vertical gloss on a vase-shaped panel (α≈0.09). Content lives in horizontal friezes framed by ornament bands, like the zones of a vase; a hero can be one wide "panel" with a meander strip above and a ray band below. Spacing is rhythmic: decide one module (e.g. 60px meander period) and align bands to it.

## Components

- **Buttons:** black pill-free rectangles with incised-clay label; primary uses the purple-red with white dot trim.
- **Cards:** reserved-clay panel framed by a thin black line and a dotted white inner border.
- **Navigation:** a meander band as the top bar; active item marked by a red band under the label.
- **Inputs:** cream with a 2px black bottom line; focus adds a red band.
- **Chat bubbles:** user in black with clay-coloured text; replies on cream clay.
- **Tables:** black header band with incised text; rows separated by thin black rules.
- **Badges:** small red bands with white dots.
- **Icons:** solid black silhouettes (dolphin, ship, lyre, rosette) with clay incisions for inner detail.
- **Loading:** a meander strip sliding sideways, or a rosette rotating.
- **Empty state:** a single dolphin leaping over a wave line.

## Motion

- **Scrolling bands:** pre-render each ornament strip at width W + period and translate it by `-(offset mod period)`. Source: top meander drifts left (~48px/s), bottom lotus band drifts right (~30px/s), plus an occasional ease-in-out jump of one full period. Good for ambient headers and loaders.
- **Dolphins and boat:** dolphins swim ±9px along their heading and tilt ±0.12rad on a 0.85s cycle, phase-shifted by 1.3 each; a boat bobs ±4px and rocks ±0.025rad on a 0.9s cycle.
- **Hanging lyre:** swings from its nail by `0.09·sin(2π·t/0.8)` (±5°).
- **Rosette:** rotates at ~1.6rad/s, alternating direction.
- **Steam / small accents:** three thin "≀" squiggles.
- **Reduced motion:** stop the bands at offset 0 and freeze the swing; keep everything visible.

## Techniques

```js
// Meander unit (period 60px, 7px black line) as SVG polyline points; repeat along x
const MEANDER = [[4,46],[4,0],[50,0],[50,34],[18,34],[18,14],[36,14],[36,22]];
```

- Lotus band: two 38px units where every other bud is filled red, so the true period is 76px. Wrap the strip on 76, not 38, or the colours jump.
- Incised detail on a silhouette: clip to the silhouette and stroke clay-coloured lines. Where two black shapes overlap (an arm over a face), clip to the union and stroke the front shape's outline in clay; it's the only way to separate black from black.
- Stripes along a curved back: sample 12–14 points at equal arc length along the outline and draw tapered marks toward the body's centre; far more natural than hand-placed stripes.
- Making a mascot fit: the mascot must use the same black-figure method as everything else. A bright cartoon animal with big orange eyes breaks the look immediately; express its colours with a red saddle band, white added colour and clay incisions.

## Performance and accessibility

- Cheap: baked ground, pre-rendered ornament strips moved with `transform: translateX`, SVG silhouettes. Avoid regenerating noise at load on mobile; ship the tile.
- Contrast: black on terracotta `#dc6a2f` ≈ 5.4:1 and on the vase panel ≈ 6.7:1 (AA for body text, but tiring for long reads). Put long text on the cream `#f3cfa4`. White on purple-red ≈ 7.2:1. Clay incised lines on black ≈ 7.5:1. Purple-red on terracotta is only ≈ 3:1; never use it for text there.
- Continuous scrolling bands in peripheral vision can distract; slow them or stop after a few seconds.

## Pitfalls

- Scrolling a pattern by exactly one period → first and last frames are identical and it looks frozen; add a slow constant drift.
- Wrap period set to the half-unit on alternating patterns → colour jumps every unit; wrap on the full repeat.
- Black hand, black cup, black face overlapping → one blob; separate with incised outlines, shrink props, add a red rim line.
- Hair outlines scaled up → helmet heads; keep silhouettes slim.
- Gold lines at 2px → invisible at thumbnail size; 2.5px minimum at desktop scale.
- Two noise layers sharing one seed → identical patterns; seed them differently.

## Nearest neighbours

- **02 · Egyptian tomb painting:** profile figures and borders too, but full flat colour with dark outlines on pale plaster. Greek is black silhouette with no outlines on orange clay.
- **04 · Roman mosaic:** same classical motifs (wave scroll, borders), but built from stone tesserae with grout and full colour. Greek is smooth glaze and two colours.

## Source

Adapted from huashu-art-motion · `03_greek.md` (built ★★★; noted shortcoming: the skirt silhouette was a little thick).
