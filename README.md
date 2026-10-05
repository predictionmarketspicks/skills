# PredictionMarketsPicks — agent skills

Three skills that teach an AI agent to answer Kalshi and Polymarket questions with the hosted PredictionMarketsPicks MCP server: live NFL player-prop prices by venue, cross-venue price gaps, Kalshi weather and 15-minute boards, perps and liquidation math, Fed rate odds, the 2026 Senate map, NHL, and EV / Kelly / Bayes calculators. Free, read-only, no API key.

| Skill | Use it for |
|---|---|
| [`prediction-markets-desk`](skills/prediction-markets-desk/SKILL.md) | Routes any prediction-market question to the right tool and sets the rules for quoting prices. |
| [`nfl-props-desk`](skills/nfl-props-desk/SKILL.md) | The in-season NFL flow: prop board → ladder → combo → win probability → edge tools. |
| [`fifteen-minute-desk`](skills/fifteen-minute-desk/SKILL.md) | Kalshi 15-minute markets and perps: windows, targets, settlement sources, liquidation math. |

## The server these skills call

Add the MCP server to your agent (Streamable HTTP, no key):

```
https://predictionmarketspicks.com/api/mcp/mcp
```

Setup for Claude, ChatGPT, Cursor, VS Code, Gemini CLI and more: https://predictionmarketspicks.com/mcp/setup · Official MCP registry: `com.predictionmarketspicks/quant`.

Claude users can install the skills and the server together as a plugin: https://github.com/predictionmarketspicks/claude-plugin

## Not financial advice

Market data and model reads are informational. An "edge" is where our model disagrees with the market; it is not a guarantee. Trade responsibly.

MIT licensed — see `LICENSE`.
