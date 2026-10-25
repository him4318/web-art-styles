# 42 · Guardi (Francesco Guardi, Venetian views and capriccios, 18th century)

**Status:** Research recipe — reworked once against two Venetian views (self-score ~78, not independently reviewed; below the catalogue's 80 bar). The broken-stroke treatment was tested in one scene; transfer to other subjects is untested.
**Layer:** Material (works on any subject) — air, broken light and loose edges can sit on cities, interiors or industrial spaces.
**Web fit:** Medium — travel, hospitality, architecture, wine/food and editorial pages that want atmosphere and a sense of place; wide hero panoramas. Weak for dense product UI: the style is about vast air with tiny details, the opposite of information density.

## Recognisable traits

- A low horizon and a huge silver-grey-green sky: air takes up most of the picture.
- Quick, dry, short strokes; broken, flickering contours; structure still readable.
- Tiny figures reduced to a dark body with one or two dots of white and red.
- Sparkling white flecks catching light on water, sails and façades.
- Different stroke directions for architecture, water and sky — never one uniform grain over everything.

## Palette

| Role | Hex | Note |
|---|---|---|
| Background (bright sky) | `#e9e4d6` (suggested) | Warm silvery light. |
| Surface / panel | `#f3efe5` (suggested) | Cards. |
| Text | `#2f2e28` (suggested) | ~10.7:1 on the sky ground. |
| Muted text | `#5f6157` (suggested) | ~5:1. |
| Silver-grey | `#c9c8b8` (suggested) | Large atmospheric fields, borders. |
| Grey-green (water/air) | `#8f9a8a` (suggested) | Secondary fills, water. |
| Accent (figure red) | `#a8402f` (suggested) | The rare dot of colour: primary CTA, alerts. |
| Highlight flecks | `#f5f1e6` (suggested) | Sparkles; decoration only. |
| Dark (boats, figures) | `#2b2a25` (suggested) | Tiny silhouettes, icons. |

All suggested from the source's signature ("silver-grey-green air, warm bright and cool dark clouds"). Relationship: low-saturation silver greys everywhere, local bright colour in tiny doses. Dark reading: a dusk lagoon (`#3a3d38` ground, `#e9e4d6` text) is plausible but untested; the style is best light.

## Typography

Elegant 18th-century Venetian flavour: **Cormorant Garamond Italic** or **Bodoni Moda** for display (thin, airy); body in **Libre Caslon Text** or a light humanist sans (`"Libre Caslon Text", "Source Sans 3", Georgia, serif`). Small labels in letter-spaced uppercase (+0.12em) in grey-green. Keep weights light to match the air — but body text at least 400.

## Surfaces, texture and edges

- **Broken strokes ("broken" medium, source):** each region (sky, buildings, water) rendered with thin, elongated, dry, low-saturation strokes in its own direction — sky: wide, horizontal, warm-light/cool-dark; buildings: short vertical; water: long horizontal sky-light streaks plus reflections. Pre-render a hero image or per-region texture tiles.
- **Loose edges:** card and image edges can dissolve slightly — a `mask-image` with a noisy edge (SVG turbulence-displaced mask), only on hero imagery, never on text panels.
- **White flecks:** a sparse layer of tiny bright dots on water and façades.
- **Texture bound to the world:** stroke textures belong to the object/region and move with it; never re-randomise the whole screen.

## Shapes and layout

Panoramic: very wide hero images with a low horizon (sky 65–75% of the height); content can sit in the "sky". Corners square, no shadows; separation by air (generous spacing) and soft silver tones. Small, dark accents (boats, figures) placed along the horizon serve as visual anchors — in UI, small dark icons along a baseline.

## Components

- **Buttons:** quiet silver-grey outlines with dark text; the primary button is the one red "figure" accent.
- **Cards:** pale panels with almost no border; headings in airy italic.
- **Navigation/sidebar:** a thin horizontal bar floating in the sky; active item marked by a tiny red dot.
- **Inputs:** bottom-border only, in grey-green.
- **Chat bubbles:** soft pale panels; avatars as tiny dark figure silhouettes with a white dot.
- **Tables:** light rules; fine for small tables.
- **Badges/tags:** small dark dots with a white or red fleck.
- **Illustrations:** long thin boats, small black figures with white/red dots, towers and domes with loose broken edges.
- **Loading:** a few white flecks shimmering on a horizontal water line.
- **Empty state:** a wide empty lagoon/sky with a tiny boat.

## Motion

- Clouds drift slowly; water shifts row by row (horizontal per-line displacement); birds cross; boats glide with their stretched, darker reflections.
- Flecks twinkle irregularly (the missing signature in the prototype).
- Smooth and slow for ambience (clouds 5–10px/s, suggested); UI interactions 200–400ms ease-out.
- Done moment: a burst of a few white flecks along the item's baseline.
- `prefers-reduced-motion`: static panorama, no shimmer, no drift.

## Techniques

- **Row displacement for water:** an SVG `feTurbulence` (low y frequency) + `feDisplacementMap` on the water area only, animating `baseFrequency` slowly — or, cheaper, a few horizontally shifting strips of a pre-rendered water texture.
- **Reflections:** redraw the boat/figure into a small canvas, flip, stretch vertically, darken, and offset per row.
- **Per-region strokes:** keep separate tiles for sky, architecture and water so their directions differ.

## Performance and accessibility

- Cheap: pre-rendered panoramas, strip-scrolled water, a sparse fleck layer. Expensive: live displacement filters over the full width — limit to a band and pause when off-screen or the tab is hidden.
- Contrast: the low-contrast silver palette is the risk. Keep body text dark (`#2f2e28`) on solid pale panels; never on the stroked sky directly.
- Twinkling flecks can bother some users — few, slow, and off under reduced motion.

## Pitfalls

- Image searches mixing in modern photos of Venice (happened in the source) → anchor on actual paintings.
- Tower strokes stacking like bricks (noted) → looser, more broken, varied lengths.
- Figures as plain silhouettes only (noted) → add the white and red dots.
- One grain over everything → different stroke direction per material.
- Re-randomised texture each frame → bind texture to objects.
- Requiring water, gondolas or ruins → not necessary; the source suggests stations, hangars and markets.

## Nearest neighbours

- **47 · Sisley:** also air and weather, but Sisley is blue-green daylight Impressionism with full colour; Guardi is silvery, dry, flickering and nearly monochrome with dots of red and white.
- **28 · Monet:** also light on water, but Monet's strokes are thick, saturated and fill the picture; Guardi's are thin, dry and leave vast quiet air.

## Source

Adapted from huashu-art-motion · `42_guardi.md` (research recipe; v2 self-score ~78, not independently reviewed; shortcomings: tower strokes like stacked bricks, figures only silhouettes, missing Guardi's flickering white flecks).
