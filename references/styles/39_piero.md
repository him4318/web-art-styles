# 39 · Piero della Francesca (Piero della Francesca, mid-15th century)

**Status:** Research recipe — not yet reworked against original paintings; the only prototype scored about 35. Treat this card as a set of design hypotheses drawn from the source's research notes, with suggested values throughout.
**Layer:** Material (works on any subject) — the source frames it as geometric order and clear volume, which transfers to non-figurative subjects.
**Web fit:** Medium — calm editorial and marketing pages, architecture, product launches, institutional sites that want serenity and order. It suits structured product UI better than most painterly styles (it *is* about measure and grids), but its pale, low-contrast palette needs care for text.

## Recognisable traits

- Big, simple forms first: heads, drapery and objects read as complete, clean volumes (cylinders, cones, blocks).
- Proportion, spacing, perspective and negative space constrain each other; order without forced central symmetry.
- Clear, even daylight with restrained warm/cool shifts; shadows are soft and transparent, never dramatic.
- Clean edges between major forms; detail is subordinate to the overall measure, with few purposeless marks.
- Stillness and poise — but the source warns that "serene" must not force slow motion.

## Palette

| Role | Hex | Note |
|---|---|---|
| Background (chalky white) | `#f1ebdf` (suggested) | Pale lime/limestone. |
| Surface / panel | `#f8f4ec` (suggested) | Lighter, cleaner panel. |
| Text | `#3a332b` (suggested) | Warm dark umber; ~10.5:1 on background. |
| Muted text | `#6b6255` (suggested) | ~5:1 on background. |
| Accent (pale sky blue) | `#b9cdd6` (suggested) | Large calm fields, selected rows. Not for text. |
| Accent 2 (deeper blue) | `#4b6a8f` (suggested) | Buttons, links (check: white text on it passes AA). |
| Rose | `#c98f86` (suggested) | Secondary accent, highlights. |
| Sage / grey-green | `#8e9b84` (suggested) | Success, landscape. |

All values are suggested; the source says saturation varies by work and medium (tempera vs oil) and warns against a fixed "cream filter". Relationship: light, clear, harmonious values; colour areas are distinct but never loud. Dark reading: not native; if needed, a dusk version with `#2b2924` ground and the same pale accents as fills, but the style is best light.

## Typography

Geometric and classical at once: **Cinzel** or **Cormorant Garamond** (upright, wide caps) for display; body in a clean humanist serif like **Source Serif 4** or a geometric sans like **Jost** (`"Jost", "Source Serif 4", system-ui, sans-serif`). Use a modular type scale (e.g. 1.25 ratio) — the measure is the style. Labels uppercase, +0.1em tracking.

## Surfaces, texture and edges

- **Matte tempera/limestone:** an almost invisible fine-grain tile (SVG `feTurbulence`, baseFrequency ~0.9, opacity 3–4%) over flat colour. No brush texture.
- **Clean volumes:** cards shaded as simple solids — a very soft, wide tonal gradient on one side (light from one consistent direction), not a drop shadow.
- **Edges:** crisp, 1px hairlines between areas where needed; no outlines around everything.

## Shapes and layout

The layout itself should be the artwork: a strict proportional grid (e.g. columns derived from a golden or 3:4 ratio), consistent gutters, and deliberate negative space. Asymmetry is allowed but measured — one large calm field balanced by a smaller dense one. Corners square or minimal (2px). Architectural elements (columns, lintels) can become dividers and section frames, but the source says colonnades are optional, not required.

## Components

- **Buttons:** solid deeper blue or umber, square, generous padding; hover shifts value slightly.
- **Cards:** pale panels with a one-sided soft tonal gradient implying volume.
- **Navigation/sidebar:** a narrow, perfectly aligned column; active item marked by a thin rose bar.
- **Inputs:** pale fields with a hairline border; focus = border deepens to blue.
- **Tables:** excellent fit — even rhythm, hairline rules, aligned numerals (`font-variant-numeric: tabular-nums`).
- **Badges/tags:** flat pale fills with dark text.
- **Illustrations/icons:** simple geometric solids lit evenly from one side; for figures, oval heads and cylindrical necks, calm poses.
- **Loading:** a slowly rotating geometric solid or ring (the prototype used turning instrument rings).
- **Empty state:** a single well-placed solid in a large, measured empty field.

## Motion

- Measured and precise: rotating rings or instruments, objects sliding along grid lines, sections settling into alignment.
- Timing follows the action; the source explicitly says calm ≠ slow. Use smooth ease-in-out, 250–500ms, no overshoot.
- Fast action must keep forms intact and occlusion readable.
- Done moment: an element clicks precisely into its grid slot; a hairline completes around it.
- `prefers-reduced-motion`: no rotation or sliding; instant alignment.

## Techniques

- **Block in low detail first:** lay out proportions and negative space with plain blocks (literally grey boxes), check perspective, then shape and colour.
- **One light:** derive every tonal gradient and soft shadow from a single direction token (e.g. `--light-angle: 110deg`) so volumes agree.
- **Perspective accents:** CSS 3D (`perspective`, `rotateX`) for a single tiled floor or receding colonnade in a hero, kept static.

## Performance and accessibility

- Cheap: flat colour, tiny grain tile, CSS gradients. Nothing heavy is required.
- Contrast is the main risk: the pale palette tempts light-on-light text. Keep body text in `#3a332b` on pale grounds (~10.5:1); never set text on `#b9cdd6` or rose in the muted colour.
- Motion: minimal; disable under reduced motion.

## Pitfalls

- Equating "serene" with slow motion → timing follows the action.
- A fixed cream/sepia filter over everything → set palette per work; keep colours clear.
- Forcing central symmetry and colonnades → order comes from proportion and spacing, not from props.
- Purposeless decorative marks → every detail obeys the overall measure.
- Treating this card as tested → it is the least developed in the batch; validate with real reference images before shipping.

## Nearest neighbours

- **40 · Raphael:** also clear and harmonious, but Raphael leads with flowing groups, linked gazes and richer red/blue; Piero leads with geometric measure, even light and stillness.
- **12 · Bauhaus:** also geometric order, but flat primaries and abstract shapes; Piero is modelled volume in pale, natural daylight.

## Source

Adapted from huashu-art-motion · `39_piero.md` (research recipe; old single-scene prototype ~35, not reworked under the reference-anchored protocol; no measured parameters).
