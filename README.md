# Artist Alley dataset: site_a (metadata repository)

**Status: LOCAL STAGING ONLY. A metadata-only local commit exists. No
remote, nothing published.**

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
- **No approved archive yet:** an older private local candidate archive was
  built before the Sponza exclusion below. It still contains Sponza, is
  superseded by that exclusion and must not be uploaded. No new archive has
  been built or approved, and nothing has been published.

## What `site_a` is

ADR 0058 (`docs/adr/0058-demo-seed-dataset.md` in the application repository)
makes `site_a` (Layer A) the public-safe tier of the demo seed. Its 2026-10-05
amendment names the per-asset `license` and `attribution` in `MANIFEST.json`,
together with `ATTRIBUTIONS.md`, as the authoritative licensing and attribution
record. Public redistribution must honour the terms of every included source.
The application repository's `RELEASING.md` documents earlier publication of
the seed dataset on Kaggle.

## Contents

The snapshot is a relative-path copy of the local `site_a` source, minus the
Sponza exclusion and with the media replacement below: 2,189 files, 2,513,030,786 bytes.

- 7 top-level metadata and attribution files, tracked in Git:
  `ATTRIBUTIONS.md`, `MANIFEST.json`, `dataset-metadata.json`, `groups.csv`,
  `kenney-hq-replacements.json`, `metadata.csv`, `posts.json`.
- 2,182 data files (2,507,224,795 bytes) under `3d/` (766), `audio/` (200),
  `documents/` (18), `fonts/` (33), `images/` (1,073) and `videos/` (92).
  These are local only and go into the release archives.
- `MANIFEST.json` has 2,005 asset rows; every `file_path` is present.
  The other 177 data files are non-manifest files under `3d/`, including model
  companions and one license notice (`3d/kenney-allin1/3d-assets-factory-kit/License.txt`).
- `posts.json` has 863 posts.

### Excluded from the public release: Sponza

By owner decision (issue #1514), the Khronos Sponza scene is excluded from
the public release payload, because the terms governing the exact local
revision are unresolved. The exclusion covers 71 files, 52,686,624 bytes:
`3d/internet/Sponza.gltf` and the 70 buffer and texture files it references
(`Sponza.bin` and 69 images under `3d/internet/`). None of them is another
manifest asset, and no other file under `3d/` names any of them.

- `MANIFEST.json` no longer has the Sponza row, and its asset ID was removed
  from the three posts that held it. Each of those posts keeps at least
  three other assets, and no post was deleted.
- `ATTRIBUTIONS.md` no longer lists Sponza.
- `SHA256SUMS` no longer lists the 71 files.
- The 71 files remain in this local working copy, ignored by Git like every
  other data file, but they are not part of the snapshot and must be left out
  of any future release archive.

The previous metadata commit keeps the earlier listing, so the exclusion can
be reversed.

### Replaced in the local snapshot: Pexels videos and 2CylinderEngine

By owner decision (issue #1514), the 75 Pexels videos and the
2CylinderEngine model were removed from this local working copy and
replaced one-to-one, so the dataset keeps 2,005 assets and 863 posts with
the same IDs and relationships:

- **75 videos** now come from U.S. National Park Service B-roll at
  `videos/nps/<park>/<item-id>.mp4` (63 Grand Canyon, 7 Death Valley,
  4 Joshua Tree, 1 Acadia). Their NPS collection pages state the footage
  is public domain; NPS media pages also say NPS multimedia should not be
  presumed public domain. The label is provisional and is not rights
  clearance. See `ATTRIBUTIONS.md`.
- **2CylinderEngine** is replaced by `machine.glb` from Kenney Factory
  Kit 3.0 (CC0), with the `colormap.png` texture it references and the
  kit's `License.txt`, under `3d/kenney-allin1/3d-assets-factory-kit/`.
- Each replaced record keeps its asset ID, posts and fixture fields, and
  records the file it replaced (path, size and SHA-256) under
  `metadata.replaced` and in `replaced_source_path`. The removed bytes are
  not part of this snapshot; they remain in the original local source and
  are not to be copied into a release payload.
- This replacement is a provisional local staging selection. No archive
  has been built from it and nothing has been published.

Excluded from the snapshot: 13 `*.bak` backup files and one `Thumbs.db`
thumbnail cache. `.gitignore` keeps them out.

`SHA256SUMS` lists the SHA-256 of all 2,189 snapshot files, sorted by path:
the 7 tracked metadata files and the 2,182 data files. It does not cover
`.gitignore`, `README.md` or itself. With the data present (this local copy,
or a clone plus the extracted release archive), check it from this directory
with:

```
sha256sum -c SHA256SUMS
```

## Not done yet

- **Release archives:** no approved archive has been built, and none has
  been published. Each GitHub Release asset must
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
