# Artist Alley dataset: site_a (metadata repository)

**Status: LOCAL STAGING ONLY. No commit, no remote, nothing published.**

This repository holds the metadata for the public `site_a` demo seed dataset
for issue #1514 (v0.11.1). It is kept separate from the artist-alley
application repository: dataset bytes do not live in the application
repository. The planned home is the Artist-Alley-Org GitHub organization.

## What Git holds, and where the bytes go

- **Git holds metadata only:** the seven top-level files listed below, this
  README, `SHA256SUMS` and `.gitignore`. No binary payload and no Git LFS
  pointers are tracked, so a clone contains no dataset bytes.
- **The binary snapshot is local only.** The six data directories (`3d/`,
  `audio/`, `documents/`, `fonts/`, `images/`, `videos/`) are present in this
  local working copy for building release archives, but are not tracked.
  `.gitignore` ignores them so they cannot be staged by accident.
- **Consumers get the bytes from versioned GitHub Release archives.** Before
  extracting and seeding, a consumer verifies the archive's checksum, then
  `SHA256SUMS` and `MANIFEST.json`.
- **A clone alone cannot run `sha256sum -c SHA256SUMS`:** the data files it
  lists are only present after the matching release archive has been
  downloaded and extracted.
- **Not built yet:** no release archive has been built or published.

## What `site_a` is

ADR 0058 (`docs/adr/0058-demo-seed-dataset.md` in the application repository)
makes `site_a` (Layer A) the public-safe tier of the demo seed. Its 2026-10-05
amendment names the per-asset `license` and `attribution` in `MANIFEST.json`,
together with `ATTRIBUTIONS.md`, as the authoritative licensing and attribution
record. Public redistribution must honour the terms of every included source.
The application repository's `RELEASING.md` documents earlier publication of
the seed dataset on Kaggle.

## Contents

The snapshot is an exact relative-path copy of the local `site_a` source:
2,258 files, 2,767,352,489 bytes.

- 7 top-level metadata and attribution files, tracked in Git:
  `ATTRIBUTIONS.md`, `MANIFEST.json`, `dataset-metadata.json`, `groups.csv`,
  `kenney-hq-replacements.json`, `metadata.csv`, `posts.json`.
- 2,251 data files under `3d/` (835), `audio/` (200), `documents/` (18),
  `fonts/` (33), `images/` (1,073) and `videos/` (92). These are local only
  and go into the release archives.
- `MANIFEST.json` has 2,006 asset rows; every `file_path` is present.
  The other 245 data files are non-manifest files under `3d/`, including model companions.
- `posts.json` has 863 posts.

Excluded from the snapshot: 13 `*.bak` backup files and one `Thumbs.db`
thumbnail cache. `.gitignore` keeps them out.

`SHA256SUMS` lists the SHA-256 of all 2,258 snapshot files, sorted by path:
the 7 tracked metadata files and the 2,251 data files. It does not cover
`.gitignore`, `README.md` or itself. With the data present (this local copy,
or a clone plus the extracted release archive), check it from this directory
with:

```
sha256sum -c SHA256SUMS
```

## Not done yet

- **Release archives:** not built or published. Each GitHub Release asset must
  be under the currently documented 2 GiB per-file limit. Compression has not
  been measured; any archive at or above that limit must be split before
  upload.
- **Rights and release gates remain open.** The new CI-release ledger and
  release manifest have not yet bound these frozen bytes to the existing
  evidence above. That is a format migration, not a new rights review.
- **Remote:** this repository has no remote yet.
- No CI job, profile or seed reads this repository yet.

The application repository's `seed/contracts/peer-overlap.v1.json` names the
two `site_a` files that the public `site_b` overlap anchors must byte-match.
