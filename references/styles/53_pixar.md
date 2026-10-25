# 53 · Pixar-style family: form, material and performance (Pixar Animation Studios, 1995–present)

**Status:** Research recipe — one older single-scene study (a faceless washing machine jams, shakes, its door opens and laundry spills, built in three.js), quality about 55, not redone under the reference-anchored review. Hypotheses only.
**Layer:** Material (works on any subject) — the lessons are about designing silhouette, distinct materials, story-led light and cause-and-effect motion; they apply to illustrations, 3D spots and UI micro-interactions.
**Web fit:** Medium — product marketing with 3D hero renders, onboarding illustrations, playful consumer apps, micro-interactions in any UI. Low as a full UI skin: CSS alone cannot produce it, and generic "cute 3D" quickly becomes cliché.

This card describes a broad studio *family* of looks, not one style, and copies no characters. The source insists on choosing one film-level range before designing (for example, *Luca*'s painterly graphic look and *Soul*'s realistic New York vs abstract soul world are different), and on not defaulting to big eyes, round faces and shiny plastic.

## Recognisable traits

- Silhouette and proportion designed first, surface detail second; a clear, readable shape at any size.
- Materials tell the story: fabric, skin, enamel, rubber, metal each look and respond differently; reflections stay inside the art direction.
- Light leads the eye to the story beat and may bend physics for clarity; it is not a fixed golden rim light or a blue-purple backdrop.
- Performance with weight: posture, balance, gaze, anticipation and follow-through; emotion comes from cause and effect, not from swapping face stickers.
- A lived-in world: small scuffs, seams, baseboards and clutter keep scenes from looking like an empty toy stage.

## Palette

From the source study (a sunlit laundry room) — a scene palette, not a studio standard.

| Role | Hex | Note |
|---|---|---|
| Background | `#bed1bd` | Soft sage room air; page ground. |
| Surface (enamel/cream) | `#efdfbd` / `#e8dfc8` | Cards as enamel panels. |
| Text | `#243e3d` | Dark teal; ~8.7:1 on cream. |
| Muted text | `#52675e` | Label colour from the study's printed tag. |
| Tile | `#94b7ab` with grout `#d5ddce` | Section backgrounds, patterns. |
| Accent (warm fabric) | `#c76e42` / `#c86239` | Primary actions with dark text (`#1d1814` ~4.8:1); light text on it fails. |
| Accent 2 (wood) | `#ae6a3d` | Secondary, tags. |
| Steel | `#b2bdb9` | Metallic UI details. |
| Light | key `#ffedcc`, sky `#fff1d5`, ground bounce `#527674` | Use as highlight and shadow tints in CSS. |

Relationship: warm key light against cool, tinted bounce shadows; materials separated by roughness as much as hue. No purple gradient backdrops; the catalogue treats blue-purple glow as AI slop. Dark reading (suggested): an evening room — `#1f2e2d` ground, cream text, warm lamp accents.

## Typography

Friendly, sturdy, highly legible sans: **Nunito Sans 700/800** or **Figtree** for headings — `"Figtree", "Nunito Sans", system-ui, sans-serif` — with body at 16–18px. Avoid imitating any film title lettering. The personality belongs in illustration and motion, not in novelty fonts.

## Surfaces, texture and edges

- **3D spots, pre-rendered:** the reliable web route is to render hero images and spot illustrations offline (Blender or three.js offline) and ship AVIF/WebP with transparent backgrounds. Live WebGL only for one hero at most.
- **Material-led UI:** suggest material in components with restraint — enamel buttons (soft top highlight, tinted bottom shade), fabric cards (a faint weave tile at 6–8%), steel toggles (a narrow linear-gradient sheen).
- **Rounded volumes:** shapes have bevelled, rounded edges (the study's machine: corner radius 0.18 of its 3.25 width ≈ 5.5%); in CSS, generous radii (12–20px) plus a highlight on the top edge.
- **Grounding shadows:** every object casts a soft contact shadow (radial gradient, `rgba(23,39,32,.48)` → 0) plus a broader ambient one.

## Shapes and layout

Rounded but not bubbly; volumes feel solid. Shadows layered: a tight contact shadow and a soft ambient shadow, both tinted with the bounce colour, not grey. Layout is cinematic: a held "camera" (fixed composition) with the action inside it; one clear focal point per section, lit brighter. Leave some lived-in details (small labels, seams) so pages don't look like empty stages.

## Components

- **Buttons:** enamel pills with a soft top highlight and tinted lower shade; press = squash to `scaleY(0.94)` with anticipation, then follow-through overshoot on release.
- **Cards:** cream enamel panels with layered contact + ambient shadows; the hero object on a card can tilt toward the cursor slightly (≤ 4°).
- **Navigation:** clean, flat bar; the active item lifts 2px with a contact shadow.
- **Toggles/sliders:** steel-like thumbs with a sheen; the thumb overshoots and settles.
- **Inputs:** cream field, soft inner shadow; focus = 2px dark-teal `#243e3d` ring (the warm orange is under 3:1 on cream).
- **Loading:** a small object with a cause-and-effect loop (a drum turning, something nudged), never a generic spinner with a face.
- **Empty state:** a single well-lit object with a tiny story (a basket waiting, a door ajar).
- **Mascots/illustrations:** design the silhouette first, give each material a role, and don't add a face to every object — the study's washing machine had no face and still acted.

## Motion

- **Anticipation → action → follow-through:** e.g. a "send" button squashes (`scaleY .92`, 80ms), launches (`translateY -6px`, 120ms), overshoots and settles (240ms, spring).
- **Cause before effect:** sequence dependent events in the right order; the study's fix was to open the door *before* releasing the cloth, and to end falling cloth exactly on the floor plane.
- **Weight:** heavier elements start slower and settle with smaller, slower overshoot; light elements flutter.
- **Held camera:** keep the frame still; let objects move (the study explicitly holds the camera).
- Use springs (`cubic-bezier(.34,1.56,.64,1)` for small overshoot, suggested) rather than linear moves. Under `prefers-reduced-motion`, remove squash/overshoot and use short opacity fades.

## Techniques

The study's three.js setup, useful for a single live hero:

- Renderer: ACES filmic tone mapping, sRGB output, soft PCF shadows (2048px shadow map), pixel ratio capped.
- Lights: hemisphere `#fff1d5` (sky) / `#527674` (ground) at 1.15; one directional key `#ffedcc` at 3.1 from upper left-front, casting shadows.
- Camera: perspective FOV 37°, held still.
- Materials by roughness/metalness: enamel roughness ~0.36; steel roughness 0.27, metalness 0.7; rubber 0.9; fabric 1.0 with a procedural weave canvas texture (alternating light/dark 1px lines every 2px).
- Cloth: an analytic deforming grid (24 × 16), not physics — fine for a short loop.
- The source warns: rigged 3D characters are not a solved capability in this pipeline; don't fake them with sphere-and-cylinder people.

## Performance and accessibility

- Expensive: live WebGL with shadows on mobile. Prefer pre-rendered images or short video loops; if live, render only in view, pause when hidden, cap DPR at 1.5–2, and provide a static poster.
- Contrast: dark teal on cream ~8.7:1. The warm orange accent is only 2.8:1 against cream — use it as a fill with dark text, never as text on cream.
- Motion with overshoot and parallax tilt can trigger vestibular issues; always respect reduced motion and keep tilts small.
- Provide `alt` text for 3D illustrations that carry meaning.

## Pitfalls

- Big eyes, round faces and glossy plastic on everything → that's a cliché, not the style; design materials and silhouettes per subject.
- Uniform plastic with chrome reflections → that's early ray-traced CGI (catalogue 15).
- Blue-purple gradient backdrop with golden rim light → AI-slop default; light should serve the story beat.
- Effects before causes (cloth falling through a closed door) → sequence events causally.
- Ball-and-stick characters or a single AI-generated background passed off as the style → the source calls this out; show the capability gap instead.
- Empty toy-stage sets → add small lived-in details.

## Nearest neighbours

- **15 · Early ray-traced CGI (built):** uniform plastic shading, checkerboard reflections, chrome spheres. The Pixar family designs distinct materials, story-led light and acting together.
- **16 · 2026 flat illustration (built):** flat colour with grain and expressive character acting in 2D. Pixar's family is volumetric, lit and material-driven, though both share anticipation and follow-through.
- **25 · Ghibli:** painted watercolour backgrounds and cel-shaded characters; Pixar's look is rendered volume and material.

## Source

Adapted from huashu-art-motion · `53_pixar.md` (research recipe; older three.js study, quality ~55, not redone; open gaps: subsurface skin and material lighting, rigged 3D characters not yet supported).
