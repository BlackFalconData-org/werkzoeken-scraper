# Werkzoeken.nl Scraper - Dutch Job Listings

Extract structured data from [Werkzoeken.nl](https://Werkzoeken.nl) — Werkzoeken.nl - Dutch job listings with salary ranges, benefits, contract types, employment type, and education filters. Incremental mode tracks new, changed, and reposted jobs via stateKey for recurring recruiter monitors.

**[Werkzoeken.nl Scraper - Dutch Job Listings on Apify →](https://apify.com/blackfalcondata/werkzoeken-scraper?fpr=1h3gvi)**

---

## Key features

**Search with filters** — Search by keyword and location. Filter by country, and more.

**Detail enrichment** — Fetch full job descriptions, structured metadata for each listing.

**Incremental mode** — Only get new or changed listings since your last run. Content hash per listing — no duplicates, no re-processing.

**Change classification** — Track unchanged, expired, cross-run repost detection across runs. Build audit trails of how listings evolve over time.

**Compact output** — Emit core fields only (AI-agent / MCP-friendly). Keeps response size small for LLM workflows.

**Description truncation** — Cap description length per listing to control output size and cost.

**Result cap** — Stop after N listings (up to 1.000). Set to 0 for the full catalog.

**Export anywhere** — Download as JSON, CSV, or Excel. Stream via Apify API, webhooks, or integrations with Make, Zapier, Airbyte, Keboola.

**Structured data** — Every listing returns the same schema with consistent field naming. All fields always present — `null` when unavailable, never omitted.

---

## Use cases

**Data pipeline automation**
Integrate with your ETL pipeline to collect structured listings from Werkzoeken.nl on a schedule. Export to CSV, JSON, or directly to your database. Use compact mode to control output size.

**Market research**
Monitor listings, track trends, and analyze market dynamics with structured, deduplicated data from Werkzoeken.nl.

**Change monitoring**
Run daily or hourly in incremental mode to capture only new, updated, or expired listings. Perfect for price-tracking, churn analysis, and alerting pipelines.

**AI / LLM training data**
Structured JSON per listing is ready for RAG pipelines, embeddings, and agent workflows. Compact mode trims tokens for LLM context windows.

---

## Quick start

```json
{
  "query": "software developer",
  "maxResults": 10,
  "includeDetails": true
}
```

---

## Input parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `query` | string | — | Job search keywords. Use JSON array for multi-query. |
| `country` | enum | `"NL"` | Which werkzoeken.nl market to search. |
| `location` | string | — | City, state, or region. Use JSON array for multi-location. |
| `radiusKm` | integer | `30` | Search radius around the location. |
| `startUrls` | array | — | Direct search or job detail URLs. |
| `maxResults` | integer | `25` | Maximum total job listings to return (0 = unlimited). Dynamic memory scales automatically: up to 50 results uses 512 MB, up to 200 uses 1024 MB, and above 200 uses 2048 MB. |
| `maxPages` | integer | `5` | Maximum SERP pages to scrape per search source. |
| `includeDetails` | boolean | `true` | Fetch each job's detail page for full description, salary data, and company info. |
| `includeCompanyProfile` | boolean | `false` | Fetch company overview pages for industry, headcount, and more. |
| `salaryMin` | integer | — | Monthly salary floor filter. |
| `publishedWithinDays` | integer | — | Filter by posting age. |
| `hours` | array | — | Hour-range filter codes from Werkzoeken.nl. |
| `educationLevels` | array | — | Education filter codes from Werkzoeken.nl. |
| `contractTypes` | array | — | Contract filter codes from Werkzoeken.nl. |
| `experienceLevels` | array | — | Experience filter codes from Werkzoeken.nl. |
| `companyTypes` | array | — | Company-type filter codes from Werkzoeken.nl. |
| `descriptionMaxLength` | integer | `0` | Truncate description to this many characters. 0 = no truncation. |
| `compact` | boolean | `false` | Output only core fields (for AI-agent/MCP workflows). |
| `incrementalMode` | boolean | `false` | Compare against previous run state. Requires stateKey. |
| `stateKey` | string | — | Stable identifier for the tracked search universe (e.g. "us-software-nyc"). |
| `emitUnchanged` | boolean | `false` | When incremental, also emit records that haven't changed. |
| `emitExpired` | boolean | `false` | When incremental, also emit records no longer found. |
| `skipReposts` | boolean | `false` | When incremental, exclude listings flagged by actor repost logic. Repost detection is actor-derived, not source-provided. |

---

## Output fields

Every listing returns the same 56-field schema. Missing values are `null` — never omitted.

- `jobId`
- `jobKey`
- `contentHash`
- `title`
- `company`
- `companyType`
- `companyLogo`
- `companyUrl`
- `companyIndustry`
- `companyEmployeeRange`
- `companyHeaderImage`
- `companyDescription`
- `location`
- `description`
- `descriptionText`
- `descriptionHtml`
- `descriptionMarkdown`
- `descriptionLength`
- `salaryText`
- `salaryRange`
- `salaryMin`
- `salaryMax`
- `salaryCurrency`
- `salaryType`
- `employmentType`
- `contractType`
- `hoursPerWeek`
- `experienceLevel`
- `educationLevel`
- `benefits`
- `postedDate`
- `publishedAge`
- `publishDate`
- `canonicalUrl`
- `applyUrl`
- `sourceUrl`
- `sourceCountry`
- `sourceDomain`
- `searchQuery`
- `searchUrl`
- `isSponsored`
- `scrapedAt`
- `fetchedAt`
- `detailFetched`
- `contentQuality`
- `extractedEmails`
- `changeType`
- `trackedHash`
- `firstSeenAt`
- `lastSeenAt`
- `previousSeenAt`
- `expiredAt`
- `isRepost`
- `repostOfId`
- `repostDetectedAt`
- `stateKey`

---

## Sample output

One object per listing. Here is a real example from a production run:

```json
{
  "jobId": "cd5c69dcae7c65a3049271694fd055928d5168e94939f094fd2372e90065c38e",
  "jobKey": "15584312",
  "contentHash": "5a7dbdfa59dc40460c5aa3b7debcd15ef75c3442ab5000cfa5be6bd16531a745",
  "title": "Schilder",
  "company": "Twente Schildersdiensten",
  "companyType": "Werving en selectie",
  "companyLogo": "https://cdn.werkzoeken.nl/logo_groot/60e434f7b15e8d147341cb362c19d485da916f5bad2257f9.png",
  "companyUrl": "https://www.werkzoeken.nl/werkgever/91993690-27e2-11ef-a6b6-00163ee78db5/?source=iframe&vacancyid=15584312&pane=info",
  "companyIndustry": "Werving en selectie",
  "companyEmployeeRange": "51 - 250 medewerkers",
  "companyHeaderImage": null,
  "companyDescription": "Ben je als schilder op zoek naar werk? Of naar een werkgever die uw belangen behartigd? De aanpak van uitzendbureau Twente Schildersdiensten is uniek binnen de uitzendbranche! Cont…"
}
```

*Truncated — full records contain 56 fields. See Output fields for the complete schema.*

**[Try Werkzoeken.nl Scraper - Dutch Job Listings now — $5 free credit, no credit card →](https://apify.com/blackfalcondata/werkzoeken-scraper?fpr=1h3gvi)**

---

## Pricing

Pay only for what you extract. No subscription required — Apify's free $5 credit covers thousands of results.

| Event | Price (USD) |
| --- | --- |
| Actor Start | $0.00005 |
| Result | $0.0015 |

See the [actor on Apify](https://apify.com/blackfalcondata/werkzoeken-scraper?fpr=1h3gvi) for current pricing.

---

## FAQ

**How do I scrape Werkzoeken.nl?**
Use this actor on Apify to extract structured data from Werkzoeken.nl. Configure your search query and filters in the input, then click Start — no coding required.

**How do I get Werkzoeken.nl data as JSON, CSV, or Excel?**
The actor writes each listing to Apify's dataset. Download as JSON, CSV, or Excel from the Console, stream via the API, or push to Make, Zapier, Airbyte, or Keboola.

**Is it legal to scrape Werkzoeken.nl?**
Web scraping of publicly available data is generally legal. This actor only accesses publicly visible information. Always check Werkzoeken.nl's terms of service for your specific use case.

**How much does it cost?**
Pay-per-event pricing — you only pay for listings extracted. Apify's free $5 credit is enough to run thousands of results before you pay anything.

**How does incremental mode work?**
Each listing gets a content hash. On subsequent runs, only new or changed listings are emitted — saving time, compute, and storage. Expired listings can be tracked separately.

**Do I need an API key or credentials?**
No. Just sign up for Apify, paste your input, and click Start. No credit card required.

---

## Related products by Black Falcon Data

- [StepStone Scraper](https://apify.com/blackfalcondata/stepstone-scraper?fpr=1h3gvi) — Job listings from 18 European portals
- [Indeed Job Scraper](https://apify.com/blackfalcondata/indeed-job-scraper?fpr=1h3gvi) — Indeed job listings with salary data
- [Glassdoor Job Scraper](https://apify.com/blackfalcondata/glassdoor-job-scraper?fpr=1h3gvi) — Glassdoor listings with company ratings
- [Arbeitsagentur Scraper](https://apify.com/blackfalcondata/arbeitsagentur-scraper?fpr=1h3gvi) — Germany's official job portal (1M+ listings)
- [SEEK Scraper](https://apify.com/blackfalcondata/seek-scraper?fpr=1h3gvi) — Australia & NZ's largest job board
- [Naukri Scraper](https://apify.com/blackfalcondata/naukri-scraper?fpr=1h3gvi) — India's largest job portal

---

## Getting started with Apify

New to Apify? [Create a free account with $5 credit](https://console.apify.com/sign-up?fpr=1h3gvi) — no credit card required.

1. Sign up — $5 platform credit included
2. Open [Werkzoeken.nl Scraper - Dutch Job Listings](https://apify.com/blackfalcondata/werkzoeken-scraper?fpr=1h3gvi) and configure your input
3. Click **Start** — export results as JSON, CSV, or Excel

Need more later? [See Apify pricing](https://apify.com/pricing?fpr=1h3gvi).

---

## About Black Falcon Data

Black Falcon Data builds production-grade web scrapers for job boards and marketplace data. Browse our full actor catalog at [www.blackfalcondata.com](https://www.blackfalcondata.com).

---

*Last updated: 2026 04*
