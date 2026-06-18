# Gov Public Records schemas

Parse permits, filings, registrations, and awards from government portals.

Each file is a standalone [JSON Schema](https://json-schema.org/) (draft-07) you can pass straight to Tabstack's `extract.json`. Field descriptions include where the value typically appears on the page, which steers extraction.

## Schemas

- `building-permit.json` — Building Permit
- `business-registration.json` — Business Registration
- `court-filing-case.json` — Court Filing / Case
- `government-contract-award.json` — Government Contract Award
- `property-tax-record.json` — Property Tax Record

## Use it

```typescript
import Tabstack from '@tabstack/sdk'
import schema from './building-permit.json' with { type: 'json' }

const client = new Tabstack() // reads TABSTACK_API_KEY from the environment

const data = await client.extract.json({
  url: 'https://permits.example.gov/record/BLD-2026-00123',
  json_schema: schema,
  effort: 'standard',
})

// `page_title` and `favicon` are auto-filled by Tabstack from page metadata.
// Stamp request-side provenance yourself — it is always correct this way:
const record = { ...data, source_url: 'https://permits.example.gov/record/BLD-2026-00123', extracted_at: new Date().toISOString() }
console.log(record)
```

## Notes

- Effort: use `min` for static pages, `standard` (default) for most, `max` for JS-heavy SPAs.
- Missing fields come back as `null` rather than failing the call, so partial pages still return useful data.
- `page_title` and `favicon` are populated from page metadata automatically when the model leaves them empty.
