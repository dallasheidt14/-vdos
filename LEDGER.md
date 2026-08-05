# LEDGER

Killed cells are as valuable as live ones. The unit here is a CELL — a
(play shape x industry) pair — not an industry. Killing "tournament
scheduling" kills one cell (aggregation x youth sports); it does not
kill the aggregation shape, which may still work in another industry.

---

## 1. Play shapes tried

Which shapes actually produce survivors. Too early to mean much, but
track it.

| Play shape | Killed cells | Live plays |
|---|---|---|
| Trigger -> personalized artifact | 2 | 2 |
| Public record -> private insight | 1 | 0 |
| Total coverage | 1 | 1 |
| Evidence replacing claims | 1 | 1 |
| Downmarket unbundling | 1 | 0 |
| Aggregation | 3 | 0 |

Early read: trigger -> artifact carries the most. Aggregation has
fired three times and survived zero — it keeps landing in spaces that
already have a funded platform.

---

## 2. Killed cells — do not re-research

The candidate that died is named so the exact idea isn't re-run, but
each kill is filed under its shape so the pattern stays visible.

| Play shape | Industry | Killed by |
|---|---|---|
| Trigger -> personalized artifact | youth sports | per-player highlight video — Trace (PlayerFocus), Veo |
| Trigger -> personalized artifact | automotive | vehicle-purchase-triggered mailers — DPPA restricts DMV data for marketing |
| Public record -> private insight | mortgage | condo / HOA doc review — Rexera (exhibiting at MBA Annual) |
| Total coverage | mortgage | wire / title fraud — FundingShield, $3T volume |
| Evidence replacing claims | micro-SaaS | verification / anti-fake-MRR — TrustMRR; also bad customer |
| Downmarket unbundling | mortgage | self-employed income analysis — IncomeXpert, Blueprint, Fannie's own calculator |
| Aggregation | youth sports | tournament scheduling engine — Fastbreak AI; also automation |
| Aggregation | youth sports | referee assigning — Refr Sports |
| Aggregation | micro-SaaS | micro-acquisition marketplace — Acquire.com, Microns, IndieMaker, Little Exits |

---

## 3. Live plays

Each carries a full Step 2 spec and Step 5 validation.

### 3.1 Per-player recruiting fit packet
- **Shape:** Trigger -> personalized artifact (also downmarket unbundling)
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
- **Shape:** Evidence replacing claims
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
- **Shape:** Trigger -> personalized artifact
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
- **Shape:** Total coverage (also downmarket unbundling)
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

## 5. Open play shapes

Second-order and structural angles the scanner doesn't surface on its
own — run them deliberately:

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
