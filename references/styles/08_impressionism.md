# 08 · Impressionism (Claude Monet and circle, 1870s)

**Status:** Built ★★
**Layer:** Material (works on any subject)
**Web fit:** Medium — lifestyle, food, travel, wellness, gardening and editorial pages; soft hero illustrations and backgrounds. Weak for data-dense UI: short brushstrokes break up small shapes (faces, icons, text) unless those stay outside the painted layer.

## Recognisable traits

- Short, visible brushstrokes of broken colour laid side by side; light grounds showing between strokes.
- Colour is light: pinks, lilacs, warm yellows and greens, with violet shadows instead of black. No dark outlines.
- Dappled sunlight: patches of warm light that shimmer and drift on floors and tables.
- Stroke direction follows surfaces: diagonal on sunny walls, near-horizontal on floors and water, vertical on curtains, radiating from plants.
- Everyday leisure subjects (gardens, poppy fields, parasols) seen in a fleeting moment.

## Palette

| Role | Hex | Note |
|---|---|---|
| Background | `#f2e6ee` | Source canvas ground showing between strokes. |
| Wall tones | `#d8bede` → `#f0c8cc` → `#f2c4d4` | Pink-lilac wall; sunlit zone in cream yellow. |
| Surface (suggested) | `#fbf4f2` | Calm panel for text, a lighter version of the ground. |
| Text | `#3a2c2e` (suggested) | Warm plum-brown, never pure black. |
| Muted text | `#6a5560` (suggested) | |
| Accent (poppy red) | `#d2463a` (suggested) | Poppies were left unbrushed in the source to stay vivid. |
| Accent 2 (lilac/violet) | `#8a76b8` (suggested) | Shadows, links. |
| Sunlight | `#fff2c4` (suggested) | Dappled light patches. |

Relationship: each region has its own swatch of 7–9 related hues (walls pink-lilac-yellow, floor rose-orange-violet, sky, grass, curtain lilac, cloth blue-violet), mixed 30–45% with the underlying base colour. Shadows go violet, highlights go cream. A dark reading is unnatural for the style; if needed, use a dusk version (deep violet ground, warm lamp strokes) and keep text panels opaque.

## Typography

Headings: a light, graceful serif such as **Cormorant Garamond 500** or **Lora italic** (`"Cormorant Garamond", "Lora", Georgia, serif`). Body: a soft humanist sans (e.g. Nunito Sans) or Lora at 17px, dark plum-brown. A handwritten face (Caveat) for one signature-like word at most. Never put text directly on brushwork.

## Surfaces, texture and edges

- **Painted backgrounds:** render once to a canvas or image: a flat base painting, then a grid of strokes (source main pass: cell 11px, length 20px, width 7px, no outline, colour jitter 26) whose direction comes from a per-region flow field and whose colour is sampled from the base and pulled toward that region's swatch.
- **Second, finer pass for detail:** small or important areas (a picture within the picture, a distant house, a figure) get cell 6, length 11, width 4.5. One coarse pass smears them away.
- **Panels:** opaque or near-opaque calm surfaces (`#fbf4f2`) with soft, irregular edges: a brushy mask (`mask-image` of a few strokes) on the panel border, not on its content.
- **Edges:** no outlines anywhere; shapes end where colour changes.

## Shapes and layout

Soft, rounded, imprecise shapes; radius 14–24px or brushy masked edges. Shadows are soft and tinted violet (`0 16px 40px -20px rgba(90,60,120,.35)`), never grey. Layout is airy, with a painted hero (a window view, a garden) and calm panels floating over it. Let one big painted field carry the mood and keep the UI on clean ground.

## Components

- **Buttons:** soft-edged pills in lilac or poppy red with white or plum text; hover adds a warm sunlight tint.
- **Cards:** pale panels with violet-tinted shadow; optional brushy top edge.
- **Navigation:** pale translucent bar over the painted hero (with a solid fallback); active item marked by a short horizontal brushstroke underline.
- **Inputs:** pale fields with a lilac bottom stroke; focus brightens it to poppy.
- **Chat bubbles:** user in soft lilac, replies in cream; a few dabs of colour in the corner.
- **Tables:** light plum hairlines on cream.
- **Badges:** a single dab of colour behind the label.
- **Icons:** soft filled shapes in the swatch colours; no hard outlines.
- **Loading:** sunlight patches brightening in turn, or a row of poppies nodding.
- **Empty state:** a small painted vignette (a parasol on a hill) with one line of copy.

## Motion

- **Dappled sunlight:** 20–30 light patches, each made of ~40 short pale strokes, with irregular brightness `0.5 + 0.5·sin(t·sp + ph)·sin(0.53·t·sp + 2ph)`, clamped, frequencies 5–11, drifting sideways ±10–44px. Draw them normally (source-over at α0.95); `screen` at α0.75 was invisible on pink.
- **Poppies:** stems fixed at the base, heads swaying `sin(3.2t + ph + 0.02x)·(6 + 0.6r)`, amplitude 10+.
- **Clouds** drift ~45px/s; curtains and plants sway gently; strokes in a garden view turn ±0.22rad with the wind.
- **Wall shimmer:** a few dozen bright diagonal strokes in the sun zone blinking at ~6fps.
- **Done moment:** a burst of sunlight dabs across the item.
- Keep the painted layer itself still; motion comes from local elements only.
- **Reduced motion:** sunlight patches fixed at mid brightness, no sway.

## Techniques

- **Stable strokes:** position every stroke deterministically per grid cell (hash of cell i, j) and consume a fixed number of random values per cell. If a palette lookup can skip a cell (returns null) and the random stream shifts, every later stroke reshuffles whenever anything moves, causing full-screen flicker.
- **Region masks:** keep a mask image of regions (wall, sunny wall, floor, sky, plants) to pick both the flow direction and the swatch. Protect pure whites (window frames: colour above ~(240,236,225)) and vivid accents (poppy red) by returning the original colour.
- **Water and reflections:** use horizontal strokes; a diagonal pass turns a sunset reflection into mush.
- **Making an illustration fit:** paint it flat first, then run fine strokes clipped inside it (`source-atop`); add only a few lilac lines for features. Any default dark outline colour in your illustration kit must be overridden (to hair colour, to fur colour), or dark seams appear.

## Performance and accessibility

- Expensive: full-screen stroke passes (~65–75ms per frame in the source); never run them per frame on the web. Draw once at load (or ship pre-rendered images at 1×/2×), cache, and animate only small overlays with transforms and opacity. Cap DPR at 2; pause when hidden.
- Contrast: plum `#3a2c2e` on the lilac wall `#d8bede` ≈ 7.8:1; white on lilac fails (≈ 1.7:1). Put text on calm panels, not on strokes; pastel buttons need dark text.
- Shimmering light patches can bother some users; keep them few and slow and stop them under reduced motion.

## Pitfalls

- Strokes reseeded every few frames → the whole page re-paints and strobes; fix stroke positions.
- One coarse pass over everything → small detail destroyed; a second fine pass for small areas.
- Window frames brushed with wall colour → frames vanish; protect whites.
- Faces and hands broken by strokes → keep features on a fine pass or paint them flatter.
- Screen-blended light on pastel ground → invisible; paint light opaquely.

## Nearest neighbours

- **28 · Monet water lilies:** the same painter, but focused on water surfaces, reflections and floating lilies with horizontal strokes. This card is the general broken-colour treatment for any subject.
- **09 · Van Gogh:** also visible strokes, but long, outlined, swirling strokes in intense blues and oranges. Impressionism uses short, unoutlined dabs in light pastels.
- **29 · Seurat:** systematic round dots of pure colour instead of loose directional strokes.

## Source

Adapted from huashu-art-motion · `08_impressionism.md` (built ★★; noted shortcoming: faces and hands get broken up by the strokes).
