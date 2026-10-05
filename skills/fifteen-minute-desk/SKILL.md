---
name: fifteen-minute-desk
description: Use for Kalshi's 15-minute up-or-down markets (bitcoin, ETH, SOL, gold, silver, oil, FX) and Kalshi perpetual futures — which windows are open, the YES price and target, what each settles on, the recent up-rate, perp price, leverage and funding, and where a leveraged perp position gets liquidated. Market data and derived math only; no picks.
---

# Fifteen-Minute & Perps Desk

Kalshi lists a new 15-minute up-or-down contract every 15 minutes on crypto, metals, energy and FX series, and runs perpetual futures with leverage. These tools show the live board and the settlement mechanics so a trader can make their own call.

## Tools

- **`fifteen_min_board`** — every series, or one: `{series: "btc"}` / `{series: "KXETH15M"}` / `{asset_class: "crypto"}` / `{open_only: true}`. Each row: `status`, `window_open`, YES mid/bid/ask in cents (two-sided books only, spread ≤ 15¢; `null` when the book cannot price), `closes_at` and `closes_et`, `target_price`, `contracts_this_window`, `settled_up_share_last_96` (the up-rate over the last 96 windows), `settles_on` (the settlement source — e.g. CF Benchmarks for bitcoin), hours, and `page_url`. Carries `next_update` with the next window open time. Free.
- **`perps_board`** — every Kalshi perp: price, leverage, funding. Free.
- **`perp_liquidation`** `{asset: "btc", side: "long", leverage: 10}` — where a leveraged position is closed out. The answer is earlier than the naive 100 ÷ leverage rule suggests; present the tool's number, not the rule of thumb. Assets: btc · eth · sol · xrp · doge · hype · link · gold · silver · platinum · palladium. Free.
- **`commodity_edge`** `{commodity}` — gold / silver / oil / bitcoin model ticket. Pro. `{}` lists what is live. A free caller gets the honest headline and the free board that answers today.

## How to present a window

> Bitcoin (KXBTC15M): YES 94¢, window closes 2:30 PM ET, target $85,432.83, settles on CF Benchmarks. 48% of the last 96 windows settled up. [Live page](page_url)

- Say the **target** and the **settlement source** every time — the contract settles on the named index at the close, not on whatever exchange the reader is watching.
- A YES at 94¢ near the close means the market sees ~94% that price finishes above the target; it is **not** a 94% chance of profit on a position taken now — the payout is $1 for a 94¢ cost.
- The up-rate over the last 96 windows is a description of the recent record, not a prediction. Do not extrapolate it.
- When `window_open` is false, say when the next window opens (`next_update`) instead of quoting a stale price.
- Do not poll the tool repeatedly inside one window; the board reprices continuously but the contract does not change until the next window.

## Words and limits

No picks, no "lock", no "guaranteed". Prices are Kalshi contract prices. If the reader asks what to do, answer with the numbers and the mechanics (target, settlement, time to close, fee at that price) and let them decide. Informational, not investment advice.
