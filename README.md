# VDOS — Venture Discovery Operating System

A play scanner. Finds repeatable mechanics — trigger -> artifact ->
channel -> buyer -> pricing — that exist because a business was shaped
around a price that no longer exists.

**Why plays, not niches:** the earlier version scanned industries and
scored niches. But an industry is not portable — everything learned in
one is stranded there. A *play* is portable. The origin case (vehicle
purchase -> AI rendering of the buyer's car in a shop's wrap -> direct
mail -> the wrap shop pays) is not an "automotive" idea; it's a
trigger -> personalized-artifact play that happens to be aimed at wrap
shops. Aim the same machinery somewhere else and it still works. So the
unit of discovery is the play shape, and the industry is just where you
point it. Build the machinery once, swap the industry.

**Core premise:** a 100x price drop doesn't make the old behavior
cheaper — it makes a *different* behavior rational. When custom renders
hit four cents, nobody saves money on mailers; they send a kind of
mailer that didn't exist. When document reading hits two cents, nobody
saves on review; they read all the documents, including the ones no one
has ever read.

## Files

| File | Purpose | Churn |
|---|---|---|
| `PROCESS.md` | The play-scanner spec (v2.0) | rarely |
| `MECHANISMS.md` | The eight screening machines that generate plays — which corpus, how to screen, what's a pipeline vs a checklist | rarely |
| `LEDGER.md` | Running record — play shapes tried, killed cells, live plays, industry notes | every pass |
| `plays/TEMPLATE.md` | Blank pass, mirrors the PROCESS steps | rarely |
| `plays/` | One file per play pass, raw evidence | every pass |

## How to run a pass

1. Copy `plays/TEMPLATE.md` to `plays/<play>.md`
2. Work through `PROCESS.md` steps 1–5
3. Append results to `LEDGER.md` — kills as (play shape x industry)
   cells, survivors as live plays

## Standing rule

The scanner only produces hypotheses. What converts a hypothesis to
knowledge lives in a costly signal, not a corpus. Cap the engine at
"good enough for ten candidates" and spend the time on Step 5
validation.
