# Curious Indian — On This Day Data

Canonical editorial dataset for the **On This Day in India** feature on [Curious Indian](https://curiousindian.in/).

This repository contains **data only**. The WordPress plugin checks `manifest.json` in the background, validates the remote dataset, and stores a last-known-good local copy in WordPress. Frontend page loads never depend on GitHub, and the plugin never executes code from this repository.

## Production files

- `manifest.json` — lightweight version, counts, and SHA-256 integrity metadata.
- `events.json` — canonical full machine-readable dataset.
- `events.csv` — editorial spreadsheet export of the same records.
- `events/01.json` … `events/12.json` — month shards used by WordPress sync.
- `image-sources.csv` — image/provenance ledger.
- `schema/SCHEMA.md` — current data contract.

## Sync architecture

1. WordPress checks only `manifest.json` on its normal twice-daily background schedule.
2. If `dataset_version` is unchanged, nothing else is downloaded.
3. If a newer version exists, the plugin downloads the 12 monthly JSON shards.
4. Every shard is checked against its SHA-256 hash, month number, dataset version, and expected event count.
5. All shards are combined and the complete dataset is schema-validated.
6. Only after every check passes does WordPress replace the local event table in a database transaction.
7. Any HTTP, checksum, schema, count, or import failure leaves the current local dataset untouched.

The full `events.json` remains the canonical export and recovery artifact. Monthly shards keep production synchronization smaller and make later data maintenance easier.

## Editorial update workflow

1. Edit the canonical dataset while preserving existing `event_id` values.
2. New events receive the next unused `CIOTD-######` ID.
3. Increase `dataset_version` using `YYYY.MM.DD.REVISION`.
4. Regenerate `events.json`, `events.csv`, the monthly shards, and `image-sources.csv`.
5. Recalculate SHA-256 hashes and update `manifest.json`.
6. Commit to `main`.
7. Curious Indian detects the new manifest automatically. A manual **Sync now** control is also available in WordPress.

## Safety invariants

- Every event has a permanent unique `event_id`.
- `date` is an exact Gregorian `YYYY-MM-DD` date.
- `status` is `covered` or `uncovered`.
- Covered events require a valid Curious Indian `post_id`.
- Uncovered events require a human-readable `title`.
- Images live in the Curious Indian WordPress Media Library; the dataset stores attachment IDs plus source/credit metadata.
- Remote data is JSON only. No remote PHP or JavaScript is downloaded or executed.
- A failed remote update can never erase the last-known-good local dataset.

## Current baseline

Dataset **2026.10.04.2** contains 740 curated moments covering all 366 calendar dates. 128 moments are linked to Curious Indian stories, and 129 moments currently have image coverage. Image enrichment is ongoing.
