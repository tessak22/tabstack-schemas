# Finance schemas

Snapshot assets, deals, and filings into structured financial records.

Each file is a standalone [JSON Schema](https://json-schema.org/) (draft-07) you can pass straight to Tabstack's `extract.json`. Field descriptions include where the value typically appears on the page, which steers extraction.

## Schemas

- `alternative-asset-listing.json` — Alternative Asset Listing
- `angel-seed-deal.json` — Angel & Seed Deal
- `crypto-asset.json` — Crypto Asset
- `public-company-filing.json` — Public Company Filing
- `vc-portfolio-page.json` — VC Portfolio Page

## Use it

```typescript
import Tabstack from '@tabstack/sdk'
import schema from './crypto-asset.json' with { type: 'json' }

const client = new Tabstack() // reads TABSTACK_API_KEY from the environment

const data = await client.extract.json({
  url: 'https://www.coingecko.com/en/coins/example-token',
  json_schema: schema,
  effort: 'standard',
})

// `page_title` and `favicon` are auto-filled by Tabstack from page metadata.
// Stamp request-side provenance yourself — it is always correct this way:
const record = { ...data, source_url: 'https://www.coingecko.com/en/coins/example-token', extracted_at: new Date().toISOString() }
console.log(record)
```

## Notes

- Effort: use `min` for static pages, `standard` (default) for most, `max` for JS-heavy SPAs.
- Missing fields come back as `null` rather than failing the call, so partial pages still return useful data.
- `page_title` and `favicon` are populated from page metadata automatically when the model leaves them empty.
