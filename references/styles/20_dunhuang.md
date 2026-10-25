# 20 · Dunhuang murals (Mogao Caves, High Tang, 8th century)

**Status:** Built ★★
**Layer:** Period specimen (strongest as tribute or easter egg)
**Web fit:** Medium for cultural, museum, tea, travel and heritage sites, festival campaigns and premium seasonal themes; Low for neutral product UI unless reduced to its palette, line and border ornaments.

## Recognisable traits

- An earth-red plaster ground with mineral colours laid flat on top: malachite green, azurite blue, white, a little gold.
- "Iron-wire" line: an even-width, unbroken dark red-brown contour around every figure (source: 2.6px at 1920 wide).
- Flying apsaras (feitian): horizontal figures with long ribbons streaming in great arcs; ribbons are the signature shape.
- Scrolling floral bands (juancao) and triangular valances in alternating green, blue, white and red along the top of walls; lotus tiles on floors.
- Age: faded saturation, flaked patches exposing pale plaster, cracks and smoke-darkening near the top.

## Palette

| Role | Hex | Note |
|---|---|---|
| Background (earth red) | `#a5452f`, dark `#7a3020` | Source plaster ground. |
| Surface (white ground) | `#ecdfc4` | Source mineral white; cards and text panels. |
| Exposed plaster | `#d8c6a2` | Source flaked-area colour; a quieter surface. |
| Text / line | `#5a2414` | Source iron-wire line; faces use a redder `#8a3a24`. |
| Malachite green | `#3f8f6e` / `#6ab08e` | Ribbons, success. |
| Azurite blue | `#2f5f8f` / `#5a86b0` | Sky, links. |
| Gold | `#d8b05a` | Beads, halos, small highlights. |

Relationship: warm earth red and cool mineral green/blue in equal standing, outlined in red-brown, all slightly faded. No pure black, no neon. Light reading: white or plaster panels on an earth-red frame. Dark reading: the dark earth `#7a3020` page with white panels.

## Typography

Headings: a serif with brushed, carved character, suggested **Noto Serif SC/TC** (CJK) and **Cormorant** for Latin (`"Cormorant", "Noto Serif SC", Georgia, serif`), medium weight, wide tracking for small caps. Body: a calm serif or humanist sans in line-brown on white plaster. Section labels can sit inside small cartouches (the inscription panels of the murals).

## Surfaces, texture and edges

- **Plaster ground:** earth-red fill plus a fine noise tile (`feTurbulence` multiplied), with a few faded rosettes scattered sparsely. Pre-render as a tile.
- **Ageing overlay (pre-render once, never live):** desaturate the composed art (`saturation` blend at α .3), multiply a pale earth texture at α .5, then flake: where `fbm(0.0032x) + 0.12·noise(0.03x) > 0.3` show plaster `#d8c6a2`, with a darker line where the value is between 0.28 and 0.3 (the flaked edge). Add ~500 random-walk cracks and a smoke gradient over the top 380px.
- **Protected zones:** never flake faces or key content. On the web, keep flaking to decorative frames, the page border and illustration backgrounds; panels with text stay intact.
- **Iron-wire outlines:** constant 2–3px strokes in `#5a2414` (SVG `stroke-width` with `vector-effect: non-scaling-stroke`).
- **Border bands:** a scrolling-floral band and a row of triangular valance flaps (alternating green, blue, white, red, with gold beads) along the top of the page or header: an SVG pattern repeated horizontally.

## Shapes and layout

Flat, outlined shapes; no shadows. Frames and bands organise the wall: a header valance, side scroll bands, lotus-tile footers. Content sits in white or plaster cartouches with a red-brown line and a small corner motif. Horizontal movement: ribbons and apsaras cross the page laterally, so lay out hero art in wide horizontal bands.

## Components

- **Buttons:** malachite or azurite fill with white text and a red-brown outline; or a white button with a line-brown border.
- **Cards:** white plaster cartouche, iron-wire border, a small floral scroll in one corner.
- **Navigation:** the valance band: items hang from a scrolling-floral rail; the active one is a gold-beaded flap.
- **Inputs:** white field, 2px line-brown border, focus with a green outline.
- **Tables:** white with line-brown rules; header row on pale plaster.
- **Badges:** small lotus medallions.
- **Loading:** a ribbon streaming (travelling wave), or a lamp flame flickering.
- **Success:** heavenly flowers (four-petal blossoms) spinning and drifting down.
- **Empty state:** an apsara gliding across an empty azurite sky.
- **Illustrations:** draw figures with the even line, flat mineral fills and Tang features (high hair bun with a hairpin and flowers, a forehead flower mark, a faint red stroke at the temple, small red lips). A halo should be translucent (α .28) with a fine line.

## Motion

- **Ribbons (pibo):** a travelling-wave point list rendered as a ribbon: `x = back + 230q + 64q·sin(4.2q − 5.5t)`, the last 30% lifting upward. Horizontal reach must be large (200px+) or it reads as a stick.
- **Apsara pass:** a horizontal figure crosses at ~300px/s with three ribbons in travelling waves, one arcing from the shoulder over the head and behind; that big arc is the most recognisable shape.
- **Auspicious clouds:** round puffs with tapering tails, drifting 40–76px/s.
- **Heavenly flowers:** ~30 four-petal flowers rotating and flipping as they fall.
- **Lamp flame:** scale `1 + 0.12·sin(17t) + 0.08·sin(29t)` with a soft glow.
- **Paint flakes:** a few pieces detach from a flaked edge, flip and fall in a parabola. A good "delete" or "archive" micro-interaction.
- Easing: smooth, floating sines. Under `prefers-reduced-motion`: ribbons and clouds static, no falling flowers or flakes.

## Techniques

- Ribbon: compute points along a travelling wave, offset ±w/2 along normals to build a polygon (SVG path regenerated per frame for a hero, or a short pre-rendered loop); add white dashed patterning and red dots along it.
- Pleated skirts (source): red ground with diagonal green trapezoid stripes every 44px, diverging downward. A good `repeating-linear-gradient` panel border.
- Layer order for an illustration: plaster → valance and bands → sky and clouds → figures → ageing overlay, with faces protected.

## Performance and accessibility

- Expensive: per-pixel ageing and flaking at runtime, many animated ribbons. Cheap: pre-rendered aged art and tiles, one SVG ribbon animated with a short loop, CSS transforms for falling flowers.
- Contrast: line-brown on white 9.4:1 and on plaster 7.4:1; white on earth red 4.6:1 (AA for body, just), on dark earth 7.0:1, on azurite 5.0:1. White on malachite `#3f8f6e` is only 3.0:1: use dark text on green or the deep line colour.
- Flaked and textured areas behind text hurt legibility; keep copy on intact plaster.

## Pitfalls

- Flaking noise too fine (frequency .006) → sesame-seed speckle that looks like dirt; lower to .0032 and raise the threshold for patch-like loss.
- Flaking across faces or content → protect them; age only the background.
- The temple red mark at full strength → looks like a scar; drop to α .32.
- A solid green halo → reads as a swim ring; make it translucent with a fine line.
- Ribbons with small sideways motion, nearly vertical → look like chair legs; give them 200px+ of lateral sweep and an upturned tail.
- Warm colours muddying after desaturation (an ochre cat turned brown-yellow) → start brighter (`#e08a3a` instead of `#c8843a`).
- Only one thing moving → add a second gentle motion (a turning head, stronger breathing) so the scene doesn't feel frozen.

## Nearest neighbours

- **55 · Dunhuang, Northern dynasties (research):** same caves, earlier period: drier, darker, with bolder flying lines and more abstracted figures. This card is the fuller, more colourful High Tang look.
- **02 · Egyptian tomb painting:** also flat colour, even outlines and ancient plaster, but strict profile figures, hieroglyph registers and a sand/ochre palette. Dunhuang has flowing ribbons, curving motion, mineral green and blue on earth red.

## Source

Adapted from huashu-art-motion · `20_dunhuang.md` (built ★★, period specimen; noted shortcoming: the apsara figures were crudely shaped).
