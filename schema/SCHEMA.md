# Dataset schema v1

`events.json` is an object with:

- `schema_version` — currently `1`.
- `dataset_version` — release string in `YYYY.MM.DD.REVISION` form.
- `generated_at` — ISO-8601 timestamp.
- `events` — array of event objects.

Each event contains:

| Field | Type | Meaning |
|---|---|---|
| `event_id` | string | Permanent `CIOTD-######` identifier |
| `date` | string | Exact `YYYY-MM-DD` historical date |
| `kind` | string | Editorial event category |
| `title` | string | Display title for uncovered events; linked events may use article title |
| `note` | string | Concise event description |
| `status` | string | `covered` or `uncovered` |
| `post_id` | integer | WordPress post ID for covered events, otherwise `0` |
| `source` | string | Internal provenance label |
| `source_name` | string | Human-readable research source |
| `source_url` | string | Supporting source URL |
| `confidence` | string | Editorial confidence label |
| `research_batch` | string | Provenance batch identifier |
| `image_id` | integer | WordPress Media Library attachment ID, otherwise `0` |
| `image_source` | string | Image provenance label |
| `image_source_url` | string | Original image/source page URL |
| `image_credit` | string | Credit/license text shown where required |

`manifest.json` contains the currently published dataset version, event/date counts, minimum plugin version and SHA-256 checksum of the exact `events.json` response body.
