# B2B Intel schemas

Build structured company intelligence from public profile, funding, and pricing pages.

Each file is a standalone [JSON Schema](https://json-schema.org/) (draft-07) you can pass straight to Tabstack's `extract.json`. Field descriptions include where the value typically appears on the page, which steers extraction.

## Schemas

- `api-documentation-endpoint.json` — API Documentation Endpoint
- `company-profile.json` — Company Profile
- `funding-round.json` — Funding Round
- `press-release-news-mention.json` — Press Release & News Mention
- `product-changelog-entry.json` — Product Changelog Entry
- `saas-pricing-page.json` — SaaS Pricing Page
- `tech-stack.json` — Tech Stack

## Use it

```typescript
import Tabstack from '@tabstack/sdk'
import schema from './company-profile.json' with { type: 'json' }

const client = new Tabstack() // reads TABSTACK_API_KEY from the environment

const data = await client.extract.json({
  url: 'https://www.crunchbase.com/organization/example-co',
  json_schema: schema,
  effort: 'standard',
})

// `page_title` and `favicon` are auto-filled by Tabstack from page metadata.
// Stamp request-side provenance yourself — it is always correct this way:
const record = { ...data, source_url: 'https://www.crunchbase.com/organization/example-co', extracted_at: new Date().toISOString() }
console.log(record)
```

## Notes

- Effort: use `min` for static pages, `standard` (default) for most, `max` for JS-heavy SPAs.
- Missing fields come back as `null` rather than failing the call, so partial pages still return useful data.
- `page_title` and `favicon` are populated from page metadata automatically when the model leaves them empty.
