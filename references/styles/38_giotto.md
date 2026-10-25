# 38 · Giotto (Giotto di Bondone, early 14th century)

**Status:** Research recipe — reworked once against original frescoes (self-score ~65–70, independent review ~53; below the catalogue's 80 bar). Everything below is a tested-once hypothesis, not a proven treatment.
**Layer:** Period specimen (strongest as tribute or easter egg); the fresco *surface* is transferable.
**Web fit:** Medium-low — editorial and storytelling pages (museum, history, education, a narrative "about us"), illustrated onboarding where people hand things to each other. Poor fit for dense product UI: the style's strength is weighty figures and clear gestures, which a dashboard doesn't have.

## Recognisable traits

- Figures as simple, heavy volumes: bell-shaped robes whose folds show weight and gravity; faces and hands carry the relationships between people.
- Gazes and gestures lock together — you can read who is looking at whom and why.
- Deep ultramarine sky (with gold stars in the Scrovegni Chapel), set against earth-red, ochre and olive robes.
- Blocky, slanted grey-pink architecture and rock that stage the event rather than follow later photographic perspective.
- Matte, fine-grained fresco surface and painted decorative borders (a red-and-gold band, red/white lozenge friezes).

## Palette

| Role | Hex | Note |
|---|---|---|
| Background (plaster) | `#e9dfcc` (suggested) | Warm lime plaster for the page. |
| Surface / panel | `#f2e9d8` (suggested) | Slightly lighter "fresh plaster" for cards. |
| Text | `#2b231c` (suggested) | Umber; ~11.7:1 on plaster. |
| Muted text | `#6b5a48` (suggested) | ~5:1 on plaster. |
| Accent (ultramarine) | `#23407f` (suggested) | Sky blocks, primary buttons; cream text on it ~8.4:1. |
| Accent 2 (earth red) | `#9e3b2a` (suggested) | Borders, alerts, the decorative band. |
| Ochre | `#c08a3e` (suggested) | Robes, secondary fills. |
| Olive | `#6f7a45` (suggested) | Success, robes. |
| Gold (halo, stars) | `#c9a245` (suggested) | Small marks only; ~4.1:1 on ultramarine — not for small text. |
| Stone grey-pink | `#d9c3b6` (suggested) | Architecture blocks, dividers. |

No source hexes exist for this card; all values are suggested from the stated signature (ultramarine sky, earth-red/ochre/olive robes, grey-pink buildings, red-gold band). Relationship: broad, complete local colour areas with gentle tonal modelling; background and figures belong to one matte world. Dark reading: an ultramarine night ground (`#1a2c55`) with plaster-coloured cards works for a hero, but keep reading text on plaster.

## Typography

A sturdy, humanist serif with some weight: **Cormorant SC** or **Alegreya SC** for small-caps headings, **Alegreya** or **Crimson Pro** for body (`"Alegreya", "Crimson Pro", Georgia, serif`). Labels in small caps, +0.08em tracking, earth red. A display uncial (e.g. **Uncial Antiqua**) only for a single ornamental word, never for UI labels.

## Surfaces, texture and edges

- **Fresco medium (source recipe):** light blur on the illustration layer → a lime "veil" (a pale wash at low opacity) → plaster texture multiplied over → faint pinholes and *giornata* seams (the day-patches of fresco). For the web: one pre-rendered plaster tile (fine noise + a few soft blotches) with `mix-blend-mode: multiply`, plus 1–2 faint seam lines across hero art.
- **Matte, not cracked:** cracks and fading are conservation damage, not the style. Don't use a craquelure overlay to fake age.
- **Painted frames:** wrap hero panels and feature cards in a painted border: an earth-red band with a thin gold line, or a red/white lozenge frieze (CSS `repeating-linear-gradient` at 45°, or a small SVG pattern).
- **Sky blocks:** ultramarine with soft, slightly mottled patches and scattered small gold stars (SVG pattern of 8-point stars, irregular spacing).

## Shapes and layout

Square or barely rounded corners (0–3px) — painted panels, not pills. Shadows: none, or soft tonal modelling inside shapes; no drop shadows. Composition: one clear event per panel, with figures/objects grouped and pointing at the focal point; slanted architectural blocks frame it. In page terms: a sequence of framed narrative panels (like the chapel's registers), each with a single message, read left to right.

## Components

- **Buttons:** ultramarine fill, cream text, square corners, a thin gold inner line; hover deepens the blue.
- **Cards:** plaster panel inside a painted earth-red border band.
- **Navigation/sidebar:** a vertical painted frieze (red band + lozenges) with plain text items; active item marked by a small gold star or halo disc.
- **Inputs:** plaster field with an umber bottom rule; focus = a thin gold line.
- **Chat bubbles:** plain plaster panels; avatars as round haloed discs (a gold ring).
- **Tables:** thin umber rules; header row in small caps on stone.
- **Badges/tags:** small painted lozenges in ochre/olive/red.
- **Illustrations/mascot:** heavy bell-shaped silhouettes, big heads, almond eyes with a clear gaze direction, hands as palm + fingers (not mittens). Make the gesture tell the story — e.g. two figures really supporting a box between their hands.
- **Loading:** a gold star that slowly brightens and dims on an ultramarine disc.
- **Empty state:** a framed panel with a single figure looking toward the call to action.

## Motion

- Fresco is still; motion should be minimal and meaningful: a handed-over object on an arc between two figures, a robe hem barely swaying, a bird crossing the sky, dust settling after something is set down.
- Calm doesn't mean slow motion: timing follows the action; keep moves short (300–600ms, ease-in-out) and rare.
- Hover: a gold star or halo brightens; no lift.
- Done moment: a small gold halo ring fades in around the completed item's icon.
- `prefers-reduced-motion`: no ambient motion; state changes as cross-fades or instant.

## Techniques

- **Paint the medium over the illustration, not around it:** the fresco treatment must cover figures too, or they look pasted on. Apply the plaster multiply and veil to the whole illustration group.
- **Silhouette first:** build illustrations as readable silhouettes and volumes, layered by contact (who touches what), then add matte colour and fold lines. Don't substitute surface texture for form.
- **Lozenge frieze:** `background: repeating-linear-gradient(45deg, #9e3b2a 0 8px, #f2e9d8 8px 16px)` clipped into a 12–16px band, edged with gold 1px lines.
- **Gold stars:** a small SVG `<pattern>` with jittered positions; keep gold flat (no metallic gradient) — this isn't gilded panel painting.

## Performance and accessibility

- Cheap: one plaster tile, SVG patterns, static frames. Expensive: live blur over large illustrations — pre-render the treated illustration.
- Contrast: umber on plaster ~11.7:1; cream on ultramarine ~8.4:1. Gold is decorative only. Don't set text over the mottled sky without a solid plaster or blue backing.
- Motion: very little; disable all ambient motion under reduced motion.

## Pitfalls

- Treating cracks, fading and flaking as the style → that's age damage; keep the surface intact and matte.
- Laying gold over everything → gold ground belongs to panel painting (a different medium); in fresco it's halos and stars only.
- Figures that look like generic bell-shaped dolls with identical faces (the rework's main weakness) → vary robe widths and silhouettes, give hands real fingers, make gazes connect.
- Plaster texture too grainy (noted shortcoming) → fine, soft noise at low opacity.
- Even, scattered grass tufts and props → group them, like the figures.
- Making haloes and scenes religious by default → the halo, ultramarine and border are the recognisable signature; the subject can be modern (the source gave halos to dock workers, as a joke and a signature).

## Nearest neighbours

- **39 · Piero della Francesca:** also clear volumes, but Piero leads with geometry, proportion and even pale light; Giotto leads with gesture, weight and human relationship, and is earthier and darker.
- **43 · Lorenzetti:** same fresco family, but Lorenzetti is a whole city of small figures and stacked buildings; Giotto is a few large figures in one event.

## Source

Adapted from huashu-art-motion · `38_giotto.md` (research recipe; v2 rework self-score ~65–70, independent review ~53; shortcomings: simple faces and hands, lacking crowd density and layered glazing in the folds, plaster too grainy, grass too even).
