# Jobs schemas

Normalize job postings and labor-market signals from any ATS or careers page.

Each file is a standalone [JSON Schema](https://json-schema.org/) (draft-07) you can pass straight to Tabstack's `extract.json`. Field descriptions include where the value typically appears on the page, which steers extraction.

## Schemas

- `company-hiring-velocity.json` — Company Hiring Velocity
- `executive-leadership-change.json` — Executive Leadership Change
- `job-posting.json` — Job Posting
- `layoff-event.json` — Layoff Event
- `salary-data-point.json` — Salary Data Point

## Use it

```typescript
import Tabstack from '@tabstack/sdk'
import schema from './job-posting.json' with { type: 'json' }

const client = new Tabstack() // reads TABSTACK_API_KEY from the environment

const data = await client.extract.json({
  url: 'https://boards.greenhouse.io/example/jobs/4099999',
  json_schema: schema,
  effort: 'standard',
})

// `page_title` and `favicon` are auto-filled by Tabstack from page metadata.
// Stamp request-side provenance yourself — it is always correct this way:
const record = { ...data, source_url: 'https://boards.greenhouse.io/example/jobs/4099999', extracted_at: new Date().toISOString() }
console.log(record)
```

## Notes

- Effort: use `min` for static pages, `standard` (default) for most, `max` for JS-heavy SPAs.
- Missing fields come back as `null` rather than failing the call, so partial pages still return useful data.
- `page_title` and `favicon` are populated from page metadata automatically when the model leaves them empty.
