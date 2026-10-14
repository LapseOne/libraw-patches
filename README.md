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

`patches/0.22.2/` — apply in numeric order onto the official 0.22.2 source tree.

| Patch | Description | Files |
|---|---|---|
| `0001-pana8-c8-multisegment.patch` | Panasonic C8 multi-segment decode support (widen stripe arrays 5→8, add `stripe_total`, `row_base` in `pana8_decode_strip`) plus debug-output cleanup. | `libraw/libraw.h`, `libraw/libraw_internal.h`, `src/decoders/pana8.cpp`, `src/metadata/tiff.cpp` |
| `0002-sigma-x3f-color-fix.patch` | Sigma X3F color rendering fix for SD9/SD10/SD14/DP1/DP1S: corrected per-model color matrices and levels, a per-model decoding-domain transform, and neutral `cam_mul` (the stock greybox auto-WB fallback applies a second white-balance pass on already-balanced Foveon data). All other models are bit-identical to stock. | `src/tables/colordata.cpp`, `src/utils/open.cpp`, `src/x3f/x3f_parse_process.cpp` |
| `0003-colordata-xe5.patch` | Add the Fujifilm X-E5 colordata entry (X-Trans 5 HR; same color matrix as X100VI/X-T5) missing from 0.22.2. | `src/tables/colordata.cpp` |
| `0004-pana6-tail-tolerance.patch` | Tail tolerance for pana6 files without a `data_size` tag (e.g. DC-GH5M2) so trailing appendages do not break the size check. | `src/utils/open.cpp` |
| `0005-kodak-dc120-model.patch` | Truncate the Kodak DC120 model string so lookup keys match. | `src/metadata/identify.cpp` |
| `0006-ahd-cbrt-shared-table.patch` | Shared cbrt lookup table for AHD interpolation, pinning the table bits so MSVC and clang builds produce bit-identical output. | `src/demosaic/ahd_demosaic.cpp`, `src/demosaic/ahd_cbrt_shared.h` |

## How to apply

```sh
curl -LO https://www.libraw.org/data/LibRaw-0.22.2.tar.gz
tar xzf LibRaw-0.22.2.tar.gz
cd LibRaw-0.22.2
for p in ../libraw-patches/patches/0.22.2/*.patch; do
  patch -p1 < "$p"
done
```

Patches apply in numeric order; `git apply --check` passes against a fresh checkout of the official tarball. Build the library as usual after applying.

## License

LibRaw is dual-licensed under **LGPL-2.1** or **CDDL-1.0** (at your option). In accordance with LibRaw's license statement, the modifications in this repository are also offered under both licenses: **LGPL-2.1 and CDDL-1.0**. See the license texts in the upstream source tree (`LICENSE.LGPL`, `LICENSE.CDDL`, `COPYRIGHT`).

When distributing binaries built with these patches, provide a link to this repository and to the official upstream source (above) so that recipients can obtain the corresponding source of the modified library.

## Status

- Not merged upstream.
- Applied at build time only; the official LibRaw source tree is never modified in place.
