# AgenticTrader — Daily Run Logs

This is an **experiment** in growth + momentum investing run by an automated agent
on a Robinhood **cash** account (••••1338, $500 starting capital), now focused on the
**storage / AI-memory sector**. **Not** an investment strategy or financial advice —
it's a research log to study whether an agent can trade growth/momentum signals
competently in a narrow sector.

## Universe

Core: MU (Micron), SNDK (SanDisk), WDC (Western Digital), STX (Seagate), plus an
each-run attempt at SK Hynix via its OTC ADR ticker `HXSCL` (SK Hynix is KRX-listed
with no sponsored US ADR, so this only trades if Robinhood confirms it's tradable —
otherwise it's skipped and logged, never faked). Secondary/backup names — AMAT, LRCX,
MRVL — are only considered if nothing in the core list shows momentum.

## One consolidated pass, run every 30 minutes (all write to the same day's file)

Previously this ran as three separate daily passes (open / mid-day / close), each with
different rules (entries allowed vs. review-only). That's now merged into a single
strategy — decisive exits, selective entries, same-day fast-fail discipline — that runs
**every ~30 minutes from market open (9:30 ET) to close (16:00 ET), 14x/day**.

Because the scheduler's minimum interval for a single cron schedule is hourly, the
half-hour cadence is achieved with **two Routines offset by 30 minutes**, both running
the identical strategy prompt:
- `Storage/AI Momentum Trade (:30 past hour)` — fires 9:30, 10:30, ..., 15:30 ET
- `Storage/AI Momentum Trade (:00 past hour)` — fires 10:00, 11:00, ..., 16:00 ET

Each run prepends its own entry to that day's `logs/YYYY-MM-DD.md` and pushes directly
to `main`. Given the higher frequency, most runs are expected to place zero orders —
anti-overtrading rules (no same-day re-entry of an exited name, at most one new buy per
run) keep the strategy from churning.

**Note:** the cron schedule is pinned to fixed UTC times based on US Eastern Daylight
Time (UTC-4). It will drift by an hour relative to actual market open/close during
Eastern Standard Time (roughly early November to mid-March) unless updated.

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
