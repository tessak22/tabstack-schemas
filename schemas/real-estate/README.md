# Real Estate schemas

Turn any property detail page into structured listing data.

Each file is a standalone [JSON Schema](https://json-schema.org/) (draft-07) you can pass straight to Tabstack's `extract.json`. Field descriptions include where the value typically appears on the page, which steers extraction.

## Schemas

- `commercial-property.json` — Commercial Property
- `foreclosure-auction-listing.json` — Foreclosure & Auction Listing
- `property-sold-history.json` — Property Sold History
- `rental-listing.json` — Rental Listing
- `residential-listing.json` — Residential Listing
- `short-term-rental.json` — Short-Term Rental

## Use it

```typescript
import Tabstack from '@tabstack/sdk'
import schema from './residential-listing.json' with { type: 'json' }

const client = new Tabstack() // reads TABSTACK_API_KEY from the environment

const data = await client.extract.json({
  url: 'https://www.zillow.com/homedetails/123-Main-St-Austin-TX-78701',
  json_schema: schema,
  effort: 'standard',
})

// `page_title` and `favicon` are auto-filled by Tabstack from page metadata.
// Stamp request-side provenance yourself — it is always correct this way:
const record = { ...data, source_url: 'https://www.zillow.com/homedetails/123-Main-St-Austin-TX-78701', extracted_at: new Date().toISOString() }
console.log(record)
```

## Notes

- Effort: use `min` for static pages, `standard` (default) for most, `max` for JS-heavy SPAs.
- Missing fields come back as `null` rather than failing the call, so partial pages still return useful data.
- `page_title` and `favicon` are populated from page metadata automatically when the model leaves them empty.
