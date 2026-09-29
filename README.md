# Casino

Every casino and card-game tool in one place, live at
**[casino.aedinlai.com](https://casino.aedinlai.com)** (also under Projects on
[aedinlai.com](https://www.aedinlai.com)).

| Folder | What it is |
|---|---|
| `index.html` | The hub: Games, Advantage Games and Calculators tabs |
| `blackjackpractice/`, `pokerpractice/`, `baccaratpractice/`, `crapspractice/`, `roulettepractice/`, `mahjongpractice/` | Practice games (with bankroll betting) |
| `blackjackadvantage/`, `pokeradvantage/`, `baccaratadvantage/`, `mahjongadvantage/` | Advantage-play trainers |
| `blackjackhelp/`, `pokerhelp/`, `baccarathelp/`, `mahjonghelp/` | Calculators |
| `pixel-casino/` | Pixel-art casino game prototype (July 2026; formerly the `casieno` repo) |

## History of the old repos

These tools used to be 11 separate repos (`blackjackpractice`, `blackjackhelp`,
`pokerpractice`, `pokerhelp`, `baccaratpractice`, `baccarathelp`,
`crapspractice`, `roulettepractice`, `mahjongpractice`, `mahjonghelp`, and the
old tile hub `webpage`). Their full commit history is merged into this repo,
so nothing was lost when they were removed:

```
git log -- history/pokerhelp        # the old pokerhelp repo's commits
git show <commit>:history/pokerhelp/index.html
```

The current version of each tool is the folder above (newer than the old repos).
Old links such as www.aedinlai.com/pokerhelp/ redirect here.
