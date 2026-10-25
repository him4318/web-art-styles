# 41 · Michelangelo (Michelangelo Buonarroti, High Renaissance, c. 1508–1540s)

**Status:** Research recipe — reworked twice along a red-chalk drawing route (self-score ~65–70, no independent review; below the catalogue's 80 bar). The drawing technique below was tested in one scene; everything else is hypothesis.
**Layer:** Material (works on any subject) — sculptural form and tension through pose are transferable; the red-chalk study sheet is the most web-ready expression.
**Web fit:** Medium-low — editorial features, portfolios, art/anatomy/fitness/architecture storytelling, "how it's made" pages with large drawings. Poor for product UI: the style lives in big, strained bodies and drawings, not in small components.

## Recognisable traits

- Sculptural form: skeleton, load, twist and compression build volume; force comes from pose, not from giving everyone muscles.
- Clear overlaps and foreshortening; the subject feels like a solid occupying real space.
- In the drawings: red chalk (sanguine) on warm paper, directional hatching that follows form, cross-hatching in the darks, highlights left as bare paper.
- "Unfinished" (non-finito) quality: forms emerging from rough stone or fading out at the lower edge of the sheet.
- A strong main mass pulling against negative space.

## Palette

| Role | Hex | Note |
|---|---|---|
| Background (laid paper) | `#efe4cf` (suggested) | Warm study paper. |
| Surface / panel | `#f6eedd` (suggested) | Lighter sheet for cards. |
| Text | `#3a2a20` (suggested) | Brown-black ink; ~10.9:1 on paper. |
| Muted text | `#6e2e1c` (suggested) | Dark sanguine; ~8.1:1 on paper — readable even for small text. |
| Accent (red chalk) | `#a0442a` (suggested) | Drawings, links, primary buttons; ~5:1 on paper. |
| Foxing / age | `#c9a77a` (suggested) | Spots and torn-edge shadows, decoration only. |
| Alt: fresco blocks | `#c46a3a`, `#6f8fa8`, `#d9b56a` (suggested) | Clear colour fields as in the Sistine ceiling, for an alternative "fresco" reading. |

All suggested; the source says the drawings define structure and should not be read as a combined palette for the frescoes. Relationship: monochrome warm red-brown on paper, contrast built by leaving paper bare in the lights. Dark reading: not native; a dark variant could use black chalk with white heightening on toned grey paper (`#6b6a64`) — untested.

## Typography

An inscriptional roman with weight and tension: **Cinzel 600** or **Cormorant 700** for display; body in **EB Garamond** or **Crimson Pro**. For annotations, a handwritten italic (**Caveat** is too modern — prefer **IM Fell English Italic**), used for small marginal notes only, never for body text.

## Surfaces, texture and edges

- **Study paper:** a pre-rendered paper tile with fibres, a few foxing spots and a slightly torn, uneven edge (SVG `clip-path` polygon with jitter on hero sheets only).
- **Hatching instead of gradients:** shade illustrations and even large icons with directional hatch lines (mostly at 50°, cross-hatching in the darks) rather than tonal gradients.
- **Silhouette contour:** one continuous outer contour, thicker and broken on the shadow side, with chalk grain; no internal outlines at part seams.
- **Non-finito:** let large drawings fade out toward the bottom (a `mask-image` linear gradient to transparent), or emerge from a rough stone texture.
- **Marginal notes:** faint illegible "annotations" in sanguine near drawings, as texture.

## Shapes and layout

Square paper sheets, slightly rotated (±0.5–1°), possibly overlapping as on a study table. No rounded corners; no drop shadows except a faint paper lift. Composition: one dominant figure or object, large and twisting across the space, a smaller repeat study in a corner (the source used a flipped, smaller copy of the same figure). Generous paper margins.

## Components

- **Buttons:** sanguine fill with paper-coloured text, or a paper button with a hand-drawn sanguine contour; hover adds a few hatch strokes.
- **Cards:** paper sheets with fibre texture and a slightly irregular edge.
- **Navigation/sidebar:** a narrow paper strip; active item underlined with a chalk stroke.
- **Inputs:** a chalk underline that thickens on focus.
- **Tables:** thin sanguine rules; works for small tables only.
- **Badges/tags:** small hand-hatched labels.
- **Illustrations/mascot:** build from a force silhouette (twist, load) before detail; shade with hatching; seamless outer contour. Avoid round "doll" heads and short arms.
- **Loading:** hatching filling a shape stroke by stroke.
- **Empty state:** an unfinished drawing fading into blank paper, with a marginal note.

## Motion

- The drawing being drawn: the illustration is revealed top to bottom while a chalk "cursor" sweeps along the reveal edge (source method).
- Mechanical or human tension: a slow twist, a held strain, release.
- Timing follows cause; reveals 1.5–3s, ease-out; UI transitions 200–400ms.
- Done moment: a final contour stroke completes around the item.
- `prefers-reduced-motion`: show the finished drawing; no reveal.

## Techniques

- **Hatched drawing (source pipeline):** grey-scale form map (overlapping light/dark ellipses for shoulder blades, spine groove, lats; light from upper left) → deterministic hatching from brightness + gradient (main direction 50°, cross-hatch in darks) → whole-silhouette contour from boundary pixels, thicker and broken on the back-lit side, with chalk grain → cache. On the web: run it offline or once on a canvas, export to PNG/SVG, and reveal with an animated `clip-path: inset(0 0 X% 0)`.
- **Seams:** never outline parts individually — use the overall silhouette, or it reads as an assembled doll.
- **Fade = unfinished:** `mask-image: linear-gradient(#000 70%, transparent)`.

## Performance and accessibility

- Cheap: pre-rendered drawings, a paper tile, a clip-path reveal. Expensive: generating hatching live — do it at build time.
- Contrast: brown-black and dark sanguine on paper pass AA comfortably; red chalk `#a0442a` passes for normal text at ~5:1 but keep it for links and headings.
- Hatching behind text is noise → keep text on clean paper.
- Motion: a reveal is a one-off; skip it under reduced motion.

## Pitfalls

- A mechanical arm with balls as "Michelangelo" (the first attempt) → the spirit is the human body's force; non-human subjects need the same load and twist.
- Outlining each body part → reads as a jointed doll; draw one silhouette.
- Heads too round, upper arms too short, no real torso twist (noted shortcomings) → study proportions; twist the torso.
- Highlights not left bare enough → push contrast: true paper in the lights.
- Defaulting to nudes, gods or the touching-fingers gesture → modern clothing and everyday tasks work if the pose has structure.

## Nearest neighbours

- **44 · Pietro da Cortona:** also big Baroque-era energy, but Cortona is many figures swirling with clouds, gold and decoration; Michelangelo is a single body's tension and structure.
- **32 · Rembrandt:** also dramatic modelling, but by warm painted light and darkness; Michelangelo here is line and hatching on bare paper.

## Source

Adapted from huashu-art-motion · `41_michelangelo.md` (research recipe; v2 self-score ~65–70; shortcomings: head too round, short upper arms, no true torso twist, highlights not bare enough).
