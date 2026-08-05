# screeners/onet/ — ON HOLD

**Do not build the O*NET pipeline as originally specced.** Research
(`../../research/onet-findings.md`) inverted the mechanism-#1 spec:
O*NET is a supply-side labor corpus and is structurally blind to the
priced-out tier — the exact thing downmarket unbundling hunts. Every
O*NET variable (wage, employment, task) describes the supply of labor
an employer pays for, not a consumer priced out of a service. A screen
that starts from O*NET is built backwards.

## Why it's on hold

- **Wage × employment measures the wrong market** — the labor an
  employer buys, not the consumer market priced out. High employment
  (e.g. bookkeeping clerks, ~1.6M) can be pure B2B back-office labor
  with no consumer service at all.
- **Routineness is a dead scoring axis** — the AI-exposure literature
  already strip-mined O*NET tasks for automatability. No edge there.
- **The corpus cannot see the priced-out tier** — those people aren't
  transacting, so no labor statistic counts them.

## What the demand-first version needs instead

1. **PRIMARY corpus = Upwork / Fiverr gig listings** — revealed prices
   for the unbundled task. No official API; scrape or manually sample.
   This is the generator. Build this scraper FIRST.
2. **O*NET / CareerOneStop as ENRICHMENT only**, to answer "what
   credential gates this task": **Job Zone 4–5** (gated) instead of
   routineness, plus CareerOneStop's **occupational-licensing API** as a
   computed regulatory field.
3. **Score** on `(consumer price × priced-out population × demand
   signal) × artifact deliverability / regulatory risk` — every term
   but the last from OUTSIDE O*NET.
4. **Regulatory gate before any build** for law / money / medicine (the
   DoNotPay precedent: UPL suits + $193K FTC settlement, Feb 2025).

## The O*NET plumbing (for when it's enrichment, not generator)

Kept here so it isn't re-researched. It's ~a weekend of work and can
wait until a demand signal justifies it:

- Bulk-download O*NET 30.x (SQL/CSV) + BLS OEWS national XLSX; load
  `Task Statements`, `Task Ratings`, `Work Context`, `Job Zones`,
  `Occupation Data`.
- Join: roll O*NET-SOC up to 6-digit SOC (truncate `.XX`), left-join
  OEWS, fall back to the broad-SOC aggregate on null. OEWS **excludes
  the self-employed** — use OOH "number of jobs" for advisors / agents
  / lawyers.
- Licensing/attribution: O*NET is CC BY 4.0 (attribution string
  required); CareerOneStop token renews every 36 months.

## Status

`../../LEDGER.md` → Mechanisms status: mechanism #1 = **researched,
spec inverted**. Three research survivors logged there (micro-business
HR/compliance docs, standalone financial plans, small-business plans),
none yet validated by a costly signal.
