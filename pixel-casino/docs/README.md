# Casino Hub

A browser-based, 8-bit styled casino hub. Static site — HTML/CSS/JS only,
no backend, hosted on GitHub Pages. Wallet and progress persist via the
browser's `localStorage`.

## ⚠️ Read this before moving any files

**Every `.dc.html` file and `support.js` inside `gen_logic/` must stay
together, in that same folder.** This is not a style preference — it's
a requirement of the runtime (`support.js`). When one page imports
another component (e.g. `Home.dc.html` importing `Coin.dc.html`), it
fetches it at a relative path — `./ComponentName.dc.html` — resolved
relative to wherever the current file lives. That's *why* everything in
`gen_logic/` can safely live one level down from the repo root: as long as
they all stay siblings of each other, the relative fetch still works.
Moving just one of them out of `gen_logic/`, or moving `gen_logic/` itself without
updating `index.html`'s redirect, will break things.

Everything that is *not* required by the live site (documentation,
design references) lives in `docs/` and `designs/`, which is safe
because nothing fetches those by name.

## File map

| File | Role |
|---|---|
| `index.html` | Redirects to `gen_logic/Home.dc.html`. Exists only because GitHub Pages looks for `index.html` at the repo root. |
| `gen_logic/support.js` | The runtime. Don't hand-edit — this is generated tooling. |
| `gen_logic/Home.dc.html` | **Page.** Landing screen: welcome banner, game tile grid, wallet state + persistence. |
| `gen_logic/Sandbox.dc.html` | **Page.** A free-play table for testing coins/UI with no win/loss logic. |
| `gen_logic/ComingSoon.dc.html` | **Page.** Placeholder screen for any game tile not built yet. |
| `gen_logic/Coin.dc.html` | **Core component.** One coin's pixel-chip artwork. Change a color or size here and it updates everywhere. |
| `gen_logic/CoinStack.dc.html` | **Core component.** Stacks up to 4 `Coin`s with a ×N badge beyond that. |
| `gen_logic/MintMachine.dc.html` | **Core component.** The up/down currency-exchange UI. |
| `gen_logic/InventoryDrawer.dc.html` | **Core component.** Quick slots + slide-out drawer + backdrop; contains `MintMachine` inside it. |
| `gen_logic/GameTable.dc.html` | **Shared game component.** Betting table, pile layout, trapdoor animation. Reusable by any future game page. |

**The rule of thumb:** a *page* owns wallet state, persistence, and
whatever's unique to that screen. Everything visual and reusable is a
*component* the page imports and hands data to via props — it never owns
state itself, it just reports intent back to the page via callbacks
(`onWalletChange`, `onSelectDenom`, etc.).

## Adding a new game

1. Duplicate `Sandbox.dc.html` as a starting point — it already wires up
   `InventoryDrawer` and `GameTable` correctly. The duplicate goes inside
   `gen_logic/`, next to everything else — see "Why design and logic
   live in the same file" below for why it can't go anywhere else.
2. Give it a distinct wallet-mutation flow for whatever the game actually
   resolves (win/loss), following the same "one function = one pure
   wallet transform" pattern used in `MintMachine.dc.html`.
3. Add a tile for it in `Home.dc.html`'s `gameDefs` array, pointing
   `href` at your new file's exact name.
4. Keep coins, stacks, and the mint machine as `<dc-import>` calls —
   never re-implement their visuals in the new page.
5. **Only if that game needs a genuinely unique, non-interactive asset**
   (a one-off graphic, sound file, or paytable data nothing else uses),
   create `games/<gamename>/` at that point and put it there. Don't
   create empty game folders ahead of time — there's nothing to put in
   them until a game actually needs something unique.

## Why design and logic live in the same file

Each `.dc.html` file is one indivisible unit in this framework — its
visual template and its interactive behavior are bundled together by
design; that's what makes it "one component." There's no way to split,
say, `Coin.dc.html` into a "chip artwork" file and a "click handling"
file that still work together as one coin. Within each file, though,
the two concerns are kept in clearly separated, commented sections (the
color palette and pixel-drawing code at the top, the interactive
`Component` class below) — so editing just the look is still easy to
do in isolation, even though it's not a separate file.

The `designs/` folder is a different kind of separation: it's not "half
of a component," it's a completely standalone reference page that
nothing in the live app ever loads — pure visual reference, safely
independent of everything above.

## Design reference

`designs/PixelAssetReference.dc.html` is the original uploaded asset
sheet (table felt, chip set, playing cards) rendered at large scale for
visual reference. It is **not** part of the live site — nothing imports
it — it's just a place to look at the full pixel-art palette in one
view. See `docs/STYLE_GUIDE.md` for the extracted rules actually in use.
