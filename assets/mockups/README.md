# Mockups

One app screen rendered in twelve art styles. Every mockup uses the same skeleton and content, so the styles can be compared directly: Orbit, a fictional SEO assistant, with a sidebar, one chat turn, a findings table, a "Fix this week" card, follow-up chips and a composer, all in the finished ("done") state at 1280×800. The sample site is `yourbrand.in`.

`overview.png` shows all twelve on one contact sheet.

| id | Style | Files | What it does |
|---|---|---|---|
| 30_matisse | Cut paper + grain | `30_matisse.html`, `30_matisse.png`, `30_matisse-thinking.png` | Scissor-cut paper cards pinned to linen, grain gathered toward one corner, swaying Matisse leaves, and a sticker that flips onto the done task. |
| 17_ink | Ink & seal | `17_ink.html`, `17_ink.png`, `17_ink-thinking.png` | Ink washes instead of boxes, a dry-brush rule and loader, weight low and left with empty air top right, and a red seal stamped on the done task. |
| 35_shinkai | Cinematic light | `35_shinkai.html`, `35_shinkai.png`, `35_shinkai-thinking.png` | Dark room with god rays from the top left, drifting dust (canvas), hard cel shadows and a sunlight slab sliding across the fix card. |
| 25_ghibli | Ghibli watercolour | `25_ghibli.html`, `25_ghibli.png`, `25_ghibli-thinking.png` | See-through watercolour washes with pooled edges, a sky strip with clouds, dappled light, grass that ripples in gusts, and a green bloom around the done task. |
| 01_cave | Cave painting | `01_cave.html`, `01_cave.png`, `01_cave-thinking.png` | Firelit rock wall, ochre and chalk panels with "boiling" charcoal outlines, tally-mark counts, rising embers, and a hand print pressed onto the done task. |
| 14_8bit | 8-bit | `14_8bit.html`, `14_8bit.png`, `14_8bit-thinking.png` | NES-era game screen: pause-menu sidebar, RPG dialog boxes, HUD, pixel sprites, brick floor, stepped motion, and a coin with +1 for the done task. |
| 12_bauhaus | Bauhaus | `12_bauhaus.html`, `12_bauhaus.png` | Mondrian grid with black grout (sidebar column, and a fix card whose tags are red/blue/yellow cells), a red circle, a swinging black rod, sound arcs and a vertical wordmark. |
| 13_pop | Pop halftone | `13_pop.html`, `13_pop.png` | A comic page: black-bordered panels, Ben-Day dot fields, speech balloons, a "Meanwhile…" caption, a Warhol strip of the mascot, and a POW! burst for the done task. |
| 26_vaporwave | Vaporwave | `26_vaporwave.html`, `26_vaporwave.png` | Neon sunset with a slit sun, wireframe mountains, a scrolling perspective grid and a VHS overlay. The text sits in flat grey Windows-95 windows; neon is used only for outlines and the active state. |
| 18_klimt | Klimt gold | `18_klimt.html`, `18_klimt.png` | Brushed gold-leaf ground with leaf seams, spiral, eye and ring ornaments, a black "robe" sidebar with a chequered edge, ivory panels in gold frames, and a shimmer sweep. |
| 29_seurat | Seurat pointillism | `29_seurat.html`, `29_seurat.png` | A canvas renderer that paints a Grande Jatte landscape in pure-colour dots (optical mixing, 8% complements, 6 fps flicker). Panels are flat cream with dotted frames, and the done task gets an orange halo around a blue mark. |
| 31_haring | Keith Haring | `31_haring.html`, `31_haring.png` | Thick, even black lines, flat primaries, dancers that hard-cut between poses on the beat, a radiating heart, and an action-line burst around the done task. |

## Using the HTML files

- Each HTML file is self-contained. Only Google Fonts are loaded from outside. CSS, SVG filters, sprites and JS are all inline, so you can open a file directly in a browser or copy it as a starting point.
- Add `#thinking` to the URL to see the loader state. The six original styles also have a rendered `-thinking.png`.
- Add `still` to the hash (`#still`, `#thinking-still`) to jump one-shot entrance animations to their end state. The PNG captures use it, because headless Chrome renders fewer frames than its virtual clock implies.
- Every file respects `prefers-reduced-motion`: CSS animations stop, and canvas scenes draw one frame.
- The class names in the shared skeleton are identical in every file (`.side`, `.main`, `.thread`, `.u`, `.turn`, `.tools`, `.reply`, `.fix`, `.item`, `.composer`, `.loader`). Each style scopes its rules under one class on `.screen` (`.A`, `.B`, `.BH`, `.PP` and so on).

## Rendering

The PNGs were captured with headless Chrome at 1280×800 and then palette-quantized to keep them small:

```
chrome --headless=new --disable-gpu --hide-scrollbars --window-size=1280,800 --virtual-time-budget=5000 \
  --screenshot=<abs>\<id>.png "file:///<abs>/<id>.html#still"
```

`01_cave` is about 500 KB even after quantization. Its rock and pigment grain doesn't compress well.
