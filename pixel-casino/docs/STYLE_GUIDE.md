# 8-Bit Style Guide

Extracted from `design/PixelAssetReference.dc.html`. Follow this when
adding new UI so new pieces don't drift from the rest of the game.

## Fonts

- **Display / headlines / section labels:** `'Press Start 2P'` — the
  chunky block font. Used sparingly (it's wide), for things like the
  welcome banner, "Inventory," "Total Bet," button labels.
- **Body / everything else:** `'VT323'` — set once on each page's root
  container so it's inherited by every child element automatically.
  Don't set font-family on individual elements unless overriding to
  Press Start 2P for a header.

Both are loaded via Google Fonts in each file's `<helmet>`:
```html
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=Press+Start+2P&family=VT323&display=swap" rel="stylesheet">
```

## Palette

| Role | Color |
|---|---|
| Page background / darkest ink | `#1a1a1a` (pixel-art black) / `#1f0d10` (page backdrop, warmer) |
| Panel / card background | `#2b1810` |
| Slot / secondary panel background | `#3a2418` |
| Input / recessed box background | `#241209` |
| Primary gold accent | `#d4a017` |
| Secondary gold (borders, labels) | `#c9a227` |
| Body text (cream/parchment) | `#f4ecd8` |
| Felt green (table surface) | `#0a5c36` |

Chip colors (from `Coin.dc.html`, ported directly from the reference
sheet's 8 combos): red / green / blue / gold, each paired with a white
or black accent ring, mapped one-to-one across the 8 coin denominations.

## Shapes & borders

- **No rounded corners** on structural UI (panels, buttons, modals) —
  `border-radius: 0`. Circles only happen where the *pixel art itself*
  draws a circle (coins), never via CSS `border-radius` on a square box.
- **Chunky borders:** `3px`–`6px` solid `#1a1a1a`, sometimes doubled with
  a gold ring via `box-shadow` (see the table's border in `GameTable.dc.html`
  for the reference pattern: `border` + two stacked `box-shadow` rings).
- **Shadows are hard-edged, never blurred.** Always `Npx Npx 0 #000` —
  zero blur radius. This is the single most important rule for staying
  "8-bit" rather than "flat modern dark mode."

## Selection / feedback

Coins use a **blinking pixel outline** (`pixelSelectBlink`, a 2-step
`steps()` animation swapping between cream and gold) instead of a smooth
glow — matches how retro UIs flash a selection cursor rather than
fading it.

## Adding a new sprite/asset

If you need a new pixel-art element (a new chip variant, a card, a new
icon), regenerate it the same way `Coin.dc.html` does: define the shape
as a 2D array of hex colors (or `'transparent'`) at a fixed small
resolution, then render it as nested `sc-for` loops of small `<div>`
cells sized by a `cellPx` constant. Don't hand-draw pixel art as PNGs
unless there's a reason CSS/JS generation won't work — keeping it as
code means every sprite can be recolored or resized from one place, the
same way the whole coin system already works.
