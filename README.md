# Naukrigulf Scraper

Extract structured job listings from [naukrigulf.com](https://naukrigulf.com) — the Gulf-region job board covering UAE, Saudi Arabia, Qatar, Kuwait, Bahrain, and Oman. Returns structured salary and contact info per listing, with incremental change tracking for recurring runs.

**[Naukrigulf Jobs Scraper - Gulf Job Board on Apify →](https://apify.com/blackfalcondata/naukrigulf-scraper?fpr=1h3gvi)**


## 🚀 How to use this actor

> ### 💚 $5 free Apify credits — every month
> No credit card required. No commitment. Cancel anytime.

### 👉 [Sign up free on Apify →](https://console.apify.com/sign-up?fpr=1h3gvi)

1. **Click sign up** — pick GitHub, Google, or email; takes ~30 seconds
2. **Open this actor** — input is pre-filled with a working example
3. **Click Start** — export results as JSON, CSV, or Excel

Your **$5 monthly platform credit** is enough to run this actor right away — and again every month — scraping typically several hundred to several thousand results per run, depending on your input.



## 🚀 How to use this actor

> ### 💚 $5 free Apify credits — every month
> No credit card required. No commitment. Cancel anytime.

### 👉 [Sign up free on Apify →](https://console.apify.com/sign-up?fpr=1h3gvi)

1. **Click sign up** — pick GitHub, Google, or email; takes ~30 seconds
2. **Open this actor** — input is pre-filled with a working example
3. **Click Start** — export results as JSON, CSV, or Excel

Your **$5 monthly platform credit** is enough to run this actor right away — and again every month — scraping typically several hundred to several thousand results per run, depending on your input.


---

## Key features









**Search with filters** — Search by keyword and location. Filter by description format, and more.

**Multiple input modes** — keyword search or direct job urls. Switch modes without re-scraping.

**Direct URL mode** — Paste a list of listing URLs and fetch each one directly. Skips search — ideal for monitoring known IDs.

**Detail enrichment** — Fetch full job descriptions, salary data, employer profiles, contact information for each listing.

**Incremental mode** — Only get new or changed listings since your last run. Content hash per listing — no duplicates, no re-processing.

**Change classification** — Track unchanged, expired, cross-run repost detection across runs. Build audit trails of how listings evolve over time.

**Compact output** — Emit core fields only (AI-agent / MCP-friendly). Keeps response size small for LLM workflows.

**Description truncation** — Cap description length per listing to control output size and cost.

**Result cap** — Stop after N listings (up to 5.000). Set to 0 for the full catalog.

**Export anywhere** — Download as JSON, CSV, or Excel. Stream via Apify API, webhooks, or integrations with Make, Zapier, Airbyte, Keboola.

**Structured data** — Every listing returns the same schema with consistent field naming. All fields always present — `null` when unavailable, never omitted.

---

## Use cases









**Data pipeline automation**
Integrate with your ETL pipeline to collect structured listings from naukrigulf.com on a schedule. Export to CSV, JSON, or directly to your database. Use compact mode to control output size.

**Market research**
Monitor listings, track trends, and analyze market dynamics with structured, deduplicated data from naukrigulf.com.

**Change monitoring**
Run daily or hourly in incremental mode to capture only new, updated, or expired listings. Perfect for price-tracking, churn analysis, and alerting pipelines.

**Compensation benchmarking**
Aggregate salary ranges across roles, industries, and locations on naukrigulf.com to inform pricing decisions, hiring plans, or candidate negotiations.

**Lead generation**
Extract employer contact details alongside listings to build outreach lists for recruiters, staffing agencies, or B2B sales teams.

**AI / LLM training data**
Structured JSON per listing is ready for RAG pipelines, embeddings, and agent workflows. Compact mode trims tokens for LLM context windows.

---

## Quick start

```json
{
  "query": "software engineer",
  "location": "dubai",
  "maxResults": 10
}
```

---

## Input parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `mode` | enum | `"search"` | How to discover jobs. 'search' runs a keyword + location search. 'jobUrls' skips search and fetches each canonical naukrigulf job URL directly (incremental works per JobId, 404s cached 30d). |
| `query` | string | — | Job keywords, e.g. 'developer', 'accountant', 'project manager'. Accepts multiple keywords separated by commas. Required when mode=search. |
| `jobUrls` | array | — | List of canonical naukrigulf.com job URLs (e.g. https://www.naukrigulf.com/<seo-slug>-jid-140426000178). Required when mode=jobUrls. Skips search and fetches the detail API directly per JobId. 404s are cached for 30 days. |
| `location` | string | — | City or country filter (e.g. 'dubai', 'riyadh', 'uae', 'qatar'). Leave empty for all Gulf locations. |
| `maxResults` | integer | `25` | Maximum number of jobs to fetch. 0 = unlimited (fetches up to the API total). |
| `includeDetails` | boolean | `true` | Fetch each job's full detail page (description, desired candidate, contact info, salary). Slower but richer output. |
| `descriptionMaxLength` | integer | `0` | Truncate description to N characters. 0 = no truncation. |
| `compact` | boolean | `false` | Emit only core fields (jobId, title, company, location, salary, url, postedAt). Useful for AI agents and MCP workflows. |
| `incrementalMode` | boolean | `false` | Compare against previous run state; emit NEW/UPDATED/UNCHANGED/EXPIRED classification. |
| `stateKey` | string | — | Stable identifier for the tracked universe (required when Incremental Mode is on). |
| `emitUnchanged` | boolean | `false` | Also emit jobs that have not changed since the last run. |
| `emitExpired` | boolean | `false` | Also emit jobs that have disappeared since the last run. |
| `skipReposts` | boolean | `false` | Skip jobs whose content matches a previously expired job (cross-run repost detection). |

---

## FAQ

**How to scrape naukrigulf?**
Use this actor on Apify to extract structured data from naukrigulf.com. Set your search query and filters in the input, then run — no coding needed.

**How can I get naukrigulf data as JSON?**
This actor returns structured JSON data from naukrigulf — ready to ingest into spreadsheets, databases, or downstream pipelines without custom parsing.

**Is it legal to scrape naukrigulf.com?**
Web scraping of publicly available data is generally legal. This actor only accesses publicly visible information. Always check the target site's terms of service for your specific use case.

**How does incremental mode work?**
Each listing gets a content hash. On subsequent runs, only new or changed listings are emitted — saving time, compute, and storage.

---

## Known limitations

- Naukrigulf.com enforces rate limits on rapid sequential requests. The actor respects these automatically via built-in request throttling.
- Contact fields (recruiter email, phone) are only present on a subset of listings — many employers mark them as hidden, in which case the corresponding `*Hidden` flag is set and the field returns null.
- Salary data is self-reported by employers and often presented in ranges or marked confidential. Only publicly visible salary values are extracted.
- Search results are limited to what Naukrigulf's search engine returns for a given keyword + location. Very broad queries may hit platform-side pagination caps.
- Some detail fields (desired candidate, localities, industry) require detail enrichment (`includeDetails: true`). Disable for faster compact runs when only core fields are needed.


## Output fields

Every listing returns the same 81-field schema. Missing values are `null` — never omitted.

- `jobId`
- `jobKey`
- `title`
- `summary`
- `company`
- `companyId`
- `companyLogo`
- `companyLogoTopEmployer`
- `companyProfile`
- `companyWebsite`
- `companyHomepage`
- `location`
- `locationType`
- `jobCountry`
- `localities`
- `description`
- `descriptionHtml`
- `descriptionMarkdown`
- `desiredCandidate`
- `desiredCandidateHtml`
- `education`
- `nationality`
- `gender`
- `experienceMin`
- `experienceMax`
- `salaryMinText`
- `salaryMaxText`
- `salaryMin`
- `salaryMax`
- `salaryCurrency`
- `salaryCurrencyBrand`
- `salaryType`
- `salaryHidden`
- `employmentType`
- `industry`
- `functionalArea`
- `vacancies`
- `keywords`
- `keywordsWhitelisted`
- `contactName`
- `contactDesignation`
- `contactCountry`
- `contactCity`
- `contactAddress`
- `contactPincode`
- `contactPhone`
- `contactEmail`
- `contactEmailHidden`
- `isTopEmployer`
- `isTopEmployerLite`
- `isFeaturedEmployer`
- `isPremium`
- `isWebJob`
- `isQuickWebJob`
- `isFormBasedApply`
- `isEasyApply`
- `isConsultantJob`
- `isConfidentialCompany`
- `isArchived`
- `isExpired`
- `expiringSoon`
- `jobRedirection`
- `recruiterActive`
- `jobSource`
- `jobType`
- `jobTag`
- `referenceNumber`
- `micrositeUrl`
- `postedAt`
- `url`
- `portalUrl`
- `searchQuery`
- `contentQuality`
- `detailFetched`
- `scrapedAt`
- `source`
- `contentHash`
- `isRepost`
- `repostOfId`
- `repostDetectedAt`
- `changeType`


## Sample output

One object per listing. Here is a real example from a production run:

```json
{
  "jobId": "d59bda0f65b592d582f2e5c4101eba143fdcd9806324c04f62fd4f07ab94ce36",
  "jobKey": "140426000178",
  "title": "Sharepoint Developer, Share Point Developer",
  "summary": "Design and develop custom SharePoint solutions, implement SharePoint environments, and collaborate with stakeholders, requiring skills in C#, PowerShell, and relevant certification…",
  "company": "Hexalyze LLC",
  "companyId": "340739",
  "companyLogo": null,
  "companyLogoTopEmployer": null,
  "companyProfile": null,
  "companyWebsite": "www.hexalyze.com",
  "companyHomepage": null,
  "location": "Dubai - United Arab Emirates (UAE)"
}
```

*Truncated — full records contain 81 fields. See Output fields for the complete schema.*


**[Try Naukrigulf Jobs Scraper - Gulf Job Board now — $5 free credit, no credit card →](https://apify.com/blackfalcondata/naukrigulf-scraper?fpr=1h3gvi)**


## Pricing

Pay only for what you extract. No subscription required — Apify's free $5 credit covers thousands of results.

| Event | Price (USD) |
| --- | --- |
| Actor Start | $0.005 |
| Result | $0.001 |

See the [actor on Apify](https://apify.com/blackfalcondata/naukrigulf-scraper?fpr=1h3gvi) for current pricing.

---

## Related products by Black Falcon Data









- [StepStone Scraper](https://apify.com/blackfalcondata/stepstone-scraper?fpr=1h3gvi) — Job listings from 18 European portals
- [Indeed Job Scraper](https://apify.com/blackfalcondata/indeed-job-scraper?fpr=1h3gvi) — Indeed job listings with salary data
- [LinkedIn Jobs Scraper](https://apify.com/blackfalcondata/linkedin-jobs-scraper?fpr=1h3gvi) — World's largest professional network — global job listings, no login required
- [Glassdoor Job Scraper](https://apify.com/blackfalcondata/glassdoor-job-scraper?fpr=1h3gvi) — Glassdoor listings with company ratings
- [Arbeitsagentur Scraper](https://apify.com/blackfalcondata/arbeitsagentur-scraper?fpr=1h3gvi) — Germany's official job portal (1M+ listings)
- [SEEK Scraper](https://apify.com/blackfalcondata/seek-scraper?fpr=1h3gvi) — Australia & NZ's largest job board

---


## Getting started with Apify

New to Apify? [Create a free account with $5 credit](https://console.apify.com/sign-up?fpr=1h3gvi) — no credit card required.

1. [Sign up free](https://console.apify.com/sign-up?fpr=1h3gvi) — $5 credit included
2. Open the actor and paste your input
3. Click Start — results download as JSON, CSV, or Excel

Need more volume? [See pricing](https://apify.com/pricing?fpr=1h3gvi).

---


## About Black Falcon Data

Black Falcon Data builds production-grade web scrapers for job boards and marketplace data. Browse our full actor catalog at [www.blackfalcondata.com](https://www.blackfalcondata.com).

---
---

*Last updated: 2026 04*
