# Historical RecompCore patches

These patches record BlueWake's RecompCore changes as they were made. They are history, not a build
input: the series starts at 0008 (0001-0007 were never exported), so it does not apply to the
upstream base 5c3611e, and the local head it led to (3476998) was never published.

The build uses a fork instead. BlueWake's is https://github.com/chrissotraidis/RecompCore, branch
`bluewake`, commit 2d6063614a9bc899f6b4d11c7e7b3cd66e4d96f3: it contains the changes here through 0097
(some were revised by later ones), the files that were never committed on the development Mac, and the
DolRecomp submodule pointing at https://github.com/chrissotraidis/DolRecomp (5c91d6e). Wind Waker Recomp
builds from its own copy, https://github.com/elliotttate/RecompCore, branch `bluewake`, commit
8ab24da: that tree plus 0098 to 0112, with DolRecomp at https://github.com/elliotttate/DolRecomp
(b8b5345, 5c91d6e plus patches/dolrecomp/0019). The Builder fetches it at the commit pinned in
`scripts/builder/profiles/bluewake.sh`; see docs/status/DEVICE_BUILD.md.

The Mac checkout also applies the exact delta recorded in
`config/recompcore-patches.json`. Patch 0150 combines the indexed, CPU-deformed
vertex interpolation fix from 0113, the stable screen-space HUD from 0140,
and the Windows lava fix from 0120
(RecompCore `81d7345`, with the test's line endings corrected in `7c62903`).
The combined patch applies to the pinned `8ab24da` base. Bootstrap, desktop/iOS
CMake and the builder verify its checksum and reject unrelated dependency edits.
