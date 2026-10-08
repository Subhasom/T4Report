# T4 Report: setup and use

`prompt.json` is the complete recipe for the **T4 Report**, a daily 8-page PDF on hourly volatility windows (the "V-Window") for **MCX Crude Oil Mini** and **MCX Natural Gas Mini**. It holds the scripts, the dark theme, your strikes and the rules for each run, so the AI can rebuild the same PDF with fresh data in any new chat.

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
| 7 | Your strikes against today's estimate, expiry calendar, **Recommendation tool** (clickable link to [https://lcv6.pro/](https://lcv6.pro/)) |
| 8 | Appendix: every hour for both contracts |

The chat reply also gives a short summary: what changed since the last report, a before-and-now strike table, and upcoming events.

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
| `clients` | Notes on running in Claude and in ChatGPT |
| `user_context` | Your watchlist, strikes, expiry dates, event rules |
| `per_run_edits` | The text the AI must refresh each run |
| `qa_checklist` | Checks done before sending the PDF |
| `setup` | Fonts and tools needed |
| `last_run_snapshot` | The last run's key numbers, used as the comparison point |
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
- [EIA Weekly Petroleum Status Report release schedule](https://www.eia.gov/petroleum/supply/weekly/schedule.php)
