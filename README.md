# Curious Indian — On This Day Data

Canonical editorial dataset for the **On This Day in India** feature on [Curious Indian](https://curiousindian.in/).

This repository contains **data only**. The WordPress plugin downloads and validates `manifest.json` and `events.json`, then stores a last-known-good local copy in WordPress. The plugin never executes code from this repository.

## Production files

- `manifest.json` — lightweight version/checksum file checked by WordPress.
- `events.json` — canonical machine-readable dataset.
- `events.csv` — editorial spreadsheet export of the same records.
- `image-sources.csv` — image/provenance ledger.
- `research/` — month-by-month research batches that expanded calendar coverage.

## Update workflow

1. Edit the editorial dataset.
2. Preserve existing `event_id` values. New events receive the next unused `CIOTD-######` ID.
3. Increase `dataset_version` using `YYYY.MM.DD.REVISION` format.
4. Regenerate `events.csv` from the JSON dataset.
5. Recalculate the SHA-256 checksum of the exact `events.json` bytes.
6. Update `manifest.json` with the new version, counts, timestamp and checksum.
7. Commit the changes to `main`.
8. Curious Indian WordPress will detect the newer version during its twice-daily background check. A manual **Sync now** button is also available in WordPress under Tools → On This Day.

## Safety invariants

- Every event has a permanent unique `event_id`.
- `date` is an exact Gregorian `YYYY-MM-DD` date.
- `status` is `covered` or `uncovered`.
- Covered events require a valid Curious Indian `post_id`.
- Uncovered events require a human-readable `title`.
- Images live in the Curious Indian WordPress Media Library. The dataset stores the WordPress attachment ID and source/credit metadata.
- Remote data changes never directly execute PHP or JavaScript.
- A failed checksum/schema/import leaves the site's current local dataset unchanged.

## Current baseline

The first cloud-ready baseline contains 740 curated moments covering all 366 calendar dates. 128 moments are linked to Curious Indian stories. Image enrichment is ongoing.
