# T4 Report: how to use `t4report.json`

`t4report.json` is the complete recipe for the **T4 Report**: a daily 8-page PDF on hourly volatility windows (the "V-Window") for **MCX Crude Oil Mini** and **MCX Natural Gas Mini**. It holds the scripts, the dark theme, your strikes and the rules for each run, so Claude can rebuild the same PDF with fresh data in any new chat.

Version 2.2, last updated 7 Oct 2026.

## How to run it

1. Start a chat with Claude where the **TradingView connector** is on.
2. Attach `t4report.json`.
3. Say **"run the report"** (or "run today").

Claude sends back one file: the PDF. It does not send the JSON or this readme again unless you ask.

If you stay in the same chat, you don't need to attach the file again. Just say "run the report".

## What you get

A PDF named `T4.DDMMYYYY.HHMM.pdf`, using the IST time it was generated (for example `T4.07102026.1000.pdf`).

| Page | Contents |
|---|---|
| 1 | Title, data note, headline, the blue V-Window card, "What changed in this run", event clock, first combined chart |
| 2 | Average-hour combined chart, "Continental opens: who moves what" table |
| 3–4 | Crude Oil Mini: price estimate, six metric tiles, three charts, target tiles, session clock, "How often it happened" |
| 5–6 | Natural Gas Mini: same structure |
| 7 | Your strikes against today's estimate, expiry calendar |
| 8 | Appendix: every hour for both contracts |

Claude's reply also gives a short summary: what changed since the last report, a before-and-now strike table, and upcoming events.

## What stays the same every run

- **Theme:** dark page (`#101216`), dark cards, Urbanist font.
- **Colours:** crude red `#f04438`, gas blue `#2f7bff`, lime `#cce15d`, green `#4fee5f`, coral `#ce7979`, V-Window blue `#0056fe`.
- **Layout:** no box around the title, page edges with no white strips, cards that keep their padding and rounded corners when they run onto the next page.
- **Wording rule:** the phrase "not trading advice" never appears.
- **Method:** the V-Window is 17:30–21:30 IST. A swing is an hour's high-to-low range as a percentage of price.

## What changes every run

- Prices, the rupee estimates and every number in the charts and tables.
- The "What changed in this run" card and the event clock.
- The sample dates (the latest 12 or so complete sessions).
- Which of your strikes are in, at or out of the money.

## What is inside the JSON

| Section | What it holds |
|---|---|
| `instructions_for_claude` | The steps Claude follows, including "deliver only the PDF" |
| `output` | File name, page order, deliverables |
| `theme` | Font, every colour, layout rules |
| `data` | Which TradingView symbols to pull and how |
| `analysis` | How each number is calculated |
| `user_context` | Your watchlist, strikes, expiry dates, event rules |
| `per_run_edits` | The text Claude must refresh each run |
| `qa_checklist` | Checks Claude does before sending the PDF |
| `setup` | Fonts and tools needed |
| `last_run_snapshot` | The last run's key numbers, used as the comparison point |
| `files` | The four scripts that build the PDF |

## How to change things

Tell Claude what you want changed, then ask for the JSON to be updated so the change sticks. For example:

- "My November strikes are … update the json."
- "Add a strike" or "remove 7000 PE".
- "Change the window to 18:30–22:30."
- "Update the json" (after any design change, or to refresh the comparison numbers).

You can also edit `user_context` yourself in a text editor: strikes are under `strikes`, dates under `expiries`.

## Things to know

- **MCX data is blocked on your TradingView plan.** The report uses NYMEX WTI crude (`CL1!`) and Henry Hub gas (`NG1!`) as stand-ins, converted to rupees with USD/INR. Rupee figures are estimates, not MCX quotes. Claude re-tests MCX access on every run.
- **Option premiums are not in the report.** Live MCX premiums, volume and open interest need MCX data or your broker's option chain.
- **"What changed" needs a comparison point.** In the same chat, Claude compares with the previous run. In a new chat, it compares with `last_run_snapshot`, which is from the day the JSON was last updated. If that is several days old, the card will describe changes over that whole gap.
- **Expiries.** Crude options expire 15 Oct 2026 and gas options 23 Oct 2026. After each date Claude will ask for your November strikes.
- **Clock changes.** On 25 Oct the UK clocks change, so the London open moves to 13:30 IST. On 1 Nov the US clocks change, so the V-Window moves to 18:30–22:30 IST and MCX closes at 23:55.
- **EIA reports.** Crude inventories are normally Wednesday 20:00 IST and gas storage Thursday 20:00 IST. In a week with a Monday US holiday, the crude report moves to Thursday 21:30 IST. The next case is Thu 15 Oct 2026.
- **Small sample.** The counts ("7/12 sessions") are what happened over about 12 sessions. They are not forecasts.

## If something goes wrong

| Problem | What to do |
|---|---|
| Claude says TradingView is not available | Turn on the TradingView connector and try again |
| The PDF looks different from earlier ones | Say "follow t4report.json exactly" and attach the file again |
| Strikes or expiries are out of date | Give Claude the new ones and say "update the json" |
| You get a rate-limit or data error | Wait a minute and say "run the report" again |
