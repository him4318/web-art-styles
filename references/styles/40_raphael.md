# 40 · Raphael (Raffaello Sanzio, High Renaissance, c. 1505–1520)

**Status:** Research recipe — reworked once against three Vatican frescoes (self-score ~70, no independent review yet; below the catalogue's 80 bar). Techniques are hypotheses tested in one scene.
**Layer:** Period specimen (strongest as tribute) — the underlying "harmonious organisation" can transfer to group compositions.
**Web fit:** Medium — editorial, education, institutional and "team/community" marketing pages, especially ones showing several people or features working together. Weak for dense product UI; the perspective vault and checkerboard are hero devices, not app chrome.

## Recognisable traits

- Continuous contours and rounded, harmonious modelling of bodies and drapery; idealised figures with clear actions.
- Groups linked by gaze and gesture: main and secondary groups point, look and hand things to each other (a triangle is one option, not a rule).
- A deep central-perspective barrel vault with coffers, receding to a patch of sky; a checkerboard floor; niches with statues.
- Layered reds and blues balanced by neutrals, readable skin tones, soft volume.
- Clear information priority: architecture, props and figures all support one main event.

## Palette

| Role | Hex | Note |
|---|---|---|
| Background (warm stone) | `#f3ece0` (suggested) | Light marble/plaster. |
| Surface / panel | `#fbf7ef` (suggested) | Cards. |
| Text | `#3b2e25` (suggested) | ~11.2:1 on background. |
| Muted text | `#6b5a4a` (suggested) | ~5.6:1 on background. |
| Accent (ultramarine) | `#2f4f8f` (suggested) | Primary buttons, links; cream text ~6.8:1. |
| Accent 2 (vermilion red) | `#b23a2e` (suggested) | Secondary CTA, highlights; white text ~5.9:1. |
| Gold-brown (decorative band) | `#a8803f` (suggested) | Frames and borders; only ~3.6:1 with text either way — decoration only. |
| Floor/stone tan | `#d8c9ad` (suggested) | Checkerboard squares, dividers. |

The source names "gold-brown / red / ultramarine decorative bands" and saturated robe colours in groups; the hexes are suggested. Relationship: saturated red and blue in balanced pairs, held by a lot of warm neutral; the source says to re-pick the main colours per scene. Dark reading: not native; a dusk vault (`#2a2420` ground) can work in a hero.

## Typography

Classical, harmonious roman: **Cormorant Garamond 600** or **Playfair Display** for display; **EB Garamond** or **Libre Caslon Text** for body (`"EB Garamond", "Libre Caslon Text", Georgia, serif`). Inscription-style uppercase labels (Trajan-like: **Cinzel**) in the gold-brown, +0.12em tracking, large only.

## Surfaces, texture and edges

- **Fresco/marble ground:** a soft, fine plaster tile at low opacity; no cracks.
- **Lunette frames:** semicircular-top hero panels (`border-radius: 50% 50% 0 0 / 30% 30% 0 0`, or an SVG arch path) edged by a gold-brown band with red and blue inner lines.
- **Coffered vault (hero):** see Techniques; pre-render as SVG.
- **Checkerboard floor:** a perspective checkerboard as the base of a hero or section divider (CSS `conic-gradient` checker inside a `perspective`/`rotateX` plane, or SVG).
- **Niches:** rounded-top recesses with a soft inner shadow, used to frame portraits, avatars or feature icons.

## Shapes and layout

Arches and lunettes for frames; otherwise square-ish panels with small radii (2–4px). Shadows soft and tonal, not offset. Composition: one clear focal group, secondary groups linked to it by lines of sight. On a page: a centred hero with receding perspective, then feature "groups" arranged so their icons/figures face the main CTA; team pages where portraits look toward each other or the centre.

## Components

- **Buttons:** ultramarine or vermilion fill with cream/white text, small radius; hover warms the colour slightly.
- **Cards:** stone panels in a thin gold-brown frame; feature cards can sit in niches.
- **Navigation/sidebar:** a horizontal top bar styled as a cornice band (gold-brown with thin red/blue lines).
- **Inputs:** light fields with a tan bottom border; focus = ultramarine underline.
- **Chat bubbles:** soft rounded panels; avatars in arched niches.
- **Tables:** alternating stone tones (checkerboard as a gentle zebra), umber text.
- **Badges/tags:** red or blue pills with light text.
- **Illustrations/people:** groups whose hands and eyes point at each other or a shared object (book, instrument, scroll); vary faces and poses — sameness was the rework's main flaw.
- **Loading:** coffers receding toward the vanishing point, a slow pulse of light in the far sky.
- **Empty state:** an empty niche with one line of copy.

## Motion

- Harmonious, natural, connected: groups turning toward each other, a small figure walking along the floor in the distance, light shifting softly in the far sky.
- Smooth ease-in-out, 300–600ms; no stepped motion, no bounce.
- Hover: an icon or figure turns slightly toward the cursor/CTA.
- Done moment: the completed item's icon gains a soft glow from the "sky" and secondary elements turn toward it.
- `prefers-reduced-motion`: static composition; instant state changes.

## Techniques

- **Coffered vault (source method):** rings recede toward a vanishing point by a geometric series (N = 9 layers); each ring is cut into coffers by angle; side walls alternate pilasters and niches; the floor is a checkerboard drawn by layer and column. In SVG, compute ring radii `r_k = r_0 · q^k` (suggested q ≈ 0.78) and draw coffer quads between successive rings; render once.
- **Gaze map first:** sketch the relationships (who looks/points at what) before choosing layout; then unify figures and setting in colour blocks, light direction and soft/hard edges.
- **Perspective checkerboard:** `background: conic-gradient(#d8c9ad 25%, #f3ece0 0 50%, #d8c9ad 0 75%, #f3ece0 0) 0 0/80px 80px; transform: perspective(600px) rotateX(60deg);` on a hero floor element.

## Performance and accessibility

- Cheap: pre-rendered SVG vault, CSS checkerboard, static frames. Avoid animating 3D transforms on large planes.
- Contrast: umber on stone ~11:1; light text on ultramarine/vermilion passes AA. Gold-brown is decorative only.
- Busy perspective backgrounds behind text hurt legibility → keep text on solid panels.
- Motion: subtle; disable under reduced motion.

## Pitfalls

- Copying *The School of Athens* layout (arches, two central figures) or putting every scene in a central triangle → re-compose for the content.
- Bell-shaped robes and identical faces → vary silhouettes and faces.
- Niche statues that look like candles (noted) → give them real figure proportions or use icons instead.
- Flat skin and fabric → the original has warm skin and fabric sheen; add soft highlights.
- Turning every body into a straining sculpture → that's Michelangelo; Raphael is easy and graceful.

## Nearest neighbours

- **39 · Piero della Francesca:** also order and clarity, but Piero is about measure, pale even light and stillness; Raphael is about organic connection between figures and richer colour.
- **06 · Renaissance sfumato:** same era, but sfumato is soft smoky blending and varnish; Raphael here is clear contours, perspective architecture and linked groups.

## Source

Adapted from huashu-art-motion · `40_raphael.md` (research recipe; v2 self-score ~70; shortcomings: bell-shaped robes and similar faces, niche statues like candles, lacking skin and fabric sheen).
