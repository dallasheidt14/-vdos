# MECHANISMS

Eight ways to make money where AI is the enabling piece. Not eight
ideas — eight *machines* that produce ideas.

Shared shape:

    independent demand signal  x  cheap/free filter corpus  x  expensive-to-evaluate judgment

AI changed the middle term — the corpora were always public, reading
them at scale wasn't. But the FIRST term is what generates an idea.
Anyone can list 3M expired patents; the value is joining them to proof
that people buy that object — and that proof has to come first, or you
rank junk.

**Demand-first rule — applies to every SCREENABLE mechanism.**
Supply-side corpora (O*NET, BLS, Census) *filter* candidates. They
never *generate* them. Demand evidence generates; supply data narrows.
If a screen starts from a government database, it's built backwards. So
the demand join is the FIRST filter, not the last: supply data is blind
to the priced-out tier by definition — those people aren't transacting,
so no labor or revenue statistic counts them. A supply corpus can
describe the incumbent; only demand evidence finds who's priced out.

**TWO CONSECUTIVE KILLS, SAME CAUSE.** O*NET (mechanism #1) and expired
patents (mechanism #3) both died because the screen started from a
supply corpus. Supply data describes what exists and what it costs to
produce. It is structurally blind to what anyone wants. Before
researching any mechanism, state where the DEMAND evidence comes from.
If the answer is "we'll figure that out after the screen," the
mechanism is already dead.

**Status key:** SCREENABLE = corpus exists, build a pipeline.
CHECKLIST = judgment over an enumerable list, do NOT build a pipeline.

---

## 1. Downmarket unbundling — SCREENABLE ⭐ START HERE

Take a service gated behind an expensive licensed human and sell it to
the tier that was priced out. Not cheaper for existing buyers — a new
customer tier that could never buy at any price.

**Spec inverted by research** (`research/onet-findings.md`). The
original wage x employment / routineness screen started from O*NET and
measured the labor market an employer pays for — not the consumer
market priced out of a service. O*NET is now enrichment, not the
generator.

**PRIMARY corpus (demand): Upwork / Fiverr gig listings.** Gig prices
are revealed prices for the UNBUNDLED task — direct proof a market
exists below the professional rate. If freelancers already sell "the
task" at $X, that's your price floor and your proof of demand. No
official API; scrape or manually sample. This is the generator.

**ENRICHMENT corpus (supply): O*NET + CareerOneStop.** Used only AFTER
a demand signal exists, to answer "what credential gates this task."
- O*NET (onetcenter.org): ~18,800 occupation-specific task statements,
  ~2,000 detailed work activities, CC BY 4.0, bulk download or v2 API.
- CareerOneStop pre-joins O*NET tasks -> BLS OEWS wages + projections,
  and exposes an **occupational-licensing API** (license requirements
  by state/occupation) — join it as a COMPUTED regulatory field.
- Join gotcha: roll O*NET-SOC up to 6-digit SOC before joining OEWS
  wages (many-to-one; fall back to the broad-SOC aggregate on null).
  OEWS excludes the self-employed, so it understates advisor / agent /
  lawyer populations — use OOH "number of jobs" for those.

**Screen (demand-first):**
1. DEMAND: sample Upwork/Fiverr for unbundled tasks selling at 5–20x a
   plausible AI delivery cost, with live gigs and rising "[service]
   cost" search interest. No revealed price, no candidate.
2. GATE — Job Zone, not routineness: keep only tasks whose occupation
   is Job Zone 4–5 (credential-gated). The gate is what creates a
   priced-out tier. Do NOT score on routineness — the AI-exposure
   literature already strip-mined O*NET tasks for automatability, so
   "the LLM says it's routine" is a dead edge.
3. REGULATORY (required field, not an afterthought): join CareerOneStop
   occupational licensing and name the governing statute. Regulation is
   the real filter, not crowding. Law / money / medicine -> see the
   DoNotPay warning before building.
4. ARTIFACT: LLM pass on survivors only — name the concrete
   deliverable, classify consumer-facing vs internal, flag UPL /
   licensing exposure.
5. Score: (consumer price x priced-out population x demand signal) x
   artifact deliverability / regulatory risk. Every term but the last
   comes from OUTSIDE O*NET.

**Still START HERE** — the priced-out-tier thesis is the cleanest of
the eight. Just start from the demand scrape, not the government
database.

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

**Screen (demand-first):**
1. DEMAND: for a category, confirm it's WANTED in the metro where it's
   absent — "[service] near me" search interest, Consumer Expenditure
   category spend. No demand in the target metro, no opportunity.
2. Establishments per capita, same NAICS, across all MSAs; flag
   high-variance categories (e.g. 40 per 100K in one metro, 3 in
   another)
3. LLM pass: is the gap climate/regulation/culture (artifact) or
   genuine absence (opportunity)?
4. Historical depth gives the transition version: which NAICS are
   growing fastest in which metros

---

## 3. Free IP -> product — SCREENABLE

Public-domain IP where AI does the value-add and there is no factory,
no container, and no competitor who can suspend your listing. The
surviving versions are digital or print-on-demand — NOT expired-patent
manufacturing (killed variant below).

**PRIMARY: public-domain books / audio -> Kindle KDP + ACX / Audible.**
Zero inventory, zero tariffs, zero factory, zero takedown-by-competitor
risk. AI is the genuine enabler: translation, modernization,
summarization, narration, cover art. Strongest fit for a solo builder.
- Corpus: Project Gutenberg, Internet Archive, LibriVox.
- Demand is checkable DIRECTLY on Amazon's book charts / category BSR —
  the demand-first join is native, not bolted on.
- Screen: category demand on Amazon first -> pick a public-domain work
  whose AI-enhanced edition (new translation, modernized text, fresh
  narration) fills a visible gap -> ship. No physical goods.

**SECONDARY: open-access museum images -> print-on-demand.** Met,
Rijksmuseum, Smithsonian, NYPL open collections -> posters / apparel /
home goods via Printful / Printify on Etsy / Amazon Merch. No inventory,
no tariff. Demand visible via Etsy / Merch search. AI enables curation,
upscaling, mockups.

**KILLED VARIANT — expired patents -> manufacture in China -> Amazon.**
Do NOT resurrect this. Reasons (full analysis:
`research/expired-patents-findings.md`, logged in LEDGER):
- The screen (PatentsView + USPTO maintenance-fee file) is a free-API
  weekend project — fails our own weekend-replication rule.
- Lapse is a NEGATIVE demand signal: ~54% of patents are abandoned for
  unpaid fees because the owner judged them worthless. Cheap patents
  are disproportionately rejected products.
- Patents are legal documents, not build plans — drawings omit
  dimensions deliberately; you still need a designer, CAD, prototype.
- De minimis is gone (China May 2025, global Aug 2025, indefinite June
  2026); ~30–35% tariffs hit a US reseller, not the factory selling
  direct. Chinese sellers are now >50% of Amazon's active base.
- Amazon IP takedowns (RAV/APEX) presume you guilty and are timed for
  Q4; a copy of a formerly-patented product is the exact takedown
  target. ~$10–12.5k single-SKU capital, $5k–50k+ OEM tooling.

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

**Screen (demand-first):** per-source, no universal pipeline. The
record only matters if someone downstream PAYS to act on the decision
it informs — establish that buyer before building any ETL. Then score
sources by (records per year) x (decision value per record) /
(acquisition difficulty).

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

## Dead on arrival — do not re-research

Most are now free features inside ChatGPT / Canva / TurboTax and the
like. A developer replicates the core in a weekend against the same
APIs. Feature, not business — do not open a pass on any of them:

- Resumes / cover letters
- Logos and brand kits
- Document translation (non-certified)
- Consumer meal plans / macro coaching
- Generic legal templates
- Consumer tax filing (1040)
- Private-label Amazon FBA arbitrage for a US solo operator —
  structural cost disadvantage vs Chinese factory-direct sellers
  (>50% of Amazon's active base) plus ~30–35% tariff exposure since
  de minimis ended. Not a "free feature" failure — a structural-cost
  one, but just as dead.

The pattern for the first six: raw generation, no proprietary workflow,
no liability assumed, no regulatory barrier navigated — "I call an LLM
with a nice prompt." The last is different: the economics are lost
before AI enters. Either way, do not open a pass.

---

## Regulatory landmine — the DoNotPay precedent

Standing warning for anything touching **law, money, or medicine.**
DoNotPay ("the world's first robot lawyer") drew multiple
unauthorized-practice-of-law (UPL) suits AND an FTC settlement,
finalized **February 2025**, requiring **$193,000 in monetary relief**
plus notice to its 2021–2023 subscribers. UPL statutes are enforced,
and marketing overreach draws separate consumer-protection liability.

Rule: any candidate touching law, money, or medicine needs **a lawyer's
hour before building** — for "not advice" framing, disclaimers, and
(e.g. financial planning) whether the feature set trips RIA
registration. Licensing status predicts viability better than crowding.

---

## Build order

1. **Downmarket unbundling** — demand from Upwork/Fiverr, O*NET only as
   enrichment (spec inverted — see `research/onet-findings.md`)
2. **Free IP -> product** — public-domain books/audio (KDP/ACX),
   demand checked on Amazon book charts; museum images -> print-on-
   demand secondary. Expired-patent manufacturing KILLED (see #3)
3. **Geographic arbitrage** (CBP) — demand in the absent metro first
4. **Unread corpus** — pick one source, ETL moat is real
5-8. Checklists, not pipelines. Do NOT build a screener for these.

## Standing rules

- **Demand generates, supply narrows.** Supply-side corpora (O*NET,
  BLS, Census) filter candidates; they never generate them. If a screen
  starts from a government database, it's built backwards. The demand
  join is the FIRST filter, not the last — it's the only step that
  separates a list from a business.
- Cheap rules before LLM passes, always. Staging matters more than
  logic — LLM 3M records and you spend four figures before learning
  anything.
- If a developer can replicate the core in a weekend calling the same
  APIs, it's a feature, not a business. Ask what makes it hard to
  assemble. (See Dead on arrival.)
- Anything touching law, money, or medicine gets a lawyer's hour before
  building. The DoNotPay UPL suits + $193K FTC settlement (Feb 2025)
  are the precedent; licensing status predicts viability better than
  crowding.
- Research retrieves published information. Only a costly signal —
  something a stranger does that costs them effort, money, or
  reputation — generates unpublished information. Every live idea
  needs one, plus a kill number.
