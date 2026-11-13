# Artist Alley dataset: site_a CI subset edition `v2026.11.01-sm`

**This tag holds the metadata for edition `v2026.11.01-sm`. Download the matching GitHub Release payload and verify it against the pins below before seeding. This is not rights clearance.**

This is the metadata-only snapshot of the small `site_a` edition `v2026.11.01-sm`, a byte-identical subset
of the full edition `v2026.11.01` for pull-request CI. It lives in the same repository as the full
edition (<https://github.com/Artist-Alley-Org/artist-alley-dataset-site-a>), as the tag `v2026.11.01-sm` on its own branch; it does not change `main` or `v2026.11.01`.

## Full edition and small edition

| | full `v2026.11.01` | small `v2026.11.01-sm` |
| --- | ---: | ---: |
| assets | 2,005 | 165 |
| posts | 863 | 91 |
| videos | 92 | 9 |
| payload files | 2,182 | 207 |
| payload bytes | 2,507,224,795 | 665,709,517 |
| release archive | 2,430,808,110 bytes in 2 parts | 658,160,717 bytes in 1 part |

- **Full edition (`v2026.11.01`):** the whole catalogue, used for the scheduled (nightly) scale and
  federation runs.
- **Small edition (`v2026.11.01-sm`):** only the rows and files Artist Alley's `aa seed --profile ci` selects
  from the full edition, plus the two declared cross-peer overlap anchors and the selected rows' licence
  notices. It still covers all 131 coverage dimensions, so pull-request CI
  seeds the same fixture it would seed from the full edition, from a much smaller download.

`EDITION.json` records the source edition's pins, the selection command and report digest, every count,
the per-file row rules and every corrected metadata value.

## What Git holds

Metadata only: this README, `.gitignore`, `ATTRIBUTIONS.md`, `EDITION.json`, `MANIFEST.json`,
`dataset-metadata.json`, `groups.csv`, `kenney-hq-replacements.json`, `metadata.csv`, `posts.json` and
`SHA256SUMS`. The 207 payload files ship only in the GitHub Release for this tag.
`SHA256SUMS` lists the 8 metadata files and the 207 payload files; it does not cover this
README, `.gitignore` or itself, so a clone alone cannot pass `sha256sum -c SHA256SUMS`.

## Immutable pins

- Repository `Artist-Alley-Org/artist-alley-dataset-site-a`, tag `v2026.11.01-sm`, and the metadata commit the tag resolves to.
- `release.json` SHA-256 `c6838e5020b1bc56f048c0602c2021c1c840e3e3ddf236827eb7358b521a503f`; it pins `SHA256SUMS` (SHA-256
  `1037fc6985718ba47aaecab016916c315ecc5fea2ceb161c87de0e59813a6ab1`), every metadata file and the archive part:
  - `site_a-payload.tar.gz.part0000`: 658,160,717 bytes, SHA-256 `139292dcd564bff3b513356087acf9992544b0e4b9a3a5e8f8f412d856f31f88`
- A tag alone is not an integrity pin: always check the `release.json` SHA-256.

## Download and verify

With the Artist Alley application repository checked out, from a metadata-only checkout of this tag:

```
python3 -I seed/scripts/fetch_release_dataset.py --repo Artist-Alley-Org/artist-alley-dataset-site-a \
  --tag v2026.11.01-sm --site site_a --expect-manifest-sha256 c6838e5020b1bc56f048c0602c2021c1c840e3e3ddf236827eb7358b521a503f \
  --metadata-root <this checkout> --output <new site root>
```

It downloads `release.json` first and checks its SHA-256, then each part (size and SHA-256 while
streaming), then rebuilds the site root and checks every file against `SHA256SUMS`. By hand: download
`release.json` and the part, check `sha256sum release.json`, `cat` the part(s) in order through
`tar -xz` in a new directory, copy the metadata files beside the payload, and run
`sha256sum -c SHA256SUMS`.

## Rights

Every asset keeps its own `license` and `attribution` in `MANIFEST.json`, unchanged from the full
edition; `ATTRIBUTIONS.md` summarises the pairs this edition contains and the caveats that apply to
them (one provisional NPS clip, the Factory Kit model whose source review is pending, two replacement
rows, six assets labelled mature). Honour the per-asset terms. This snapshot is not rights clearance and
not release approval.
