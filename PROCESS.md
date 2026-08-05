# VDOS — Play Scanner, Process v2.0

A play is a repeatable mechanic, not a market:

    trigger -> artifact -> channel -> buyer -> pricing

Example (the origin case): vehicle purchase -> AI rendering of that
buyer's car model in the shop's wrap -> direct mail -> wrap shop pays.

Plays port across industries. Industries do not port. Build the
machinery once, swap the industry.

## Core premise
A 100x price drop doesn't make the old behavior cheaper — it makes a
different behavior rational. Plays exist where a business was shaped
around a price that no longer exists.

## Step 1 — Pick a mechanism, then branch on how it's worked
The eight money mechanisms — what each is, its corpus, its screen, and
worked detail — live in `MECHANISMS.md`. Pick one from there. Do not
re-enumerate them here; that list is canonical in one place.

**Branch before doing anything else — the two kinds are not worked the
same way:**

- **SCREENABLE** (a public corpus exists): the work is a pipeline.
  Build it under `screeners/<mechanism>/`, cheap rules before any LLM
  pass, and finish with the demand join. Then continue to Step 2.
- **CHECKLIST** (judgment over an enumerable list): do NOT build a
  screener. Over-engineering a pipeline for a judgment call is a
  documented failure mode of this project. Skip Steps 2–3 and go
  straight to Step 4 (landmines), then Step 5 (costly-signal
  validation).

Disqualifier (both branches): if the mechanism reduces to "they do
their current task faster," discard. That's automation and it's
saturated.

## Step 2 — Specify the play
- Trigger: what detectable event fires it?
- Artifact: what gets produced that was previously too expensive
  per-recipient?
- Channel: how does it reach the recipient?
- Buyer: who pays — the business, or the end consumer?
- Pricing: per-lead, subscription, rev share, one-off?

## Step 3 — Aim it (score candidate industries)
| Factor | Good | Bad |
|---|---|---|
| Ticket size to the buyer | high customer LTV | low |
| Trigger data | public or purchasable, legally usable | restricted |
| Incumbents FOR THIS PLAY | nobody | funded and shipping |
| Buyer fragmentation | many small buyers | few, or one PE sponsor |
| Channel legality | unconstrained | governed by statute |

Note: PE presence in an industry only matters if YOUR BUYERS are
consolidated. Selling into a fragmented base of independents is fine
even when roll-ups are active — often better, since the independents
are losing to platforms and are motivated.

## Step 4 — Landmines (both are required)
- LEGAL: what statute governs how these people may market, share data,
  or compensate each other? (DPPA killed the original car-wrap trigger
  source; RESPA reshaped the mortgage version.)
- WHY NOT ALREADY: if it's obvious and missing in a funded space,
  someone tried. Find out why before building.

## Step 5 — Validation by costly signal
Do NOT use "call N people" as the test. The requirement is one costly
signal: something a stranger does that costs them effort, money, or
reputation.
Every live play must carry:
- Cheapest artifact (what can be handmade this weekend, no software)
- Costly signal (the specific stranger behavior that means continue)
- Kill number (the specific result that means stop)

Research retrieves published information. A costly signal generates
unpublished information. Only the second kind is an edge.

## Standing rules
Apply to every mechanism (the first two mainly to SCREENABLE ones):

- **Cheap rules before LLM passes, always.** Stage the screen so
  deterministic filters cut the corpus down before any token spend.
  LLM-ing millions of raw records burns four figures before you learn
  anything.
- **The demand join is mandatory, not optional.** Joining the corpus
  to independent proof that people buy the thing is the only step that
  separates a list from a business.
- **Weekend-replicable = feature, not business.** If a developer can
  rebuild the core in a weekend against the same public APIs, the moat
  is the assembly (fragmented ETL, licensed data, distribution), not
  the code. Name what makes it hard to assemble.
- **Costly signal + kill number for every live idea** (see Step 5).
  Research only retrieves published information; a costly signal
  generates the unpublished kind.

---

See `MECHANISMS.md` for the eight mechanisms — what each is, which
public corpus feeds it, how to screen it, and which are pipelines vs
judgment checklists.
See `LEDGER.md` for play shapes tried, killed cells, and live plays.
See `plays/TEMPLATE.md` to run a new play pass.
