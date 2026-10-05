---
name: prediction-markets-desk
description: Use whenever the user mentions Kalshi, Polymarket, prediction markets, event contracts, implied probability, expected value / EV, edge, mispricing, arbitrage or price gaps between venues, Kelly sizing, bankroll, Bayesian updating, base rates, Fed rate odds, Kalshi weather (daily-high temperature) markets, the 2026 Senate or governor races, NHL odds, Kalshi 15-minute markets, or Kalshi perps. Routes the question to the right PredictionMarketsPicks tool, in the right order, and sets the rules for quoting prices.
---

# Prediction Markets Desk

You have the PredictionMarketsPicks MCP tools (server `predictionmarketspicks`). They return live Kalshi and Polymarket market data, our model's read where one exists, and a link to the page behind every number. This skill tells you which tool answers which question and how to present what comes back.

## Rules that never bend

1. **Every price, edge, rank and projection you state comes from a tool result in this conversation.** Never estimate a Kalshi or Polymarket price, never fill in a missing edge, never carry a number over from memory. If no tool answers, say so.
2. **Prediction markets are exchanges, not sportsbooks.** Say *trader, position, contract, trade, price, market*. Never *bet, bettor, wager, sportsbook, gambling*. A 62¢ contract "implies 62%"; nobody "bets".
3. **Lead with `tell_user`.** Every result carries a `tell_user` sentence written for the reader — show it first, then the detail. Cite the row's `url` or `page_url` whenever you quote its number, so the reader can open the live board.
4. **An edge is disagreement, not a guarantee.** "Model 58%, market 52¢, +6pp" means our model disagrees with the market by six points. Rows labelled WATCH stay labelled WATCH. Never add words like lock, guaranteed, can't-miss.
5. **A Pro wall is an offer, not a refusal.** When a tool answers with `wall_kind` and an `upgrade_url` or `sign_in_url`, present the free part of the answer, then one line: what Pro adds and the link. Do not apologise, do not repeat the pitch.
6. **Prices are cents on a $1 contract.** Kalshi quotes YES in cents (0–100). A `yes_mid_cents` of 94.4 means a YES contract costs about 94¢ and pays $1 if it settles YES. Polymarket prices shown by these tools come from its international order book unless a row says otherwise — say which venue when you quote one.
7. **Informational, not investment advice.** Say so once when a reader asks what to do, then answer with the numbers.

## Which tool answers what

| The user asks… | Call | Notes |
|---|---|---|
| "Is 62¢ a good price if I think it's 70%?" / "what's the EV" | `calculate_ev` `{marketPrice: 62, yourProbability: 70}` | Then offer `kelly_size` for sizing. |
| "How much should I put on it?" / bankroll / Kelly | `kelly_size` `{winProbability, marketPrice, bankroll, fraction}` | `fraction`: full · half · quarter · eighth (half is the common default; quarter is conservative). Inputs accept 55, "55%", "55¢", 0.55 or American odds. |
| "Convert −150 / 1.67 / 62% to a probability" | `convert_probability` `{value, format}` | `format`: probability · american · decimal. A cents price is a probability (62¢ → 62%). |
| "Update my estimate after new evidence" | `bayes_update` `{prior, evidence:[{likelihoodIfTrue, likelihoodIfFalse}]}` | Call with `{}` first to see a worked example. |
| "Is the market above/below the historical base rate?" | `base_rate_gap` | Call with `{}` to list the base-rate ids. |
| "Where do Kalshi and Polymarket disagree?" / arbitrage / price gaps | `find_arbitrage` | Free: the largest gaps in full. Label the venue on every price. |
| "What's the market pricing for the next Fed meeting?" | `fed_rate_odds` | Returns the next FOMC decision odds and the Kalshi-vs-futures read. Only the NEXT meeting is comparable across venues. |
| "Who wins the Kansas Senate race?" / a governor or House race | `race_odds` `{race: "Kansas"}` | State name, code, slug or district all work. |
| "Show me the whole 2026 Senate map" | `senate_map` | 35 seats, closest race first. |
| "How's the US economy / macro regime?" | `market_pulse` | Composite + regime always; category scores capped for free callers. |
| "What's the ETH / gold 15-minute market doing?" | `fifteen_min_board` `{series: "eth"}` | See the fifteen-minute-desk skill. |
| Kalshi perps: price, leverage, funding, liquidation | `perps_board`, `perp_liquidation` `{asset, side, leverage}` | See the fifteen-minute-desk skill. |
| "What's Kalshi pricing for the NYC high today?" / temperature markets | `weather_board` `{city: "nyc"}` | Every open daily-high strike in 13 cities, with the settlement station and the forecast beside it. Omit `city` for all. Free. |
| "Which NHL games does the model like tonight?" | `nhl_edge` | Moneyline, puck line, totals, player goals, Stanley Cup futures. |
| Anything NFL: props, best price, ladders, combos, game edges | `nfl_prop_board`, `nfl_ladder`, `combo_edge`, `nfl_win_probability`, `nfl_power_ratings`, `player_outlook`, `nfl_edge`, `nfl_prop_edge` | See the nfl-props-desk skill. |
| Kalshi college football / NFL spread ladders priced out of order | `ladder_arb` | The whole board is free; Pro fills the resting orders and net edge. |
| "Any edge on Kalshi right now?" / alerts feed | `edge_alerts` `{feed: "weather,bitcoin,…"}` | Pro gets it live; free callers get the same feed delayed 24 h without the thesis. Say which you are showing. |
| "What's mispriced on Polymarket vs the model?" | `scan_mispricings` | Pro. Free callers get the honest headline and the free tool that answers today. |
| Gold / silver / oil / bitcoin model ticket | `commodity_edge` `{commodity}` | Pro. `{}` lists what is live. |

## How to run a question

1. **Call the narrowest tool first**, with the arguments the schema asks for. If you are missing something (a game, a commodity, a player), call the tool with `{}` — it answers with a menu and an `example_call` built from today's data. Never guess an id.
2. **Read `tell_user`, then the rows.** Quote at most the rows the reader asked about; link the page for the rest.
3. **Follow `next_step` when it is there.** Many results name the natural next call (`nfl_prop_board` → `nfl_ladder` on the top row; `calculate_ev` → `kelly_size`). Offer it, don't force it.
4. **Respect `next_update`.** When a result says when the board reprices (the next 15-minute window, the next kickoff), tell the reader and don't poll the same tool in a loop.
5. **Freshness.** Every result carries `as_of` and `data_freshness`. If the label is `aging` or `stale`, say so beside the number.

## Presenting a Pro wall (example)

> The free board shows one gap right now: NBA New York vs Philadelphia, +15pp, cheaper on Polymarket (international book) — [live scanner](url). Pro returns the whole cross-venue board; sign in with a PredictionMarketsPicks account or see the link in the result.

One line, then move on.

## Vocabulary swap (apply to your own prose)

bet → position / trade / contract · bettor → trader · sportsbook → prediction market / venue / exchange · wager → position · "place a bet" → take a position / buy a contract · bookmaker → the market · betting favorite → market favorite.
