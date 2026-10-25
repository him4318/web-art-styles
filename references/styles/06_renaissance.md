# 06 · High Renaissance sfumato (Leonardo da Vinci, c. 1503)

**Status:** Built ★★
**Layer:** Period specimen (strongest as tribute or easter egg)
**Web fit:** Low–Medium — museum, luxury, wine, publishing, or a dark "gallery" theme where content sits in lit panels. The soft blur and dark varnish work against crisp UI; use them on imagery and backgrounds, never on text or controls.

## Recognisable traits

- **Sfumato:** smoky, soft transitions; edges dissolve into shadow instead of being outlined, while features stay legible.
- Deep warm-brown darkness with one window of light: a single pool of daylight, everything else falling off into vignette.
- **Aerial perspective:** distance becomes bluer and paler in layered hills, with a silvery winding river.
- Old-master surface: yellowed varnish, canvas weave, faint brushwork, and a craquelure crack network.
- Volume from gradients, not lines: drapery folds, velvet sleeves with a sheen along the form.

## Palette

| Role | Hex | Note |
|---|---|---|
| Background (wall) | `#43321f` → `#22180e` | Source wall, darker at the top and edges. |
| Surface (lit panel, suggested) | `#efe3c8` | Like the white tablecloth in the light pool; content cards. |
| Text on panel | `#2a1d12` (suggested) | Warm umber. |
| Text on dark | `#efe3c8` (suggested) | |
| Accent (Venetian red) | `#5c2418` | Source floor tile; deep red buttons. |
| Accent 2 (ochre) | `#a88c64` | Source floor tile; borders, muted highlights. |
| Distance blues | `#9fb4b8` → `#7d9294` | Far hills; info states, image backdrops. |
| Velvet green (suggested) | `#4f6a45` | Sleeves; success. |

Relationship: warm umbers and ochres in shadow, cool blue only in the distance; light is warm and comes from one side. This style is inherently dark; a light reading means a cream "lit panel" page surrounded by a dark frame, not a white UI.

## Typography

Headings in a Renaissance humanist roman: **Cormorant Garamond 600** or **EB Garamond** (`"Cormorant Garamond", "EB Garamond", Garamond, serif`), small caps for labels with +0.06em tracking. Body in EB Garamond or a calm sans at 17px+. Leonardo's mirror-written notebook script makes a fun easter egg for a single caption (CSS `transform: scaleX(-1)`), never for content.

## Surfaces, texture and edges

- **Overlay layer (bake once):** vignette (α0.3 at 55% radius rising to α0.8 at the edge), yellow varnish at α0.07, canvas weave lines every 3px, ~9,000 random short brush marks at α0.022–0.028 (5–10px wide, random directions), and craquelure at α0.32. For lifted-varnish edges, add the crack image again turned white, offset 1px, at α0.16 with `lighter`. Ship as one PNG/WebP over the page with `pointer-events: none`.
- **Sfumato on imagery:** draw the image twice: a blurred copy (`filter: blur(1.6px)`) fully opaque, plus a sharp copy at α0.82 on top. Soft edges, readable features. The background can take a heavier `blur(1.3px)` once.
- **Light pool:** a soft-light radial gradient where the main content sits; darken everywhere else.
- **Light shaft:** a wedge pre-blurred at ~28px and added with `screen`; never a hard-edged polygon.

## Shapes and layout

Gently rounded or square panels framed like paintings (a thin ochre inner line, a dark wide outer frame). No outlines on illustrations. Shadows are soft and deep. Compositions are calm and centred, often with a window or arch opening onto a blue landscape; a one-point perspective grid (receding floor tiles) can anchor a hero. Lots of dark negative space around a lit focal panel.

## Components

- **Buttons:** deep red or umber with cream text and a thin ochre border; hover brightens as if stepping into the light.
- **Cards:** cream lit panels with soft, deep shadows on the dark wall, plus a faint canvas weave.
- **Navigation:** dark rail with small-caps labels; active item lit by a soft warm glow.
- **Inputs:** cream with an ochre bottom line.
- **Tables:** cream panel, hairline ochre rules, small caps headers.
- **Badges:** small ochre or red plaques.
- **Icons:** thin engraved line icons in the style of notebook drawings (sepia ink).
- **Loading:** a Leonardo-style ornithopter flapping, or dust motes rising in a light shaft.
- **Empty state:** a sepia notebook sketch with one line of mirror-script flavour and one line of real copy.

## Motion

- **Ornithopter fly-by:** a bat-wing flying machine (leading-edge spar, 6 radiating ribs, scalloped membrane) crossing slowly while bobbing and pitching ±0.07rad; wing beat `up = 0.5 + 0.5·sin(12t + φ)`, far wing scaled 0.78, darker and half a cycle out of phase. A flat paper-plane shape isn't recognisable; the ribs are.
- **Dust in the light:** 46–70 motes drifting up a light shaft and twinkling.
- **River glints:** ~24 small lights moving downstream and blinking.
- **Cloud shadow:** the light shaft's brightness drifts `0.75 ± 0.35` as if clouds pass the sun.
- **Birds:** a few small distant birds crossing the landscape.
- Pacing is slow and quiet; the style felt too static at first, so give it 2–3 small ambient motions rather than one big one.
- **Reduced motion:** static light shaft, no motes or flyers.

## Techniques

- Perspective floor: `y = horizon + f·h/Z`, `x = centre + f·X/Z` (source: f = 1029, h = 5.75, Z stepping by 1.4 from 16), tiles alternating ochre and red, then a soft-light light pool and a radial darkening around it.
- Aerial perspective: three hill layers from fbm outlines, each farther one bluer and paler; river width grows from 3px to 16px as it approaches.
- Volume without lines: folds as alternating light (`rgba(200,70,60,.3)`) and dark (`rgba(40,0,0,.45)`) gradient strokes; velvet as a light band along the limb (`rgba(170,200,140,.45)`); faces lit from one side with eye-socket shadow.
- Making an illustration fit: no outlines, soft gradient modelling, inner edge darkening kept light (14px at α0.22 rather than 26px at α0.45, which made a white body look like a glass dome).

## Performance and accessibility

- Expensive: live `filter: blur()` on large or animated elements, per-frame overlay generation. Cheap: bake the overlay and blurred backgrounds; animate motes on one small canvas; apply sfumato only to images.
- Contrast: umber `#2a1d12` on cream ≈ 12.9:1; cream on the dark wall `#43321f` ≈ 9.6:1. Deep red `#5c2418` with cream text ≈ 9.6:1. Avoid mid-browns for text; the varnish overlay lowers contrast, so keep it out of text panels or under 0.1 opacity there.
- Never blur text; never put the craquelure overlay over form controls.

## Pitfalls

- Brush texture too dense and noise-directed (26,000 strokes at α0.05) → the wall becomes Van Gogh swirls; 9,000 strokes, α0.025, random directions.
- Blur on the subject heavier than on the background → depth of field reads inverted; keep the subject sharper.
- Floor tiles too bright and contrasty → vector-illustration look; darken around a single light pool.
- Light shaft drawn as a hard polygon → fake; pre-blur it.
- Too little motion → feels like a still; add motes, glints and a slow flyer.

## Nearest neighbours

- **32 · Rembrandt chiaroscuro:** also dark with one light source, but harder light-dark contrast, candle and window light multiplied over the image, thicker paint. Leonardo's sfumato is softer and adds blue aerial distance.
- **39 · Piero della Francesca / 40 · Raphael (research recipes):** also Renaissance, but clear daylight, geometric order and crisp volumes rather than smoky shadow and varnish.

## Source

Adapted from huashu-art-motion · `06_renaissance.md` (built ★★; noted shortcoming: the cat illustration was too cartoonish and the overall scene too static).
