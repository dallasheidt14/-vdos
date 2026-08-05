# VDOS — Venture Discovery Operating System

A niche scanner. Finds businesses shaped around a price that no longer
exists.

**What changed from v2.0:** the original architecture tried to forecast
where technology and markets would be in 3–12 months, then derive
opportunities from that forecast. That was dropped. Forecasting is the
hard part and the least valuable part — and an opportunity you can see
six months out is one everyone else can see too. Predictable futures
are crowded futures.

This version does the opposite: it notices the present faster than
other people, using evidence that already exists.

**Core premise:** a 100x price drop doesn't make the old behavior
cheaper — it makes a *different* behavior rational. When custom renders
hit four cents, nobody saves money on mailers; they send a kind of
mailer that didn't exist. When document reading hits two cents, nobody
saves on review; they read all the documents, including the ones no one
has ever read.

## Files

| File | Purpose | Churn |
|---|---|---|
| `PROCESS.md` | The locked v1.0 spec | rarely |
| `LEDGER.md` | Running record — niches scored, candidates killed and live | every pass |
| `niches/` | One file per pass, raw evidence | every pass |

## How to run a pass

1. Copy `niches/TEMPLATE.md` to `niches/<niche>.md`
2. Work through `PROCESS.md` steps 0–6
3. Append results to `LEDGER.md`

## Standing rule

The scanner only produces hypotheses. The information that converts a
hypothesis to knowledge lives in phone calls, not corpora. Cap the
engine at "good enough for ten candidates" and spend the time on Step 6.
