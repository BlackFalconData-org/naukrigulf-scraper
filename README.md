# Naukrigulf Scraper

Extract structured job listings from [naukrigulf.com](https://naukrigulf.com) — the Gulf-region job board covering UAE, Saudi Arabia, Qatar, Kuwait, Bahrain, and Oman. Returns structured salary and contact info per listing, with incremental change tracking for recurring runs.

**[Naukrigulf Jobs Scraper - Gulf Job Board on Apify →](https://apify.com/blackfalcondata/naukrigulf-scraper?fpr=1h3gvi)**

---

## Key features


**Search with filters** — Search by keyword and location.

**Detail enrichment** — Fetch full job descriptions, salary data, employer profiles, contact information for each listing.

**Incremental mode** — Only get new or changed listings since your last run. Content hash per listing — no duplicates, no re-processing.

---

## Use cases


**Data pipeline automation**
Integrate with your ETL pipeline to collect structured listings from naukrigulf.com on a schedule. Export to CSV, JSON, or directly to your database. Use compact mode to control output size.

**Market research**
Monitor listings, track trends, and analyze market dynamics with structured, deduplicated data from naukrigulf.com.

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

---

## Related products by Black Falcon Data


- [StepStone Scraper](https://github.com/BlackFalconData-org/stepstone-scraper) — Job listings from 18 European portals
- [Indeed Job Scraper](https://github.com/BlackFalconData-org/indeed-job-scraper) — Indeed job listings with salary data
- [Glassdoor Job Scraper](https://github.com/BlackFalconData-org/glassdoor-job-scraper) — Glassdoor listings with company ratings


## Getting started with Apify

New to Apify? [Create a free account with $5 credit](https://console.apify.com/sign-up?fpr=1h3gvi) — no credit card required.

1. [Sign up free](https://console.apify.com/sign-up?fpr=1h3gvi) — $5 credit included
2. Open the actor and paste your input
3. Click Start — results download as JSON, CSV, or Excel

Need more volume? [See pricing](https://apify.com/pricing?fpr=1h3gvi).

---

---

*Last updated: 2026 04*
