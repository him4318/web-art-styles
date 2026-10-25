# 33 · Rubber-hose cartoon (American theatrical animation, c. 1928–1935)

**Status:** Built ★★★
**Layer:** Period specimen (strongest as tribute, retro brand, game UI or easter egg)
**Web fit:** Medium — entertainment, games, retro-themed products, playful onboarding, a "vintage mode", loading screens. Low for serious or data-heavy UI: monochrome film grain and constant bouncing compete with content. Describe the general approach; don't reproduce specific studio characters.

## Recognisable traits

- Characters with "rubber hose" limbs: smooth constant-width tubes with no elbows or knees, ending in white four-fingered gloves with flared cuffs.
- Pie-cut eyes: black pupils with a wedge sliced out, on big white eye shapes.
- Everything bounces to the music beat: furniture, flowers, the sun with a face — all alive, all squash and stretch.
- Black-and-white (or sepia) film: grain, flicker, scratches, dust, jitter, a rounded film gate with a vignette.
- Stepped motion "on twos" (12 drawings a second), which gives a slightly jerky charm.

## Palette

| Role | Hex | Note |
|---|---|---|
| Background (paper) | `#f2e8d2` | Source: multiply tint for the faded film look. |
| Surface | `#fbf7ec` | Source: the white of gloves and chests; cards. |
| Text / ink | `#1a1712` (suggested) | Dense black ink. |
| Muted text | `#5e574b` | Source: tabby grey; secondary text. |
| Mid grey | `#a39a84` | Source: the mid tone (stood in for orange). |
| Accent | `#1a1712` on `#fbf7ec` (suggested) | The style has no colour accent; emphasise by value and size. |
| Optional sepia accent | `#9a6a3a` (suggested) | Only if a single tint is unavoidable (e.g. a focus ring). |

Relationship: monochrome — every colour is a value. Everything is desaturated once at the end so no warm colour survives (gloves, cups, buttons too). Dark reading: possible as a projected "film on a dark screen" — a black page with the cream film frame in the middle; the frame content stays light.

## Typography

- Display: a 1930s title-card face — **Limelight**, **Rye** or **Bungee Shade** for a single title (`"Limelight", "Rockwell", serif`); outlined or drop-shadowed white lettering on black reads as a title card.
- Body: **Courier Prime** or a clean serif such as **Libre Baskerville** at regular weight. Keep body away from the grain layer.
- Labels: uppercase, wide tracking, inside a title-card frame (double border, rounded corners).

## Surfaces, texture and edges

- **Paper ground (cached):** vertical wallpaper stripes plus small diamond dots, faint floorboard lines, corners darkened like a soft watercolour vignette.
- **Film layer (overlay):** jitter ±1.5/±2px, a per-frame flicker (white α 0.02–0.09, then black 0–0.06), 3 vertical scratches that move every ~1/6s (so each lives several frames), ~14 dust specks per frame and the odd hair, grain shifted randomly ±20px each frame, and a rounded film gate (radius 60 at 1080p) with a vignette.
- **Desaturate last:** apply `filter: grayscale(1)` (or `saturate(0)`) to the whole illustrated layer, then multiply with `#f2e8d2` — after everything is drawn, so nothing keeps its colour.
- **Outlines:** thick black, constant weight; limbs get a black edge about 10px wider than their fill at 1080p (≈ 2–4px in UI).

## Shapes and layout

Round everything: big circular heads (source radius 82), bean shapes, tubes, rounded corners on all containers (12–24px), and a rounded film gate framing the whole page or hero. No straight edges on organic things (smooth polygons — see Pitfalls). Hard black offset shadows are acceptable as "cel" shadows; no blur. Centred, stage-like composition: content plays in the middle of the frame like a vaudeville stage.

## Components

- **Buttons:** cream pill with thick black outline and black text; on press, squash (scaleX up, scaleY down) and spring back; on hover, bounce once on the beat.
- **Cards:** cream panels with thick outline and rounded corners; title-card style headings.
- **Navigation:** items as small title cards; active one in reversed black with cream text.
- **Inputs:** cream field, thick outline; caret blinks on the beat.
- **Chat bubbles:** round balloons with a thick outline and a curly tail.
- **Tables:** cream panel, black rules; no grain over the table.
- **Badges:** small round medallions.
- **Icons:** round, thick-lined, with gloved hands or pie-cut eyes for personality.
- **Loading:** an object bouncing on the beat, or a film leader countdown.
- **Done moment:** everything on screen gives one squash-and-stretch hop together; a sun with rays spins.
- **Empty state:** a dancing flower (stem swinging ±0.35rad, leaves waving like hands).
- **Mascot:** big round head, pie-cut eyes with 3 lashes, hose arms as a single quadratic Bézier from shoulder to hand at constant width, white gloves (4 fingers, flared cuff, 3 lines on the back).

## Motion

- **Beat bounce:** `bnc(t, phase) = |sin(π(t/BEAT + phase))|`, BEAT = 60/128 s. Each object hops about its own floor contact point; on landing it squashes (`sx = 1 + 0.09k, sy = 1 − 0.09k`), in the air it stretches ~5%.
- **Phase offsets of 0.25 beat** between objects (e.g. 0, 0.25, 0.5, 0.75) so the scene ripples; accessories (bows, tails) lag the body by ~0.05 beat as follow-through.
- **Stepped at 12fps:** all characters and motifs use `t = floor(t·12)/12` (CSS `steps()`); the film layer steps at 24fps.
- Hose tails wave `sin(7q − 9t)·26q`; sun rays rotate 1.6rad/s and the sun nods on the beat; a puff of smoke per beat; curtain hems flick on the beat.
- `prefers-reduced-motion`: no bouncing, no film jitter/flicker/scratches; keep the static grain and vignette. Flicker in particular must go.

## Techniques

```css
:root { --beat: calc(60s / 128); }
.hop { transform-origin: 50% 100%; animation: hop var(--beat) steps(6) infinite; }
.hop:nth-child(4n+2) { animation-delay: calc(var(--beat) * -.25); }
.hop:nth-child(4n+3) { animation-delay: calc(var(--beat) * -.5); }
@keyframes hop {
  0%, 100% { transform: translateY(0) scale(1.09, .91); }   /* landed, squashed */
  50% { transform: translateY(-10px) scale(.97, 1.05); }    /* in the air, stretched */
}
.film::after {                    /* grain + flicker overlay; pre-rendered frames swapped by steps */
  content: ""; position: fixed; inset: 0; pointer-events: none;
  background: url(grain-sheet.png) 0 0 / 400% 100%; mix-blend-mode: multiply;
  animation: grain .33s steps(4) infinite;
}
@keyframes grain { to { background-position: 133.33% 0 } }
```

- Pie-cut pupil: a black circle with an arc wedge removed (source arc −1.45..−0.85rad), in SVG a path with a notch.
- Pre-render the film artefacts (scratch, dust, grain) as a short sprite sheet and step through it; don't generate per frame.

## Performance and accessibility

- Cheap: stepped CSS animations, SVG characters, a sprite-sheet film layer. Expensive: per-frame canvas grain and dust over the full viewport, `filter: grayscale` on animated large layers (apply it to static art or bake it in).
- Contrast: black on cream `#f2e8d2` ~15:1. Keep grain and scratches off text, or at α ≤ 0.08 over it. Muted `#5e574b` on cream is ~6:1, fine.
- Motion and flicker: global bouncing and flicker are the main risks; restrict the beat to decorative elements and moments, keep flicker luminance changes small (< 10%), and turn both off under reduced motion.

## Pitfalls

- Bodies made from polylines → straight edges are obvious in a cartoon. Always smooth curves.
- Desaturating before all elements are drawn → gloves and cups kept a warm tint. Desaturate once, at the end, over everything.
- Most of the "motion" was film noise (40% of the frame changing, mostly grain) → make sure real content moves on the beat; the film layer is seasoning.
- Elbows in hose arms → a single smooth curve from shoulder to hand, constant width.

## Nearest neighbours

- **31 · Keith Haring:** also thick lines and beat-driven bouncing, but Haring is flat saturated colour with featureless figures and radiating ticks; rubber-hose is monochrome film with pie-cut eyes, gloves and grain.
- **34 · Chinese shadow puppet:** also a theatrical, stepped (10–12fps) period medium with a lamp or projector feel, but shadow puppets are translucent carved coloured leather on a glowing screen with stiff riveted joints; rubber-hose is opaque ink with boneless limbs.

## Source

Adapted from huashu-art-motion · `33_rubberhose.md` (built ★★★; noted shortcoming: most of the motion was film noise).
