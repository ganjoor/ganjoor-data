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
- `GET poets/{poetSlug}/{catPath}/{poemSlug}.json` — a poem: metre, rhyme, sections, verses, and `Tags` (see below) when it has any

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

## People, places and tagged couplets

Couplets are tagged with places, dates and people (the people may be historical or legendary
figures, not only poets). Only approved tags are exported, with no information about who suggested
or approved them. A couplet is addressed by its poem id and `CoupletIndex` (the same value as in
the poem file's `Verses`, counted from 0). A tag whose `CoupletIndex` is -1 applies to the whole poem.

- `GET locations.json` — places: id, name, latitude, longitude
- `GET people.json` — people: names, aliases, description, gender, life dates, birth/death place ids
- `GET person-relations.json` — kinship (Parent, Sibling, Spouse, Ancestor). For Parent/Ancestor,
  `Person1Id` is the parent/ancestor. `Evidence` lists the attesting couplets; `MasterCatId` is
  the category id of the *book* in which a couplet attests the relation — the same two people can
  be related differently in different books (e.g. a different father in another recension), so a
  family tree should be built per book from the evidence, not from the bare relation list.
- `GET person-affiliations.json` — non-family ties (minister, patron, panegyrized, ...), same
  evidence shape
- `GET geotags.json` — every place/date tag in one file, for maps and timelines
- `GET index/person-tags/{bucket}.json` — where a person is tagged: person id X is in shard
  `{X / 500}` (integer division) and maps to the list of poem id / couplet index
  pairs; `PersonMention` is `Participant` (part of what the couplet tells) or `Allusion` (a passing
  reference)
- the per-poem file also carries the same tags in its `Tags` list, with `LocationId` / `PersonId`
  pointing into the files above

Many tags and relations are created with AI assistance and reviewed by a human moderator;
`MachineGenerated` marks tags and records that were created by the AI.

## Literary devices (آرایه‌های ادبی)

Poems are annotated with literary devices such as تشبیه, استعاره or جناس. Only approved devices and
tags are exported, with no information about who suggested or approved them.

- `GET literary-devices.json` — the devices: id, key, name, category, definition, example and `Slots`.
  A tag is made of one value per slot (e.g. a تشبیه has «مشبه» and «مشبه‌به»); a slot's `Kind` says what
  its value holds: `Span` (words of the poem), `SpanConcept` (words and the concept they stand for),
  `Concept` or `Text`. `Required` / `AllowMultiple` tell whether the slot must be filled / can repeat.
- `GET literary-concepts.json` — the concepts slot values refer to (e.g. «شمع», «زلف»): id, name,
  aliases (separated by `|`) and an optional `ParentId`
- `GET literary-sources.json` — origin and license of imported annotations. Tags with
  `Origin` = `AcademicCorpus` name their entry in `SourceKey`; redistributing those tags means
  keeping that attribution and license
- `GET index/literary-tags/{deviceId}.json` — every tag of one device across all poems, each with its `PoemId`
- the per-poem file carries its own tags in `LiteraryTags`

In a tag, `CoupletIndex` is the index of the main couplet, counted from 0 exactly like `CoupletIndex`
in the poem file's `Verses` (`-1` means the whole poem; files of schema version 3 and older counted
couplets from 1 and used 0 for the whole poem). A part located in the text has its own `CoupletIndex`
(0-based, `-1` if not located), `Misra` (1 or 2) and
`WordStart` / `WordEnd` (1-based, inclusive; the words of a hemistich split at whitespace).
`Uncertain` marks tags the annotator was not sure about. `Origin` is `Editor`, `AcademicCorpus`,
`Algorithm` or `AI`; `MachineGenerated` is true for the last two.

## Not included

No comments, bookmarks, reading history, edit/correction history, or any other user-account-linked
data — see the repo this data set is exported from for details. Only `Published` poets/categories/
poems are included. Tags and relations carry no information about who suggested or approved them.

## Search

Not available as a static endpoint in this data set yet.
