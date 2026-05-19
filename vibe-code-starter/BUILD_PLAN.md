# Build Plan

## Project

Hummingbirds TAM Expansion Tool

## Approach

Build a modular Python scraper that collects CPG brand data from major US retailers, filters by Hummingbirds' ICP, deduplicates, enriches, and outputs to Google Sheets. Each phase is independently runnable and testable. Start with the cleanest, most reliable data source (Kroger API) and expand outward.

---

## Phase 1 — Project scaffold + Kroger API

**Goal:** Working end-to-end pipeline for one retailer.

Tasks:
- [ ] Set up project structure (`/scrapers`, `/data`, `/output`, `/config`)
- [ ] Create `config.yaml` (target retailers, ICP categories, output settings)
- [ ] Set up `requirements.txt` and virtual environment
- [ ] Build Kroger API client (OAuth2 + product search by category)
- [ ] Map Kroger product categories → Hummingbirds ICP categories (Food, Beverage, Beauty, Pet, Everyday Goods)
- [ ] Extract brand names + websites from Kroger product data
- [ ] Save raw results to CSV
- [ ] Write deduplication logic (normalize brand names, fuzzy match)

**Checkpoint:** Running `python run.py --source kroger` produces a CSV of brands found at Kroger, filtered to ICP categories.

---

## Phase 2 — Instacart coverage (multi-retailer)

**Goal:** Dramatically expand retailer coverage using Instacart as an aggregator, and add the Tier 1 specialty retailers missing from the Kroger API.

Retailers covered via Instacart: Target, Costco, Publix, Meijer, Sprouts, Safeway, Albertson's, and more.

Additional Tier 1 retailers to scrape directly (not on Instacart or Kroger API):
- **Petco** — Tier 1, critical for Pet brands
- **PetSmart** — Tier 1, critical for Pet brands
- **Ulta** — Tier 1, critical for Beauty brands
- **Walmart** — supplement Instacart with direct scraping for broader coverage

Tasks:
- [ ] Build Instacart scraper (search by category across connected retailers)
- [ ] Build Petco product catalog scraper (petco.com/brand pages)
- [ ] Build PetSmart product catalog scraper (petsmart.com/brand pages)
- [ ] Build Ulta brand directory scraper (ulta.com/brand pages)
- [ ] Map all retailer categories → ICP categories
- [ ] Extract brand name, product name, retailer from results
- [ ] Merge all Phase 2 results with Phase 1 data (deduplicate across sources)
- [ ] Add `Tier 1 Retailers` and `Tier 2/3 Retailers` columns (separate, not combined)

**Checkpoint:** Running `python run.py --source instacart` adds brands from Target, Costco, Publix, and others. Combined dataset has 300+ unique brands.

---

## Phase 3 — Retailer website scrapers (gap fill)

**Goal:** Cover retailers not available via Kroger API or Instacart.

Priority targets:
- Whole Foods brand/department pages
- H-E-B (Texas — not on Instacart, has own product catalog)
- Hy-Vee (Midwest — not fully covered by Instacart)
- Wegmans (Northeast)
- Natural Grocers by Vitamin Cottage (Western US health food — high ICP signal)
- Fresh Thyme Market (Midwest health food — high ICP signal)
- Thrive Market (high ICP signal)
- RangeMe (emerging brands seeking retail — excellent ICP filter; catches brands with limited distribution that may only appear at one regional chain)

Tasks:
- [ ] Build Whole Foods brand directory scraper
- [ ] Build H-E-B product catalog scraper (heb.com)
- [ ] Build Hy-Vee scraper
- [ ] Build Natural Grocers + Fresh Thyme scrapers
- [ ] Build Thrive Market category scraper
- [ ] Build RangeMe scraper (public-facing product listings)
- [ ] Modular scraper interface so new retailers can be added easily
- [ ] Merge all sources, re-deduplicate

**Checkpoint:** Running `python run.py --all` produces a unified brand list from 8+ retailers.

---

## Phase 4 — Qualification scoring + data enrichment

**Goal:** Apply Hummingbirds' official ICP qualification logic to every brand, and enrich records for sales outreach.

**Qualification logic to implement:**
```
tier1_full    = brand has full enterprise-wide distribution at any Tier 1 retailer
tier1_partial = brand has regional/partial presence at a Tier 1 retailer
tier2_3_count = number of distinct Tier 2/3 retailers with full distribution

if tier1_full:
    → "Qualified" (Path A)
elif tier2_3_count >= 3:
    → "Qualified" (Path B)
elif tier1_partial and tier2_3_count >= 2:
    → "Qualified" (Path C)
else:
    → "Not Qualified — Too Early"
```

**Full vs. partial distribution detection:**
- Use store location count as proxy where available (Kroger API provides this)
- If a brand appears in only 1 geographic cluster of a Tier 1 retailer → flag as "Appears Regional — Needs Review"
- If a brand appears across multiple regions of the same Tier 1 chain → flag as "Appears National"
- When uncertain, default to "Needs Review" — never auto-qualify on ambiguous data

Tasks:
- [ ] Build `retailer_classifier.py` — maps each retailer name to Tier 1 or Tier 2/3
- [ ] Build `qualification_engine.py` — applies the 3-path logic above
- [ ] Add `Distribution Scope` detection per Tier 1 retailer using store count signals
- [ ] Resolve brand website from name (Google search fallback if not in source data)
- [ ] Scrape LinkedIn company URL for each brand (using brand name + website)
- [ ] Assign ICP sub-category (e.g., "Frozen Food", "RTD Beverage", "Dog Treats")
- [ ] Add `Retailer Count`, `Tier 2/3 Count`, `Date Scraped` columns

**Checkpoint:** Every brand in the output has a `HB Qualification` status and `Qualification Path` populated. Sales team can filter to "Qualified" rows immediately.

---

## Phase 5 — HubSpot deduplication + Google Sheets output

**Goal:** Produce the final net-new account list ready for sales use.

Tasks:
- [ ] Accept HubSpot CRM export CSV as input
- [ ] Match exported accounts against scraped brand list (by website domain and brand name)
- [ ] Tag each scraped brand as "Existing Account" or "Net New" in `CRM Status` column
- [ ] Filter output to Net New only (separate tab or filtered view)
- [ ] Push final output to Google Sheet via Google Sheets API
- [ ] Format GSheet: freeze header row, color-code Net New vs Existing, auto-resize columns

**Checkpoint:** User uploads HubSpot export CSV, runs `python run.py --dedup hubspot_export.csv --output gsheet`, and receives a shareable Google Sheet link with two tabs: All Brands and Net New Only.

---

## Phase 6 (Future) — Contact prospecting

**Goal:** For each net-new brand, identify key decision-maker contacts.

*Out of scope for V1. Planned for Phase 2 of the project.*

Likely approach:
- LinkedIn Sales Navigator or Apollo.io API
- Target titles: VP Marketing, CMO, Head of Retail, Brand Manager
- Add contact fields to output: First Name, Last Name, Title, Email (if available), LinkedIn URL

---

## File structure

```
hummingbirds-tam/
├── config/
│   └── config.yaml          # Retailers, categories, API keys
├── scrapers/
│   ├── kroger.py            # Kroger API client
│   ├── instacart.py         # Instacart web scraper
│   ├── whole_foods.py       # Whole Foods scraper
│   ├── thrive_market.py     # Thrive Market scraper
│   └── range_me.py          # RangeMe scraper
├── enrichment/
│   ├── website_resolver.py  # Find brand website from name
│   └── linkedin.py          # Find LinkedIn company URL
├── processing/
│   ├── dedup.py             # Fuzzy deduplication logic
│   ├── categorizer.py       # Map categories to ICP taxonomy
│   └── hubspot_dedup.py     # Cross-reference HubSpot CRM export
├── output/
│   └── gsheets.py           # Google Sheets API writer
├── data/
│   ├── raw/                 # Raw scraper output (CSV per source)
│   └── processed/           # Merged, deduplicated, enriched data
├── run.py                   # Main entry point
├── requirements.txt
└── .env                     # API keys (gitignored)
```

---

## Decisions made

| Decision | Rationale |
|---|---|
| Python over Node.js | Better scraping ecosystem (requests, playwright, pandas) |
| Kroger API first | Free public API — most reliable starting point |
| Instacart as aggregator | Covers 10+ retailers in one source |
| Flat CSV / GSheet output | Non-technical users; no database needed |
| No HubSpot API in V1 | Manual export/import is sufficient, avoids API auth complexity |
| Fuzzy matching for dedup | Brand names vary slightly across retailers ("Olipop" vs "OLIPOP") |

---

## Risk register

| Risk | Likelihood | Mitigation |
|---|---|---|
| Full vs. partial distribution misclassified | High | Default to "Needs Review" when uncertain; never auto-qualify on ambiguous data; use store count as proxy |
| Kroger subsidiary stores miscounted as Tier 1 | High | Hardcode subsidiary banner list (Harris Teeter, King Soopers, etc.) as Tier 2/3 in classifier |
| Retailer blocks scraper | Medium | Rate limiting, rotating user agents, respectful crawl delays |
| Instacart changes page structure | Medium | Modular scraper, easy to update |
| Brand name normalization errors | High | Fuzzy matching + manual review pass |
| Brand qualifies on paper but is too early/niche | Medium | "Needs Review" flag for sales team; tool surfaces, humans decide |
| Kroger API rate limits | Low | Caching raw results locally |
| Google Sheets API quota | Low | Batch writes, well within free tier limits |

---

## Active task list

- [ ] Review and approve PROJECT_BRIEF.md
- [ ] Review and approve BUILD_PLAN.md
- [ ] Register for Kroger Developer API
- [ ] Set up Google Sheets API credentials
- [ ] Begin Phase 1 scaffold

## Blockers

- Awaiting user approval of brief and build plan before writing code

## Next checkpoint

Requirements approved → begin Phase 1 scaffold
