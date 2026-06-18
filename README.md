# Tabstack Schemas

Ready-to-use [JSON Schema](https://json-schema.org/) definitions for [Tabstack](https://tabstack.ai)'s `extract.json`. Point one at any URL and get structured, typed data back. 46 schemas across 10 categories.

Grab a file straight from this repo (or the docs site) and pass it to the API.

## Use it

```typescript
import Tabstack from '@tabstack/sdk'
import schema from './real-estate/residential-listing.json' with { type: 'json' }

const client = new Tabstack() // reads TABSTACK_API_KEY from the environment

const url = 'https://www.example.com/listing/123'
const data = await client.extract.json({
  url,
  json_schema: schema,
  effort: 'standard', // 'min' | 'standard' | 'max'
})

// page_title and favicon are auto-filled by Tabstack from page metadata.
// Stamp request-side provenance yourself so it is always correct:
const record = { ...data, source_url: url, extracted_at: new Date().toISOString() }
```

Python and the other SDKs take the same `json_schema` object.

## Categories

| Category | Schemas | Includes |
|---|---|---|
| [`b2b-intel`](b2b-intel/) | 7 | API Documentation Endpoint, Company Profile, Funding Round, Press Release & News Mention, Product Changelog Entry, SaaS Pricing Page, Tech Stack |
| [`developer-ecosystem`](developer-ecosystem/) | 2 | GitHub Repository, Package Registry Entry |
| [`e-commerce`](e-commerce/) | 5 | Category Search Results, Grocery & CPG Product, Marketplace Seller Profile, Product Listing, Product Review |
| [`finance`](finance/) | 5 | Alternative Asset Listing, Angel & Seed Deal, Crypto Asset, Public Company Filing, VC Portfolio Page |
| [`gov-public-records`](gov-public-records/) | 5 | Building Permit, Business Registration, Court Filing / Case, Government Contract Award, Property Tax Record |
| [`healthcare`](healthcare/) | 4 | Clinical Trial, Drug Formulary Entry, Medical Device Recall, Provider Directory Entry |
| [`jobs`](jobs/) | 5 | Company Hiring Velocity, Executive Leadership Change, Job Posting, Layoff Event, Salary Data Point |
| [`real-estate`](real-estate/) | 6 | Commercial Property, Foreclosure & Auction Listing, Property Sold History, Rental Listing, Residential Listing, Short-Term Rental |
| [`social-content`](social-content/) | 3 | News Article, Social Profile, Software Review |
| [`travel`](travel/) | 4 | Attraction & Experience, Flight Price Snapshot, Hotel Listing, Restaurant Listing |

Each category folder has its own README with a runnable example. [`index.json`](index.json) is a machine-readable manifest of every schema.

## How these schemas are built

- Standard JSON Schema (draft-07). Every field has a `description` that also notes where the value usually appears on the page, which steers extraction.
- Closed enums include an `"other"` value so off-list page wording is captured instead of dropped.
- Missing fields come back as `null` rather than failing the call, so partial pages still return useful data.
- Provenance (`source_url`, `extracted_at`) is left to the caller, which keeps it accurate. `page_title` and `favicon` are auto-filled by Tabstack when present.

## Structure

```
<category>/
  README.md
  <schema-name>.json
index.json
```
