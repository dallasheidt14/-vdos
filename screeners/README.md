# screeners/

One subfolder per **SCREENABLE** mechanism — each holding a pipeline
that turns a public corpus into a ranked shortlist of candidates.

**CHECKLIST mechanisms do NOT get a folder here.** Over-engineering a
screener for a judgment call is a documented failure mode of this
project — see `../PROCESS.md` Step 1. A checklist mechanism is worked
by judgment: skip straight to landmines and costly-signal validation.

## Layout

One directory per screenable mechanism, in build order:

- `onet/` — **mechanism 1, downmarket unbundling** (O*NET → BLS OEWS
  wages). First to build; **not built yet.**
- geographic arbitrage (Census CBP) — later
- free IP → product (USPTO / PatentsView) — later
- unread corpus → insight (county records, dockets, …) — later

Track which are started/killed in `../LEDGER.md` → Mechanisms status.

## Every screener follows the standing rules

- **Cheap deterministic rules before any LLM pass.** Stage the cut so
  you never spend tokens on millions of raw records.
- **The demand join is mandatory.** Join the corpus to independent
  proof people actually buy the thing — it's what separates a list from
  a business.
- **Weekend-replicable = feature, not business.** If the core is
  rebuildable in a weekend against the same APIs, the moat is the
  assembly, not the code.

See `../MECHANISMS.md` for each mechanism's corpus and screen design.
