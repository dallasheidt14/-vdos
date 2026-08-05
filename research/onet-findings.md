# O*NET as a "Downmarket Unbundling" Opportunity Screener: Plumbing, Screen Logic, and a First Pass at Candidates

## TL;DR

- **O*NET is a good *starting* corpus but a mediocre *idea generator on its own*.** It gives you a free, CC BY 4.0-licensed, machine-readable list of occupation-specific task statements (18,796 rows in the current O*NET 30.x Task Statements file) and just over 2,000 Detailed Work Activities joined to wages and employment — but it tells you nothing about consumer prices or unserved demand, which is the actual crux of a downmarket-unbundling bet. Build the pipeline, but treat O*NET as the *supply-side skeleton* and bolt a mandatory demand-join on top.
- **The plumbing is genuinely easy and free:** bulk-download the O*NET database (Excel/CSV/SQL), download BLS OEWS national XLSX for wages+employment, join on SOC. CareerOneStop's free Web API pre-joins O*NET+OEWS+projections and is the fastest single source for a prototype. The join has one real gotcha: O*NET-SOC codes (1,016 titles) are more granular than BLS SOC (~830 OEWS occupations), so you must roll O*NET up to 6-digit SOC before joining wages.
- **Most of the obvious candidates are already crowded** (legal docs, resumes, tax, nutrition, interior design). The interesting residue is in *boring, licensed-but-unbundleable information work* where incumbents charge four figures and the priced-out tier is enormous — but nearly all of the best ones carry unauthorized-practice-of-law/medicine/accounting risk, which is the real filter, not routineness.

-----

## Key Findings

1. **The whole supply-side data stack is free and commercially usable.** O*NET 30.3 is CC BY 4.0 (attribution required, commercial use explicitly allowed). BLS OEWS is public-domain U.S. Government work. CareerOneStop's API is royalty-free but the license expires every 36 months and must be renewed.
1. **O*NET already contains quantitative ratings that partially substitute for an LLM "routineness" judgment** — Task frequency, importance, and relevance ratings, plus the Work Context "Structured versus Unstructured Work" and "Degree of Automation" items, and Job Zones (education/experience gating). Use these first; they're free and deterministic. Use the LLM for the thing O*NET can't give you: *what artifact the task produces and what it's worth to a consumer.*
1. **The screen logic as written is backwards in one important way.** High median wage + high employment identifies *expensive labor bought at scale by employers*, not *services bought by consumers*. Payroll employment ≠ consumer demand. The demand-join is not "critical missing piece #3" — it should be the *first* filter, not the last.
1. **The "weekend replication" test kills more candidates than regulation does.** Resume writing, cover letters, basic logo/graphic design, meal plans, and generic legal templates are all now features inside ChatGPT/Canva/incumbent apps. The defensible-enough opportunities are ones requiring proprietary workflow, trust/liability assumption, or regulatory navigation — not raw generation.

-----

## PART 1 — The Data Plumbing

### 1.1 O*NET database (onetcenter.org)

**Current version:** O*NET 30.3 Database (released 2025; O*NET updates quarterly with the primary update in Q3). It uses the **O*NET-SOC 2019 taxonomy** (1,016 occupational titles, 923 of which are data-level occupations).

**How to get it:** `https://www.onetcenter.org/database.html` → download in Excel, CSV, JSON, SQL (MySQL/Oracle/SQL Server/etc.), or RDF. For a full-corpus screen, **bulk download beats the API** — you want all tasks/ratings/DWAs locally so you can run set-based SQL, not paginate through a REST API 900+ times. The API is better for on-demand per-occupation lookups in a live product.

**Scale of the corpus (current O*NET 30.x, per the data dictionaries):** the Task Statements file contains **18,796 task statements** ("There are a total of 18,796 rows of data in this file"); the DWA framework contains just over 2,000 DWAs; and the **Tasks-to-DWAs mapping file has 23,850 rows** ("There are a total of 23,850 rows of data in this file").  (The frequently cited "19,450 tasks / 2,164 DWAs" figures come from O*NET's 2014 Work Activities Project technical report and reflect an earlier release — the corpus size drifts each quarterly update, so pull the count from the current data dictionary rather than hard-coding it.)

**The files you specifically need** (tab-delimited):

|File                     |What it contains                                       |Key columns                                                 |
|-------------------------|-------------------------------------------------------|------------------------------------------------------------|
|`Occupation Data`        |O*NET-SOC code, title, description                     |O*NET-SOC Code, Title, Description                          |
|`Task Statements`        |The 18,796 occupation-specific task statements         |O*NET-SOC Code, Task ID, Task, Task Type (Core/Supplemental)|
|`Task Ratings`           |Importance, Relevance, Frequency per task              |O*NET-SOC Code, Task ID, Scale ID, Data Value               |
|`Task Categories`        |Frequency category breakdown                           |Task ID, Category, Data Value                               |
|`DWA Reference`          |The Detailed Work Activities                           |Element ID, IWA ID, DWA ID, DWA Title                       |
|`IWA Reference`          |Intermediate Work Activities                           |Element ID, IWA ID, IWA Title                               |
|`Tasks to DWAs`          |Maps task statements → DWAs → occupations (23,850 rows)|O*NET-SOC Code, Task ID, DWA ID                             |
|`Work Context`           |Content Model work-context values per occupation       |O*NET-SOC Code, Element ID, Scale ID, Data Value            |
|`Work Context Categories`|Category anchors for work-context items                |Element ID, Category, Category Description                  |
|`Job Zones`              |Education/experience/training zone (1–5) per occupation|O*NET-SOC Code, Job Zone                                    |
|`Scales Reference`       |29 scales, min/max                                     |Scale ID, Scale Name, Minimum, Maximum                      |
|`Content Model Reference`|Element hierarchy                                      |Element ID, Element Name                                    |

The `Tasks to DWAs` file is your join backbone if you want to screen at the *DWA level* (cross-occupation matching) rather than the raw task level. DWAs are the "intermediate descriptor that allows cross-occupational matching"  — good for de-duplicating the same underlying activity that appears in many occupations.

**Task Type matters for filtering:** O*NET flags each task as **Core** (relevance ≥ 67% AND mean importance ≥ 3.0)  or **Supplemental** (relevance ≥ 67% but importance < 3.0, or relevance < 67%).  Filtering to Core tasks cuts noise substantially before any LLM pass.

### 1.2 O*NET Web Services API

- **Base:** `https://services.onetcenter.org/ws/` — current **API Version 2.0** (v1.9 still supported for legacy apps). Serves the O*NET 30.x database.
- **Auth:** HTTP Basic auth with a username/password issued when you register a project at `services.onetcenter.org`. Free.
- **Rate limits:** O*NET states it does **not currently impose a hard maximum rate limit** but publishes per-second and per-day guidance in its Terms of Service and asks batch users to build in delays. Services are "best-effort."  Treat it as "be polite," not "unlimited."
- **Endpoints of interest:** `/online/occupations/{code}/details/tasks`, `/detailed_work_activities`, `/work_context`, plus taxonomy/crosswalk services.
- **Format:** JSON or XML.
- **Licensing:** O*NET 30.3 Database content is **CC BY 4.0**. Required attribution (verbatim): *"This page includes information from the O*NET 30.3 Database by the U.S. Department of Labor, Employment and Training Administration (USDOL/ETA). Used under the CC BY 4.0 license. O*NET® is a trademark of USDOL/ETA."* You must credit, link the license, and indicate changes.  Commercial use is explicitly permitted. **Note the trademark:** O*NET® is a USDOL/ETA trademark — you can use the data commercially but cannot imply endorsement or brand your product as an "O*NET" product.

### 1.3 BLS OEWS (Occupational Employment and Wage Statistics)

- **What it gives you:** employment counts and mean/median/percentile (10/25/50/75/90) wages for ~830 occupations, at **national, state, and ~530 metro/nonmetro areas**, plus national industry-specific (NAICS) cells. Current release: **May 2024** (published spring 2025).
- **Best way to get the full corpus: bulk XLSX download**, not the API. National files live at `https://www.bls.gov/oes/current/oes_nat.htm` (and `/oes/2024/may/`). One national file has every SOC × wage/employment cell — a single download beats thousands of API calls.
- **API option:** the BLS Public Data API v2 (`https://api.bls.gov/publicAPI/v2/timeseries/data/`) works but requires constructing cryptic OEWS series IDs (e.g., `OEUN000000000000000000001` = national total employment;  the encoding packs area+industry+occupation+datatype). A registration key raises daily limits. Series-ID construction is poorly documented; the GovEx GitHub tutorial (`github.com/govex/bls-oews-api-tutorial`) is the practical reference. **For a full screen, don't bother with the API — download the XLSX.**
- **Critical caveat:** OEWS **excludes the self-employed.** For occupations like real estate agents, financial advisors, and lawyers, a large share of practitioners are self-employed, so OEWS employment materially *understates* the true population. BLS's Employment Projections "number of jobs" figure (in the Occupational Outlook Handbook) includes self-employed and is larger.
- **Another caveat:** OEWS explicitly warns its data are **not designed for time-series comparison** (methodology changes year to year). Use a single vintage.
- **Licensing:** U.S. Government work, no copyright, free for commercial use. Citation requested but not legally required.

### 1.4 CareerOneStop Web API

CareerOneStop (U.S. DOL / sponsored by DOL Employment and Training Administration) **pre-joins O*NET occupational data to BLS OEWS wages and BLS Employment Projections** — exactly the join you'd otherwise build yourself.

- **Access:** register at `careeronestop.org/Developers/WebAPI/registration.aspx`. You get a **userId (GUID)** and an **API token** (Bearer auth). **Free / royalty-free**, but the license **expires 36 months** from application  and must be renewed. Registration requires accepting the DEED CareerOneStop data-sharing agreement.
- **Base:** `https://api.careeronestop.org/v1/...`
- **Relevant endpoints:**
  - **Get Occupation Details:** `/occupation/{userId}/{onetCode}/{location}` → returns O*NET title/description/tasks/DWAs/tech + **OEWS wages (national/state/local)** + employment projections + education/training.  This is the "everything joined" call.
  - **Get Salary Details:** `/comparesalaries/{userId}/wage?keyword={code}` → hourly + annual at 10/25/50/75/90 percentiles; accepts SOC, O*NET, or OEWS codes. 
  - **Employment Patterns / LMI by Occupation** for lighter-weight outlook data.
  - **Occupational licensing** endpoint — license requirements by state/occupation (see Part 5; join this as your regulatory triage field).
- **Is it a better single source than joining O*NET+BLS yourself?** **For a prototype, yes** — one call per occupation gives you tasks + wages + projections already crosswalked, saving you the SOC roll-up work. **For a full-corpus batch screen, no** — you'd still be making ~900 calls and are subject to their terms and geocoding restrictions (Bing-licensed geocodes cannot be stored). Best pattern: **bulk-download O*NET + OEWS for the batch screen; use CareerOneStop live in the shipped product** for per-occupation detail and licensing lookups.
- **Licensing gotcha:** CareerOneStop's own terms (36-month renewal, no storing geocodes) are *more* restrictive than the underlying O*NET (CC BY 4.0) and OEWS (public domain) data. If you want permanent, unrestricted rights, go to the primary sources.

### 1.5 The join keys and the crosswalk gotcha

- **O*NET-SOC** extends the federal SOC with extra digits after the 6-digit SOC: format `XX-XXXX.XX`. Example: SOC `11-1011` Chief Executives → O*NET-SOC `11-1011.00` (maps 1:1)  *and* `11-1011.03` Chief Sustainability Officers (a granular child). O*NET-SOC 2019 has **1,016 titles / 923 data-level**  vs. **867 detailed SOC** / ~830 OEWS-published occupations.
- **The gotcha:** to join O*NET tasks to OEWS wages you must **roll O*NET-SOC up to the 6-digit SOC** (truncate `.XX`), then join to OEWS. But it's *many-to-one*: several O*NET-SOC children (`.00`, `.01`, `.02`…) share one SOC wage cell. So every child inherits the *same* wage/employment — you cannot get wage differences below the SOC level.
- **Worse:** OEWS sometimes **aggregates** SOC detailed occupations it can't reliably survey (it combined 21 detailed SOCs into 10 aggregates in 2017).  And O*NET has "title-only" non-data occupations plus 73 "rolled-up" 2019 occupations.  So a clean join requires: (1) O*NET-SOC → 6-digit SOC (truncate), (2) SOC → OEWS published level (some SOCs only exist inside a broader OEWS aggregate — handle by falling back to the broad/aggregate wage). BLS's own Monthly Labor Review article "Mapping Employment Projections and O*NET data" documents that the published crosswalks give *no guidance* for the non-1:1 cases  — you must decide an imputation rule (BLS/OFLC's rule: apply the broad-occupation wage when a detailed cell is missing).
- **Practical recipe in Python/Postgres:**
  
  ```
  onet_soc = '19-3033.00'
  soc6 = onet_soc[:7]          # '19-3033'
  # join Task Statements (O*NET-SOC) -> aggregate to soc6
  # join soc6 -> OEWS on soc6; if null, try broad SOC 'XX-XXX0' aggregate
  ```

### 1.6 Licensing summary (all sources)

|Source                      |License               |Commercial use|Catch                                                         |
|----------------------------|----------------------|--------------|--------------------------------------------------------------|
|O*NET Database              |CC BY 4.0             |✅ Yes         |Must attribute + link license + note changes; O*NET® trademark|
|BLS OEWS                    |U.S. Gov public domain|✅ Yes         |Citation requested, not required; no time-series              |
|BLS Employment Projections  |U.S. Gov public domain|✅ Yes         |Same                                                          |
|CareerOneStop API           |Royalty-free license  |✅ Yes         |36-month renewal; can't store Bing geocodes                   |
|Census Service Annual Survey|U.S. Gov public domain|✅ Yes         |Industry (NAICS) level, not occupation                        |

-----

## PART 2 — Critique of the Screen Logic

The proposed pipeline:

1. Pull occupations + tasks + wages + employment
1. Rules-based filter (wage above threshold, employment above threshold)
1. LLM pass per task: routine info work? artifact? incumbent price?
1. Score: (wage × employment) / routineness
1. Output ranked list

**What's right:** cheap deterministic filtering before expensive LLM calls is correct engineering. Naming the specific task to unbundle is the right output granularity.

**What's wrong:**

- **Wage × employment measures the wrong thing.** It measures *the size of the labor market an employer pays for*, not *the size of the consumer market priced out of a service*. Bookkeeping clerks number ~1.6 million at a $49,210 median — but that's B2B back-office labor, not a consumer service anyone was ever "priced out of." High employment proves the *task* is done at scale; it does *not* prove a *consumer service* is "actually bought at scale." Conflating the two is the central flaw.
- **Median wage is a weak proxy for consumer price.** What a consumer pays (a ~$3,000 financial plan, a $273 itemized return) is a function of billing model, liability, and market structure — not the practitioner's hourly wage. A $102,140-median financial advisor and a $151,160-median lawyer both sell four-figure engagements, but the *ratio* of consumer-price-to-wage is wildly different across occupations. Wage tells you almost nothing about the unbundleable price.
- **"Routineness" as denominator is under-specified and double-counts.** If you compute routineness with an LLM, you're paying for a judgment O*NET already encodes (frequency, structured-vs-unstructured work context). If you compute it with O*NET ratings, fine — but then it's not clear it belongs in the denominator at all. Routineness predicts *automatability*, but the unbundling thesis isn't "automate the routine 90%," it's "sell the routine 90% to people who bought 0% before." Those are different axes.
- **The score has no demand term.** As written, `(wage × employment) / routineness` can rank a candidate #1 with *zero evidence any consumer wants it*.

**Better axes O*NET already gives you (use instead of an LLM routineness call):**

- **Task Frequency rating** (7-point scale) — how often the task is performed. High frequency = standardizable.
- **Task Importance & Relevance** — filter to Core tasks.
- **Work Context: "Structured versus Unstructured Work"** and **"Degree of Automation"** — direct O*NET items measuring exactly the "how routine/automatable" axis, already surveyed from incumbents. These are free and deterministic; prefer them over an LLM guess.
- **Job Zone (1–5)** — proxy for credential-gating. *High Job Zone (4–5) is the interesting signal for unbundling:* it means the service is currently gated behind expensive education/licensing, which is precisely what creates the priced-out tier. This is more useful than routineness.

**Where the LLM genuinely adds value** (and O*NET can't): (a) naming the **concrete deliverable artifact** a task produces (a document, a plan, a filing, a design), (b) judging whether that artifact is **consumer-facing vs. internal**, and (c) flagging **unauthorized-practice / licensing** exposure. Reserve LLM spend for those three, run on Core tasks in high-Job-Zone occupations only.

**Revised scoring concept:**
`score = (consumer_price_estimate × addressable_priced_out_population × demand_signal) × artifact_deliverability / regulatory_risk`
— where price and population come from *outside* O*NET, demand_signal is the mandatory join (Part 3), and O*NET only supplies the shortlist of gated, structured, artifact-producing tasks.

-----

## PART 3 — The Mandatory Demand Join

O*NET + OEWS tells you what workers *do* and are *paid*. It is silent on (a) what a *consumer* pays and (b) whether there's *unserved* demand below the current price. Independent demand signals, ranked by accessibility:

**Free / accessible:**

- **Google Trends** — free web tool; relative 0–100 interest index (not absolute volume). An official **Google Trends API entered alpha in 2025 with ~1,500 queries/day and OAuth 2.0**.  Good for *trend direction and relative comparison* ("cost of X near me"), weak for absolute demand. Rule of thumb from practitioners: require 4–8 weeks of sustained growth to distinguish signal from news spikes. 
- **Census Service Annual Survey (SAS)** — free, authoritative; national **revenue by NAICS industry** (services-sector revenue was ~$15.5T in 2016).  Tells you the *total dollars flowing through an industry* (e.g., NAICS 5412 accounting, 5413 architecture/engineering, 6116 education). **Industry-level, not occupation-level** — you join it via a SOC→NAICS bridge, imperfectly. Best for sizing the incumbent market, not for detecting priced-out demand.
- **Census County Business Patterns / Economic Census** — establishment counts and payroll by NAICS, free.
- **BLS Consumer Expenditure Survey** — what households actually spend by category; free; the closest public source to *consumer* (not employer) demand.
- **Reddit / forum / review scraping** — qualitative "I wish I could afford X" signal; free but noisy.

**Freemium / paid (better for real price + demand):**

- **Upwork / Fiverr gig pricing** — the single best *revealed-price* signal for unbundled information tasks. If freelancers already sell "the task" at $X, that's your price floor and proof of a market. No official API; requires scraping or manual sampling. **This is arguably the most valuable join** because it prices the *task*, not the *occupation*.
- **Keyword tools with absolute volume** — Glimpse (converts Trends to real volume),  DataForSEO, Semrush, Ahrefs. Paid. Give absolute monthly search volume for "how much does X cost" / "X near me" — a strong intent-to-buy proxy.
- **IBISWorld / Statista** — industry reports with market size, growth, and sometimes average consumer price. Paid, expensive, but authoritative for a specific vertical you're serious about.

**Recommended concrete join for a solo dev:** For each shortlisted task, compute a **Demand Score** from three cheap-ish sources: (1) Google Trends direction for "[service] cost" queries, (2) count + median price of matching Upwork/Fiverr gigs, (3) Census SAS revenue for the parent NAICS. Only tasks that clear all three advance. This is the step that separates your screener from "anyone can produce a ranked list."

-----

## PART 4 — First-Pass Candidates

Wages and employment below are **BLS OEWS/OOH May 2024** (median annual wage; employment is OOH "number of jobs, 2024" including self-employed unless marked OEWS). Consumer prices are from 2025–2026 market research. **Incumbent check and regulatory flags are the decisive columns — read those first.**

### Candidate table (summary)

|# |Occupation (SOC)                           |Task to unbundle                                                    |Median wage|Employment             |Incumbent consumer price                          |AI-delivered price|Crowded?                                        |Regulatory landmine                                    |
|--|-------------------------------------------|--------------------------------------------------------------------|-----------|-----------------------|--------------------------------------------------|------------------|------------------------------------------------|-------------------------------------------------------|
|1 |Personal Financial Advisors (13-2052)      |Produce a standalone comprehensive financial plan                   |$102,140   |~326,000 (OEWS 270,480)|~$3,000 per plan; $300/hr median                  |$99–$499          |Moderate (robo-advisors, but *planning* less so)|⚠️ Investment advice = SEC/state RIA registration       |
|2 |Tax Preparers (13-2082)                    |Prepare/file a 1040 + Schedule A                                    |$49,010    |~83,000 (OEWS)         |$273 itemized (NSA survey); $778 multi-schedule   |$0–$50            |🔴 Very crowded (TurboTax, FreeTaxUSA, Column)   |Moderate — preparers need PTIN; software is legal      |
|3 |Lawyers (23-1011) — doc drafting           |Draft demand letters, LLC operating agreements, simple wills        |$151,160   |864,800                |$200–$1,000+/doc; wills $300–$1,000               |$18–$199          |🔴 Crowded (LegalZoom, Rocket Lawyer, DoNotPay)  |🔴🔴 **Unauthorized practice of law**                    |
|4 |Educational/Career Counselors (21-1012)    |College admissions strategy + essay coaching                        |$65,140    |~376,300               |$4,000–$12,000 package (IECA); $200/hr            |$20–$200          |Moderate–high (many startups)                   |✅ Mostly clean (not licensed)                          |
|5 |Interior Designers (27-1025)               |Single-room e-design (layout + shopping list)                       |$63,490    |87,100                 |E-design $199–$999/room; local $2,000–$12,000/room|$29–$199          |🔴 Crowded (Havenly, Modsy, Spacejoy, Decorilla) |✅ Clean (title-protected in few states)                |
|6 |Dietitians/Nutritionists (29-1031)         |Personalized meal plan / macro coaching                             |$73,850    |90,900                 |$100–$300/session; $1,200–$7,200/yr               |$10–$20/mo        |🔴 Very crowded (MyFitnessPal, dozens of AI apps)|⚠️ "Medical nutrition therapy" is licensed              |
|7 |HR Specialists / recruiters (13-1071)      |Resume + cover letter + LinkedIn rewrite                            |$72,910    |944,300                |$200 (entry) – $700 (mid) – $3,500 (exec)         |$0–$40/mo         |🔴🔴 Feature, not a business                      |✅ Clean                                                |
|8 |Interpreters/Translators (27-3091)         |Document translation (non-certified)                                |$59,440    |75,300                 |$0.10–$0.30/word; $30–$100/page                   |~$0 (DeepL/GPT)   |🔴🔴 Commoditized (DeepL, Google)                 |⚠️ *Certified* translation still needs human attestation|
|9 |Clinical/Counseling Psychologists (19-3033)|Structured self-help / CBT-style coaching                           |$96,100    |71,730 (OEWS)          |$100–$250/session                                 |$0–$20/mo         |High (Woebot, Wysa, dozens)                     |🔴 Therapy/diagnosis is licensed                        |
|10|Management Analysts (13-1111)              |Small-business plan / market analysis doc                           |$101,190   |1,075,100              |$2,000–$6,000 project                             |$49–$299          |Moderate (LivePlan + AI)                        |✅ Clean                                                |
|11|Graphic Designers (27-1024)                |Logo + basic brand kit                                              |$61,300    |214,260 (OEWS)         |$300–$2,500 (freelance); $5,000+ (agency)         |$0–$40            |🔴🔴 Feature (Canva, Looka, Midjourney)           |✅ Clean                                                |
|12|Real Estate Agents (41-9022)               |CMA / listing description                                           |$56,320    |420,900                |Bundled in 5–6% commission (~$15k on $500k home)  |$0–$99            |Moderate (Zillow Zestimate, listing tools)      |🔴 Brokerage/agency licensed                            |
|13|Paralegals (23-2011)                       |Form-filling: uncontested divorce, small claims, immigration forms  |$61,010    |376,200                |LDA/paralegal $150–$500; attorney $1,000s         |$50–$199          |Moderate (Hello Divorce, some)                  |🔴🔴 **UPL** unless structured as self-help prep         |
|14|Accountants/Auditors (13-2011)             |Small-business bookkeeping categorization + basic statements        |$81,680    |~1,579,800             |$200–$500/mo bookkeeping                          |$10–$50/mo        |🔴 Crowded (Bench, Pilot, QBO auto-categorize)   |⚠️ Audit/attest is licensed (CPA); bookkeeping is not   |
|15|HR Specialists (13-1071) — compliance      |Employee handbook / offer letters / policy docs for micro-businesses|$72,910    |944,300                |$1,500–$5,000 (attorney/consultant)               |$49–$299          |Low–moderate (underserved niche)                |⚠️ Employment-law-adjacent; template disclaimers needed |

### Detailed notes on the decisive columns

- **The DoNotPay precedent (candidates 3, 13).** DoNotPay, "the world's first robot lawyer,"  was hit with multiple unauthorized-practice-of-law (UPL) suits and settled a California class action (Faridian) alleging "substandard" legal documents.  Separately, the **FTC's DoNotPay order, finalized February 2025, required the company to pay $193,000 in monetary relief and notify subscribers from 2021–2023** — per the FTC, "the final order requires DoNotPay to pay $193,000 in monetary relief and notify consumers who subscribed to the service between 2021 and 2023 about the FTC settlement" (approved 5-0). This is the canonical warning for any AI product that touches legal deliverables: UPL statutes are enforced and marketing overreach draws separate consumer-protection liability.
- **Financial planning price (candidate 1).** The unbundleable artifact is the standalone comprehensive plan, which averages **~$3,000** per the Kitces Report (via SmartAsset: "standalone project fees for a comprehensive plan tend to average around $3,000");  the median hourly rate is **$300/hr**,  and the 2026 State of Financial Planning Fees study found average annual retainers surged 52% since 2023  to **$6,815**. Robo-advisors attacked *asset management*, not *planning* — the planning artifact remains gated behind four-figure engagements.
- **College admissions demand story (candidate 4).** The priced-out tier is quantifiable: the **national student-to-school-counselor ratio was 372:1 in 2024-25**, per the American School Counselor Association (132,270 counselors serving ~49.3 million students) — versus ASCA's recommended 250:1. Private comprehensive packages run **$4,000–$12,000** (IECA: "the average family using a private admissions consultant spends between $4,000 and $12,000 for a comprehensive package";  ~$6,500 average).  The gap between "no usable school counseling" and "$6,500 private consultant" is the exact priced-out tier the thesis targets.
- **Tax prep (candidate 2).** The National Society of Accountants survey puts an **itemized Form 1040 with Schedule A plus a state return at $273** (regional highs ~$333 New England, ~$329 Pacific; low ~$210 in AL/KY/MS/TN); multi-schedule returns average $778. The problem is not price headroom — it's that DIY software ($0–$50) already fully occupies the downmarket, making this crowded *and* PTIN/regulatory-adjacent.

### The candidates worth actually building (my ranked take)

**Tier A — genuinely attractive (gated, four-figure incumbent price, real priced-out tier, survivable regulatory path):**

- **#15 Micro-business HR/compliance docs (employee handbooks, offer letters, policies).** The priced-out tier — sub-20-employee businesses that never hire an HR consultant or employment lawyer — is huge and real. Incumbents charge $1,500–$5,000; the routine 80% is highly templatable and state-variable (which is exactly where AI + a rules layer beats a generic template). Least crowded of the strong candidates. Regulatory risk is employment-law-adjacent but manageable with "not legal advice" framing and attorney-reviewed templates. **This is the sleeper.**
- **#1 Standalone financial *plans* (not investment management).** The ~$3,000 comprehensive plan is the classic downmarket-unbundling example. The priced-out tier is the mass-affluent who have questions but not $250k to hand a fee-only advisor. **But** the moment you give personalized investment *advice* you trip SEC/state RIA registration — the viable version is *education + planning frameworks*, not "buy VTI." Regulatory navigation is the moat.
- **#10 Small-business plans / market analysis.** Clean regulatory profile, four-figure incumbent price, and the priced-out tier (first-time founders, loan applicants) is real. Crowded-ish (LivePlan) but not saturated with AI-native players.

**Tier B — real demand but crowded or regulatory-heavy:**

- **#4 College admissions** — big, *quantified* priced-out tier (372:1 counselor ratio), clean regulation, but many well-funded startups already here.
- **#13 Legal form-filling (divorce/immigration)** — enormous priced-out tier, but UPL risk is severe unless you copy the California Legal Document Assistant model (self-help, no advice). Hello Divorce shows it's doable.

**Tier C — avoid (features, not businesses):**

- **#7 Resumes, #8 translation, #11 logos, #6 consumer nutrition** — all now free features inside general-purpose tools. A developer can replicate the core in a weekend calling the same APIs → feature, not a business. #3 generic legal docs and #2 tax filing are both crowded *and* regulatory — worst of both.

-----

## PART 5 — Honest Assessment: Is O*NET a Good Corpus for This?

**Verdict: O*NET is a good *filter/skeleton* and a poor *idea generator*. Use it, but don't trust it to find the opportunity.**

**Structural limitations that make it weak as an idea generator:**

1. **It's an employer/labor lens, not a consumer/market lens.** Every variable — wage, employment, tasks — describes the *supply of labor*. Downmarket unbundling is fundamentally a *demand-side* bet ("who is priced out?"). O*NET literally cannot see the priced-out tier because those people are, by definition, *not* transacting and *not* in any labor statistic. The corpus is blind to the exact thing you're hunting.
1. **Occupation ≠ service.** Many high-value consumer services don't map cleanly to one SOC (e.g., "wedding planning," "small-business setup" span several). And many high-employment occupations (bookkeeping clerks, HR specialists) are internal B2B labor with no consumer-service analog. The task list is granular but occupation-indexed, so cross-occupation *consumer jobs-to-be-done* are invisible.
1. **Task statements describe activity, not deliverable or price.** "Prepare financial plans" doesn't tell you it's a ~$3,000 artifact. You always need an external price join — O*NET gives you the verb, never the invoice.
1. **Update latency and averaging.** O*NET updates a rolling subset of occupations per cycle; some ratings are years old. And rolling everything up to SOC for the wage join destroys within-occupation granularity.
1. **It over-indexes on routineness signals that everyone already has.** The AI-exposure literature (Pew's O*NET analysis, the "GPTs are GPTs"-style occupational studies, Anthropic's economic index) has already strip-mined O*NET tasks for automatability. If your edge is "LLM says this task is routine," you have no edge — that map is fully drawn.

**What a smarter screener would use instead or in addition:**

- **Start from the demand side, not the supply side.** Seed with *consumer* signals — Consumer Expenditure Survey categories, "how much does ___ cost" search volume, Upwork/Fiverr revealed prices — and only *then* map back to O*NET tasks to find the specific gated task producing that spend. Invert the pipeline.
- **Use Upwork/Fiverr as the primary corpus, O*NET as the enrichment.** Gig marketplaces already price the *unbundled task* and prove a market exists below the professional price point. That's a far better idea generator than a labor taxonomy.
- **Add a regulatory-exposure database.** CareerOneStop has an **occupational licensing API** (license requirements by state/occupation) — join it so the regulatory landmine is a *computed field*, not a manual afterthought. For this thesis, licensing status is more predictive of viability than routineness.
- **Weight by Job Zone × (consumer price / DIY substitute price).** The gap between what a pro charges and what a tool can deliver, gated by credential requirements, is the actual opportunity size.

**Bottom line for the user:** Build the O*NET+OEWS pipeline — it's a weekend of work, free, and gives you a clean, licensed, deduplicated list of every gated, structured, artifact-producing task in the economy. But wire the **demand join in as the first filter, not the last**, and expect O*NET to *narrow* candidates rather than *generate* them. The winning ideas will come from the demand data; O*NET's job is to tell you which specific licensed task to point the AI at, and CareerOneStop's licensing API is your regulatory triage. Given how thoroughly the "routine task → automate" space has been mined, your edge is not routineness detection — it's finding the gated four-figure service with a large, invisible, priced-out tier and a survivable path around the licensing statute. On current evidence that points at **micro-business compliance docs, unbundled financial planning, and small-business planning** far more than at the crowded resume/logo/tax/nutrition space.

-----

## Recommendations

1. **Weekend 1 — build the skeleton.** Bulk-download O*NET 30.3 (SQL format into Supabase/Postgres) and the OEWS May 2024 national XLSX. Load `Task Statements`, `Task Ratings`, `Work Context`, `Job Zones`, `Occupation Data`. Write the SOC roll-up join (truncate O*NET-SOC to 6-digit, left-join OEWS, fall back to broad-SOC aggregate on null). Attribution string goes in your footer now.
1. **Weekend 2 — deterministic filter.** Filter to Core tasks, Job Zone ≥ 4, high Task Frequency, and high Work-Context "Structured Work." This yields a few hundred gated, standardizable, artifact-producing tasks *without any LLM spend.*
1. **Weekend 3 — the demand join (do not skip).** For the top ~50 tasks, pull Google Trends direction for "[service] cost," count+price matching Upwork/Fiverr gigs, and Census SAS revenue for the parent NAICS. Drop anything that fails all three.
1. **Weekend 4 — LLM pass, narrowly scoped.** Run an LLM only on survivors to (a) name the deliverable artifact, (b) classify consumer-facing vs. internal, (c) flag UPL/licensing exposure. Join CareerOneStop's licensing API for a computed regulatory field.
1. **Pick one Tier-A candidate and pressure-test the "weekend replication" question honestly.** If your differentiation is only "I call GPT with a nice prompt," stop — it's a feature. Ship only if you add proprietary workflow, assume liability/trust the incumbent won't, or navigate a regulatory barrier that scares off casual builders.
1. **Get a lawyer's hour before building anything in Tiers A/B that touches law, money, medicine, or real estate.** The DoNotPay UPL suits and $193,000 FTC settlement show the risk is real and enforced. Budget for "not advice" framing, disclaimers, and (for financial planning) checking whether your feature set trips RIA registration.

**Thresholds that would change the recommendation:** if a demand-join source shows a candidate has *rising* search volume AND live Upwork gigs priced 5–20× above a plausible AI delivery cost AND a clean licensing status, promote it to build. If the only signal is high wage × employment with no consumer demand evidence, kill it regardless of score.

-----

## Caveats

- OEWS **excludes self-employed workers**, so employment for advisors, agents, and lawyers understates the true practitioner population; I used OOH "number of jobs" (which includes self-employed) where noted, and those two concepts are not interchangeable (e.g., Personal Financial Advisors: OEWS 270,480 vs. OOH ~326,000).
- OEWS data are **not valid for year-over-year comparison**; all figures are a single May 2024 vintage.
- **Median wage is a poor proxy for consumer price** — I researched actual market prices separately for each candidate; treat the wage column as market-size context, not pricing input.
- Consumer prices are from 2025–2026 secondary sources (industry blogs, service-provider pages, and the National Society of Accountants and IECA surveys) and vary widely by geography and complexity; verify before pricing a product.
- The corpus-size figures drift between O*NET releases (the current Task Statements file has 18,796 rows; older technical reports cite 19,450). Always read the count from the data dictionary of the exact release you download.
- The Google Trends "official API" is described as **alpha** as of 2025; treat its availability and limits as provisional and have a scraping/paid fallback.
- Regulatory flags here are **directional, not legal advice.** Unauthorized-practice statutes vary by state and are actively litigated; a licensed attorney's review is mandatory before launch in any flagged vertical.
