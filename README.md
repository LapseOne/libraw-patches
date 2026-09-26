# libraw-patches

Local modifications to [LibRaw](https://www.libraw.org), maintained by LapseOne.

Changes are distributed as patch files so the official LibRaw source tree stays pristine; patches are applied at build time only.

## Upstream baseline

| Item | Value |
|---|---|
| LibRaw version | 0.22.2 |
| Official source | https://www.libraw.org/data/LibRaw-0.22.2.tar.gz |
| SHA-256 | `DE86B035655ACCFF8D4010F1A221FDF50D353CB7B1422BA26F14A0DB92612CFA` |

The source trees in the project are byte-identical to the official 0.22.2 package (verified 2026-08-08). The patches apply to both the Windows and macOS source trees (`LibRaw/win/LibRaw-0.22.2` and `LibRaw/mac/LibRaw-0.22.2`), because the modified files are platform-independent.

## Patches

`patches/0.22.2/`

| Patch | Description | Files |
|---|---|---|
| `0001-pana8-c8-multisegment.patch` | Panasonic Compression 8 (pana8) multi-segment decode support: widen `pana8_tags_t` stripe arrays 5→8, add the `stripe_total` field, parse the total entry count in `parse_tiff_ifd`, and add a `row_base` parameter to `pana8_decode_strip` so files whose C8 data is split into multiple vertically stacked stripe segments decode correctly. | `libraw/libraw.h`, `libraw/libraw_internal.h`, `src/decoders/pana8.cpp`, `src/metadata/tiff.cpp` |

## How to apply

```sh
curl -LO https://www.libraw.org/data/LibRaw-0.22.2.tar.gz
tar xzf LibRaw-0.22.2.tar.gz
cd LibRaw-0.22.2
git apply ../libraw-patches/patches/0.22.2/0001-pana8-c8-multisegment.patch
# or: patch -p1 < ../libraw-patches/patches/0.22.2/0001-pana8-c8-multisegment.patch
```

`git apply --check` passes against a fresh checkout of the official tarball. Build the library as usual after applying.

## License

LibRaw is dual-licensed under **LGPL-2.1** or **CDDL-1.0** (at your option). In accordance with LibRaw's license statement, the modifications in this repository are also offered under both licenses: **LGPL-2.1 and CDDL-1.0**. See the license texts in the upstream source tree (`LICENSE.LGPL`, `LICENSE.CDDL`, `COPYRIGHT`).

When distributing binaries built with these patches, provide a link to this repository and to the official upstream source (above) so that recipients can obtain the corresponding source of the modified library.

## Status

- Not merged upstream.
- Applied at build time only; the official LibRaw source tree is never modified in place.
