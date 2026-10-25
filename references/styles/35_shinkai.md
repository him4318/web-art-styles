# 35 · Shinkai light (Makoto Shinkai anime backgrounds, c. 2016)

**Status:** Built ★★
**Layer:** Material (works on any subject)
**Web fit:** High — product UIs, chat apps and landing pages that want warmth, atmosphere and a "late afternoon" mood behind normal content. Works best when the light stays *behind* the interface; weak for dense data screens if rays and bloom are left at full strength.
**Mockup:** `assets/mockups/35_shinkai.png` (shown with 24 · Hopper hard light, as in the app's "cinema" look)

## Recognisable traits

- Light is the subject: god rays (Tyndall beams) slanting in from one side, drifting lit dust, lens flare and a soft bloom over everything bright.
- Towering cumulonimbus clouds: blue-grey silhouettes with sunlit white cores pushed toward the sun.
- Cel shading on objects: one crisp, hard-edged shadow band offset away from the light, plus a thin warm rim on the lit side — no soft gradient shading.
- Realistic, detailed backgrounds versus clean, flat-shaded figures.
- Warm/cool split: warm amber light against cool shadow and sky blues (a soft-light colour grade).

## Palette

| Role | Hex | Note |
|---|---|---|
| Background, dark ("dusk") | `#131010` | From the app implementation; deep umber, not black. |
| Background, light ("morning") | `#dcd5ca` | From the app implementation; linen. |
| Surface (card), dark | `#1f1a1a` | Mock panel .C. |
| Text, dark | `#ebe8e3` | ~14:1 on `#1f1a1a`. |
| Muted text, dark | `#a39b97` | ~6.3:1 on `#1f1a1a`. |
| Accent (sunlight amber) | `#e6a640` | Send button, active nav glow, running state. Dark text `#1c1819` on it is ~8.3:1. |
| Sky / cloud | `#b7c6e6` → `#8297c6` | Source cloud silhouette gradient. Imagery only. |
| Deep navy | `#2c3a66` / `#1b2446` | Source uniform colours; good for a cool shadow tint. |
| Cel line | `#6a4632` | Source outline colour (warm brown, never black). |
| Warm rim | `rgba(255,220,160,.55)` | Source back-light rim. |

Relationship: warm light, cool shade. Amber is the only saturated UI colour; blues stay in sky imagery or shadow tints. Avoid letting the sky blue become a blue-to-purple UI gradient (the catalogue's "AI slop" warning). Light reading: golden wash and a hard window slab on linen. Dark reading: warm rays and dust over deep umber.

## Typography

A calm serif for display, used thinly so it reads like a film title: **Fraunces 400** or **Newsreader** (`"Fraunces", "Newsreader", Georgia, serif`), with a faint amber text-shadow glow (`0 0 22px` at ~34% amber). Body: a neutral sans (`"Inter", system-ui, sans-serif`) at normal weight. Labels small, sentence case; let the light carry the mood, not the type.

## Surfaces, texture and edges

- **Cards as objects in the light:** a crisp cel shadow offset bottom-right (away from a top-left sun) plus a 1px lit rim on the top-left: `box-shadow: inset 1px 1px 0 var(--rim), 8px 10px 0 -3px var(--shadow)`. Zero blur is what makes it "cel".
- **Sun glow:** a radial gradient anchored just off the top-left corner (`radial-gradient(60% 55% at 4% -6%, …)`).
- **Ground:** a subtle linen texture tile under the gradient, so the dark theme isn't flat black.
- **Bloom:** soft amber `box-shadow` glows only on things that "catch" the sun: the primary button, the active nav item, the focused composer.

## Shapes and layout

Moderate radii (14px) with one tighter corner pointing at the light source on chat bubbles (`14px 14px 4px 14px` for the user, `4px 14px 14px 14px` for replies). Shadows are offset and unblurred, always to the same side. Composition: a single light direction for the whole page; keep the top-left (the "window") lighter and emptier, and let content sit in the falling light. Sidebar can turn slightly translucent on desktop so rays pass across it.

## Components

- **Buttons:** primary in amber with dark text and a bloom glow; secondary as surface objects with a cel shadow; on press, the shadow offset shrinks.
- **Cards:** cel shadow + lit rim. Headlines inside may catch the light (cream `#fff3e0` with a faint glow on dark).
- **Navigation/sidebar:** active item = a warm gradient wash fading to the right, with a small glowing marker.
- **Inputs:** cel shadow at rest; on focus, an amber bloom joins the shadow (`0 0 28px -6px`).
- **Chat bubbles:** see shapes above; replies sit on the surface colour.
- **Tables:** hairline rules at ~7% white; first column in full text colour, rest muted.
- **Badges/tags:** pill outlines at 10% white; "running" tags bloom amber.
- **Icons/mascot:** a warm `drop-shadow` glow; brighter when "done". For an illustration or mascot to belong here: flat fills, one offset hard shadow band, a warm rim on the lit edge and a warm brown line, not black.
- **Loading:** a glowing sun orb (radial cream→amber) pulsing scale 1 → 0.78.
- **Empty state:** an empty wall with the window slab of light and a few motes.

## Motion

- Idle: god rays breathe (opacity 1 → 0.55, 9s ease-in-out); dust motes rise slowly (6–22px/s) with sideways sway, brightest near the light; source rays: `α 0.75 + 0.25·sin(3.4t)`.
- Clouds (hero imagery): drift ~10px/s, each lump breathes radius by `0.05·sin(1.3t)`.
- Flare streaks rotate slowly (source: 0.25rad/s) — hero only.
- Thinking: the sun orb pulses (1.8s); a running tool chip blooms and a hard slab of light sweeps across it (3.2s loop).
- Done moment: one sweep of hard sunlight across the finished card (1.3s), then it lifts 10px and fades.
- Easing is slow and smooth (ease-in-out); nothing bounces.
- `prefers-reduced-motion`: keep the light, drop the movement — static rays and glow, a single still frame of dust, no sweeps or ghosts.

## Techniques

```css
/* God rays: a fan of soft wedges, blurred once (static), only opacity animates */
.rays {
  position: fixed; inset: -20% -10% 10% -30%; pointer-events: none; z-index: -1;
  filter: blur(14px);
  background: repeating-conic-gradient(from 100deg at 8% 0%,
    var(--ray) 0deg 3deg, transparent 3deg 9deg, var(--ray-2) 9deg 11deg, transparent 11deg 17deg);
  mask-image: radial-gradient(75% 75% at 10% 0%, #000 15%, transparent 75%);
  animation: breathe 9s ease-in-out infinite;
}
@keyframes breathe { 50% { opacity: .55 } }
```

- **Dust:** one pre-rendered 32px radial sprite per theme, drawn with `globalAlpha` on a fixed canvas; 18–56 motes depending on viewport area; DPR capped at 2; rAF paused when the tab is hidden.
- **Cel shading for illustrations (source method):** shadow = shape ∩ ¬(shape shifted by (lx·sd, ly·sd)); with light from the upper left (lx −1, ly −0.55), the band falls bottom-right. In SVG: clip a darker copy by the shape, and mask out a translated copy. Offsets: large parts 22–30px, faces 10, small 8; rim 3–9px. Order: fill → shadow → rim → line.
- **Clouds (hero art):** ① every lump as a blue-grey silhouette (`#b7c6e6`→`#8297c6`), ② a white radial highlight of 0.82r offset toward the sun (+0.22r, −0.3r), ③ blur the layer 3px and lay a 55% sharp copy over it. Pre-render to an image.
- **Lens flare:** sun core r ≤ 130; 5 hexagonal ghosts on the sun→centre line at 0.45/0.75/1.25/1.6/2.0, α 0.04–0.12; a horizontal streak ≤ 840px, α 0.35.
- **Bloom (source, for canvas art):** render small (480×270), blur 6px, brightness 1.1, screen it back at α 0.28.
- **Hopper slab (pairs well):** a skewed (−20°) hard-edged band with a mullion gap, drifting via `transform` over 70s.

## Performance and accessibility

- Cheap: a static blurred ray layer (only opacity animates), sprite dust, `box-shadow` cel shadows, transform-only slab drift. Expensive: animating `filter: blur`, full-screen bloom passes, per-frame gradients. Keep the light stage at `z-index: -1` under content (`isolation: isolate` on the app root).
- Contrast: text sits on solid surface colours, never directly on rays or sky. Ratios above pass AA; keep ray alpha low (≤ 0.12 dark, ≤ 0.5 light) behind text areas.
- Motion: breathing and drifting only; all of it stops under reduced motion.

## Pitfalls

- Sun placed at a frame corner spills glow onto the wall like a lamp → keep the sun inside its "window" and the halo radius ≤ 130px.
- One radial gradient per cloud lump plus a "lit arc" reads as soap bubbles → silhouette + offset highlight + blurred/sharp double layer.
- A full-width 1800px flare streak looks like a rendering bug → keep it near the sun (≤ 840px, α 0.35).
- Raindrops drawn as solid circles look like bubbles → draw only the lower rim arc, α 0.22, plus one highlight dot.
- Blue sky turning into a blue/purple UI gradient → keep blue in imagery; UI accent is amber.
- Interior/background density was the noted weakness → give hero illustrations real detail; don't rely on rays to fill emptiness.

## Nearest neighbours

- **24 · Hopper:** also hard window light, but Hopper is still and sparse, with matte paint and no bloom, dust or glow. Shinkai adds atmosphere: rays, motes, flare, bloom and sky.
- **25 · Ghibli:** also anime backgrounds, but Ghibli is watercolour washes with paper edges and gentle daylight; Shinkai is photographic light, lens effects and saturated warm/cool contrast.

## Source

Adapted from huashu-art-motion · `35_shinkai.md` (built ★★; noted shortcoming: interior background density was too low). Web practice comes from building this style (with 24 · Hopper light) as a switchable look in a production web app (see `../web-techniques.md`).
