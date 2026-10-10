# T4 Report: setup and use

`prompt.json` is the complete recipe for the **T4 Report**, a daily 9-page PDF on hourly volatility windows (the "V-Window") for **MCX Crude Oil Mini** and **MCX Natural Gas Mini**. It holds the scripts, the dark theme, your strikes and the rules for each run, so the AI can rebuild the same PDF with fresh data in any new chat.

`prompt.json` replaces the older `t4report.json`. Version 2.2 (7 Oct 2026); this version label stays fixed when the file is updated.

## What you need

- **A TradingView account on the Essential plan or higher.** Trial plans don't include MCP access.
- **Claude** (any plan, including Free) **or ChatGPT** (Business, Enterprise or Edu; Pro has read-only access, which this report should be enough for).
- **Claude's code and file-creation feature turned on** (in Settings), so it can run the scripts and make the PDF.

The report was built and tested in Claude. See [Using ChatGPT](#using-chatgpt) for its limits.

## Step 1: Connect the TradingView MCP

TradingView's official MCP server (in beta) uses one address for every app:

```
https://mcp.tradingview.com/mcp
```

You sign in with your TradingView account; no API key is needed.

### In Claude (web, desktop and mobile)

You only add the connector once; it then works everywhere you use Claude.

1. Open **Customize → Connectors**. (TradingView's guide calls this Settings → Connectors.)
2. Click **+ Add**, then **Add custom connector**.
3. Name it `TradingView` and paste `https://mcp.tradingview.com/mcp`. Click **Continue**.
4. Keep the detected sign-in settings and click **Continue**. Under **Authentication**, choose **Sign in now**, then click **Add**.
5. Sign in with your TradingView account and allow access.

**Turn it on in a chat:** click **+** at the bottom left of the message box, choose **Connectors**, and switch **TradingView** on.

- **Free plan:** you can add one custom connector.
- **Team or Enterprise plan:** an owner adds it first, under **Organization settings → Connectors → Add → Custom → Web**. You then open **Customize → Connectors**, find TradingView, and click **Connect**.
- **Claude Code (terminal):** run `claude mcp add --transport http mcp-tradingview https://mcp.tradingview.com/mcp`, then `/mcp` to sign in.

### In ChatGPT (web)

1. **Turn on developer mode:** go to **Settings → Apps → Advanced settings → Developer mode**.
   - On Business, only admins and owners can turn it on.
   - On Enterprise and Edu, admins can, and so can members an admin has given developer access.
2. **Create the app:** go to **Settings → Apps → Create**.
   - Enter the name `TradingView` and the server address `https://mcp.tradingview.com/mcp`, and choose OAuth sign-in.
   - Click **Scan Tools**, sign in with TradingView, then click **Create**.
3. **Use it:** in a new chat, pick TradingView from the tools menu or type `@TradingView`. The choice applies to one message only, so mention it again in follow-ups.

**ChatGPT desktop app:** TradingView's own guide uses a different path. Go to **Settings → Plugins → Add → MCPs → Add MCP Server** and fill in:
- Name: `Tradingview Official MCP Server`
- Type: **Streamable HTTP**
- URL: the server address above

Click **Save**, then open **Plugins → MCPs** and click **Authenticate**. Menus differ by plan and app version, so use whichever your app shows.

- **Codex (terminal):** run `codex mcp add tradingview --url https://mcp.tradingview.com/mcp`.
- **Mobile:** ChatGPT doesn't support these apps on mobile.

### Using ChatGPT

ChatGPT can follow `prompt.json` and fetch the data. However, its code environment may not have the browser engine (Chromium) or the internet access the scripts use to draw the PDF and load the Urbanist font. If it can't make the PDF exactly as designed, the file tells it to say so rather than send a different-looking report. For the exact PDF, use Claude.

## Step 2: Run the report with prompt.json

1. Start a new chat with the TradingView connector switched on.
2. Attach `prompt.json`.
3. Say **"run the report"** (or "run today").

You get one file back: the PDF. The JSON and this readme are not sent again unless you ask.

In the same chat you don't need to attach the file again; just say "run the report". If the app asks permission to use TradingView or run code, allow it.

## What you get

A PDF named `T4.DDMMYYYY.HHMM.pdf`, using the IST time it was generated (for example `T4.08102026.1054.pdf`).

| Page | Contents |
|---|---|
| 1 | Title, data note, headline, the blue V-Window card, "What changed in this run", event clock, first combined chart |
| 2 | Average-hour combined chart, "Continental opens: who moves what" table |
| 3–4 | Crude Oil Mini: price estimate, six metric tiles, three charts, target tiles, session clock, "How often it happened" |
| 5–6 | Natural Gas Mini: same structure |
| 7 | Your strikes against today's estimate, expiry calendar, **Recommendation tool** (LCV6, with a clickable link to [https://lcv6.pro/](https://lcv6.pro/)) |
| 8 | Appendix: every hour for both contracts |
| 9 | **T4 Session indicator:** link, chart example, how the sessions map to the report, and why to use it |

The chat reply also gives a short summary: what changed since the last report, a before-and-now strike table, and upcoming events.

## Companion tools

### T4 Session indicator

[T4 Session on TradingView](https://www.tradingview.com/script/3JINBYxx-T4-Session/) is a free indicator by Subhasom Mandal. It marks the Sydney, Tokyo, London and New York sessions on your chart with a range box, a label and each session's high and low. The report tells you which hours usually move; T4 Session shows those hours live on the chart you trade, including MCX futures and options.

| Session | Default hours (IST) | What it means in the report |
|---|---|---|
| Sydney | 02:30–11:30 | Overnight and the MCX morning: the quietest hours for both contracts |
| Tokyo | 05:30–14:30 | Asia open: moves crude before MCX opens and sets the 9:00 gap |
| London | 12:30–21:30 | Europe hours: crude's 12:30–13:30 move |
| New York | 18:30–03:30 | US session: the heart of the V-Window; London and New York overlap from 18:30 to 21:30 |

**Why use it with the T4 Report:**

- **See the V-Window on the chart.** The New York box opens at 18:30 IST, inside the 17:30–21:30 window, and the London and New York overlap is shaded in both colours.
- **Ready-made reference levels.** Each finished session leaves its high and low on the chart, such as the London range going into the US session.
- **Know where the day stands.** The live box and label show which session is running and how far price has already travelled in it.
- **Alerts instead of clock-watching.** A "Session Started" alert for each session, such as London at 12:30 IST and New York at 18:30 IST.
- **Built for the charts you trade.** Works on MCX crude and gas futures and options on any intraday timeframe, and each session's colour, hours, box and lines can be adjusted.

The session times are set in UTC and don't change for daylight saving. After the UK clock change on 25 Oct, set London to 0800–1700. After the US change on 1 Nov, set New York to 1400–2300.

### LCV6

[LCV6](https://lcv6.pro/) is machine-learning trend intelligence for TradingView, by Subhasom Mandal. It compares current conditions with similar past market moments to help read direction, trend changes and activity. It appears in the report as the **Recommendation tool** on page 7.

**What it shows on the chart:**

| Mark | Meaning |
|---|---|
| Buy and Sell triangles | Green below the bar is Buy, red above is Sell. They appear when the model's view changes and the trend line agrees. |
| Trend line | A smoothed line: teal while rising, red while falling |
| Circle and diamond | Orange circles mark the trend turning up; blue diamonds mark it turning down |
| Break | A labelled square where price closes through a trendline drawn from recent swing highs or lows |
| V-Window | A dotted box around a high-volatility stretch, showing its high, low and % move |
| Liquidity Volume bubbles | Green for unusually large volume on up-trading, red for down-trading; bigger means rarer |

**What it does:**

- **Pattern recognition:** finds similar historical conditions and uses what followed them.
- **Directional scoring:** a bullish-to-bearish score shown on the chart.
- **Adaptive context:** uses several market features rather than one fixed pattern.
- **Summary table:** backtests the signals over the loaded bars, filling at the next bar's open and including trading costs.
- **Alerts:** 14 in total, including Buy, Sell, Break, V-Window open and close, and green or red bubbles.
- **Markets:** any TradingView symbol and timeframe; it suits commodities and intraday charts best.

**Trial:**

1. Add **LCV6 Trial** to your TradingView favourites.
2. Add it to a chart. Its summary table shows "Expired or Not Active".
3. Fill in the request form on [lcv6.pro](https://lcv6.pro/).
4. Enter the code you receive under **Settings → Inputs → Activation Code**.

How the trial code works:
- Each code works until the end of its expiry day (India time); after that, request a new one with the same form.
- Alerts stop at expiry and need to be recreated after you enter a new code.
- In the trial, only the V-Window and Liquidity Volume settings can be changed; everything else runs at its defaults.

As the website notes, LCV6 classifies likely trends from past signals and can't guarantee future results. It uses TradingView data only and doesn't see news or sentiment.

## What stays the same every run

- **Theme:** dark page (`#101216`), dark cards, Urbanist font.
- **Colours:**

| Use | Colour |
|---|---|
| Crude | red `#f04438` |
| Gas | blue `#2f7bff` |
| Accent | lime `#cce15d` |
| Good / in the money | green `#C1FF72` |
| Bad / out of the money | coral `#ce7979` |
| V-Window card | blue `#0056fe` |

- **Layout:** no box around the title, page edges with no white strips, cards that keep their padding when they run onto the next page.
- **Wording rule:** the phrase "not trading advice" never appears.
- **Method:** the V-Window is 17:30–21:30 IST. A swing is an hour's high-to-low range as a percentage of price.

## What changes every run

- Prices, the rupee estimates and every number in the charts and tables.
- The "What changed in this run" card and the event clock.
- The sample dates (the latest 12 or so complete sessions).
- Which of your strikes are in, at or out of the money.

## What is inside prompt.json

| Section | What it holds |
|---|---|
| `instructions_for_claude` | The steps the AI follows, including "deliver only the PDF" and "never change the version" |
| `output` | File name, page order, deliverables |
| `theme` | Font, every colour, layout rules (including the recommendation-tool card) |
| `data` | The TradingView MCP address and which symbols to pull |
| `analysis` | How each number is calculated |
| `related_tools` | The T4 Session and LCV6 links and short descriptions |
| `clients` | Notes on running in Claude and in ChatGPT |
| `user_context` | Your watchlist, strikes, expiry dates, event rules |
| `per_run_edits` | The text the AI must refresh each run |
| `qa_checklist` | Checks done before sending the PDF |
| `setup` | Fonts and tools needed |
| `last_run_snapshot` | The last run's key numbers, used as the comparison point |
| `assets` | The T4 Session chart image used on page 9 |
| `files` | The four scripts that build the PDF |

## How to change things

Tell the AI what to change, then say **"update the json"** so the change sticks. The version stays 2.2. For example:

- "My November strikes are … update the json."
- "Add a strike" or "remove 7000 PE".
- "Change the window to 18:30–22:30."

You can also edit `user_context` in a text editor: strikes are under `strikes`, dates under `expiries`.

## Things to know

- **MCX data is blocked on your TradingView plan.** MCX symbols return "permission denied (mcx_free)", so the report uses NYMEX WTI crude (`CL1!`) and Henry Hub gas (`NG1!`), converted to rupees with USD/INR. Rupee figures are estimates, not MCX quotes. MCX access is re-tested every run.
- **Data is delayed.** TradingView's MCP data is delayed by about 15 minutes or more, and the last hourly bar is left out while it is still forming.
- **Rate limit.** The server allows about 100 tool calls per minute; one report uses only a few.
- **Option premiums are not included.** Live MCX premiums, volume and open interest need MCX data or your broker's option chain.
- **"What changed" needs a comparison point.** In the same chat, the AI compares with the previous run. In a new chat, it compares with `last_run_snapshot` (8 Oct 2026 in this file). Say "update the json" now and then to keep it current.
- **Expiries.** Crude options expire 15 Oct 2026 and gas options 23 Oct 2026. After each date the AI will ask for your November strikes.
- **Clock changes.** On 25 Oct the UK clocks change, so the London open moves to 13:30 IST. On 1 Nov the US clocks change, so the V-Window moves to 18:30–22:30 IST and MCX closes at 23:55.
- **EIA reports.** Crude inventories are normally Wednesday 20:00 IST and gas storage Thursday 20:00 IST. In a week with a Monday US holiday, the crude report moves to Thursday 21:30 IST. The next case is Thu 15 Oct 2026, crude options expiry day.
- **Small sample.** The counts ("7/12 sessions") are what happened over about 12 sessions, not forecasts.

## If something goes wrong

| Problem | What to do |
|---|---|
| TradingView isn't available in the chat | Switch the connector on for that chat (Claude: **+ → Connectors**; ChatGPT: pick it or type `@TradingView`) |
| The TradingView sign-in fails or says no access | Check you are on Essential or higher; trial plans are excluded |
| "permission denied (mcx_free)" | Expected on your plan; the report uses NYMEX instead |
| The PDF looks different from earlier ones | Attach `prompt.json` again and say "follow prompt.json exactly" |
| ChatGPT can't make the PDF | Run it in Claude, where the scripts were tested |
| Rate-limit or data error | Wait a minute and say "run the report" again |

## Sources

- [TradingView MCP server setup guide](https://www.tradingview.com/mcp/docs)
- [Claude: get started with custom connectors using remote MCP](https://support.claude.com/en/articles/11175166-get-started-with-custom-connectors-using-remote-mcp)
- [ChatGPT: developer mode and full MCP connectors](https://help.openai.com/en/articles/12584461-developer-mode-and-full-mcp-connectors-in-chatgpt)
- [T4 Session on TradingView](https://www.tradingview.com/script/3JINBYxx-T4-Session/)
- [LCV6](https://lcv6.pro/)
- [EIA Weekly Petroleum Status Report release schedule](https://www.eia.gov/petroleum/supply/weekly/schedule.php)
