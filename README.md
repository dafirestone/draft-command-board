# Draft Command — published board

The projection board [Draft Command](https://github.com/dafirestone) fetches at
launch. One file per season.

`board-2026.json` carries every player the app can price: a stat-line
projection, expected games, bye week and rookie flag, split into `priced`
(everyone a room could plausibly draft) and `draftable` (the tail behind them,
so a name can always be found mid-draft).

## Where the numbers come from

ESPN's public projections and draft ranks, FantasyFootballCalculator's ADP over
thousands of real drafts, Sleeper's roster and depth-chart truth, and twelve
seasons of nflverse history — blended, and weighted per position by how well
each source's ordering predicts what real auction rooms actually paid. Every
source is free and needs no account.

## How it is updated

A scheduled job regenerates the board daily during the season, scores it
against six captured real auction rooms, and checks every meaningful price
move against what the feeds now say about that player. **It publishes only
when every mover has a cause** — an injury designation, a club change, a
depth-chart move across the starter line. Anything it cannot account for is
held for a person to look at.

## What the app does with it

The app ships with a board compiled in and treats it as the floor. A fetched
board is used only when it is valid, for the right season, and newer than what
is already there; a truncated download, a wrong-season file or a rollback
serving something older all fall back to the compiled board silently.

Nothing is ever swapped mid-session. What is fetched now opens the *next*
launch, because a board that changed during a live auction would rebuild every
price on screen under someone mid-draft.

## Format

```
season       int          the NFL season these are for
generatedAt  ISO 8601     when the sources were fetched; decides what is newer
source       string       provenance, carried from the generator
priced       [Player]     everyone a room could draft
draftable    [Player]     the tail behind them
byeWeeks     {club: int}  for players carried without one
```

Licensed for use by the app. The underlying data belongs to its sources.
