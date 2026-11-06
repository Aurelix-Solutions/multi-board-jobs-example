**Multi Job Board Scraper - LinkedIn + Career Sites.** LinkedIn, about 50 company career sites, Remote OK and We Work Remotely jobs in one deduplicated list. Price, example input and output, and FAQ: [Multi Job Board Scraper on the Aurelix tools site](https://aurelix-solutions.github.io/tools/multi-job-board-scraper/). Run it on the Apify Store: [apify.com/aurelix/multi-job-board-scraper](https://apify.com/aurelix/multi-job-board-scraper).

<!-- PUBLISH AFTER: multi-board-jobs is public on the Apify Store -->
# Multi Job Board Scraper: Python example

> Multi Job Board Scraper is being prepared for the Apify Store. The link below goes live when it is published.

A small, runnable example for [Multi Job Board Scraper](https://apify.com/aurelix/multi-job-board-scraper), an Apify Actor by Aurelix.

[![Multi Job Board Scraper: 75-second demo video](https://api.apify.com/v2/key-value-stores/twkCCtTWzsi8fVbx5/records/multi-job-board-scraper-demo-thumb-v1.png)](https://api.apify.com/v2/key-value-stores/twkCCtTWzsi8fVbx5/records/multi-job-board-scraper-demo-v1.mp4)

**[▶ Watch the 75-second demo](https://api.apify.com/v2/key-value-stores/twkCCtTWzsi8fVbx5/records/multi-job-board-scraper-demo-v1.mp4)**

## What it does

Multi Job Board Scraper runs one job search across LinkedIn's public job search, a bundled list of about 50 company career sites (25 searched per run by default) on Greenhouse, Lever, Ashby and SmartRecruiters (plus any boards you add), and the Remote OK and We Work Remotely feeds, and returns one deduplicated list. Every row has the same fields: title, company, location, ISO country, workplace type, job types, pay where published, posted date, job and apply links and description, plus the sources it was found on. Give keywords and a country once; the same job on LinkedIn and the employer's own site becomes one row, charged once.

## Why this over alternatives

Compared with the leading alternatives on the Apify Store (full table on the Store page):

- LinkedIn and ~50 company career sites (Greenhouse, Lever, Ashby, SmartRecruiters) in one search, which neither the most-used LinkedIn scraper nor the remote-board aggregator does.
- $1.50 per 1,000 jobs, 25% under both ($0.90 vs $1.00 and $2.00 on paid plans), with the same job across sources merged and charged once.

## When to use it

- Job seekers and career coaches running one daily search instead of three
- Job boards and newsletters backfilling deduplicated postings with the new-jobs-only mode
- Recruiters comparing a role's pay and posting date across LinkedIn and the employer's own site
- Market researchers counting hiring demand without double-counting cross-posted jobs

## Input example

This is the `data-engineer-jobs-toronto` input used in `main.py`:

```json
{
  "datePosted": "pastWeek",
  "maxResultsPerSource": 50,
  "keywords": [
    "data engineer"
  ],
  "country": "CA",
  "location": "Toronto, ON"
}
```

Every input field is documented on the [Store page](https://apify.com/aurelix/multi-job-board-scraper).

## How to run

1. Get an Apify API token from Apify Console (Settings, API & Integrations).
2. Install and run:

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
export APIFY_TOKEN=your_token_here
python main.py
```

The script starts a run of `aurelix/multi-job-board-scraper`, prints a few fields from the first results and writes every result to `results.csv`. Lists and nested objects are stored in the CSV as JSON text. Each run is billed to your Apify account (see Pricing below).

## Sample output

One record as returned by a real run (long text is shortened):

```json
{
  "job_id": "careersite:lever:pointclickcare:6ec25484-b204-48a6-ab91-7b0b1625aa02",
  "source": "careersite",
  "source_job_id": "lever:pointclickcare:6ec25484-b204-48a6-ab91-7b0b1625aa02",
  "title": "Intermediate Software Engineer",
  "company": "PointClickCare",
  "company_url": "https://www.linkedin.com/company/pointclickcare",
  "location": "Mississauga/Toronto/Kitchener/Waterloo/Cambridge",
  "city": "Mississauga",
  "region": "ON",
  "country": "CA",
  "workplace_type": "hybrid",
  "job_types": [
    "fullTime"
  ],
  "seniority": null,
  "salary_min": 102600,
  "salary_max": 128300,
  "salary_currency": "CAD",
  "salary_interval": "year",
  "salary_text": "At PointClickCare, base salary is one of the many components that make up our total rewards package. The CAD base salary range for this position is $102,600 $128,300 (not overtime eligible) + bonus + benefits. Compensation is assessed individually and aligned to experience, skills, and market contex... [67 more characters]",
  "posted_at": "2026-10-21T20:57:35Z",
  "posted_date": "2026-10-21",
  "job_url": "https://jobs.lever.co/pointclickcare/6ec25484-b204-48a6-ab91-7b0b1625aa02",
  "apply_url": "https://jobs.lever.co/pointclickcare/6ec25484-b204-48a6-ab91-7b0b1625aa02/apply",
  "description_text": null,
  "description_html": null,
  "search_keyword": "intermediate software engineer",
  "sources": [
    "careersite",
    "linkedin"
  ],
  "also_listed_on": [
    {
      "source": "linkedin",
      "job_id": "linkedin:4481154739",
      "job_url": "https://www.linkedin.com/jobs/view/4481154739/"
    }
  ],
  "dedupe_key": "pointclickcare|intermediate software engineer|mississauga toronto kitchener waterloo cambridge",
  "source_url": "https://jobs.lever.co/pointclickcare/6ec25484-b204-48a6-ab91-7b0b1625aa02",
  "scraped_at": "2026-11-04T06:18:09Z"
}
```

## Export to CSV

`main.py` writes `results.csv` with the `csv` module. You can also download any run's results directly:

```
https://api.apify.com/v2/datasets/<datasetId>/items?format=csv&clean=true
```

Send your token in an `Authorization: Bearer <token>` header (or `?token=`) if the dataset is not readable without one. Other formats: `format=json`, `format=xlsx`.

## Google Sheets

- Simplest: use Apify's Google Sheets integration on the Actor or a saved task, so each run's results land in a sheet with no token in the sheet itself.
- Quick pull of one run: `=IMPORTDATA("https://api.apify.com/v2/datasets/<datasetId>/items?format=csv&clean=true")`. This only works without a token if your account lets anyone with the dataset ID read it. Do not paste an API token into a sheet formula: anyone who can view or copy the sheet can read it and use your account.

## n8n

- Apify node: choose "Run an Actor", enter `aurelix/multi-job-board-scraper`, paste the input above, then read the run's dataset items.
- HTTP Request node: `POST https://api.apify.com/v2/acts/aurelix~multi-job-board-scraper/run-sync-get-dataset-items` with header `Authorization: Bearer <token>` (stored as an n8n credential) and the input as the JSON body. The response is the list of results. This endpoint waits for the run to finish, so for large runs start the run asynchronously and fetch the dataset when it ends.

## Limits

- LinkedIn caps results per search at about 1,000; split broad searches.
- Career sites cover only the bundled employers plus the boards you add, not every company in the country.
- Career-site and remote-board jobs are matched by keyword in the title (and company, for remote boards), not by full-text relevance.
- Dedupe is by company, title and overlapping location: two genuinely different openings with the same title in a shared city are merged. Rows are saved in batches during long runs (after the first minute); a duplicate found after its first copy was saved is dropped, but is not added to that row's 'also_listed_on'.
- LinkedIn shows a posted date without a time; posted_at is null for LinkedIn rows.
- Pay and remote flags are only as complete as each source publishes them.
- Sources can change their pages; the daily health run catches breakage within 24 hours.

## Pricing

At the time of writing:

Pay per event: you pay only for what the run returns, plus a small start fee. Bronze, Silver, Gold and higher Apify plans pay less per result.

| Event | Free plan | Paid plans (Bronze, Silver, Gold) |
|---|---|---|
| `job-result` | $1.50 per 1,000 jobs | $0.90 per 1,000 jobs |
| `apify-actor-start` | $0.001 per run | $0.001 per run |

Example: 1,000 jobs cost $1.50 on the free plan and $0.90 on a paid plan, plus the start fee. Apify's free plan includes $5 of credit a month, enough for about 3,300 jobs.

Proxy included. A job found on several sources is one row and is charged once. Each run's start fee covers searching every source even when nothing matches.

Check the [Store page](https://apify.com/aurelix/multi-job-board-scraper) for current prices.

## Links

- Actor: https://apify.com/aurelix/multi-job-board-scraper
- Questions, bugs and feature requests: the Issues tab on the Store page.

## Licence

The example code in this repository is released under the MIT licence (see `LICENSE`). The Actor itself runs on Apify and is billed through your Apify account.
