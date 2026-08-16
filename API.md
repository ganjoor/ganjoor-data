# Ganjoor public data — static API

This repository is served as a static API through jsDelivr's GitHub CDN. There is no server —
every "endpoint" below is a file in this repo, fetched over plain HTTPS with CORS enabled.

Base URL (tracks the latest commit on `main`):

    https://cdn.jsdelivr.net/gh/ganjoor/ganjoor-data@main/

Note: since this currently tracks `@main` rather than tagged releases, jsDelivr's edge cache
(up to ~7 days) means a fetch can lag behind the newest commit. If you need a frozen snapshot,
pin to an exact commit instead of `@main`:

    https://cdn.jsdelivr.net/gh/ganjoor/ganjoor-data@<commit-sha>/

## Discovery

`GET manifest.json` — schema version, generation timestamp, poet/poem counts, the list of poets
with their paths, and the URL templates below (`manifest.json` is the source of truth for this
file; if they ever disagree, trust `manifest.json`).

## Content

- `GET poets/{poetSlug}/poet.json` — poet biography
- `GET poets/{poetSlug}/{catPath}/_cat.json` — a category/collection: title, description, ordered child categories and poems
- `GET poets/{poetSlug}/{catPath}/{poemSlug}.json` — a poem: metre, rhyme, sections, verses

`{poetSlug}`/`{catPath}`/`{poemSlug}` are exactly the path segments of the poem's Ganjoor
URL, e.g. the poem at ganjoor.net/hafez/ghazal/sh1 is at `poets/hafez/ghazal/sh1.json`.

## Resolving a numeric id

If you have a poet/category/poem id (not a slug), resolve it via the id index instead of
guessing a path:

- `GET index/poets-by-id.json` — small enough to fetch whole: `{ "1": "/hafez", ... }`
- `GET index/cats-by-id/{bucket}.json` / `GET index/poems-by-id/{bucket}.json` — bucketed. The shard file for id `X` is
  `bucket = X / 2000` (integer division), so e.g. poem id 4321 with the
  current shard size of 2000 lives in shard `2`,
  i.e. `index/poems-by-id/2.json`.
  Each shard maps ids in its range to a `FullUrl` you then fetch with the `poets/{poetSlug}/{catPath}/{poemSlug}.json` pattern above.

## Not included

No comments, bookmarks, reading history, edit/correction history, or any other user-account-linked
data — see the repo this data set is exported from for details. Only `Published` poets/categories/
poems are included.

## Search

Not available as a static endpoint in this data set yet.
