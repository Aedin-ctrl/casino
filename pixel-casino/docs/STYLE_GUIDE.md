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

The UI chrome is grayscale — a classic "Windows 95 Minesweeper" look.
Coins are the one deliberate exception: they keep their own distinct
colors (see below) since color is how you tell denominations apart at a
glance.

| Role | Color |
|---|---|
| Page background | `#7b7b7b` |
| Panel / card / button background | `#c0c0c0` (classic Windows gray) |
| Slot / secondary panel background | `#d4d4d4` |
| Input / recessed box background (sunken fields) | `#ffffff` |
| Ink / borders / primary text | `#404040` and `#1a1a1a` |
| Secondary text | `#808080` |
| Body text on the page background | `#000000` |
| Felt green (table surface — unchanged, it's a real card-table color) | `#0a5c36` |

Chip colors (from `Coin.dc.html`, ported directly from the reference
sheet's 8 combos): red / green / blue / gold, each paired with a white
or black accent ring, mapped one-to-one across the 8 coin denominations.
These are intentionally NOT part of the grayscale rule.

## Shapes, borders & bevels

- **No smooth CSS `border-radius` rounding, ever** — real curves read as
  "modern flat UI," not 8-bit. Corners are rounded a different way: a
  `clip-path: polygon(...)` that cuts each corner diagonally, giving a
  chunky pixel-octagon look instead of a smooth curve. Always paired
  with `border-radius:0` (kept for browsers that don't support
  `clip-path`, and as a reminder nothing here uses real radius).
  Three standard cut sizes, by how prominent the rounding should read:
  ```
  /* subtle -- most buttons and panels */
  clip-path: polygon(8% 0,92% 0,100% 8%,100% 92%,92% 100%,8% 100%,0 92%,0 8%);

  /* pronounced -- game frames/surfaces specifically (GameTable) */
  clip-path: polygon(15% 0,85% 0,100% 15%,100% 85%,85% 100%,15% 100%,0 85%,0 15%);

  /* replaces anything that used to be a smooth border-radius:50% circle
     (empty coin-slot placeholders, nav/close buttons, quick-slot rings,
     larger decorative tile icons) -- NOT used on tiny (<18px) decorative
     dots, where a cut corner wouldn't read as anything at that size */
  clip-path: polygon(25% 0,75% 0,100% 25%,100% 75%,75% 100%,25% 100%,0 75%,0 25%);
  ```
  Coins are the one exception: their "roundness" is drawn directly into
  the pixel bitmap in `Coin.dc.html`, not via CSS at all.
- **Windows-95-style raised bevel** on panels/buttons: a solid dark
  outer border, plus a stacked `box-shadow` combining a light inset
  (top-left) and dark inset (bottom-right) to fake a 3D raised edge,
  plus a hard offset drop shadow. The standard recipe used everywhere:
  ```
  box-shadow: inset 1px 1px 0 #fff, inset -1px -1px 0 #808080, 3px 3px 0 #000;
  ```
  (scaled to `2px`/`5px` for bigger panels like the drawer or the game
  table border). Never add blur — the inset lines are what read as a
  bevel, blur would just look like a modern soft shadow.

## Hover feedback

Every clickable button/icon/tile does two things on hover, via
`style-hover="..."`:
1. A small `transform: scale(...)` pop.
2. A **pixel shimmer** — a hard-edged outline that blinks between black
   and white using a 2-step `steps()` animation (`pixelShimmer`,
   defined once per file's `<helmet><style>`), not a smooth glow:
   ```
   outline:2px solid #000;outline-offset:2px;animation:pixelShimmer .5s steps(2,jump-none) infinite
   ```

## Selection / feedback

Coins use a **blinking pixel outline** (`pixelSelectBlink`, a 2-step
`steps()` animation swapping between cream and gold) instead of a smooth
glow — matches how retro UIs flash a selection cursor rather than
fading it. Buttons/tiles use the separate `pixelShimmer` keyframe above
for the same reason — both are deliberately blocky, not eased.

## Adding a new sprite/asset

If you need a new pixel-art element (a new chip variant, a card, a new
icon), regenerate it the same way `Coin.dc.html` does: define the shape
as a 2D array of hex colors (or `'transparent'`) at a fixed small
resolution, then render it as nested `sc-for` loops of small `<div>`
cells sized by a `cellPx` constant. Don't hand-draw pixel art as PNGs
unless there's a reason CSS/JS generation won't work — keeping it as
code means every sprite can be recolored or resized from one place, the
same way the whole coin system already works.
