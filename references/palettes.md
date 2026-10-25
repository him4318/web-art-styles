# Palettes

Pick one palette per product or look and stay inside it. Lighten, darken or add transparency to its colours as needed, but don't introduce new hues. Each style card also lists its own palette; use these when the style leaves the UI roles open, or as the calm base a style sits on.

## Roles

Every palette has six roles, which map directly to CSS custom properties:

| Role | CSS token | Used for |
|---|---|---|
| `bg` | `--bg` | Page ground |
| `surface` | `--surface` | Cards, panels, paper, inputs |
| `ink` | `--text` | Body text and primary icons |
| `sub` | `--muted` | Secondary text, hairlines, captions |
| `accent` | `--accent` | The one thing you want clicked or noticed on a screen |
| `accent2` | `--accent-2` | A second, rarer emphasis (links, a status, a chart series) |

An accent points at one thing per screen. If everything is accented, nothing is.

## Eight ready palettes

| Name | Suits | bg | surface | ink | sub | accent | accent2 |
|---|---|---|---|---|---|---|---|
| `paper` | Editorial, history, reading, explaining an idea | #F3EDE2 | #FBF8F2 | #1B1A17 | #6B655B | #C8402F | #2F5D62 |
| `poster` | Opinion, campaigns, one bold message | #FFD23F | #FFF6D6 | #111111 | #4A4433 | #FF5A36 | #14213D |
| `ink` | Tech, night, serious topics | #121417 | #1E2226 | #F2EFE8 | #A39E93 | #F2B33D | #5FB3A1 |
| `navy` | Product, data, devices (flat, never a gradient) | #0D1B2A | #16293D | #EEF3F6 | #8FA3B3 | #5CC8E0 | #F2A93B |
| `bauhaus` | Design, structure, geometry, process | #EFE9DD | #FFFFFF | #111111 | #555555 | #D62828 | #1D4E89 |
| `snow` | Winter, health, clean, lab | #E9EEF0 | #FFFFFF | #1F2A30 | #5E6B72 | #D9482B | #3E7C8C |
| `wood` | Lifestyle, kitchen, craft, home | #EDE3D1 | #F7F1E6 | #2B2622 | #7A6E62 | #C4552D | #3F5B4B |
| `chalk` | Classroom, maths, step-by-step | #23302B | #2E3D37 | #F1EFE6 | #A9B3AA | #F4D35E | #E87D5A |

```css
:root[data-palette="paper"] {
  --bg: #F3EDE2; --surface: #FBF8F2; --text: #1B1A17;
  --muted: #6B655B; --accent: #C8402F; --accent-2: #2F5D62;
}
```

## The no-go zone

**Blue-to-purple gradient grounds, deep-purple starfields, neon cyan-and-purple glows and purple frosted glass.** They are the default look of machine-made design, and people read them as "AI-made" at a glance.

Measurable rule: avoid hues between 225° and 300° with saturation ≥ 0.30 for backgrounds and large areas. Flat navy (hue ≈ 210°, as in `navy` above) is fine: it's a deliberate single ground, not a glowing gradient.

When the style itself needs that zone (vaporwave, a cyberpunk brief the client asked for, Van Gogh's night skies), use it and write down why. Even then, keep body text on calm, high-contrast panels.

## Making your own

- **3–6 colours:** one ground, one surface, one text, one muted, one or two accents.
- **Decide light or dark first** and stay there for the whole product, apart from a deliberate theme switch.
- **Texture gives depth, not gradients:** paper grain, film grain or two flat tones. Avoid gradient grounds and neon outer glows unless the style calls for them.
- **Borrow accents from real material:** if you have real photos or screenshots, take one or two accent colours from them and keep the ground from your palette.
- **Supporting both themes:** design the dark version as its own palette with the same relationships (calm ground, readable surface, one accent). Don't just invert. Check the accent on both grounds.
- **Hue-driven accents:** to let one product switch accent colour, define the accent ramp in OKLCH with fixed lightness per step (e.g. `oklch(0.74 0.14 var(--accent-h))`). Contrast then stays close to the original whichever hue is chosen.

## Self-check

Look at five screens side by side:
- Is the ground the same lightness band everywhere?
- Are there only one or two accents?
- Did any screen sneak in "another product's colours", such as a white-ground chart, a purple hero or a neon border?

If so, redraw that screen in the palette.

## Source

Adapted from huashu-art-motion · `references/色板.md`. The palettes and the blue/purple rule come from there; the CSS roles, theme notes and OKLCH accent ramp are additions.
