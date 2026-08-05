# Free IP → Product: A Demand-First Assessment of Mining Expired Patents to Sell on Amazon

## TL;DR

- **The screening idea is the easy 5% of a business whose hard 95% — sourcing capital, IP due diligence, tariffs, and a marketplace now majority-controlled by Chinese factory-direct sellers — is unchanged and brutal. As specified ("look up expired patents, build in China, sell on Amazon"), this is a weak business in 2026. A solo developer can replicate the patent-screening core in a weekend by calling the free PatentsView and USPTO maintenance-fee APIs, which by your own project's rule makes it a feature, not a business.**
- **The framing is backwards, and that is the core finding: patents are a SUPPLY corpus. They tell you what was invented, never what anyone wants to buy. Start from Amazon demand signals (Keepa/Helium 10/Jungle Scout, Product Opportunity Explorer) and only *then* ask whether a lapsed patent supplies a design — and the honest answer is that expired patents rarely map to a validated Amazon demand gap, because a formerly-patented product that lapsed usually lapsed because nobody was making money on it.**
- **If you want to build something here, build the demand-first screening TOOL (an "Amazon opportunity scanner") and sell it to sellers, or pursue a genuinely better "free IP" angle (public-domain books/audio via Kindle/ACX, open-access museum images on print-on-demand) where there is zero inventory risk, zero tariff exposure, and no factory. The expired-patent angle's one defensible niche is that no productized "expired-patent → Amazon-opportunity" tool exists yet — but that whitespace exists because the underlying business is weak, not because nobody thought of it.**

## Key Findings

1. **Government patent data is free and fully accessible via Python, but it describes supply.** PatentsView's PatentSearch API (search.patentsview.org) exposes 27 endpoints including `g_claims`, `g_detail_desc_texts`, `g_draw_desc_texts`, and CPC classification,  and the USPTO publishes weekly cumulative maintenance-fee event files at bulkdata.uspto.gov.  You can build the lapsed-patent screen. That is precisely the problem — it is trivial.
1. **The "most patents lapse" claim is TRUE and verified.** The most-cited academic source, Kimberly A. Moore's "Worthless Patents" (George Mason Law & Economics Research Paper No. 04-29, 2004), found that "53.7% of all patentees allow their patents to expire for failure to pay maintenance fees"; more recent practitioner data shows only ~40–50% of utility patents are maintained to the 20-year mark (about 86% pay the first fee at 3.5 years; only ~44% survive the third at 11.5 years). Critically, this cuts *against* the thesis: patents lapse because owners judged them commercially worthless. Lapse is a negative demand signal, not a treasure map.
1. **Patents are legal documents, not manufacturing files.** Patent drawings deliberately omit dimensions and tolerances;  a formerly-patented product still needs a real CAD/engineering pass before a factory can quote. AI can summarize a spec and draft a starting CAD model, but a solo operator will still need an industrial designer or engineer for anything mechanical. Patents supply the *idea*, not the *product*.
1. **Expiration frees the invention but not the product.** Trademarks and trade dress can protect a product's appearance indefinitely;  improvement patents and design patents may still be live; the Supreme Court's *TrafFix* decision means an expired *utility* patent is strong evidence features are functional (hence copyable), but expired *design* patents can still be blocked by trade dress. Real freedom-to-operate due diligence requires a patent attorney and costs real money per product.
1. **Amazon's IP-complaint regime is the single biggest practical risk, and it presumes you guilty.** A competitor can file through Report a Violation (design/trademark/trade dress) or APEX (utility patents) and get your listing suspended during review regardless of whether the claim is ultimately valid. APEX costs $4,000 per side (refunded to the winner).  False and abusive complaints are well-documented, reinstatement is slow and sometimes costs five figures, and complaints are timed to Q4 to maximize damage.
1. **Tariffs and the end of de minimis have structurally worsened the import-and-sell model.** The $800 de minimis exemption is gone (China May 2025, global August 2025, codified indefinitely June 2026).  Chinese consumer goods now carry roughly 30–35% effective tariffs. Every shipment requires formal customs entry and DDP shipping. The cost advantage that made cheap-China-arbitrage work is materially eroded — for you, but *not* for the Chinese sellers manufacturing at the factory.
1. **The marketplace has consolidated against exactly this operator.** Per Marketplace Pulse, Chinese sellers reached 50.03% of Amazon's global active seller base in 2025 and 55.9% of the top 10,000 US sellers (US sellers fell to 40.5%). New-seller launches hit a decade low — just 165,000 in 2025, down 44% YoY — and active sellers fell from 2.4 million in 2021 to 1.65 million by end of 2025 in what Marketplace Pulse calls "the Great Compression." Amazon fees consume 30–45% of selling price. Realistic single-SKU startup capital is ~$10,000–$12,500, and a typical private-label net margin is 15–30% *if it works at all*.
1. **The idea is widely known; the tool niche is empty for a reason.** Patent attorneys (Rich Goldstein), ebooks (Tony Laidig's "Expired Patents"), and a weekly newsletter ("Start My Idea" on beehiiv) all evangelize expired-patent product mining. No purpose-built AI tool screens expired patents *as Amazon opportunities* — a genuine whitespace, but one that exists because the demand-first economics don't hold.

## Details

### Part 1 — The demand side (research this first)

The prior O*NET lesson applies with full force here. Amazon demand data is the thing worth screening; patents are not. Here is what a solo operator can actually get:

**Third-party research tools.** Helium 10 and Jungle Scout are the two dominant tools, both starting around $29/month. Accuracy of their sales estimates is contested and vendor-funded: Jungle Scout claims 84.1% accuracy vs Helium 10's ~74%;  Helium 10 claims 89.59% vs Jungle Scout's 60% in a 29,906-product study;  an independent EcomCrew test found Helium 10 consistently *underestimates* (workable) while Jungle Scout's estimates swung wildly in some categories, but Jungle Scout was closer on *search volume* (19 of 25 keywords).  **Takeaway: treat all sales estimates as directional, ±30%, and cross-check two tools.** Both have APIs/bulk tools on higher tiers.

**Keepa** is the most important tool for a builder. It tracks price, sales rank, Buy Box, rating, and review-count *history* on 5B+ products, and — crucially — has a real API with a Python client (`pip install keepa`). The `product_finder` endpoint lets you filter the catalog by current sales rank, price, review count, etc., and `stats_parsed` exposes current sales rank.  This is the closest thing to a programmable demand screen. It is token-metered and paid. 

**Amazon's own data.** Product Opportunity Explorer (free in Seller Central, requires a Professional account at $39.99/mo) is the gold standard because it uses *real Amazon search data*: search volume, units sold, product count, search conversion rate, selling-partner count, average brand age, out-of-stock rate, and % of listings using Sponsored Products. This last cluster is exactly your "demand but no good product" screen — a niche with high search volume, old incumbent brands, high out-of-stock rates, and low ad saturation is a real opportunity signal. **However**: Explorer is largely *not* available via SP-API. Amazon's SP-API `searchCatalogItems` (Catalog Items v2022-04-01) returns `salesRanks`, and Customer Feedback API v2024-06-01 exposes review data, but the rich Explorer niche metrics are UI-only, and sellers have complained Amazon degraded the search interface. SP-API also requires a Professional seller account and developer registration — a non-seller cannot access demand data through it.

**Free alternatives.** Google Trends (unofficial `pytrends`), Amazon autocomplete scraping, and the "Customers bought in past month" 50+/100+/500+ badges (coarse but free). These are directional only.

**The key screen IS partially screenable at scale** — via Keepa's product_finder plus Explorer — but the output is a demand-first product list that has *nothing to do with patents.* That is the tell.

### Part 2 — The patent supply side

**Data access is excellent and free:**

- **PatentsView PatentSearch API** (search.patentsview.org/api/v1/): POST queries in JSON, API key required (free). Endpoints include `patent`, `g_claims`, `g_brf_sum_texts`, `g_detail_desc_texts`, `g_draw_desc_texts`, `cpc_class/subclass/group`, `assignees`, `inventors`.  Full text of claims and descriptions is in beta, backfilling from 2023 backward. 
- **USPTO Open Data Portal / bulk data** (data.uspto.gov/bulkdata, bulkdata.uspto.gov): full-text grants, CPC classification files, and the maintenance-fee event file.
- **Maintenance fee events file** — the key to finding lapsed patents. `MaintFeeEvents` at bulkdata.uspto.gov/data/patent/maintenancefee/, ASCII, cumulative, updated weekly (Tuesdays). Documentation: `MaintFeeEventsFileDocumentation.doc`. Event codes flag payments at 3.5/7.5/11.5 years and expirations for non-payment. The USPTO also publishes weekly "Notice of Expiration of Patents Due to Failure to Pay Maintenance Fee"  in the Official Gazette.

**Utility vs design:** Only *utility* patents (20 years from filing) carry maintenance fees and thus lapse early — these are your target for the "cheap early lapse" angle. *Design* patents (15 years from issuance) and plant patents have NO maintenance fees  and simply run their full term. For a physical consumer product, you usually care about both the utility patent (the function) and any design patent (the appearance) — and both must be clear.

**Lapse statistics (verified):** ~53.7% of patentees historically let patents lapse for non-payment (Moore, 2004); modern practitioner data puts full-term maintenance at ~40–50%. Interpreted correctly, this is a *warning*: the vast pool of "cheaply available" early-lapsed patents is disproportionately full of inventions the market rejected.

**Patents as manufacturing inputs — concrete reality:** Patent drawings are 2D, black-and-white, dimensionless by design (adding exact measurements can legally narrow a claim, so applicants avoid it). Engineering drawings — with dimensions, tolerances, materials, and a 3D CAD model — are what a factory quotes from. So the workflow is: patent spec → human/AI interpretation → CAD model → prototype → factory quote. An AI (e.g., a vision-language model reading the drawings plus an LLM reading the claims) can produce a first-draft summary and even a rough CAD starting point, but for any mechanical or safety-relevant product you still need an industrial designer/engineer. This is a real cost and a real skill gap, not an AI-solved problem in 2026.

### Part 3 — The legal reality (be skeptical)

**Trade dress is frequently the binding constraint.** Trademarks and trade dress (a product's total visual appearance) can last indefinitely as long as they remain distinctive and in use. The leading case, *TrafFix Devices v. Marketing Displays* (2001), holds that an expired *utility* patent is strong evidence the disclosed features are *functional* and therefore *not* protectable as trade dress — good news for copying functional inventions. But non-functional, ornamental elements can still be trade dress, and an expired *design* patent's subject matter may still be locked up by trade dress. The brand name is a trademark you must never touch; packaging art and manuals carry copyright.

**Overlapping live patents** are the quiet killer: a single product is often covered by a *family* — an original (possibly expired) patent plus continuations and improvement patents with later expiration dates.  Clearing the base patent does not clear the product.

**Due diligence** therefore requires a real freedom-to-operate review by a patent attorney per SKU — checking patent family, legal status, design patents, and trade dress. This is not a weekend API call; it is billable legal work, and it must happen *before* you tool a mold.

**Amazon's takedown regime — the biggest practical risk.** This is where the business most often dies:

- **APEX (Amazon Patent Evaluation Express)**, for US utility patents: the patent owner (must be Brand Registry–enrolled) names up to 20 accused ASINs; each side pays a $4,000 deposit directly to a neutral patent-attorney evaluator; the loser forfeits their $4,000, the winner is refunded. Defenses are limited to non-infringement — there is no invalidity or bad-faith review. If the accused seller *ignores* the notice (three-week window), the listing is simply removed. Resolution in ~30 days to 12 weeks vs. 2–3 years and $500k+ for litigation. Note: **design patents, non-US patents, and expired patents are NOT eligible for APEX** — those go through the faster, cheaper (for the complainant) Report a Violation path. APEX launched in 2022, following the 2019 Utility Patent Neutral Evaluation beta.
- **Report a Violation (RAV)** for design patents, trademarks, and trade dress: faster and cheaper for the complainant, and the documented locus of abuse.
- **Abuse is real and documented.** Blake Brittain's Bloomberg Law report "Amazon's Judging of IP Claims Questioned in Seller Lawsuits" (Feb. 12, 2020) described how "Amazon.com Inc. halted sales of a 'puppy sleep aid' after being told a storefront…infringed two patents—one registered in 1895, and another directed to a Japanese 'combustion device.' 'Neither patent is enforceable. Neither patent is owned by any Defendant.'" Harvard Law's Rebecca Tushnet (Frank Stanton Professor of First Amendment Law): "Amazon, with its size, now substitutes for government in a lot of what it does… It is being asked to run a judicial system, without the commitments to transparency and precedent of a real judicial system." IP attorney Marsha Gentner: trolls "can just file a complaint with Amazon, and get the equivalent of injunctive relief"  without the showing a court requires. Complaints are timed to Black Friday–Christmas to bankrupt competitors.  One seller (WhoIs) reported paying more than $10,000 in consultants to get reinstated. 
- **Reinstatement reality:** the fastest path is a rights-owner *retraction* (Amazon's Notice Retraction Form), not an appeal. A DMCA-style counter-notice shifts the burden but can provoke an actual lawsuit within 10–14 days.  Amazon's own "99%+ of page views land on non-flagged pages" statistic measures customer experience, not the false-positive rate against sellers — it does not rebut the abuse claim.

The irony for this business: a copy of a formerly-patented product, sold by a solo US operator, is a *prime target* for exactly these complaints — and you would be defending, not attacking.

### Part 4 — Manufacturing and market reality (2026)

**Tariffs / de minimis (current state):**

- De minimis $800 exemption eliminated: Executive Order 14256 ended it for China/Hong Kong May 2, 2025; EO 14324 (signed July 30, 2025) suspended it globally effective August 29, 2025; CBP's interim final rule (91 FR 37789, document 2026-12670), published June 24, 2026, indefinitely suspends de minimis for all modes except international postal, and statutory repeal is set for July 1, 2027 under the One Big Beautiful Bill Act. Restoration would require an Act of Congress — considered very unlikely. 
- Effective tariff on most Chinese consumer goods: roughly 30–35% (Section 301 List rates of 7.5–25% depending on HTS code — most consumer goods on List 3 at 25%, apparel/footwear on List 4A at 7.5% — plus residual layers). The Supreme Court struck down the IEEPA tariffs in February 2026, lowering the peak, but Section 301 is untouched, and a new Section 301 investigation launched March 2026 could raise rates. A separate 12.5% Section 301 forced-labor tariff took effect July 24, 2026.
- Practical effect: every shipment needs formal customs entry and DDP shipping; a $5 factory unit now effectively lands around $6–6.50 before freight and FBA fees.  Small replenishment orders can no longer sneak in duty-free.

**Sourcing economics (Alibaba/1688):**

- ODM (factory-designed, you brand it): tooling $0–20k, MOQ 500–3,000, 1–3 month lead time.
- OEM (your design/spec — which is what an expired-patent product requires): tooling $50k–200k for complex parts, MOQ 5k–10k, 6–9 month lead time. Custom molds commonly $5,000–$50,000+.
- Samples: typically $50–a few hundred each, 10–20 days.
- **This is the killer for the patent angle:** building a formerly-patented invention is an *OEM* job (custom spec, custom tooling), not a cheap ODM rebrand — so the tooling and MOQ costs are at the high end, and de minimis is gone, so you pay duty on samples too.

**FBA fees (2026):**

- Referral fee: 8–45%, most categories 15%, $0.30 minimum.
- Fulfillment fee: ~$3–7+ for standard sizes; +3.5% fuel surcharge effective April 17, 2026; new Low-Inventory-Level fee; Amazon discontinued FBA prep services Jan 1, 2026. 
- Storage: $0.78/cu ft (Jan–Sep), $2.40/cu ft (Oct–Dec) standard.
- Total Amazon fees typically consume 30–45% of selling price.

**Realistic single-SKU P&L (illustrative, kitchen-gadget class):**

- Sell price $24.99; landed cost ~$6.50 (unit + freight + duty + packaging); Amazon referral (15%) $3.75; FBA fulfillment ~$5–6; storage/returns/misc ~$1; PPC ~$3–4 (often half of net margin during launch). Net ≈ $4–6/unit *if* it ranks — a ~16–24% margin before you amortize tooling, and razor-thin against a Chinese competitor selling the identical item at the factory gate.
- Startup capital for one SKU: independent 2026 estimates cluster at **$3,500–$7,000 minimum**, with a fuller model (two inventory batches + aggressive PPC) at **~$10,350–$12,349**.

**Is private-label FBA still viable for a US solo operator?** Marginally, and structurally against you. Chinese sellers are >50% of the global active base (50.03%) and 55.9% of the top 10,000 US sellers; US new-seller launches hit a decade low (165,000 in 2025, −44% YoY); Marketplace Pulse calls 2025 "the Great Compression." US sellers still generate more revenue per seller ($884,958 average vs $393,557 for Chinese sellers, per Marketplace Pulse; US sellers hold ~$157B of Amazon.com's $305B third-party GMV vs $132B for Chinese sellers), so it is not hopeless — but the winning US playbook is *brand and differentiation*, the exact opposite of "copy an expired patent and sell a commodity."

### Part 5 — Incumbent check

The idea is **widely known and repeatedly monetized as content**, not as a defensible operating business:

- **Newsletter:** "Start My Idea" (startmyidea.beehiiv.com) runs a weekly "Expiring Patent Edition" listing specific expiring patents with business ideas. 
- **Patent attorneys as evangelists:** Rich Goldstein (Goldstein Patent Law) teaches the "expired patent hack" to Amazon sellers, explicitly including checking USPTO maintenance-fee data for early lapses;  featured by Amazing.com/Amazing at Home.
- **Info-products:** Tony Laidig's "Expired Patents" Kindle ebook.
- **Enterprise patent-expiry AI tools exist but serve R&D, not Amazon sellers:** PatSnap Eureka's Patent Expiry Checker,  Cypris.ai, IamIP legal-status watch. Amazon-seller tools (Opportunity Explorer, SmartScout, SellerSprite) are demand-based; SellerSprite's design-patent search is *defensive* (avoid infringement),  the opposite use case.
- **The whitespace:** no productized tool marries expired-patent screening with Amazon-opportunity scoring. That is a real gap — but it is empty because the demand-first economics are weak, not because it is hard to build.

### Part 6 — Honest assessment and alternatives

**Is this a good business for a solo technical operator in 2026? No — not as specified.** The interesting part (screening) is trivial and free to replicate; the hard parts (OEM tooling capital, per-SKU legal FTO, 30–35% tariffs, guilty-until-proven-innocent takedowns, and a marketplace won by factory-direct Chinese sellers) are unchanged and brutal. By your own standing rule — "if a developer can replicate the core in a weekend calling the same APIs, it's a feature, not a business" — the patent-screening core fails: PatentsView + the maintenance-fee file is a weekend project.

**The demand-first reframing, stated plainly:** patents are the wrong *starting point*. If you begin from Amazon demand and find a real gap (high search volume, weak aging incumbents, low ad saturation, high stockout), you will almost never find that an expired patent is the missing supply — you'll find that a slightly better version of an existing commodity is. And when a patent *has* lapsed, it usually lapsed because it wasn't making money. The two datasets rarely intersect where you need them to.

**Better "free IP → product" angles, ranked by demand-first strength:**

1. **Public-domain books and audio → Kindle/KDP and ACX/Audible.** Zero inventory, zero tariffs, zero factory, zero takedown-by-competitor risk. AI is a genuine enabler: translation, modernization, summarization, narration, cover art. Demand is checkable directly on Amazon's book charts. This is the strongest fit for a solo developer.
1. **Open-access museum image collections (Met, Rijksmuseum, Smithsonian, NYPL) → print-on-demand** (posters, apparel, home goods) via Printful/Printify + Etsy/Amazon Merch. No inventory, no tariff, demand visible via Etsy/Merch search. AI enables curation, upscaling, and mockups.
1. **Open-source hardware (CERN-OHL, etc.)** — legitimate, but carries the same manufacturing/tariff burden as the patent play; weaker.
1. **Lapsed trademarks / government-funded research (Bayh-Dole march-in, public-access)** — niche and legally intricate; not recommended for a solo operator.

**What I would actually recommend (staged):**

*Stage 0 (this week, ~$0):* Kill the "manufacture in China" version. It fails the weekend-replication test and the tariff/takedown reality.

*Stage 1 (build the tool, not the inventory):* If you want to stay near this space, build a **demand-first Amazon "opportunity scanner"** in your stack (FastAPI + Supabase + Next.js): ingest Keepa product_finder + Explorer-style signals, score niches for "demand but no good product," and layer AI to summarize competitor review complaints into product-improvement briefs. Sell it as SaaS to sellers. This is a real, recurring-revenue business where AI is the enabling piece, and it sidesteps inventory, tariffs, and takedowns entirely. Benchmark to proceed: can you get 20 paying sellers at $29–49/mo from a free demand-report lead magnet?

*Stage 2 (if you insist on a physical product):* Do it demand-first and brand-first, treat any expired patent as at most a design *reference*, budget $10k–12k per SKU, get a one-time paid FTO opinion, and enroll in Brand Registry *yourself* so you hold the takedown weapon rather than fearing it. Only proceed if a validated niche shows >$5k/mo revenue on the top listings with aging incumbents.

*Stage 3 (the actually-good free-IP play):* Pivot the "free IP" instinct to public-domain books/audio or open-access images, where AI does the value-add and there is no factory, no container, and no competitor who can suspend your listing on a whim.

## Recommendations

1. **Do not build the expired-patent → China → Amazon business as conceived.** It is a weak approach: the screen is a free-API weekend project, and every hard part has gotten harder in 2026 (tariffs ~30–35%, de minimis dead, Chinese factory-direct sellers dominant, takedown regime hostile to copycats).
1. **If you build anything in this space, build and sell the demand-first screening tool**, not the products. Use Keepa's API + Product Opportunity Explorer signals; add AI that turns competitor review complaints into "what to build better" briefs. Threshold to double down: 20 paying users in 60 days.
1. **Redirect the "free IP" instinct to public-domain books/audio (KDP/ACX) or open-access museum images (print-on-demand).** These are demand-first-checkable on Amazon/Etsy, need no factory, carry no tariff or inventory risk, and let AI be the core enabler. Start here for fastest cash with least capital.
1. **If you ever manufacture a physical product,** go demand-first and brand-first, budget ~$10–12k/SKU, pay for a per-SKU freedom-to-operate opinion, and enroll in Amazon Brand Registry yourself. Treat any expired patent as a reference drawing, not a business plan.
1. **Kill-switch benchmarks:** abandon the physical-product path if (a) the target niche's top listings show <$5k/mo revenue or fresh well-reviewed incumbents; (b) an FTO opinion flags a live improvement patent, design patent, or trade dress; or (c) landed cost + FBA fees + PPC leave <20% net margin.

## Caveats

- **Sales-estimate accuracy is genuinely uncertain and vendor-conflicted.** Every accuracy figure cited comes from a party with an incentive; treat estimates as ±30% and corroborate two independent tools before spending on inventory.
- **Tariff and trade policy are volatile.** Rates and the de minimis status reflect mid-2026 conditions; a pending Section 301 investigation and litigation could move them either way. Verify the specific HTS code for any product before modeling unit economics.
- **No hard public statistic exists for the false-complaint rate on Amazon's IP tools.** The abuse evidence is strong but anecdotal/legal (lawsuits, expert commentary, individual reinstatement costs); Amazon's "99%" figures measure customer page views, not seller false-positives.
- **PatentsView full-text (claims/descriptions) is still backfilling** from 2023 backward as of the latest documentation; older patents' full text may require the bulk grant files rather than the API.
- **This report assesses viability, not legality of any specific product.** Any actual product requires a real freedom-to-operate review by a licensed patent attorney; nothing here is legal advice.
