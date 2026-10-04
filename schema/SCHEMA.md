# Dataset schema v2

The production sync format is sharded by month. `manifest.json` is the entry point.

## Manifest

Required top-level fields:

- `schema_version` — currently `2`.
- `dataset_version` — release string in `YYYY.MM.DD.REVISION` form.
- `generated_at` — ISO-8601 timestamp.
- `minimum_plugin_version` — oldest plugin version allowed to consume the release.
- `event_count` — total events across all shards.
- `date_count` — unique month/day keys represented.
- `linked_event_count` — covered events linked to Curious Indian.
- `image_ready_count` — events with a WordPress attachment ID.
- `canonical_events_path` and `canonical_events_sha256` — recovery/full-export artifact.
- `sync_mode` — `monthly_shards`.
- `shards` — one descriptor for each calendar month.

Each shard descriptor contains `month`, `path`, `event_count`, and the SHA-256 of the exact JSON response body.

## Monthly shard

Each `events/MM.json` file contains:

- `schema_version` — `2`.
- `dataset_version` — must exactly match the manifest.
- `month` — integer 1–12.
- `events` — event records for that month only.

## Event record

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

The plugin validates all shards and the combined dataset before replacing the local WordPress copy.
