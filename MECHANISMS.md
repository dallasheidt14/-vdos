# MECHANISMS

Eight ways to make money where AI is the enabling piece. Not eight
ideas — eight *machines* that produce ideas.

Shared shape:

    cheap/free input  x  expensive-to-evaluate filter  x  independent demand signal

The middle term is what AI changed. The corpora were always public;
reading them at scale wasn't possible. The third term is what everyone
skips, and it's the whole thing — anyone can list 3M expired patents,
the value is joining them to proof that people buy that object.

**Status key:** SCREENABLE = corpus exists, build a pipeline.
CHECKLIST = judgment over an enumerable list, do NOT build a pipeline.

---

## 1. Downmarket unbundling — SCREENABLE ⭐ START HERE

Take a service gated behind an expensive licensed human and sell it to
the tier that was priced out. Not cheaper for existing buyers — a new
customer tier that could never buy at any price.

**Corpus:** O*NET (onetcenter.org)
- 19,000+ occupation-specific task statements
- 2,000+ detailed work activities
- Free API, quarterly updates, CC-BY licensed
- CareerOneStop joins O*NET tasks -> BLS OEWS wages (national/state/
  local) + employment projections

**Screen:**
1. Pull all occupations with tasks + wages + employment
2. Rules filter: median wage above threshold, employment above
   threshold (proves the service is bought at scale)
3. LLM pass per task statement: is this routine information work?
   what artifact does it produce? what's the incumbent price?
4. Score: (wage x employment) / task routineness
5. Output: ranked list of expensive services with the specific task to
   unbundle already named

**Why first:** smallest corpus, cleanest structure, output is a ranked
list of services rather than a list of markets to go research.

---

## 2. Geographic arbitrage — SCREENABLE

A business that works in one market and doesn't exist in another.
Used to require living in both.

**Corpus:** Census County Business Patterns (CBP)
- Establishments, employment, payroll by 2-6 digit NAICS
- Geography: national / state / county / MSA / CSA / ZIP /
  congressional district
- Free API, annual series back to 1986
- Complement: BLS QCEW (adds government, quarterly, from UI records)

**Screen:**
1. Establishments per capita, same NAICS, across all MSAs
2. Flag high-variance categories (e.g. 40 per 100K in one metro, 3 in
   another)
3. LLM pass: is the gap climate/regulation/culture (artifact) or
   genuine absence (opportunity)?
4. Historical depth gives the transition version: which NAICS are
   growing fastest in which metros

---

## 3. Free IP -> product — SCREENABLE

Expired patents, public domain books/art/music, lapsed designs. AI's
job is the SCREEN, not the building. ~90% of patents lapse early,
mostly unpaid maintenance fees, so the pool is enormous and mostly
junk.

**Corpus:** USPTO bulk data / PatentsView. Also Gutenberg, museum open
access, Internet Archive.

**Screen:**
1. Filter to utility patents lapsed for unpaid maintenance
2. Cheap rules FIRST (no LLM): CPC codes in consumer-product classes,
   has drawings, reasonable claim count. Must cut ~3M -> ~50K before
   any token spend
3. LLM pass: abstract + first claim + drawing description ->
   {what it is, materials, mfg complexity 1-5, tooling required, unit
   cost tier, product category}
4. Auto-kill: electronics, safety certification, tooling over budget
5. Demand join: category -> Amazon sales volume + seller count
6. Score: demand / mfg complexity, penalized by seller count

**Traps:** patent expiry frees the invention, NOT the brand — the
trademark is often where the value was. Most patents lapsed because
the product didn't sell, so the demand join is mandatory, not
optional.

---

## 4. Unread corpus -> insight — SCREENABLE

Documents nobody reads because it was never worth a human hour.
Permits, dockets, filings, inspection reports, meeting minutes.

**Corpus:** county/municipal records, court dockets, UCC-1 filings,
Certificate Transparency logs, trademark filings, NIH/NSF grant
awards, import manifests.

**Note:** county-level fragmentation IS the moat here. One API call is
not a business; joining 3,000 counties is eighteen months of ETL that
a weekend competitor can't replicate.

**Screen:** per-source, no universal pipeline. Score sources by
(records per year) x (decision value per record) / (acquisition
difficulty).

---

## 5. Trigger -> artifact — CHECKLIST (aiming is SCREENABLE)

Detect a signal that predicts a want, then show the person their own
thing with the want filled.

**The origin cases:** car wrap (event trigger — vehicle purchase),
pool (state trigger — no pool in a pool-dense ZIP).

**Trigger taxonomy:**

| Type | What it is | Example |
|---|---|---|
| State | a condition that persists | no pool in a pool-dense ZIP |
| Event | a discrete moment | bought a car, closed on a house |
| Transition | a state *changing* | pool appeared since last imagery |
| Countdown | a clock running out | roof permit dated 2004 |
| Mismatch | two facts that don't fit | $900K home, 25-yr-old roof |
| Peer delta | you vs. everyone around you | only house on block without X |
| Sequence | one purchase predicts the next | new pool -> fence, furniture |
| Exposure | something happened to an area | hail path, flood map redraw |
| Declared intent | announced publicly | permit filed, job posted, RFP |

**Underworked:** transitions (nothing diffs two imagery snapshots) and
countdowns (permit dates = end-of-life prediction before the owner is
thinking about it).

**Surfaces beyond satellite:** legal/regulatory (permits, licenses,
dockets), financial (UCC-1, grants, SEC), digital (SSL certs via CT
logs, DNS, job posts), trade (import manifests), human (obituaries,
probate).

**Required refinement — the density check:** the absence must be
anomalous against the LOCAL norm. No pool in Scottsdale is strange;
no pool in Portland is Tuesday. Compute prevalence by ZIP and only
fire where the absence is a minority state. The unspoken message
becomes "everyone around you has this."

**Aiming is screenable via CBP:** establishments by employment size
class = buyer fragmentation; payroll per establishment = ticket size
proxy. Ranks where to point a play.

**Legal landmine:** DPPA killed the original car-wrap trigger source
(DMV records restricted for marketing). RESPA reshaped the mortgage
version. Always ask what statute governs how these people may market,
share data, or compensate each other.

---

## 6. Format arbitrage — CHECKLIST

Valuable content stuck in the wrong format. Manuals -> video. Text ->
audio. Dense PDF -> searchable tool. Lectures -> structured courses.
Source is often free or cheaply licensable; AI does the conversion at
zero marginal cost.

---

## 7. Dead asset revival — CHECKLIST (partially screenable)

Expired domains with real backlink profiles, abandoned apps, lapsed
brands with residual search volume.

**Partial corpus:** expired domain drop lists + backlink/traffic data.
Screenable if you pay for the backlink layer.

---

## 8. Claims -> evidence — CHECKLIST (half-screenable)

A market runs on opinion or gamed reviews; substitute computed data.
**This is PitchRank's shape** — which is why it works.

**Half-corpus:** data.gov CKAN API covers 300,000+ datasets, but holds
only METADATA (descriptions, URLs) — not the data. So you can screen
for "an official outcome dataset exists for X." Whether the
consumer-facing rating for X ignores it has no corpus. Stays judgment.

**Candidates that don't make sense (all rare + emotional + high
stakes + existing number is wrong or missing):**
- School ratings — proxy for household income, not value-add; moves
  billions in home prices
- HOA quality — inherit six-figure assessment risk with zero
  information; reserve studies, minutes, litigation all public; NO
  score exists at all
- Landlord quality — reviews gamed; eviction filings and code
  violations are public court records
- Daycare — state inspection reports public and unreadable
- Contractor quality — stars are noise; permit pull rate and
  first-time inspection pass rate are in county records
- Nursing homes — CMS stars gameable, families choose in crisis
- Quote fairness — no comp system for any large home purchase

---

## Build order

1. **Downmarket unbundling** (O*NET) — cleanest corpus, ranked output
2. **Free IP -> product** (USPTO) — biggest corpus, physical output
3. **Geographic arbitrage** (CBP) — same API as trigger aiming
4. **Unread corpus** — pick one source, ETL moat is real
5-8. Checklists, not pipelines. Do NOT build a screener for these.

## Standing rules

- Cheap rules before LLM passes, always. Staging matters more than
  logic — LLM 3M records and you spend four figures before learning
  anything.
- The demand join is not optional. It's the only step that separates a
  list from a business.
- If a developer can replicate the core in a weekend calling the same
  APIs, it's a feature, not a business. Ask what makes it hard to
  assemble.
- Research retrieves published information. Only a costly signal —
  something a stranger does that costs them effort, money, or
  reputation — generates unpublished information. Every live idea
  needs one, plus a kill number.
