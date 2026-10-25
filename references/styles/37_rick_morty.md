# 37 · Adult flat cartoon (Rick & Morty–style TV animation, 2010s)

**Status:** Built ★★
**Layer:** Period specimen (listed so in the source catalogue) — but the flat-fill, thin-rounded-outline method transfers to any playful UI
**Web fit:** High — playful product UIs, onboarding, empty states, mascots and marketing pages for consumer or developer tools. Medium for dense dashboards (outlines add visual weight to every cell). Use the *approach* only: never reproduce the show's characters, logos, portals or catchphrases.

## Recognisable traits

- Thin, even, dark-grey outlines with round caps and round joins around everything — figures, props and background alike.
- Hard-edged flat fills; at most one darker same-hue shadow shape, no realistic lighting or gradients.
- Slightly muted, warm, everyday colours (khaki walls, wood floors, pale skies) with low-contrast mottling in backgrounds only.
- Faces built from outline proportions: big white eyes, tiny dot pupils, a few curves for lids and mouth, no glossy highlights.
- Wobbly-but-deliberate shapes: smooth curves for cheeks and limbs, sharp corners for hair spikes and props.

## Palette

| Role | Hex | Note |
|---|---|---|
| Background (wall) | `#cdbf96` | Source warm grey-khaki. Outline on it ~8.6:1. |
| Surface (card) | `#f4efe3` (suggested) | Pale paper so body text is easy to read. |
| Text / outline | `#222323` | Source outline colour; ~13.7:1 on the surface. |
| Muted text | `#5c5a52` (suggested) | Secondary labels. |
| Accent (sky) | `#a7d6ea` | Source sky; selected states, info. |
| Accent 2 (wood) | `#9b7752` | Source floor; secondary buttons, headers. |
| Skin / warm highlight | `#efc4a0` | Source; works as a soft highlight fill. |
| Suggested "sci-fi" pop | `#8fd14f` (suggested) | One slimy green for success or the primary CTA — use sparingly. |

Relationship: flat mid-value fills, all held together by the same dark-grey line. Shadows are the same hue a step darker, never black or grey overlays. Dark reading (suggested): `#1d1f22` background, `#2a2d31` panels with `#e9e4d6` text (~10.9:1); coloured fills keep their muted hues and a near-black `#0d0e0f` outline — the line must stay darker than every fill it surrounds.

## Typography

Rounded, friendly but not childish: **Nunito 800** or **Baloo 2** for headings (`"Nunito", "Baloo 2", system-ui, sans-serif`); body in **Nunito 400/500** or a neutral sans. Titles can have a 2px dark-grey text stroke (`-webkit-text-stroke` or SVG) and a flat offset shadow in a fill colour. Avoid the show's logo lettering.

## Surfaces, texture and edges

- **Outlines everywhere:** `border: 2px solid #222323` (source 2.35px at 1920 wide; 1.5–2.5px in UI). For SVG, `stroke-linecap: round; stroke-linejoin: round`. Detail lines 1.6–2.6px at full scale (thinner than the main contour).
- **Flat fills + one shadow shape:** a darker same-hue shape clipped inside the element (e.g. a `::before` crescent at the bottom), hard edged.
- **Mottled backgrounds:** a seeded, low-contrast blotch texture (a few hundred soft blobs, ±4–6% lightness of the wall colour), rendered once to an SVG/PNG tile. Only on backgrounds — never on cards or figures.
- **No realistic lighting:** no blur shadows, no gloss, no bevels.

## Shapes and layout

Rounded but irregular corners (8–16px), slightly asymmetric if drawn in SVG. A uniform line weight is the system: buttons, cards, inputs, icons and illustrations all share the same stroke. Shadows, when used, are a flat offset copy in a darker fill (3–4px). Layout can be a normal grid; the style comes from line and fill, so keep composition clean and let illustrations carry the humour.

## Components

- **Buttons:** flat fill with a dark-grey outline and a flat offset shadow; on press, translate into the shadow.
- **Cards:** pale paper, outline, a single darker crescent or band as the "shadow shape".
- **Navigation/sidebar:** wood or wall-coloured panel with an outline; active item as a sky-blue pill with outline.
- **Inputs:** white field, outline; focus = outline thickens to 3px plus a sky-blue fill behind.
- **Chat bubbles:** outlined, round tails drawn as part of the same SVG path.
- **Tables:** outer outline only; inner rules thinner (1px at 40%).
- **Badges/tags:** small outlined pills in flat colours.
- **Icons:** outlined in the same stroke as everything else (a stroke-based icon set like Lucide at 2px, recoloured `#222323`).
- **Mascot/illustration lesson:** build an original character from smooth paths (limbs, cheeks) and sharp mixed paths (hair, props); big white eyes with dot pupils; group body, head, arms, eyelids and pupils separately so you can animate them from fixed anchors.
- **Loading:** an outlined blob or eye that blinks; three outlined dots bobbing.
- **Empty state:** an original outlined character shrugging on a mottled wall.

## Motion

- Limited TV-animation acting: small head turns, gaze shifts (pupils move within the eye, −1..1 per axis), deterministic blinks, a slowly wagging tail, a slow lift of a held object.
- A jump is a sine arc where body and attached parts share one offset; there was no landing squash in the source, so add only a small one if needed.
- Hover: the button's pupils/eye-icons look at the cursor, or a 2–3° wobble.
- Done moment: a quick outlined "burst" shape pops behind the item (scale 0 → 1.1 → 1, 250ms) and the mascot blinks.
- Timing: snappy ease-out, 150–300ms; idle loops seeded by time, not random per frame.
- `prefers-reduced-motion`: no blinking/gaze following; static expressions and instant state changes.

## Techniques

- **SVG first:** this style is natively vector. Build a small kit: an outlined rounded-rect primitive, a smooth-path helper (Catmull-Rom → cubic) for organic curves, a mixed-path helper for sharp corners, and a seeded texture generator.
- **Tails, ribbons, cords:** generate from a centreline + width into one closed outline; animate the centreline. Never regenerate a random shape every frame.
- **Deterministic animation:** drive pose from `seek(t, state)` style functions (state = look x/y, blink 0..1, etc.) so the same time always renders the same frame — useful for screenshots and tests.
- **SVG filters:** for `userSpaceOnUse` filter regions use real coordinates; `0,0,1,1` silently clips.
- **Mixing with canvas:** isolate SVG and canvas modules (IIFE or ES modules) to avoid global collisions.

## Performance and accessibility

- Cheap: SVG strokes and flat fills, a static mottled tile, transform-based limb animation. Watch DOM size if every list item has an illustrated SVG — reuse via `<use>` or sprite sheets.
- Contrast: dark-grey on pale paper ~13.7:1; on the khaki wall ~8.6:1. Avoid body text on `#9b7752` wood (dark text there is only ~3.9:1 — use it for large labels only, or darken the wood to `#5e4630` for white text at ~8.8:1). Muted `#5c5a52` on paper is ~6:1.
- Partial rasterisation can make SVG anti-aliasing vary between screenshots; if pixel-diff testing, render in a fixed Chromium config.
- Motion: small and brief; switch off gaze following and blinking under reduced motion.

## Pitfalls

- Copying recognisable characters, portals or logos → design original characters; borrow only line, fill and proportion rules.
- Black (#000) heavy outlines → that's rubber-hose or pop; keep thin dark-grey.
- Gradient shading or glossy eyes → flat fills, dot pupils, no highlights.
- Mottling on figures or cards → background only.
- Random shape per frame for tails/hair → generate from a stable centreline.
- Claiming walk cycles → the source had no walk rig; side-on walking needs real IK (front-facing sideways walks scissor the legs).

## Nearest neighbours

- **52 · South Park:** also flat TV cartoon, but construction-paper cut-outs with limited "flap" animation and almost no outline variation; this style has continuous thin outlines and smooth curved acting.
- **33 · Rubber hose:** also outlined cartoon, but heavy black lines, pie-cut eyes, white gloves, 12fps bounce and film grain; this style is modern, muted-colour, thin-lined and deadpan.

## Source

Adapted from huashu-art-motion · `37_rick_morty.md` (built ★★; noted shortcoming: standalone SVG pipeline, character shapes and complex actions could improve, no walk rig).
