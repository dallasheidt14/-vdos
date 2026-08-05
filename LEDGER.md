# LEDGER

Killed cells are as valuable as live ones. The unit here is a CELL — a
(mechanism x industry) pair — not an industry. Killing "self-employed
income analysis" kills one cell (downmarket unbundling x mortgage); it
does not kill downmarket unbundling, which may still work in another
industry.

---

## Mechanisms status

The eight money mechanisms from `MECHANISMS.md`, worked one at a time.
This is the top-level tracker. Status: not started / in progress /
killed.

| # | Mechanism | Type | Corpus | Status |
|---|---|---|---|---|
| 1 | Downmarket unbundling | SCREENABLE ⭐ | Upwork/Fiverr (demand, primary) + O*NET/CareerOneStop (enrichment) | **researched, spec inverted** |
| 2 | Geographic arbitrage | SCREENABLE | Census CBP (+ BLS QCEW) | not started |
| 3 | Free IP → product | SCREENABLE | USPTO / PatentsView (+ Gutenberg, museum OA, Internet Archive) | not started |
| 4 | Unread corpus → insight | SCREENABLE | County/municipal records, dockets, UCC-1, CT logs, trademarks, NIH/NSF, import manifests | not started |
| 5 | Trigger → artifact | CHECKLIST (aiming screenable via CBP) | Trigger taxonomy; CBP to aim | not started |
| 6 | Format arbitrage | CHECKLIST | — (source content, free or licensable) | not started |
| 7 | Dead asset revival | CHECKLIST (partial) | Expired-domain drop lists + backlink/traffic (paid layer) | not started |
| 8 | Claims → evidence | CHECKLIST (half) | data.gov CKAN (metadata only) | not started |

Build order: 1 → 3 → 2 → 4, then the checklists are run by judgment, not
built. Start with mechanism 1's demand scrape (Upwork/Fiverr), NOT
O*NET — the spec was inverted to demand-first (see below).

> Note: the sections below are now filed by mechanism (#1–#8). Four
> legacy kills fall **outside the eight** — three "aggregation" ideas
> (two were really automation, one a marketplace) and one
> "total-coverage" verification play. The old *Aggregation* and *Total
> coverage* shapes had no mechanism equivalent, and that is itself the
> finding: those framings mostly produced automation, not AI-enabled
> money mechanisms, which the reframe now screens out earlier.

### Mechanism #1 — research survivors (not yet validated)

Downmarket unbundling, spec inverted to demand-first (research:
`research/onet-findings.md`). O*NET was demoted from generator to
enrichment because it's a supply-side labor corpus, blind to the
priced-out tier. Three candidates cleared the research screen; each
still needs Step 5 costly-signal validation before it's a live play:

1. **Micro-business HR / compliance docs** — handbooks, offer letters,
   policies for sub-20-employee firms. Incumbent **$1,500–5,000**
   (attorney/consultant). **Least crowded** of the three — the sleeper.
   Landmine: employment-law-adjacent — "not legal advice" framing +
   attorney-reviewed templates.
2. **Standalone financial plans** (not investment management). Incumbent
   **~$3,000** comprehensive plan. Constraint: personalized investment
   advice trips **SEC / state RIA registration** — viable version is
   education + planning frameworks, not "buy VTI."
3. **Small-business plan writing / market analysis.** Incumbent
   **$2,000–6,000**. **Cleanest regulatory profile** of the three;
   priced-out tier is first-time founders and loan applicants.

Dead on arrival for this mechanism (do not re-open): resumes, logos,
document translation, consumer nutrition, generic legal templates,
consumer tax filing — see `MECHANISMS.md`.

---

## 1. Mechanisms tried

Which mechanisms actually produce survivors. Too early to mean much,
but track it. (Numbering matches the Mechanisms status table.)

| Mechanism | Killed cells | Live plays |
|---|---|---|
| #1 Downmarket unbundling | 1 | 1 |
| #4 Unread corpus -> insight | 1 | 0 |
| #5 Trigger -> artifact | 2 | 2 |
| #8 Claims -> evidence | 1 | 1 |
| Outside the eight (automation / aggregation / verification) | 4 | 0 |

Mechanisms #2, #3, #6, #7 not yet tried.

Early read: trigger -> artifact carries the most survivors so far. The
four "outside the eight" kills all died as automation, a marketplace,
or verification against a funded incumbent — exactly the framings the
reframe now screens out before they reach the ledger.

---

## 2. Killed cells — do not re-research

The candidate that died is named so the exact idea isn't re-run, but
each kill is filed under its mechanism so the pattern stays visible.

| Mechanism | Industry | Killed by |
|---|---|---|
| #1 Downmarket unbundling | mortgage | self-employed income analysis — IncomeXpert, Blueprint, Fannie's own calculator |
| #4 Unread corpus -> insight | mortgage | condo / HOA doc review — Rexera (exhibiting at MBA Annual) |
| #5 Trigger -> artifact | youth sports | per-player highlight video — Trace (PlayerFocus), Veo |
| #5 Trigger -> artifact | automotive | vehicle-purchase-triggered mailers — DPPA restricts DMV data for marketing |
| #8 Claims -> evidence | micro-SaaS | verification / anti-fake-MRR — TrustMRR; also bad customer |
| Outside the eight — automation | youth sports | tournament scheduling engine — Fastbreak AI; also automation |
| Outside the eight — automation | youth sports | referee assigning — Refr Sports |
| Outside the eight — marketplace/aggregation | micro-SaaS | micro-acquisition marketplace — Acquire.com, Microns, IndieMaker, Little Exits |
| Outside the eight — verification (total-coverage) | mortgage | wire / title fraud — FundingShield, $3T volume |

---

## 3. Live plays

Each carries a full Step 2 spec and Step 5 validation.

### 3.1 Per-player recruiting fit packet
- **Mechanism:** #5 Trigger -> artifact (secondary: #1 downmarket unbundling)
- **Industry:** youth sports

Step 2:
- Trigger: player finishes a season / a recruiting window opens
- Artifact: a recruiting packet built on the player's actual
  competitive record — opponents faced, opponent strength, rating
  trajectory — matched to programs that actually fit. Target-school
  lists were generic only because custom was expensive; families pay
  $2,000–$5,000 for services the industry admits do work they could do
  free. Strength-of-schedule context is exactly what a college coach
  lacks when a highlight reel arrives.
- Channel: direct to parents, forwarded parent-to-parent
- Buyer: end consumer (parents)
- Pricing: one-off per packet (or per-player seasonal)

Step 5:
- Cheapest artifact: 5 hand-built packets, no software
- Costly signal: a parent forwards it unprompted to another parent
- Kill number: nobody forwards it

Moat: the 77k-team dataset, not the model.
Risk: brutal churn (kids age out), hard seasonality.

### 3.2 Club evidence reports
- **Mechanism:** #8 Claims -> evidence
- **Industry:** youth sports

Step 2:
- Trigger: tryout season / club recruiting cycle
- Artifact: club comparison built on results (evidence) instead of
  reviews (opinion). ClubScout proves demand for club comparison but
  runs on opinion.
- Channel: direct to families; and to club directors for their pitch
- Buyer: two — families at tryout season, and club directors who score
  well and want it in their pitch. Director side is the better
  business — recurring, and clubs already spend to recruit families.
- Pricing: director subscription; family one-off — TBD

Step 5:
- Cheapest artifact: TBD
- Costly signal: TBD
- Kill number: TBD

### 3.3 Per-agent referral artifacts — LANDMINE
- **Mechanism:** #5 Trigger -> artifact
- **Industry:** mortgage

Step 2:
- Trigger: a rate move, or a new/updated listing from the agent
- Artifact: custom per-agent analysis (affordability on *their*
  listings, rate-sensitive pendings) vs the generic rate newsletter
  every LO sends every agent
- Channel: LO -> real-estate agent
- Buyer: the LO (business)
- Pricing: something the agent buys, or a conversation tool the LO uses
  — NOT something handed over as compensation

Step 4 landmine: RESPA Section 8. Giving a referral source something of
value in exchange for referrals is exactly what the statute prohibits.

Step 5:
- Cheapest artifact: none yet — validate the RESPA-safe form first
- Costly signal: an LO puts their name on a hand-built sample and sends
  it to one of their agents (old go signal: LOs admit they send agents
  something nobody opens)
- Kill number: LOs are happy with what they already send

### 3.4 Past-client portfolio monitoring
- **Mechanism:** #1 Downmarket unbundling (total-coverage character: monitors the entire book, not the top 20)
- **Industry:** mortgage

Step 2:
- Trigger: a rate move against a past client's known rate / balance /
  address — every closed loan is a known set of these
- Artifact: a per-borrower artifact across the ENTIRE book, not the top
  20 an LO checks by hand when rates move
- Channel: LO -> past client
- Buyer: independent brokers — the tier never servable at the
  enterprise pricing of Sales Boomerang / Total Expert
- Pricing: subscription — TBD

Step 5:
- Cheapest artifact: TBD
- Costly signal: TBD
- Kill number: TBD

This is the "who can now be served" question, not automation.

---

## 4. Industry notes

Reusable findings about an industry that apply to ANY play aimed there.

**PE-active in 2026 (buyer-consolidation risk).** Only a problem if
YOUR buyers are the ones being rolled up: HVAC, plumbing, electrical,
roofing, pest control, dental, behavioral health, MSP/IT, vet, garage
door, auto repair, accounting, home health, car washes, waste,
insurance brokerage, death care.

**Death care — correction.** Heavily consolidated (Foundation Partners
~250+ locations, StoneMor ~250+, plus SCI, Carriage, Park Lawn). An
earlier ledger entry called it a good vertical; it is not.

**Home services — still founder-led.** ~80% founder-led despite the
roll-ups; 110k+ HVAC contractors, 130k+ plumbing; top 10 hold under
20% share. A fragmented, motivated buyer base (independents losing to
platforms).

**Youth sports.** Fragmented, fast decisions, light regs; consolidating
at the platform layer (Stack + PlayMetrics, 2025). Strong access via
PitchRank / the 77k-team dataset.

**Mortgage.** Heavy regulation (RESPA / TRID), PE-funded incumbents.
Validate cheap here; the durable build may be elsewhere.

**Micro-SaaS.** No gathering point that isn't also the competition, and
no access edge — a delivery mechanism, not a servable industry.

**Regulatory landmines seen.** DPPA — restricts DMV data for marketing
(killed the automotive trigger source). RESPA Section 8 — governs what
a mortgage referral source may be given.

---

## 5. Open search modes

Second-order and structural angles none of the eight mechanisms
surface on their own — run them deliberately:

- **Second-order effects:** what breaks when *everyone* adopts the
  first-order thing
- **Constraint inversion:** for every 100x price drop, name what got
  *more* scarce — the durable money sits there
- **Customer axis:** who was never servable at any price, rather than
  what task got cheaper
- **Firm boundary:** functions moving from internal to buyable, and
  vice versa
- **Forced spending:** regulation creating mandatory purchasing with a
  deadline and no incumbent vendors

---

## Standing note

The scanner only produces hypotheses. What converts a hypothesis to
knowledge lives in a costly signal, not a corpus. Cap the engine at
"good enough for ten candidates" and spend the time on Step 5
validation.
