# ganjoor-data

**[▶ Live demo](https://ganjoor.github.io/mini/)** — مین‌گنجور, a minimal reading app built
entirely on this data, running client-side in your browser with no server of its own.

Public, git-tracked export of [Ganjoor](https://ganjoor.net)'s poetry content — poets,
categories, and poems, allowlisted to contain none of the site's user-account-linked data
(comments, bookmarks, edit history, etc.). Generated from
[GanjoorService](https://github.com/ganjoor/GanjoorService); see that repo if you're looking for
the application this data set is exported from, or want to run your own copy of Ganjoor locally
using this data.

Currently tracks **236 poets** / **132843 poems**, generated 2026-08-23T18:36:10.7034138Z.

## Where do I start?

- **[`manifest.json`](manifest.json)** — the full list of poets (id, nickname, and path), plus the
  schema version and URL templates for every other file kind in this repo. This is the list to
  start from if you're building a client and need "which poets exist and what are their ids".
- **[`API.md`](API.md)** — how to fetch any of this over plain HTTPS (no server needed, via
  jsDelivr), including how to resolve a bare numeric id to its path.

## Not included

No comments, bookmarks, reading history, edit/correction history, or any other data linked to a
user account. See `API.md` for the full list of what each file kind does and doesn't contain.
