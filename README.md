# How to Scrape California State Bar Attorney Directory in Node.js

Extract attorney records from the California State Bar public directory using the [CA State Bar Attorney Scraper](https://apify.com/devanshlive/ca-state-bar-attorney-scraper) actor on Apify -- no browser automation or proxies required.

## What this example does

- Calls the CA State Bar Attorney Scraper actor via the Apify client
- Passes search value (name or bar number) and optional filters
- Waits for the run to complete
- Fetches results from the actor's dataset
- Prints each attorney record to the console

## Prerequisites

- [Node.js](https://nodejs.org/) v18 or higher
- An [Apify account](https://console.apify.com/sign-up) (free tier available)
- An [Apify API token](https://console.apify.com/settings/integrations)

## Installation

```bash
npm install
```

## Environment setup

```bash
cp .env.example .env
```

Open `.env` and replace `your_apify_token_here` with your actual Apify API token.

## Usage

```bash
npm start
```

## Code example

```javascript
import { ApifyClient } from 'apify-client';
import 'dotenv/config';

const client = new ApifyClient({
  token: process.env.APIFY_TOKEN,
});

const input = {
  searchValue: 'Smith',
  maxResults: 10,
  includeDetails: false,
};

const run = await client.actor('devanshlive/ca-state-bar-attorney-scraper').call(input);

console.log('Results from dataset');
console.log(`Check your data here: https://console.apify.com/storage/datasets/${run.defaultDatasetId}`);
const { items } = await client.dataset(run.defaultDatasetId).listItems();
items.forEach((item) => {
  console.dir(item);
});
```

## Example output

See `sample-output.json` for a full example. Each attorney record includes:

| Field | Description |
|-------|-------------|
| `name` | Attorney name as listed on CA Bar (Last, First, Middle) |
| `barNumber` | California bar license number (unique identifier) |
| `city` | City of record (mailing/office address) |
| `status` | License status: Active, Inactive, Suspended, Resigned, Deceased, Disbarred |
| `admittedDate` | Month and year admitted to California bar |
| `detailUrl` | Link to full detail page on CA Bar website |
| `phone` | Phone number (present only if `includeDetails: true`) |
| `email` | Email address (present only if `includeDetails: true`) |
| `address` | Full office/mailing address (present only if `includeDetails: true`) |
| `practiceAreas` | Array of practice areas (present only if `includeDetails: true`) |

## Use cases

- **Legal-tech data enrichment:** Bulk-verify attorney credentials and enrich client lists
- **Law firm lead generation:** Find attorneys by practice area, city, or bar number
- **Compliance verification:** Check attorney status for due diligence and risk assessment
- **Journalist research:** Background checks and disciplinary history cross-reference
- **Competitive intelligence:** Track competitor attorneys and practice area presence

## Try the actor on Apify

[Open the CA State Bar Attorney Scraper on Apify](https://apify.com/devanshlive/ca-state-bar-attorney-scraper)

## Related resources

- [California State Bar Attorney Search](https://apps.calbar.ca.gov/attorney/LicenseeSearch/QuickSearch)
- [Apify Client for JavaScript](https://docs.apify.com/api/client/js/)

## License

MIT
