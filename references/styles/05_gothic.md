# 05 · Gothic illuminated manuscript (Western Europe, c. 1290)

**Status:** Built ★★★
**Layer:** Period specimen (strongest as tribute or easter egg)
**Web fit:** Medium — reading apps, literary or fantasy products, games, invitations, a "story mode" or a premium/celebration screen. The gold sweep makes a beautiful success moment. Too ornate for everyday dense UI; blackletter fails as body text.

## Recognisable traits

- Burnished gold leaf: fine horizontal brushed grain, a tooled diamond lattice, faint seams between leaf squares, and a highlight that glints across it as light moves.
- Vellum pages in warm cream, with blackletter text and red rubrics (opening words in vermilion).
- Gothic architecture as frames: pointed arches, gables, pinnacles, columns, stained-glass windows.
- Marginalia: ivy vines with three-lobed leaves in blue, red and gold, gold bezants with tendrils, and playful drolleries (snails, mice, a sly cat).
- Flat, saturated colour areas (ultramarine, vermilion) with thin dark outlines; halos as tooled gold discs.

## Palette

| Role | Hex | Note |
|---|---|---|
| Background (vellum) | `#ecdfc2` | Source parchment (236,223,194). |
| Surface | `#f4ead2` (suggested) | Lighter page for content panels. |
| Text (ink) | `#2a1c10` (suggested) | Iron-gall brown-black. |
| Muted text | `#5e4a34` (suggested) | |
| Accent (vermilion rubric) | `#b8322a` (suggested) | Opening words, primary actions. |
| Accent 2 (ultramarine) | `#2b4a9a` (suggested) | Cloaks, links, secondary panels. |
| Gold | `#d9a63a` / low `#9c6a1a` / high `#f8de86` | Source leaf colours; fills and frames only. |

Relationship: gold, ultramarine and vermilion on cream vellum, each colour flat and outlined; gold is the light source of the page. Dark reading: a deep ultramarine or near-black page with gold frames and cream text works well (gold `#d9a63a` on dark ink ≈ 7.4:1).

## Typography

Display: **UnifrakturMaguntia** (source used it at 44px) for titles and drop caps only (`"UnifrakturMaguntia", "Old English Text MT", serif`). Body: a readable book serif such as **EB Garamond** or **Cormorant Garamond 500** at 17–19px. Rubrics: the first word or two of a section in vermilion. Drop caps: a gold square with a coloured initial and vine tendrils. Watch line length: blackletter at 50px+ overflows quickly.

## Surfaces, texture and edges

- **Gold leaf tile:** pre-render a tile: brightness `0.52 + fbm·0.42 + horizontal brushed noise (y-frequency ≥ 20× x-frequency)·0.5 + light from the upper left + ±0.05 grain`, mapped between gold-low, gold and gold-high. Overlay a diamond lattice at 46px spacing: each line drawn twice, dark and then light offset 1.2px, so it looks tooled in; 4 dots in each diamond centre (lit top left, dark bottom right). Add faint seams every ~118×125px for individual leaves.
- **Vellum:** cream with a subtle fbm mottling tile and fine grain; slightly darker toward the page edges.
- **Frames:** pointed arch or gable shapes as SVG paths filled with the gold tile, outlined in dark brown.
- **Edges:** clean, outlined; age comes from the vellum, not from rough edges.

## Shapes and layout

Pointed arches (`clip-path` or SVG) for hero frames and image masks; columns as vertical rails. Thin dark outlines (1.5–2px). No drop shadows; gold carries the depth. Layout like a manuscript page: a text column (or two) in a framed block, generous margins filled with vines and small drolleries, a gold-ground miniature at the top. Margins are where personality goes; keep the text block calm.

## Components

- **Buttons:** vermilion or ultramarine with a thin gold border; primary actions can have a gold-leaf fill with dark text.
- **Cards:** vellum panel with a thin gold rule and a small drop cap.
- **Navigation:** a column of items with a vine running down the side; active item gets a gold bezant.
- **Inputs:** vellum with a dark bottom rule; focus adds a gold rule.
- **Chat bubbles:** vellum leaves; the speaker's first word as a rubric.
- **Tables:** ruled like a manuscript: thin red rules, ink text.
- **Badges:** gold bezants (discs) with a tendril; stars as four-point glints.
- **Loading:** a snail crawling along the bottom margin, or a gold glint sweeping a frame.
- **Empty state:** a drollery (a cat holding a mouse by the tail) in a margin with one line of copy.

## Motion

- **Gold glint sweep:** a diagonal highlight band (slope ≈ 0.45, "/") crossing every gold surface at constant speed, added with `lighter`/`plus-lighter` at α0.62–0.85. Four-point star glints pop at points the band passes. Constant speed matters: with ease-in-out the band left the gold before reaching the main subject.
- **Vines:** stems sway `sin(2.1t + 0.09i)·2.5px` (×2 for more life), leaves rock around their stems by `sin(3.1t + 1.7k)·0.28–0.42rad`. Use leaf scale 1.05–1.45; smaller looked sparse.
- **Stained glass:** 26–40 points flicker on when `sin(5t + h) > 0.6`.
- **Drolleries:** a snail crawls ~62px/s with wobbling antennae; a mouse dashes across a margin; a dangling mouse swings `0.32·sin(5.5t)`.
- **Done moment:** a gold sweep over the completed item plus one star glint.
- **Reduced motion:** static gold with a fixed highlight, still vines, no glints.

## Techniques

- **Gold mask without redrawing shapes:** derive the mask from colour: `goldness = clamp((r − b − 70)/50)·clamp((g − b − 30)/40)·clamp((r − 140)/40)`, used as alpha. Vellum (r − b ≈ 42) and pink areas fall out automatically, so every gold element gets the sweep.
- **CSS version:** give gold elements a shared class; on each, layer `background: linear-gradient(115deg, transparent 40%, rgba(255,240,190,.75) 50%, transparent 60%) , url(gold-tile.png)` and animate `background-position` of the first layer. Or one fixed sweep layer masked with `mask-image` of the gold artwork.
- Ivy leaf: three lobes, alternating blue/red/gold every 3–5 points along a vine; some stops end in a gold ball with 3 tendrils.
- Making a mascot fit: give it the drollery treatment: a flat body, a cream human-ish face with almond eyes glancing sideways, raised brows, rosy cheeks. Mischief is part of the genre.

## Performance and accessibility

- Expensive: per-pixel gold shading at runtime. Cheap: ship the gold tile; animate the sweep via `background-position` or a transformed overlay; vines as SVG with CSS transforms. Cap glint count.
- Contrast: ink on vellum ≈ 12.5:1; ultramarine on vellum ≈ 6.2:1; vermilion `#b8322a` on vellum ≈ 4.5:1 (just AA; keep rubrics large or bold). Gold on vellum ≈ 1.7:1: never use gold for text on the light page.
- Blackletter is hard to read for many people, including dyslexic readers; restrict it to a few display words and give headings real text with `aria-label` if letterforms are decorative.

## Pitfalls

- Ease-in-out on the glint → it misses the main subject; move it at constant speed.
- Leaves too small (×0.85) → sparse, stingy margins; scale up.
- Blackletter body text → unreadable; book serif for body.
- Gold text on vellum → invisible; gold for fills, frames and dark-mode text only.
- Hair drawn as a gold mass around a head → reads as a hood; separate strands and outline.

## Nearest neighbours

- **18 · Klimt golden period:** reuses the same gold-leaf method, but with spiral and eye patterns on robes and realistic faces, Art Nouveau not Gothic. Gothic has vellum, blackletter, arches and marginal vines.
- **51 · Persian miniature:** also gold, flat colour and precise narrative pages, but layered high-horizon spaces, floral carpets and turquoise/lapis palettes without blackletter or pointed arches.

## Source

Adapted from huashu-art-motion · `05_gothic.md` (built ★★★; noted shortcoming: the figure's blond hair read like a hood).
