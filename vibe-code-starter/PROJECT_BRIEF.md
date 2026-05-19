# Project Brief

## Project name

Hummingbirds TAM Expansion Tool

## One-sentence goal

Build a web scraping and data enrichment tool that identifies CPG brands stocked at major US retailers, filters them against Hummingbirds' ICP, and outputs a HubSpot-ready Google Sheet of net-new accounts to target.

## Problem being solved

Hummingbirds' sales and marketing team has limited visibility into the full universe of CPG brands currently in retail that fit their ICP. Manually researching retailer shelves across dozens of major chains is not scalable. Without a systematic approach, the team risks missing large portions of their addressable market, targeting brands already in their CRM, or spending outbound time on brands that don't fit the profile.

This tool automates the discovery layer — finding every qualifying brand across major US retailers — so the team can focus on outreach and conversion, not research.

## Primary users

- **Pre-Sales / Sales team** — primary consumers; use output to drive net-new outbound targeting
- **Marketing** — use for campaign segmentation and audience building
- **Leadership** — use for TAM sizing and strategic planning

All users are non-technical. Output must require zero technical knowledge to use (Google Sheet, shareable link, copy-paste ready for HubSpot).

## Core user outcomes

Users should be able to:

- [x] Get a comprehensive list of CPG brands stocked at major US retailers that match Hummingbirds' ICP
- [x] Export that list as a Google Sheet with fields formatted for HubSpot import
- [x] Cross-reference the output against their existing HubSpot CRM export to isolate net-new accounts
- [ ] (Phase 2) Identify key contacts at each brand for prospecting

## Hummingbirds ICP — Brand Profile

Hummingbirds sells to CPG brands that:

- Sell **physical consumer products** in **US retail stores** (required — they must be shelf-present at qualifying scale)
- Fall into one of 5 categories: **Food, Beverage, Beauty, Pet, Everyday Goods**
- Have a **distinct brand identity** — not private label, not commodity, not a store brand
- Skew toward **challenger / mid-market brands** — emerging to established (think Olipop, Daily Harvest, Dropps, Wellness Pet, Function of Beauty, not Heinz or Tide)
- Typically have some DTC presence, strong social media, or mission-driven positioning
- Have budget to invest in marketing programs (Hummingbirds ACV is ~$18K, so brands need meaningful marketing spend)
- **Hummingbirds is now ~90% retail-focused** — pure DTC brands do not qualify regardless of category

### Official retail qualification criteria (from ICP flow chart)

A brand qualifies for Hummingbirds if they meet **any one** of these three conditions:

| Path | Criteria |
|---|---|
| A | **Full enterprise-wide distribution** in any Tier 1 retailer |
| B | **Full distribution in 3+ Tier 2/3 retailers** |
| C | **Partial Tier 1 availability + full distribution in 2+ Tier 2/3 retailers** |

**Critical rule:** "Full enterprise-wide distribution required — regional tests or limited rollouts do not qualify." A brand in 300 Target stores all in Texas = partial/regional = does NOT satisfy Path A alone. It would need to also meet the Tier 2/3 threshold (Path C).

### Retailer tier classification

**Tier 1 (9 retailers — full enterprise-wide distribution required):**
Costco, Target, Sprouts, Ulta, Whole Foods, Walmart, Kroger, Petco, PetSmart

**Tier 2 & 3 (qualifying regional/specialty retailers):**
H-E-B, Sephora, Meijer, Publix, Fresh Thyme, Hy-Vee, Trader Joe's, Safeway, Sam's Club, Natural Grocers, The Fresh Market, Albertson's, Food Lion, Wegman's, Harris Teeter, Giant Eagle, King Soopers, Central Market, Schnucks, Fareway, Jewel Osco

**Important nuance — Kroger subsidiaries:** Harris Teeter, King Soopers, and other Kroger-family banners are classified as **Tier 2/3**, not Tier 1. Only enterprise-wide "Kroger" distribution (across all banners nationally) qualifies as Tier 1.

### ICP examples by category

| Category | Example Customers |
|---|---|
| Food | Goodles, Hudsonville Ice Cream, Good Good, Mush, Daily Harvest, Seven Sundays |
| Beverage | Olipop, Pop & Bottle, Recess, Bubbl'r, Sol-ti, Good Boy Vodka |
| Beauty | Plum Beauty, Function of Beauty, Ethique Beauty, Fairy Tales Hair Care |
| Pet | Blue Dog Bakery, Wellness Pet, The Honest Kitchen, Reveal, Dr. Tim's |
| Everyday Goods | Dropps, Graza, Ritual, Slate, Marley Spoon |

### What to exclude

- Legacy CPG giants and their core brands (e.g., Kraft Heinz, P&G, Unilever flagship brands)
- Private label / store brands
- Brands with no qualifying retail distribution (see threshold above)
- Pure DTC brands with no physical retail presence
- Brands in regional-only / limited rollout — not yet at qualifying scale

## Project type

- [x] Internal tool
- [x] Small automation
- [ ] Prototype
- [ ] Production application

## Target retailers to scrape (V1)

Organized by Hummingbirds' official tier classification:

**Tier 1 retailers (full national distribution = instant qualification):**
- Kroger (enterprise-wide)
- Target
- Whole Foods
- Walmart
- Costco
- Sprouts
- Ulta *(Beauty brands)*
- Petco *(Pet brands)*
- PetSmart *(Pet brands)*

**Tier 2/3 retailers (brands need 3+ of these to qualify):**
- H-E-B
- Publix
- Meijer
- Safeway / Albertson's
- Wegmans
- Hy-Vee
- Harris Teeter *(Kroger subsidiary — counts as Tier 2/3, not Tier 1)*
- King Soopers *(Kroger subsidiary — counts as Tier 2/3, not Tier 1)*
- Jewel Osco
- Giant Eagle
- Sephora *(Beauty brands)*
- Sam's Club
- Natural Grocers
- Fresh Thyme
- The Fresh Market
- Food Lion
- Central Market
- Schnucks
- Fareway

**High ICP signal sources (not retailers, but great for brand discovery):**
- Thrive Market *(curates challenger/indie brands)*
- RangeMe *(brands actively seeking retail placement)*

## Data sources strategy

| Source | What it covers | Why |
|---|---|---|
| Kroger API (public) | Kroger family of stores | Official API — structured, reliable, accurate |
| Instacart (web) | Target, Costco, Publix, Meijer, Sprouts, and many more | Single source covering dozens of retailers |
| Retailer websites | Whole Foods brand pages, Target brand directory | Structured brand/category browsing pages |
| RangeMe | Emerging CPG brands seeking retail | High ICP signal — brands actively trying to get into retail |

## Scope

### In scope for V1

- [x] Scrape top US retailers for brands in the 5 ICP categories
- [x] Deduplicate brands across sources (same brand at multiple retailers = one record)
- [x] Enrich each brand with: name, website, category, retailer(s) carrying them, data source
- [x] Export to Google Sheet formatted for HubSpot import
- [x] Support manual cross-reference with HubSpot CRM export (user pastes in existing accounts, tool flags duplicates)

### Out of scope for V1

- [ ] Contact / prospect identification (Phase 2)
- [ ] Automated HubSpot API integration (manual export/import is fine for V1)
- [ ] International markets (US only)
- [ ] Real-time monitoring or alerts
- [ ] Brand scoring or ranking algorithm
- [ ] App or web UI (script output to GSheet is sufficient)

## Functional requirements

The system must:

- [x] Accept a list of target retailers and run scraping/collection across all of them
- [x] Filter results to the 5 ICP categories (Food, Beverage, Beauty, Pet, Everyday Goods)
- [x] Deduplicate brands that appear across multiple retailers
- [x] Enrich each brand record with structured fields (see output schema below)
- [x] Flag or exclude brands that match a user-provided list of existing CRM accounts
- [x] Export results to a CSV / Google Sheet

## Output schema (Google Sheet columns) — LOCKED

| Column | Description |
|---|---|
| Brand Name | Official brand name |
| Website | Brand website URL |
| Category | Food / Beverage / Beauty / Pet / Everyday Goods |
| Sub-category | e.g., Frozen Food, RTD Beverage, Haircare, Dog Treats |
| Tier 1 Retailers | Retailer name + store count inline: "Target (~850), Whole Foods (~520)" |
| Tier 2/3 Retailers | Same format: "H-E-B (~360), Publix (~240)" |
| Total Estimated Store Count | Sum of all store counts across all retailers — sortable number |
| Tier 1 Count | Number of distinct Tier 1 retailers where brand is found |
| Tier 2/3 Count | Number of distinct Tier 2/3 retailers (key for path B/C qualification) |
| Distribution Scope | "Appears National" / "Appears Regional — Needs Review" |
| Retail Footprint Summary | Human-readable one-liner: "National at Target + Whole Foods; Regional at H-E-B, Publix" |
| HB Qualification | "Qualified" / "Likely Qualified — Verify" / "Not Qualified" |
| Qualification Path | A (Full Tier 1) / B (3+ Tier 2/3) / C (Partial Tier 1 + 2+ Tier 2/3) |
| Instagram Handle | Brand Instagram handle (social presence signal for creator platform fit) |
| LinkedIn URL | Brand LinkedIn page URL (for Phase 2 contact prospecting) |
| Headquarters | City, State |
| Founded Year | Brand founding year — may be blank if not publicly findable |
| Data Source | Which scraping source(s) identified this brand |
| Date Scraped | Date record was created |
| CRM Status | "Existing Account" or "Net New" (populated during HubSpot dedup step) |
| Notes | Manual annotations or flags for sales team review |

## Tech stack

### Language

Python 3.11+

### Key libraries

- `requests` / `httpx` — HTTP calls
- `playwright` or `selenium` — dynamic site scraping
- `beautifulsoup4` — HTML parsing
- `pandas` — data processing and deduplication
- `gspread` — Google Sheets API output
- `python-dotenv` — secrets management

### Package manager

pip + `requirements.txt`

### Database

None for V1 — flat CSV / Google Sheet is sufficient

### Hosting / deployment target

Local machine (user runs script manually)

### External services / integrations

- Kroger Developer API (free public API)
- Google Sheets API (for output)
- Instacart web scraping (public pages)
- HubSpot — manual export/import only (no API in V1)

## Inputs and outputs

### Inputs

1. Configuration file listing target retailers and categories
2. (Optional) HubSpot CRM export CSV with existing accounts (for deduplication)

### Outputs

1. Google Sheet with all discovered brands meeting ICP criteria
2. Filtered view showing only net-new accounts (not in existing CRM)

### Example output row

```
Brand Name: Olipop
Website: drinkolipop.com
Category: Beverage
Sub-category: Functional Soda
Retailers: Whole Foods, Target, Kroger, Sprouts
Retailer Count: 4
LinkedIn: linkedin.com/company/drinkolipop
Data Source: Instacart, Kroger API
Date Scraped: 2026-05-19
CRM Status: Net New
```

## Edge cases to handle

- [x] Same brand listed under slightly different names across retailers (fuzzy deduplication)
- [x] Brands with no website listed (capture what's available, flag as incomplete)
- [x] Rate limiting / anti-bot measures from retailers (respectful scraping with delays)
- [x] Retailer page structure changes breaking scrapers (modular scraper design)
- [x] Large result sets (pagination handling)
- [ ] Brands that are subsidiaries of larger conglomerates (flag parent company if detectable)

## Success criteria

This project is successful when:

- [x] The tool runs end-to-end and produces a populated Google Sheet with 500+ brand records
- [x] Results are filtered to the 5 ICP categories with no obvious non-ICP brands in the output
- [x] The Sales team can take the sheet, cross-reference against HubSpot, and immediately begin outbound targeting on net-new accounts
- [x] The scraper covers at least 8 of the 13 target retailers

## Constraints

- Must run locally — no server or cloud deployment needed
- Output must be usable by non-technical users (Google Sheet, no code required to read)
- Must use only publicly accessible data (no login-gated content, no paid APIs in V1)
- Must respect robots.txt and rate limits to avoid IP blocks

## Open questions

- [ ] What is the exact HubSpot field/column name used for "Company Name" and "Website" in their CRM export? (Needed to match dedup logic)
- [ ] Are there any categories within the 5 that should be deprioritized? (e.g., is Everyday Goods lower priority than Food/Beverage?)
- [ ] Should legacy/large brands like Outshine (Nestlé) be included or filtered out? (Outshine appears in their customer list, suggesting larger brands are sometimes in scope)
- [ ] Is there a target number of net-new accounts the team is hoping to identify?

## Current status

In planning — PROJECT_BRIEF.md complete, BUILD_PLAN.md in progress.
