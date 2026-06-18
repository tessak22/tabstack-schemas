# Developer Ecosystem schemas

Pull repository and package-registry metadata for developer tooling.

Each file is a standalone [JSON Schema](https://json-schema.org/) (draft-07) you can pass straight to Tabstack's `extract.json`. Field descriptions include where the value typically appears on the page, which steers extraction.

## Schemas

- `github-repository.json` — GitHub Repository
- `package-registry-entry.json` — Package Registry Entry

## Use it

```typescript
import Tabstack from '@tabstack/sdk'
import schema from './github-repository.json' with { type: 'json' }

const client = new Tabstack() // reads TABSTACK_API_KEY from the environment

const data = await client.extract.json({
  url: 'https://github.com/Mozilla-Ocho/tabstack-cli',
  json_schema: schema,
  effort: 'standard',
})

// `page_title` and `favicon` are auto-filled by Tabstack from page metadata.
// Stamp request-side provenance yourself — it is always correct this way:
const record = { ...data, source_url: 'https://github.com/Mozilla-Ocho/tabstack-cli', extracted_at: new Date().toISOString() }
console.log(record)
```

## Notes

- Effort: use `min` for static pages, `standard` (default) for most, `max` for JS-heavy SPAs.
- Missing fields come back as `null` rather than failing the call, so partial pages still return useful data.
- `page_title` and `favicon` are populated from page metadata automatically when the model leaves them empty.
