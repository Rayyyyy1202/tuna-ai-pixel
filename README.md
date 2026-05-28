# TUNA.AI — Pixel Arcade

A single-file pixel-art / arcade-themed landing page for **TUNA.AI** — the agency that builds AI agents which design and ship software for enterprises.

> **Keep moving. Or stop existing.**

This is one design variant in the TUNA.AI exploration: the website is staged as an underwater arcade game, with a cursor-following pixel tuna as the player's companion.

---

## Run it

No build step. Open `index.html` in any modern browser.

```sh
# Mac
open index.html

# Or via a local server (recommended so Google Fonts cache properly)
python3 -m http.server 8000
# then visit http://localhost:8000
```

Or deploy via GitHub Pages (Settings → Pages → Deploy from branch → `main` / root).

---

## What's inside

| File | Purpose |
|---|---|
| `index.html` | The whole site — markup, CSS, JS, sprite SVG, all inline |
| `tuna-logo.svg` | Brand pixel mark (32×17 on an 8px grid, 320 rects) — embedded inside `index.html` and also kept here for asset reuse |
| `BRAND_COPY.md` | Canonical brand document — voice, taglines, hero copy, founder bios. All on-page copy comes from this file verbatim |

---

## Architecture

- **Framework**: none. Plain HTML + CSS + a small chunk of JS.
- **Animation library**: [GSAP 3.13](https://gsap.com) (core + ScrollTrigger), MIT-licensed, loaded via unpkg CDN.
- **Type**: `Press Start 2P` (display) + `VT323` (body), both from Google Fonts.
- **Palette**: locked to ~10 colors — deep-ocean blues, bioluminescent cyan, sand, brand neon. No gradients except in atmospheric layers; hard edges everywhere else.

### Layered ocean scene

A stack of fixed-position layers behind the main content gives the scene its arcade-underwater feel:

1. Gradient ocean (surface → abyss)
2. 4×4 Bayer dither overlay
3. Subtle CRT scanlines
4. Radial vignette
5. Rising pixel bubbles (CSS animation)
6. Bioluminescent specks (step-blink flicker)

### The cursor-following tuna

The brand logo is wrapped inline in `index.html`, with the tail rects (x ≤ 40) grouped under `<g class="tail-group">`. A CSS keyframe rotates the tail group ±7° around the joint at `(48, 64)` in SVG userspace; JS sets a CSS variable `--wag-dur` to adjust wag speed based on cursor velocity.

Behavior:

- Damped follow toward the cursor (lerp ≈ 0.10)
- Velocity-vector rotation, capped at ±26° to avoid going belly-up
- Horizontal scaleX flip on direction change (no roll)
- Tail wag rate snaps to one of four buckets based on speed
- Eye blinks every 3–6 seconds (the `#B4D8EB` highlight rect swaps fill to dark for 110ms)
- Idle behavior: orbit slowly around the last cursor position after 1.2s still
- Touch-device support (`touchmove`)
- Fades out on window blur or mouse-leave; reduced-motion users see no sprite

### Scroll choreography

ScrollTrigger drives:

- A depth meter HUD in the top-right that ticks from `0M` to `4000M`
- A "STAGE 0X" banner that sweeps in when each section enters the viewport
- Per-section content reveals with step-stagger
- A glitch effect on "OR STOP EXISTING." (random char substitution every ~1.1s while in view)
- A 5-digit scoreboard counter that ticks from `00000` to `10000`

### Buttons

Chunky pixel borders + offset shadow. On press: shadow collapses, button shifts 4px down-right, and an 8-direction pixel-spark burst emits from the click point.

---

## Accessibility

- `prefers-reduced-motion: reduce` short-circuits the cursor follow, freezes ambient animations, and reveals all content immediately.
- All atmospheric/sprite layers carry `aria-hidden="true"`.
- Body text uses VT323 at 20-22px — meant to be readable as paragraphs, unlike full-pixel display fonts.

Known gaps (open to PRs):

- No skip-nav link.
- No `:focus-visible` styling on buttons.
- "STAT.01/02/03" headings are `<div>` not `<h3>`.

---

## Brand source

The story, taglines, and founder bios are sourced verbatim from [`BRAND_COPY.md`](./BRAND_COPY.md) v1.0. Voice guide: direct, opinionated, concrete. No "leverage", no "synergize". The tuna metaphor is used once on the surface, then retired.

---

## License

This project bundles GSAP via CDN (MIT). The brand copy, logo, and any TUNA.AI-trademarked assets remain the property of TUNA.AI.
