# Decision Log

Use this file to prevent context rot. Record meaningful product, architecture, dependency, and workflow decisions.

---

## 2026-05-19 — Python as the implementation language

### Decision
Build the tool in Python 3.11+.

### Why
Best scraping ecosystem available — requests, playwright, BeautifulSoup, pandas are mature and well-documented. The team running this tool is non-technical, so the language only matters for maintenance; Python has the widest contributor base for future changes.

### Alternatives considered
Node.js (Puppeteer/Playwright), which has comparable scraping capability.

### Tradeoffs
Python wins on data processing (pandas for deduplication and enrichment) and has a simpler path to Google Sheets API integration. Node.js would be faster for async scraping at scale, but scale is not a V1 concern.

### Revisit if
Performance becomes a bottleneck at very large result sets (10,000+ brands), or if a team member with Node expertise takes over maintenance.

---

## 2026-05-19 — Kroger API as Phase 1 starting point

### Decision
Begin scraping with the Kroger public API before any web scraping.

### Why
Kroger offers a free public developer API that returns structured product and brand data. It is the most reliable, accurate, and rate-limit-friendly starting point. Structured API data reduces early-stage complexity and gives us a clean baseline to validate the pipeline before adding scraping complexity.

### Alternatives considered
Starting with Instacart scraping (wider retailer coverage from day one) or Whole Foods (high ICP signal).

### Tradeoffs
Kroger API gives clean data but covers only the Kroger family. Instacart covers more retailers but is scraping-dependent and more fragile. Starting with the API lets us validate the end-to-end pipeline cheaply before adding fragile scrapers.

### Revisit if
Kroger API access is denied, rate-limited severely, or returns insufficient brand-level data.

---

## 2026-05-19 — Instacart as multi-retailer aggregator (Phase 2)

### Decision
Use Instacart's public-facing web platform as a single scraping source to cover Target, Costco, Publix, Meijer, Sprouts, Safeway, Albertson's, and many others.

### Why
Instacart aggregates inventory from 30+ major US retailers in one place. Scraping it once gives broader coverage than building individual scrapers for each retailer. Most of the Tier 1 and Tier 2/3 retailers in Hummingbirds' flow chart are accessible through Instacart.

### Alternatives considered
Building individual scrapers for each retailer from the start.

### Tradeoffs
Instacart as a single dependency means a page structure change can break coverage for many retailers at once. Mitigated by the modular scraper design — each retailer source is an independent module, so Instacart is one module even if it covers many chains.

### Revisit if
Instacart adds bot detection that makes scraping unreliable, or if a major retailer (e.g., Target) leaves the Instacart platform.

---

## 2026-05-19 — No database — flat CSV and Google Sheets output only

### Decision
Store all data as flat CSV files locally and push final output to Google Sheets. No relational database.

### Why
The primary users (Sales, Marketing, Leadership) are non-technical. The entire value of the output is a shareable, filterable spreadsheet they can use immediately. A database adds setup complexity, requires a technical operator, and provides no user-facing benefit for V1.

### Alternatives considered
SQLite (local, lightweight), Airtable (collaborative, API-driven).

### Tradeoffs
No database means no queryable history or incremental updates across runs. Each full run produces a fresh output. Acceptable for V1 where runs are manual and infrequent. SQLite or Airtable would be the right upgrade if the tool runs on a schedule or needs to track changes over time.

### Revisit if
The team wants to run the tool on a recurring schedule and track net-new brands added since the last run.

---

## 2026-05-19 — No HubSpot API integration in V1

### Decision
HubSpot deduplication is done via manual export/import — user exports existing CRM accounts as CSV, tool cross-references it, user re-imports net-new accounts.

### Why
HubSpot API integration requires OAuth setup, credential management, and handling of HubSpot's object model — meaningful added complexity for a V1 internal tool. Manual export/import achieves the same result with zero API risk.

### Alternatives considered
Full HubSpot API integration using the HubSpot Python SDK.

### Tradeoffs
Manual process adds a few minutes of human steps per run. Acceptable for V1 where runs are infrequent. Full API integration would be the right Phase 2 upgrade if the tool runs frequently or the team wants one-click imports.

### Revisit if
The team is running deduplication more than once a week, or if HubSpot import/export becomes a pain point in practice.

---

## 2026-05-19 — Fuzzy matching for brand deduplication

### Decision
Use fuzzy string matching (e.g., `thefuzz` / `rapidfuzz` library) to deduplicate brand names across sources, rather than exact string matching.

### Why
The same brand appears differently across data sources — "OLIPOP" vs "Olipop" vs "Olipop Inc." Exact matching would create duplicate rows for the same brand. Fuzzy matching with a configurable threshold catches these variants automatically.

### Alternatives considered
Exact match only (simpler but produces duplicates), manual deduplication (not scalable).

### Tradeoffs
Fuzzy matching can produce false positives — merging two different brands that have similar names. Mitigated by setting a conservative threshold and flagging low-confidence matches for human review rather than auto-merging.

### Revisit if
False positive merge rate is high in practice, or if a better dedup signal emerges (e.g., matching on website domain instead of brand name).

---

## 2026-05-19 — Default to "Needs Review" for ambiguous distribution data

### Decision
When the tool cannot confidently determine whether a brand has full enterprise-wide vs. regional distribution at a Tier 1 retailer, it defaults to flagging the record as "Appears Regional — Needs Review" rather than auto-qualifying.

### Why
Hummingbirds' qualification criteria require full enterprise-wide distribution for Tier 1 to count. Incorrectly auto-qualifying a regional brand wastes the sales team's time on unqualified outreach. False negatives (flagging a qualified brand for review) are cheaper than false positives (calling an unqualified brand).

### Alternatives considered
Auto-qualifying based on presence alone (simpler but inaccurate), requiring manual verification for all Tier 1 brands (too slow).

### Tradeoffs
"Needs Review" flags add work for the sales team, but only on genuinely ambiguous cases. The Kroger API (which returns store-level location data) reduces ambiguity significantly for Kroger-family brands.

### Revisit if
The "Needs Review" volume is too high in practice and becomes a bottleneck. Would then consider building a store-count threshold rule (e.g., >500 stores at a Tier 1 chain = auto-qualify as national).

---

## 2026-05-19 — 3-path ICP qualification logic from official flow chart

### Decision
Implement Hummingbirds' qualification logic exactly as defined in their official ICP flow chart:
- **Path A:** Full enterprise-wide distribution at any Tier 1 retailer → Qualified
- **Path B:** Full distribution at 3+ Tier 2/3 retailers → Qualified
- **Path C:** Partial Tier 1 availability + full distribution at 2+ Tier 2/3 retailers → Qualified
- **Otherwise:** Not Qualified

### Why
This is Hummingbirds' own published framework, confirmed by the CSO. It is the authoritative definition of a qualified brand. Building any other logic would diverge from how the sales team actually qualifies prospects.

### Alternatives considered
Simpler single-threshold rule (e.g., "present at 2+ retailers"), custom scoring model.

### Tradeoffs
The 3-path logic is more complex to implement than a single rule, but it exactly matches the sales team's mental model. A simpler rule would either over-qualify (waste sales time) or under-qualify (miss real opportunities).

### Revisit if
Hummingbirds updates their qualification criteria — the ICP flow chart should be treated as the source of truth and this logic updated to match.

---

## 2026-05-19 — Kroger subsidiaries classified as Tier 2/3, not Tier 1

### Decision
Harris Teeter, King Soopers, Jewel Osco, and other Kroger-family banner stores are classified as Tier 2/3 retailers in the qualification engine. Only enterprise-wide "Kroger" distribution (across all banners nationally) qualifies as Tier 1.

### Why
Hummingbirds' official flow chart explicitly lists Harris Teeter and King Soopers under Tier 2/3, not Tier 1. A brand present only in Harris Teeter stores (Southeast US) has regional, not national, distribution despite being technically within the Kroger corporate family.

### Alternatives considered
Treating all Kroger-family stores as Tier 1 (would over-qualify regional brands).

### Tradeoffs
Requires maintaining an explicit list of Kroger subsidiary banner names in the retailer classifier. This list may need updating if Kroger acquires or divests banners.

### Revisit if
Hummingbirds updates their tier classification, or if a Kroger subsidiary achieves truly national scale that warrants reclassification.

---

## 2026-05-19 — 21-column locked output schema

### Decision
The Google Sheet output uses a fixed 21-column schema. See `PROJECT_BRIEF.md` for the full column definitions.

### Why
Locking the schema early ensures the scraping, enrichment, and output phases are all building toward the same target format. Columns were chosen to serve three specific jobs: (1) qualification decision, (2) sales team readability, (3) HubSpot import compatibility.

### Alternatives considered
Flexible/dynamic schema that adds columns as data sources provide them (more fragile, harder for non-technical users to rely on).

### Tradeoffs
A fixed schema means some columns will be blank for brands where data is unavailable (e.g., Founded Year). This is acceptable — blank is better than inconsistent column positions across runs.

### Revisit if
The sales team requests additional fields after using the output in practice, or HubSpot import requires different field names.

---

## 2026-05-19 — Modular scraper architecture

### Decision
Each data source (Kroger, Instacart, Whole Foods, H-E-B, Petco, etc.) is implemented as an independent module with a consistent interface. The main runner calls each module and merges results.

### Why
Retailer websites change. When one scraper breaks, it should not break the entire pipeline. Modular design also makes it easy to add new retailers without touching existing scrapers.

### Alternatives considered
Single monolithic scraper script (simpler initially, becomes unmaintainable quickly).

### Tradeoffs
Slightly more upfront structure to set up. Pays off immediately when any one retailer's page structure changes — fix one module, rest of pipeline unaffected.

### Revisit if
The number of data sources grows so large that a plugin/registry pattern would be cleaner than individual module files.
