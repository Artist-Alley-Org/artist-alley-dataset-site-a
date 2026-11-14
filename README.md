# Artist Alley dataset: site_a (metadata repository)

**Status: public metadata repository. Edition `v2026.11.01` is published as a
provisional GitHub prerelease, and nothing here is rights clearance.**

This repository holds the metadata for the public `site_a` demo seed dataset
for issue #1514 (v0.11.1). It is kept separate from the artist-alley
application repository: dataset bytes do not live in the application
repository. Its home is the Artist-Alley-Org GitHub organization.

## What Git holds, and where the bytes go

- **Git holds metadata only:** the seven top-level files listed below, this
  README, `SHA256SUMS` and `.gitignore`. No binary payload and no Git LFS
  pointers are tracked, so a clone contains no dataset bytes.
- **The binary snapshot is not in this repository.** The six data directories
  (`3d/`, `audio/`, `documents/`, `fonts/`, `images/`, `videos/`) exist only in
  the maintainers' local working copy, where they are used to build release
  archives. They are not tracked, and `.gitignore` ignores them so they cannot
  be staged by accident.
- **Consumers get the bytes from versioned GitHub Release archives.** Before
  extracting and seeding, a consumer verifies the archive's checksum, then
  `SHA256SUMS` and `MANIFEST.json`.
- **A clone alone cannot run `sha256sum -c SHA256SUMS`:** the data files it
  lists are only present after the matching release archive has been
  downloaded and extracted.

## Published editions

Both editions are GitHub prereleases. The project owner reviewed the
recorded source evidence and chose to publish them as provisional; that is
not rights clearance (see "Not done yet"). A tag alone is not an integrity
pin: always check the `release.json` SHA-256.

- **Full edition
  [`v2026.11.01`](https://github.com/Artist-Alley-Org/artist-alley-dataset-site-a/releases/tag/v2026.11.01):**
  the tag resolves to metadata commit
  `62ebfa6a642f96ceda829ffdad42601d4d9fe525`, and `release.json` has SHA-256
  `84996745cda978476dacfe7cf8ae1030d0d0d21faa7f1593e3aa713e0c092da5`. It pins
  `SHA256SUMS`, every metadata file and the two payload parts,
  `site_a-payload.tar.gz.part0000` (1,899,999,999 bytes) and
  `site_a-payload.tar.gz.part0001` (530,808,111 bytes): one gzip stream of
  2,430,808,110 bytes, split so each asset stays under GitHub's 2 GiB limit.
- **CI subset edition
  [`v2026.11.01-sm`](https://github.com/Artist-Alley-Org/artist-alley-dataset-site-a/releases/tag/v2026.11.01-sm):**
  a byte-identical subset of the full edition for pull-request CI (165 assets,
  91 posts, 207 payload files in one part). Its tag is on its own branch,
  `edition/v2026.11.01-sm`, at metadata commit
  `3f078b1500ce066d5df04adeba11d38fc8ad7277`, and its `release.json` has
  SHA-256 `c6838e5020b1bc56f048c0602c2021c1c840e3e3ddf236827eb7358b521a503f`.
  The README at that tag describes it.
- **Asset names:** both editions use the original names, `release.json` and
  `site_a-payload.tar.gz.partNNNN`, and they will not change. Later editions
  will carry the site and tag in every asset name.
- **Download and verify:** in a checkout of the edition's tag (not a later
  `main` commit), check `release.json` against its SHA-256 and each part
  against its `sha256` and `bytes` in `release.json`. Then run
  `cat site_a-payload.tar.gz.part0000 site_a-payload.tar.gz.part0001 | tar -xzf -`
  at the repository root for the full edition, and `sha256sum -c SHA256SUMS`
  (2,189 entries). The Artist Alley tool `seed/scripts/release_dataset.py verify`
  performs these checks, plus member, path and link checks.

## How Artist Alley CI uses it

As of 2026-11-14, the Artist Alley application pins both editions in
`seed/contracts/ci-datasets.v1.json` by repository, tag, metadata commit and
`release.json` SHA-256, and its UI workflows fetch them anonymously on
GitHub-hosted runners:

- The scheduled UI nightly downloads and verifies the full `v2026.11.01`
  edition, together with `site_b`, and seeds its CI selection from it for the
  federation and shell scenarios.
- The pull-request UI workflow, which also runs on pushes to the
  application's `main` and `dev` branches, downloads only the small
  `v2026.11.01-sm` edition and seeds the same CI selection from it.

## What `site_a` is

ADR 0058 (`docs/adr/0058-demo-seed-dataset.md` in the application repository)
makes `site_a` (Layer A) the public-safe tier of the demo seed. Its 2026-10-05
amendment names the per-asset `license` and `attribution` in `MANIFEST.json`,
together with `ATTRIBUTIONS.md`, as the authoritative licensing and attribution
record. That designation predates the 2026-11 media replacement below and does
not clear it: the 75 NPS clips are provisional, and the Factory Kit model needs
its own review. Public redistribution must honour the terms of every included
source.
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
- On 2026-11-13 the 71 files were removed from the maintainers' local working copy,
  after each matched the mounted original byte for byte. A private
  quarantine copy and the mounted original keep the exclusion reversible.
  They are not part of the snapshot and must be left out of any release
  archive.

The previous metadata commit keeps the earlier listing, so the exclusion can
be reversed.

### Replaced in the local snapshot: Pexels videos and 2CylinderEngine

By owner decision (issue #1514), the 75 Pexels videos and the
2CylinderEngine model were removed from the maintainers' local working copy and
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
- The published `v2026.11.01` edition contains these replacements. Their
  rights labels remain provisional (see "Not done yet").

Excluded from the snapshot: 13 `*.bak` backup files and one `Thumbs.db`
thumbnail cache. `.gitignore` keeps them out.

`SHA256SUMS` lists the SHA-256 of all 2,189 snapshot files, sorted by path:
the 7 tracked metadata files and the 2,182 data files. It does not cover
`.gitignore`, `README.md` or itself. With the data present (the maintainers'
local copy, or a clone plus the extracted release archive), check it from this directory
with:

```
sha256sum -c SHA256SUMS
```

## Not done yet

- **Rights review:** publication is not rights clearance. The owner
  published `v2026.11.01` as a provisional prerelease without completing the
  per-file review: the rows of the CI-release ledger for these frozen bytes
  are still pending review.
- **Provisional sources:** the 75 NPS clips and the Factory Kit model still
  need their own rights review.

The application repository's `seed/contracts/peer-overlap.v1.json` names the
two `site_a` files that the public `site_b` overlap anchors must byte-match.
