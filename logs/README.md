# AgenticTrader — Daily Run Logs

This is an **experiment** in growth + momentum investing run by an automated agent at
US market open on a Robinhood **cash** account (••••1338, $500 starting capital).
**Not** an investment strategy or financial advice — it's a research log to study whether
an agent can trade growth/momentum signals competently.

## Two daily passes (both write to the same day's file)

1. **Open pass (~9:30 ET):** growth/momentum — find and enter new positions; manage exits.
2. **Mid-day pass (~12:30 ET):** risk review — re-evaluate currently held positions
   (emphasis on ones opened that morning) and cut/trim what isn't working. No new initiations.

Each pass prepends its own entry to that day's `logs/YYYY-MM-DD.md`.

## Convention

- **One file per run day:** `logs/YYYY-MM-DD.md` (e.g. `logs/2026-06-19.md`).
  If the agent runs more than once in a day, append the new run to the top of that day's file.
- A file is written **every** run — including no-trade days — capturing the full decision
  and *why*, so the experiment is auditable.
- Each file's newest entry ends with an **`ending_value`** field. The next run reads the
  **most recent prior file** in `logs/` to get the baseline for its daily drawdown guardrail.
- Committed and pushed to `main` each run.

## Each daily entry records

1. Date/time, market status, and regime read (risk-on / risk-off)
2. News scan (POTUS comments, AI-sector news)
3. Momentum scan — candidates ranked, with the signals used
4. Current holdings reviewed, each with hold/sell reasoning
5. Orders placed (symbol, side, size, type, limit, order ID) **with the reason**
6. Ending cash / total value + `ending_value` baseline
