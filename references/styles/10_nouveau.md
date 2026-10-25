# 10 · Art Nouveau poster (Alphonse Mucha, 1890s)

**Status:** Built ★★★
**Layer:** Period specimen (strongest as tribute or easter egg)
**Web fit:** Medium–High — beauty, tea, wine, florists, theatre, festivals, editorial covers and elegant landing pages. The framed-panel structure maps well onto cards and heroes. Too decorative for dense dashboards; keep ornament at the frame and let content breathe.

## Recognisable traits

- **Whiplash lines:** flowing S-curves whose width swells in the middle and tapers to points, often ending in a spiral.
- Thick outer contour, thin inner lines: each figure or object has a heavier silhouette edge (thicker on one side) and delicate interior drawing.
- Halos and circular frames behind the subject: rings of studs, rosette petals, mosaic bands.
- Soft, muted pastel palette (sage, cream, peach, dusty pink) with gold borders and a warm brown line colour.
- Stylised flowers (poppies, lilies), flowing hair in masses with fine curling strands, lithograph grain.

## Palette

| Role | Hex | Note |
|---|---|---|
| Background | `#b9cba0` → `#cfd3a6` → `#ecc9a2` | Source vertical gradient: sage → pale olive → peach. |
| Surface | `#f6eedb` | Source cream; cards. |
| Text / line | `#4e3320` | Source warm brown line colour. |
| Muted text | `#7a5d44` (suggested) | |
| Gold frame | `#c9a24a` / dark `#9c7a2e` | Borders, rules. |
| Accent (dusty pink) | `#e6aaa5` / fold `#d48e8c` | Highlights, tags. |
| Accent 2 (wrought-iron teal) | `#3d7763` | Links, buttons; also halo teal `#5f9a8a`. |
| Poppy | `#d9706a` / `#b4504b` | Alerts, warm accents. |
| Banner green | `#8fac80` | Section headers. |

Relationship: soft, harmonious, low-contrast pastels held together by brown lines and gold frames; nothing neon, nothing pure black. Dark reading: deep teal or brown ground with gold frames and cream text; keep the pastels for accents.

## Typography

Source title: **Marcellus 400** at 76px with letter-spacing 12px, filled cream with a 7px brown outline behind it. For a more Mucha-like feel, a rounded hand-drawn display face, or custom SVG lettering with soft, swelling strokes (the source notes Marcellus was a stand-in). Fallback stack: `"Marcellus", "Cormorant", Georgia, serif`. Body: a clean, light serif or sans at 17px in brown. Labels: small caps, wide tracking.

## Surfaces, texture and edges

- **Background:** the three-stop gradient plus a polka pattern (ring r10 + dot r2.6, rows 54px apart, columns 62px, odd rows offset 31px, `rgba(120,140,100,.38)`) as an SVG `background-image` tile.
- **Lithograph mottling:** a small fbm image (240×135) stretched over the page at α0.5, plus a warm grain layer at α≈0.14 on top. Stretching a tiny noise image is hundreds of times cheaper than full-size noise.
- **Gold frames:** 6px gold with 2px brown lines inside and outside; use `border` + `outline`, or a double `box-shadow` stack.
- **Mosaic band:** a strip of 20×24px tiles cycling 9 colours in a scrambled order (`(k·7 + (k >> 2)) % 9`).
- **Heavy outer contour:** duplicate the silhouette in brown and offset it underneath ((−1.5, −1.5), (3.5, 4.5), (2, 2.5)), giving a 1.5px edge on top-left and a 4–5px edge bottom-right while inner lines stay 2px. In CSS/SVG: stacked `drop-shadow()` filters in brown, or a brown copy of the SVG behind.

## Shapes and layout

Arched and circular frames (`border-radius: 50% 50% 0 0 / 30% 30% 0 0` for arch-topped panels, circles for halos). Gold borders with brown hairlines. No soft drop shadows; depth comes from the offset brown contour. Layout is a poster: tall framed central panel, a decorative band at the bottom, a title banner, ornament radiating from the subject. Symmetry with organic flourishes at the edges.

## Components

- **Buttons:** teal or dusty pink with cream text, a 2px brown outline and slightly arched top; hover grows a small spiral flourish at one end.
- **Cards:** cream panel with a gold frame and brown hairlines; optional arched top.
- **Navigation:** a banner (`#8fac80`) with cream small-caps labels; active item marked by a gold ring.
- **Inputs:** cream with a brown hairline and gold focus ring.
- **Chat bubbles:** cream with brown outline; user bubble dusty pink.
- **Tables:** cream, brown hairlines, banner-green header.
- **Badges:** small circles with a ring of gold studs.
- **Icons:** whiplash-line icons (variable width, tapered ends).
- **Loading:** a halo of studs rotating, petals counter-rotating.
- **Empty state:** a lily stem curling into a spiral with one line of copy.

## Motion

- **Rotating halos:** an outer ring of 18 studs turning +0.9rad/s, an inner rosette of 32 petals turning −0.63rad/s; a dashed ring with moving `stroke-dashoffset`.
- **Swaying flowers:** heads move `sin(7.5t + φ)·13 + sin(3.1t + 2φ)·5` px, only the upper stem bends, leaves at 0.3× amplitude, left and right clusters in opposite phase.
- **Flowing hair / ribbons:** S-curves with a travelling wave `sin(7.4q + φ − 7t)·(6 + 20q)` (amplitude grows toward the tip), ending in a spiral; the tip follows the head less than the root.
- **Decorative steam:** ribbons (widths 26/20/14) with a wave `sin(4q + φ − 6t)·(18 + 70q)` that spread apart with height and end in 34px spirals.
- **Sunrise:** a sun rising behind a hill band, with leaded-glass rays rotating slowly. A good page-entrance moment.
- **Reduced motion:** halos and flowers still; hair static.

## Techniques

```js
// Variable-width "whiplash" ribbon: offset each point along its normal by w(q)/2 on both sides
function ribbon(pts, w) {            // pts: densified Catmull-Rom points; w(q) width at 0..1
  const L = [], R = [];
  pts.forEach((p, i) => {
    const a = pts[Math.max(0, i - 1)], b = pts[Math.min(pts.length - 1, i + 1)];
    let nx = -(b.y - a.y), ny = b.x - a.x; const d = Math.hypot(nx, ny) || 1; nx /= d; ny /= d;
    const h = w(i / (pts.length - 1)) / 2;
    L.push([p.x + nx * h, p.y + ny * h]); R.unshift([p.x - nx * h, p.y - ny * h]);
  });
  return "M" + [...L, ...R].map(q => q.join(",")).join("L") + "Z";
}
const swell = wmax => q => wmax * Math.sin(Math.PI * q); // thick middle, pointed ends
```

- Spiral endings: radius shrinking linearly to 15% over ~1.2 turns.
- All whiplash elements (stems, leaves, table legs, banner scrolls, hair, steam) use the same ribbon function, which keeps the style coherent.
- Hair: one solid mass first (outer edge waving `sin(6t + 0.035y)·9`), then 7–9 strands of width ≤ 18px tapering to 2.5, each with a cream highlight core and a fine dark-gold line, ending in spirals.

## Performance and accessibility

- Cheap: SVG ribbons generated once, CSS-rotated halos, a stretched noise image. Expensive: regenerating many ribbon paths per frame for waves; limit animated strands to a handful, or animate via a few pre-baked frames.
- Contrast: brown `#4e3320` on cream `#f6eedb` ≈ 10:1, on peach `#ecc9a2` ≈ 7.4:1. Cream on teal `#3d7763` ≈ 4.5:1 (just AA; use bold labels). Gold on brown ≈ 4.8:1 is for decoration, not body text.
- Ornament density can overwhelm; keep it at frames and empty areas.

## Pitfalls

- Line overlays drawn on top of a finished figure → hidden lines (a far sleeve) show through; draw lines in order inside an offscreen layer, then add the heavy outer contour.
- Hair as a few thick bands → sausages or noodles; one mass plus fine strands (≤ 18px) ending in spirals.
- Steam or ribbons rising straight up through a face or text → bend them away from content.
- Uniform stroke widths → loses the swelling Mucha line; always taper.

## Nearest neighbours

- **18 · Klimt golden period:** same era and decorative ambition, but dense gold leaf with spiral and eye patterns. Mucha uses soft pastels, swelling outlines and halos, with gold only as frames.
- **05 · Gothic manuscript:** also ornamental frames, vines and gold, but blackletter on vellum with saturated primaries and no whiplash curves.

## Source

Adapted from huashu-art-motion · `10_nouveau.md` (built ★★★; noted shortcomings: the cat stayed in a stiff generic pose; the title face Marcellus is a stand-in for proper hand-drawn Art Nouveau lettering).
