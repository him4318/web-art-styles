# 49 · Chagall (Marc Chagall, early 20th century onward)

**Status:** Research recipe — one older single-scene study (a box of memories opening, flowers and letters drifting into a tilted village), not redone under the reference-anchored review; quality about 40. Treat everything here as a starting hypothesis, the weakest card in this batch.
**Layer:** Material (works on any subject) in principle — emotional space and colour can be applied to new stories — but nothing has been verified across subjects.
**Web fit:** Low-medium — storytelling pages, memoir and culture sites, wedding or event microsites, a 404 or "about us" page. Poor for product UI: tilted, floating layouts work against scanning and alignment.

## Recognisable traits

- Space obeys feeling: direction, scale and adjacency can bend (tilted houses, things floating, size by importance), yet the main action still reads.
- Colour fields carry emotion together with local forms — a deep blue night, a rose-and-green flourish.
- People, animals and objects keep a recognisable character; exaggeration is linked to meaning.
- Real details and remembered images coexist in one picture; motifs come from the new story, not from a stock list.

Scope note from the source: compare early village pictures, later religious work and stained glass/stage design separately; don't flatten them into one look. Floating lovers, fiddlers, goats and Russian villages are **not** required.

## Palette

Hexes from the source study (one scene, not anchored against references — use cautiously).

| Role | Hex | Note |
|---|---|---|
| Background (night) | `#153f81` → `#2a4a93` | Deep cobalt; hero and storytelling sections. |
| Night violet | `#66517e` / `#625480` | Lower ground; use sparingly (see warning). |
| Surface (paper) | `#f4ece0` (suggested) | Reading panels. |
| Text | `#2a2440` (suggested) | Dark violet-ink on paper. |
| Moon | `#c9d6ac` | Pale green moon; accents on blue (~6.6:1). |
| Accent (rose) | `#b85e86` / `#bd6387` | Bouquet; primary actions. |
| Accent 2 (green) | `#567d80` / `#7c9d92` | Leaves; secondary. |
| House walls | `#73918d`, `#b9a1a0`, `#8b95af`, `#b29a82` | Tilted cards and tiles. |
| Roofs | `#9a5365`, `#c66e6d`, `#597d91` | Small accents, tags. |

Relationship: one dominant emotional field (night blue) with warm rose, pale green and earth accents floating in it.
**Warning:** this palette sits in the blue-violet hue band (225°–300°) the catalogue flags as the default "AI slop" look. Keep the blue deep and painterly (textured, never a smooth gradient glow), balance it with rose, green and earth tones, and prefer a light paper reading for most of a site with the night blue reserved for one story section.

## Typography

A soft, slightly irregular serif for headings: **Fraunces (soft, wonky axis on)** or **Instrument Serif** — `"Fraunces", "Instrument Serif", Georgia, serif`. Body: **Source Serif 4** or a humanist sans, 17–18px, on paper. A heading may sit on a slight tilt (−2° to 3°) as a single gesture; body text never tilts.

## Surfaces, texture and edges

- **Painted grounds:** the study filled each area with short, tapered strokes (alpha ~0.68, width 0.55–1.2× cell) whose direction varies with position (`sin(0.003x + 0.004y)·0.8`, flatter near the bottom), plus 18,000 tiny 3–26px marks in translucent blue/violet/rose over the night. Bake once to an image.
- **Faint horizontal scan of light:** 0.6px cream lines every 3px at alpha 0.035 — a barely visible canvas feel.
- **Shapes:** hand-drawn closed paths with a dark violet outline (`#3c426c`, 3px) for houses and objects; no clip-path jitter.

## Shapes and layout

Tilt is the signature: elements sit at small rotations, alternating (`−0.12 + 0.07·i` rad across a row of houses in the study). Scale by importance: a key image may be much larger than its neighbours. Floating elements drift along curved paths in empty space. Keep a stable reading column (straight, aligned) and put the tilt and float in the margins and illustrations. Radii soft; outlines thin and coloured; no hard shadows.

## Components

- **Buttons:** rose pill with white bold text at 18px+ (~4.2:1, large-text AA only) or a paper pill with rose outline and dark text; slight rotation (−1°) only on hero CTAs.
- **Cards:** paper panels at alternating tiny tilts (±1–2°), dark violet hairline outline; straighten on hover.
- **Navigation:** straight and calm, on paper; a small floating motif (a flower, a letter) marks the active item.
- **Chat bubbles:** user bubble rose, replies paper; a reply that "remembers" something can carry a tiny floating icon.
- **Badges:** roof colours.
- **Loading:** an envelope or petal floating along a curved path.
- **Empty state:** an open box with one or two objects floating out.

## Motion

- **Floating letters (source motif):** 7 items loop along a curved path at speed `p = (0.135t + i/7) mod 1`, scale `0.48 + 0.32·sin(πp)` (swelling mid-path), rotation `−0.35 + 0.5·sin(9p + i)`.
- **Petals:** 11 small ovals drift diagonally down at `p = (0.1t + 0.091i) mod 1` with `sin(7p + i)·28px` sideways sway.
- **Bouquet sway:** a large illustration rocks `−0.14 + sin(0.75t)·0.016` rad around its base.
- On the web: CSS `offset-path` with `offset-distance` keyframes for floating items; `animation-delay` staggers.
- Gentle and continuous. Under `prefers-reduced-motion`, place floating items at fixed positions along their paths.

## Techniques

- Floating along a path: `.float { offset-path: path('M…'); animation: drift 7.4s linear infinite; } @keyframes drift { to { offset-distance: 100%; } }` with a separate scale/rotate keyframe set to mimic the swell.
- Tilted rows: set `--i` per item and `transform: rotate(calc(-0.12rad + var(--i) * 0.07rad))`, clamped to ±0.2rad for text-bearing elements.
- The source's main rule for transfer: first decide the real relationships and the emotional ones, then choose where to bend scale or direction; every distortion must keep identity and contact (an object held stays in the hand).

## Performance and accessibility

- Cheap: baked backgrounds, `offset-path` animations on a handful of elements. Avoid animating many large images; cap floating items at ~10.
- Contrast: paper/dark ink easily passes; on night blue use cream `#f4f0e4` (~8.9:1) or the moon green (~6.6:1). Rose text on blue fails — use rose as fill only.
- Tilted text harms readability and can confuse screen magnifier users: only headings, ≤ 3°.
- Floating motion can trigger vestibular discomfort: honour reduced motion and keep speeds low.

## Pitfalls

- Defaulting to floating lovers, fiddlers, goats → motifs must come from the new story (source rule).
- Treating it as "liquify a realistic photo" → that's Dalí, not Chagall.
- Blue-violet gradient background → the AI-slop look; use a textured, painterly blue and warm counter-colours.
- Tilting the reading column → keep body text straight.
- Breaking object contact while floating (a cup separating from a hand) → distortions keep identity and contact continuous.

## Nearest neighbours

- **48 · Gauguin:** big flat fields and heavy outlines with gravity intact. Chagall bends space (tilt, float, scale by feeling) and uses thinner, coloured outlines with painterly fields.
- **23 · Dalí (built):** realistic illusion with melting forms and long shadows. Chagall is not realistic and not melting; it is folk-like memory space.
- **36 · Picasso Blue Period (built):** monochrome blue melancholy. Chagall's blue is a stage for warm floating colour.

## Source

Adapted from huashu-art-motion · `49_chagall.md` (research recipe; older study, not redone, quality ~40; not checked for faithfulness or across scenes).
