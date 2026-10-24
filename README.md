# ganjoor-data

**[▶ Live demo](https://ganjoor.github.io/mini/)** — مین‌گنجور, a minimal reading app built
entirely on this data, running client-side in your browser with no server of its own.

Public, git-tracked export of [Ganjoor](https://ganjoor.net)'s poetry content — poets,
categories, and poems — plus the people, places and date/person/place tags attached to couplets —
allowlisted to contain none of the site's user-account-linked data
(comments, bookmarks, edit history, etc.). Generated from
[GanjoorService](https://github.com/ganjoor/GanjoorService); see that repo if you're looking for
the application this data set is exported from, or want to run your own copy of Ganjoor locally
using this data.

Currently tracks **242 poets** / **138382 poems**, generated 2026-10-24T05:42:55.0701812Z.

## Where do I start?

- **[`manifest.json`](manifest.json)** — the full list of poets (id, nickname, and path), plus the
  schema version and URL templates for every other file kind in this repo. This is the list to
  start from if you're building a client and need "which poets exist and what are their ids".
- **[`people.json`](people.json)**, **[`locations.json`](locations.json)**,
  **[`person-relations.json`](person-relations.json)**, **[`geotags.json`](geotags.json)** — the
  people, places, family relations (attested per book) and tagged couplets; see `API.md`.
- **[`literary-devices.json`](literary-devices.json)**, **[`literary-concepts.json`](literary-concepts.json)**
  and **[`literary-sources.json`](literary-sources.json)** — the literary devices (آرایه‌های ادبی) with
  their slots, the concepts they refer to, and the license of imported annotations; the tags themselves
  are in each poem file and in `index/literary-tags/`.
- **[`API.md`](API.md)** — how to fetch any of this over plain HTTPS (no server needed, via
  jsDelivr), including how to resolve a bare numeric id to its path.

## Not included

No comments, bookmarks, reading history, edit/correction history, or any other data linked to a
user account. See `API.md` for the full list of what each file kind does and doesn't contain.
