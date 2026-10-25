# 26 · Vaporwave / neon retro (internet aesthetic, early 2010s)

**Status:** Built ★★★
**Layer:** Material (works on any subject)
**Web fit:** Medium — strong for landing pages, music/creator products, event microsites, a "night mode" easter egg or a 404. Low for long reading and dense product UI: full-screen VHS effects tire the eye and fight text.
**Mockup:** `assets/mockups/26_vaporwave.png`

## Recognisable traits

- Neon tubes: every line is a glowing coloured halo around a near-white core, in magenta and cyan.
- A striped sunset (yellow → orange → magenta disc sliced by horizontal gaps that widen toward the bottom) over a wireframe horizon.
- A perspective grid rolling toward the viewer, palm silhouettes, a marble bust: the motif kit.
- VHS playback artefacts: RGB channel split, a rolling tracking-noise band, scanlines, an on-screen "PLAY" readout and timecode.
- Early-90s computer UI quoted straight: grey Windows-95 dialog with a navy gradient title bar and a block progress bar.

## Palette

| Role | Hex | Note |
|---|---|---|
| Background | `#12052a` / `#2a0b4a` | Source deep purples; wall gradient `#0d0322 → #4a0f6a`. |
| Surface | `#1c0a38` (suggested) | Panels a step lighter than the ground. |
| Text | `#f4ecff` (suggested) | Near-white with a violet tint. |
| Muted text | `#b7a6d8` (suggested) | |
| Accent (magenta) | `#ff4fd8` | Source: main neon. |
| Accent 2 (cyan) | `#3ff6ff` | Source: second neon, floor light strip. |
| Hot pink | `#ff2f8f` | Source: alerts, sunset bottom. |
| Sun yellow / orange | `#ffd84a` / `#ff8a3a` | Source: sunset top and middle. |
| Lavender | `#8f7cf6` | Source: garment fix; good for secondary buttons. |
| Win95 grey / navy | `#c0c0c0` / `#000080` (suggested, standard system colours) | Dialog quotes. |

Relationship: magenta and cyan against near-black purple, with a warm sunset as the only warm patch. This style is the deliberate exception to "avoid purple gradients": it works because the colours are hard neon on black with VHS grit, not a soft indigo-to-violet wash behind a hero. Light reading is weak; if needed, use a pastel variant (pale pink `#ffd6f0` ground, cyan and lavender accents, no glow).

## Typography

- Display: a wide or chrome-ish face — **Monoton** (tube lettering) or **Audiowide** — sparingly. Pixel face **Press Start 2P** for OSD labels ("PLAY ▶", timecode).
- Body: a clean sans (**Inter**, **IBM Plex Sans**) or **IBM Plex Mono** for a terminal feel; never the neon face for paragraphs.
- Wide-spaced half-width labels (`letter-spacing: 0.4em`) give the "Ｖ Ａ Ｐ Ｏ Ｒ" look without relying on full-width glyphs.
- System UI quotes: `"MS Sans Serif", Tahoma, Geneva, sans-serif` at 11–13px inside the dialog only.

## Surfaces, texture and edges

- **Neon line:** stroke the same path twice — a wide coloured stroke with a big glow, then a 2–3px near-white core. In CSS: `box-shadow: 0 0 6px #ff4fd8, 0 0 18px #ff4fd8, inset 0 0 6px #ff4fd8` on a border; on text, `text-shadow` with 2–3 increasing blurs plus `color: #fff6ff`. In SVG: one blurred stroke underneath (`feGaussianBlur`) and a thin white stroke on top.
- **Scanlines:** a 4×4 repeating tile — row 0 black at alpha 0.28, row 2 at 0.12 — as a fixed overlay (`repeating-linear-gradient` works).
- **Glow pools:** radial magenta and cyan glows on the wall behind key areas (window, sign).
- **Floor light strip:** a cyan line along the bottom of the viewport or a section.

## Shapes and layout

Two vocabularies, both hard-edged: neon outlines (rounded tube ends, 2–3px cores) and bevelled Win95 boxes (1px light top-left, dark bottom-right). Content panels are dark with a thin neon border, square or 4–6px radius. Composition: horizon line in the lower third, sun centred above it, grid below; place content above the horizon so the grid stays decorative.

## Components

- **Buttons:** dark fill, neon border and glowing label; on hover the glow brightens and a 1–2px RGB-split offset appears. A secondary style: Win95 bevel button.
- **Cards:** dark panels with a neon edge on one side, or a Win95 window ("TEA.EXE") with a navy title bar for playful callouts.
- **Navigation:** text items with neon underline; active item fully lit, others "unlit tube" (dim violet).
- **Inputs:** dark field with cyan neon border on focus and a blinking block caret.
- **Chat bubbles:** user in a magenta-edged panel, replies in cyan-edged ones.
- **Tables:** thin lavender rules at low alpha; monospace numbers.
- **Badges:** small neon pills.
- **Loading:** Win95 block progress bar (source: blocks = `3 + 14·progress`, percentage `18 + 70·progress`), or a flickering sign.
- **Empty state / 404:** sunset, grid and a bust silhouette with "PLAY ▶" OSD.
- **Mascot/illustration:** give it a rim of neon — magenta on the side facing the light, cyan on the other.

## Motion

- **Sunset stripes rise:** the slices move upward continuously (source 0.9/s) while gap width follows depth (`2 + q²·16` px).
- **Grid rolls forward:** horizontal lines at `y = horizon + K/d`, with `d = k + 1 − phase`, `phase = 1.3t mod 1` (source: horizon 600, K 520). Line width grows near the viewer. CSS shortcut: a `perspective` + `rotateX(70deg)` plane with a repeating gradient whose `background-position` animates.
- **Sign flicker:** each letter goes dark when `hash(floor(30t), k) < 0.07`; one "faulty" letter flickers more (22% chance in a window of 5 of every 17 frames). Unlit = dim purple tube, not invisible.
- **Tracking band:** a ~70px band rolls down the screen, rows inside it shifted sideways by `±30·sin(πy/bh)` plus noise lines.
- **RGB split:** offset `3 + 1.5·sin(7t)` px, occasionally +6.
- **Palms** sway; the OSD "PLAY" blinks every 16 frames.
- `prefers-reduced-motion`: static sunset and grid, no flicker, no tracking band, no chromatic offset; neon glow can stay. Flicker in particular must go — it is a photosensitivity risk.

## Techniques

- **VHS pass, web version:** stack three copies of a hero element (or use one SVG filter with `feColorMatrix` per channel + `feOffset` + `feBlend mode="screen"`) offset a few pixels in R and B. Apply it to a hero image or headline only, never to the whole document.
- **Neon text without the font:** the source drew the katakana sign as polylines in unit (u,v) coordinates, 1–3 strokes per glyph, because the font lacked the glyphs. On the web, draw signs as SVG paths for the same reason — and so the glow looks like bent tube, not type.
- **Sunset slices:** an SVG circle with a vertical gradient and a mask of horizontal bars of increasing height, or `mask-image: repeating-linear-gradient(...)` driven by a CSS variable for the scroll.
- **Two-colour rim on one shape:** stroke the same outline twice, clip one copy to the left half (magenta) and one to the right (cyan).

## Performance and accessibility

- Expensive: whole-page SVG filters, animated blur, `box-shadow` animations on many elements, a per-frame canvas VHS pass (the source effect changed 25% of pixels every frame). Cheap route: VHS on one hero only, scanlines as a static tile, grid via transformed gradient, glow animation via `opacity` of a pre-blurred layer.
- Contrast: white text on `#12052a` is ~18:1. Magenta `#ff4fd8` on deep purple is fine for large display text; for body copy use near-white. Never put body text over the grid or scanline-heavy areas without a solid panel.
- Flashing: keep any flicker under 3 flashes per second and off under reduced motion.

## Pitfalls

- Full-width characters, ▶ and Japanese missing from the font → silent fallback to a system face. Use half-width with letter-spacing; draw ▶ and kana as SVG.
- Outlining a figure part by part → every joint (waist, arm against torso) glows too. For a pure outer rim, outline the merged silhouette (draw it into one shape first, or use an SVG `feMorphology` dilate-minus-source edge).
- Pink garment on pink-lit skin → the figure read as unclothed. Separate adjacent areas by hue, not just value (the fix was a lavender jacket `#8f7cf6`).
- VHS effects on body text → unreadable. Keep text crisp above the effects layer (the source drew its counter above the post-process for this reason).

## Nearest neighbours

- **14 · 8-bit:** also retro digital, but pixel-art with a fixed small palette and stepped motion, no glow; vaporwave is smooth vector neon with bloom and analogue VHS artefacts.
- **15 · Early ray-traced CGI:** also late-80s/90s computer nostalgia with chrome and grids, but rendered as plastic and mirror reflections in daylight colours; vaporwave is night, neon and tape degradation.

## Source

Adapted from huashu-art-motion · `26_vaporwave.md` (built ★★★; noted shortcoming: part boundaries glowed because outlines were drawn per part).
