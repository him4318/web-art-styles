# 44 · Pietro da Cortona (Pietro da Cortona, Roman Baroque, 1630s–1640s)

**Status:** Research recipe — reworked three times (clouds redone) against two works (the Barberini ceiling *Allegory of Divine Providence*, *The Rape of the Sabine Women*); self-score ~70, not independently reviewed; below the catalogue's 80 bar. Techniques are hypotheses tested in one scene.
**Layer:** Period specimen (strongest as tribute or a celebratory moment) — the organisation of rich, flowing groups can transfer with care.
**Web fit:** Low-medium — luxury, opera/theatre, festivals, heritage venues, a celebratory launch or milestone page, a one-off "achievement unlocked" moment. Poor for everyday product UI: gilded frames, radiant light and swirling crowds overwhelm controls.

## Recognisable traits

- Grand illusionistic ceiling: a stucco frame with gilded roundels (medallions) opening onto sky.
- A warm golden radiance at the centre, with heaped, volumetric cloud masses.
- Full-bodied figures, fabrics and sashes forming groups that push and pull, swirling around the centre and spilling over the frame.
- Rich warm colours set against cool ones (reds and blues in flying drapery), with light and dark dividing the abundance into readable layers.
- Architecture, painting and ornament run into each other while keeping their own edges and materials.

## Palette

| Role | Hex | Note |
|---|---|---|
| Background (stucco) | `#ece2cf` (suggested) | Warm white stucco for the page. |
| Surface / panel | `#f5eee0` (suggested) | Cards. |
| Text | `#2b2119` (suggested) | ~12.3:1 on stucco. |
| Muted text | `#6b5a48` (suggested) | ~5:1. |
| Gold (frames, medallions) | `#c79a3a` (suggested) | Ornament, icons; decorative — not for text on stucco. |
| Radiant centre | `#f3d69a` → `#fff4dc` (suggested) | Central glow in heroes. |
| Sky edge | `#8fa9c4` (suggested) | Outer sky; keep soft, never a blue-purple gradient. |
| Accent (crimson drapery) | `#a3322a` (suggested) | Primary CTA; white text ~6.9:1. |
| Accent 2 (drapery blue) | `#2e4f86` (suggested) | Secondary CTA, links. |

All suggested from the source signature (stucco frame, gilded roundels, warm gold radiance, red and blue sashes). Relationship: warm gold at the centre, cooler at the edges; reds and blues as flying accents; stucco calms everything. Dark reading: a night ceiling (`#2a2219` ground, gold frames, cream text) can work for a celebratory page.

## Typography

Baroque display: **Playfair Display 700 Italic** or **Bodoni Moda** for headings; body in **EB Garamond** or **Cormorant Garamond 500** at ≥ 17px (`"EB Garamond", "Cormorant Garamond", Georgia, serif`). Labels in letter-spaced caps inside gilded cartouches. Keep the body calm; let ornament carry the exuberance.

## Surfaces, texture and edges

- **Stucco frame (source):** three bands of moulded stucco + an egg-and-dart moulding + gilded roundels at the four corners. On the web: a layered `border-image` or an SVG frame with `box-shadow` insets to suggest relief; egg-and-dart as a small repeating SVG pattern; medallions as gold discs with an inset highlight.
- **Opening into sky:** inside the frame, a sky gradient warm at centre (`radial-gradient` gold → pale) to cool at edges, plus radiating light (a soft `repeating-conic-gradient` from the centre, blurred once).
- **Clouds as one bumpy mass:** a single scalloped outline (SVG path of overlapping arcs unioned), then lit along the direction toward the centre, with a few contour arcs on the outer side — not individual spheres.
- **Fresco veil:** a plaster tile over the painted parts only, not over the stucco.

## Shapes and layout

Framed openings: hero sections as an oval or rectangular "ceiling" inside a stucco frame; content within can spill over the frame (overlapping elements, `overflow: visible`). Diagonal and rotational composition: elements arranged around a centre, sashes linking groups. Below the hero, return to a calm grid on stucco. Corners: ornate frames for heroes; small radius (2–4px) for ordinary cards. Shadows: soft relief on stucco; none on painted sky.

## Components

- **Buttons:** crimson or blue with light text inside a thin gold rim; hover brightens the gold.
- **Cards:** stucco panels with a moulded border; feature cards can carry a small gold medallion.
- **Navigation/sidebar:** a cornice band across the top with gilded separators; keep it simple.
- **Inputs:** stucco fields with a gold underline on focus.
- **Badges/tags:** gold medallions with an icon.
- **Illustrations:** groups of full figures swirling around a centre with red and blue sashes; strong foreshortening and chiaroscuro are what the prototype lacked.
- **Loading:** the central radiance breathing; a sash fluttering.
- **Empty state:** an open painted sky inside the frame with a single cloud.
- **Celebration moment:** the strongest use — a frame opens, golden light radiates, sashes fly across.

## Motion

- Groups revolve slowly around the centre; ribbons and sashes flutter; the central light and a central figure "breathe"; clouds drift.
- Movement is rich but slow (rotations of a few degrees per second, suggested); celebratory reveals 1–2s with an ease-out.
- Decoration must never cover the action — keep UI controls clear of moving ornament.
- Done moment: a burst of radiance from behind the completed item, with a sash swept across.
- `prefers-reduced-motion`: static frame and light; no rotation, flutter or burst.

## Techniques

- **Ribbons/sashes (source lesson):** expand the ribbon's width along the path's normal at each point; a fixed angle makes it collapse into a hair-thin line. In SVG, generate the outline from a centreline + normals; animate the centreline.
- **Clouds (source lesson):** layered spherical radial gradients read as soap bubbles; first make one bumpy overall shape, then light it toward the source.
- **Rotating groups:** place figures on an ellipse and rotate the parent with `transform: rotate()`, counter-rotating children to keep them upright if needed.
- **Three-layer grouping:** plan three big groups and their light, use overlap and colour to control density, then add detail.

## Performance and accessibility

- Cheap: pre-rendered frame and clouds, transform-only rotation, static blurred rays with opacity breathing. Expensive: many filtered SVG figures and live blurs.
- Contrast: dark text on stucco ~12:1; white on crimson ~6.9:1. Gold is decoration, not text. Never put text on the radiant sky without a solid panel.
- Rotation and radiating light can trigger motion sensitivity → slow, small and off under reduced motion; use the effect for one-off moments, not as permanent ambience.

## Pitfalls

- Spherical gradients for clouds → soap bubbles; use one bumpy mass, lit as a whole.
- Ribbons at a fixed angle → hair-thin threads; use normals.
- Bell-shaped robes, weak foreshortening and weak chiaroscuro (noted shortcomings) → stronger diagonals, deeper darks.
- Cartoonish highlight arcs on clouds (noted) → softer, broken highlights.
- Filling every surface with angels, clouds, crests, gold and spirals → the source says none of these are required; first make the event clear.

## Nearest neighbours

- **41 · Michelangelo:** also powerful bodies, but one figure's tension and structure on bare paper; Cortona is many figures, colour, gold and cloud in a swirling whole.
- **18 · Klimt:** also lavish gold, but flat ornamental gold leaf and patterned robes; Cortona's gold is light and gilded relief around an illusionistic sky.

## Source

Adapted from huashu-art-motion · `44_cortona.md` (research recipe; v2 three rounds, self-score ~70; shortcomings: bell-shaped robes, missing strong Baroque foreshortening and chiaroscuro, cartoonish cloud highlights; only two distinct reference works).
