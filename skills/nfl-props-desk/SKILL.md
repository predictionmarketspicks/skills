---
name: nfl-props-desk
description: Use for any NFL question involving Kalshi — player props (passing/rushing/receiving yards, receptions, touchdowns), best price per venue, spread or win-total ladders, same-game combos, win probability, power ratings, or which games and props the model disagrees with the market on. Runs the PredictionMarketsPicks NFL tools in the order that answers fastest.
---

# NFL Props Desk (in season)

The NFL tools read this week's Kalshi board beside the consensus price from the prediction-market venues and the major books, plus our model (DAEPA) where it has a read. Everything here is in-season; the fantasy draft tools are parked until the 2027 offseason and answer with a redirect if called.

## The order that works

1. **`nfl_prop_board`** — *"where is the best price on Nacua receiving yards"*, *"is Kalshi cheaper than the books"*, *"what's on the prop board this week"*. Filter by `team` (`SEA`), `game` (`ne-sea`), `player` (partial name), or `statType` (`pass_yds`, `pass_tds`, `rush_yds`, `rec_yds`, `receptions`, `anytime_td`). `gapsOnly: true` shows only strikes where Kalshi and the consensus are 5¢+ apart. **A gap is a price difference, not a call.** Free.
2. **`nfl_ladder`** — depth on one player or one game: every Kalshi strike on the ladder beside our probability per strike. Our number is published only between 30% and 70%, where it is measured to be calibrated; outside that band the row is market-only and says so. Shapes: SHAPE (the gap changes sign across the ladder), LOCATION (every rung leans one way), PRICED (under 5pp everywhere), PARTIAL (fewer than 3 model rungs). Free.
3. **`player_outlook`** / **`explain_player`** / **`compare_players`** — one player's strikes and our read per strike; why the model prices a strike where it does; two to four players ranked model-vs-market. If a player has no Kalshi line this week the tool names who is on the board instead. Free.
4. **`combo_edge`** — same-game combos priced with correlation. Call `{nflGame: "ne-sea"}` to list that game's selectable legs and ids, then `{nflGame, legIds: [...]}` to price the exact combo. Call `{}` to see this week's slate. Free.
5. **`nfl_win_probability`** — win probability and projected score for a game. One team → this week's opponent; or pass `game: "ne-sea"`. Free.
6. **`nfl_power_ratings`** — PWR, points per game above an average team on a neutral field, all 32 teams, weekly. Free.
7. **`nfl_edge`** / **`nfl_prop_edge`** — *"which games / props are mispriced"*. Pro. A free caller gets the honest headline (how many positions cleared the gate this week, or that none did) plus the free tool that answers today, then one line with the Pro link. Present exactly that. An empty week is normal: the gate is strict by design.
8. **`ladder_arb`** — Kalshi CFB/NFL spread and total ladders priced out of order (locked, crossed mids, wide books). Whole board free; Pro fills the resting orders, net-of-fee edge and ¼-Kelly size.

## Presenting a prop row

Name the player, the stat and strike, the Kalshi price (net of fee where the row gives it), the consensus, the gap in cents, and the cheapest venue per side — then the `page_url`. Example shape:

> Bijan Robinson rush yds 72.5 — Kalshi YES 54¢ · consensus 58¢ · gap 4¢ · best price on YES: Kalshi. [Board](page_url)

If the row carries our probability (30–70% band), add it as "model 61%" and label the verdict the tool gives (WATCH stays WATCH). Never invent a probability for a market-only row.

## Timing

Boards reprice in the 30 minutes before each kickoff; results carry `next_update` with the next reprice. On Monday and Tuesday the "this week" board is thin until Kalshi lists the next slate — say so and offer the live boards (NHL, 15-minute, Fed) instead of calling the NFL tools in a loop.

## Words

trader / position / contract / price gap. Not bet, bettor, wager, sportsbook, units.
