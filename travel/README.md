# Travel schemas

Capture hotel, flight, restaurant, and attraction data for travel agents.

Each file is a standalone [JSON Schema](https://json-schema.org/) (draft-07) you can pass straight to Tabstack's `extract.json`. Field descriptions include where the value typically appears on the page, which steers extraction.

## Schemas

- `attraction-experience.json`: Attraction & Experience
- `flight-price-snapshot.json`: Flight Price Snapshot
- `hotel-listing.json`: Hotel Listing
- `restaurant-listing.json`: Restaurant Listing

## Use it

```typescript
import Tabstack from '@tabstack/sdk'
import schema from './hotel-listing.json' with { type: 'json' }

const client = new Tabstack() // reads TABSTACK_API_KEY from the environment

const data = await client.extract.json({
  url: 'https://www.example-travel.com/hotels/grand-plaza',
  json_schema: schema,
  effort: 'standard',
})

// `page_title` and `favicon` are auto-filled by Tabstack from page metadata.
// Stamp request-side provenance yourself so it stays accurate:
const record = { ...data, source_url: 'https://www.example-travel.com/hotels/grand-plaza', extracted_at: new Date().toISOString() }
console.log(record)
```

## Notes

- Effort: use `min` for static pages, `standard` (default) for most, `max` for JS-heavy SPAs.
- Missing fields come back as `null` rather than failing the call, so partial pages still return useful data.
- `page_title` and `favicon` are populated from page metadata automatically when the model leaves them empty.
