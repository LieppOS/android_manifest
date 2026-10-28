# Pending Android Projects

These repositories are **not** in `default.xml`. Either their GitHub remotes are
still empty, or they contain local commits that have not been published and are
therefore not remotely resolvable.

## C48 publication state

| Repository | Intended Android path | State | Publication commit | Reason |
| --- | --- | --- | --- | --- |
| `android_kernel_ulefone_gq5012bf1` | `kernel/ulefone/gq5012bf1` | `PUBLISHED_BRANCH_NOT_IN_DEFAULT` | `d0feaaee4484e2fdd5cd87ebbf4c9e273a50b8b0` | Sanitized root snapshot on remote `publication/c48`; proprietary FPSGO wrapper and four firmware/calibration-bearing Ulefone drivers withheld |
| `android_kernel_modules_mediatek_mt6878` | `kernel/modules/mediatek/mt6878` | `PUBLISHED_BRANCH_NOT_IN_DEFAULT` | `82b5cbcbf4c54da198c01fa04c9dc6e92319e3af` | Sanitized MediaTek mirror on remote `publication/c48`; FPSGO wrapper and Ulefone-owned paths excluded |
| `android_kernel_modules_ulefone_gq5012bf1` | `kernel/modules/ulefone/gq5012bf1` | `PUBLISHED_BRANCH_NOT_IN_DEFAULT_PARTIAL` | `0e00dda7f53fedd8b83092aca5c3b96542bf255f` | Partial board mirror on remote `publication/c48`; FocalTech, Hynitron, tiny LCD and SH366003 withheld pending firmware/calibration licensing |
| `android_hardware_mediatek_mt6878` | `hardware/mediatek/mt6878` | `REMOTE_EMPTY` | — | No verified HAL source imported |
| `android_hardware_ulefone_gq5012bf1` | `hardware/ulefone/gq5012bf1` | `REMOTE_EMPTY` | — | No verified board HAL source imported |
| `android_vendor_mediatek_mt6878` | `vendor/mediatek/mt6878` | `REMOTE_EMPTY` | — | Proprietary compatibility boundary unresolved |
| `android_vendor_ulefone_gq5012bf1` | `vendor/ulefone/gq5012bf1` | `LOCAL_CONFIG_ONLY` | `2685239653933c48324e2b179475198271090332` | Generated extract-utils configuration committed locally on `integration/c52-device-vendor`; no blob payload is committed and the remote remains empty |
| `android_recovery_ulefone_gq5012bf1` | `recovery/ulefone/gq5012bf1` | `REMOTE_EMPTY` | — | OrangeFox is a separate verified project outside this path |
| `android_tools_ulefone_gq5012bf1` | `tools/ulefone/gq5012bf1` | `REMOTE_EMPTY` | — | Build/AVB tools still live in research and campaign trees |

The `android_manifest` repository itself is also empty on GitHub. The local
bootstrap manifest, `local-development.xml` and `publication-candidate.xml` are
local commits and have not been pushed.

## C49/C50 local source integration (not published)

| Repository | Local branch | Local commit | Parent (published) | Change |
| --- | --- | --- | --- | --- |
| `android_kernel_ulefone_gq5012bf1` | `integration/c49-hardware-source-expansion` | `27a31fbc7a70b13efd74b50721147e44f8914e84` | `d0feaaee` | C49 `e62fd25e`, `188eae96` (vendor modules); C50 `53944a0a` (independent gen4m TKIP MIC + tests), `5e7efb0d` (gen4m build), `27a31fbc` (hostap licence notice) |
| `android_kernel_modules_mediatek_mt6878` | `integration/c49-hardware-source-expansion` | `cbc7ea9e2fd70d8136a7a8976b863a6d1b28127c` | `82b5cbcb` | Mirrors `vendor/mediatek/kernel_modules` from kernel `27a31fbc` (C49 `16690470`, C50 `9a3c6ef7`, `cbc7ea9e`) |
| `android_kernel_modules_ulefone_gq5012bf1` | — | `0e00dda7` (unchanged) | — | No Ulefone-owned source changed |

These commits are recorded only in `local-development.xml`. They are not
reachable from GitHub and must not appear in `default.xml` or
`publication-candidate.xml` until they are pushed and verified. See
`docs/integration/C49-SOURCE-INTEGRATION.md` and
`docs/integration/C50-PUBLICATION-READINESS.md` in the LieppOS workspace.

## C52 local device/vendor integration (not published)

| Repository | Local branch | Local commit | Parent | Change |
| --- | --- | --- | --- | --- |
| `android_device_ulefone_gq5012bf1` | `integration/c52-device-vendor` | `ad24a166f22fa58e742f1e49b0904df4ddd961c5` | `58e7e2d1` (published `master`) | Android 17 product evaluation, stock-vendor compatibility matrix and VNDK 34, stock vbmeta split, disabled C51 vibrator exclusions, Android 14/15 legacy-gate fix |
| `android_vendor_ulefone_gq5012bf1` | `integration/c52-device-vendor` | `2685239653933c48324e2b179475198271090332` | none (unborn `main`) | Generated vendor configuration only; proprietary payload gitignored |

Neither commit is reachable from GitHub. The vendor repository remote is empty,
so the vendor path is not remotely syncable. See
`docs/integration/C52-DEVICE-VENDOR-INTEGRATION.md` in the LieppOS workspace.

## C53 recovered Android 17 build (not published)

| Repository | Local branch | Local commit | Parent | Change |
| --- | --- | --- | --- | --- |
| `android_device_ulefone_gq5012bf1` | `integration/c52-device-vendor` | `a36009aeff8cb0547c84c5b21d0775d95d0a5f5f` | `ad24a166f22fa58e742f1e49b0904df4ddd961c5` | Recovered the Android 17 product build: init_boot header v4, AVB test-key inputs, unavailable-module removal, stock-vendor `EXTRACT_ONLY` staging, build-time VINTF sepolicy split |
| `android_vendor_ulefone_gq5012bf1` | `integration/c52-device-vendor` | `1810485a1a3790fd2703bf4a0c4996e07df8d7ee` | `2685239653933c48324e2b179475198271090332` | Regenerated configuration; duplicate stock/AOSP install paths removed; proprietary payload remains gitignored |

C53 did not push, build a full ROM, or touch the phone. The extracted vendor
payload is partial (62 sources), and a rebuilt vendor image remains unsafe until
the MediaTek vendor SELinux policy is resolved. See
`docs/integration/C53-SOONG-BUILD-RECOVERY.md` in the LieppOS workspace and
`research/ulefone-gq5012bf1/campaigns/c53-soong-build-recovery/`.

## C54 vendor payload/SELinux integration (not published)

| Repository | Local branch | Local commit | Parent | Change |
| --- | --- | --- | --- | --- |
| `android_device_ulefone_gq5012bf1` | `integration/c52-device-vendor` | `9bb55ae5cf998147a4e83b01e755563669f0c2dc` | `a36009aeff8cb0547c84c5b21d0775d95d0a5f5f` | Curated 2,970-entry vendor manifest (2,652 installed, 318 `EXTRACT_ONLY`); modem/audio/camera/GPU/TEE/SELinux analysis additions; Android 17 source `wpa_supplicant` selected |
| `android_vendor_ulefone_gq5012bf1` | `integration/c52-device-vendor` | `f9606328d6486ac6051800a28d89eaad4f87e045` | `1810485a1a3790fd2703bf4a0c4996e07df8d7ee` | Regenerated vendor configuration; 778 MB `vendor.img` and 15 MB `vendor_dlkm.img` built locally; proprietary payload remains gitignored |

C54 supersedes C53's 62-source partial payload with a 1.991 GB local curated
payload. `m nothing`, `m vendorimage` and `m vendor_dlkmimage` pass; the C52
VINTF contract passes against both stock and rebuilt vendor metadata with the
stock ODM manifest. C54 is not a full ROM: 318 entries are `EXTRACT_ONLY`, 198
stock vendor files remain deliberately unselected (100 MB camera tuning DB,
401 MB calibration data, 2 keybox files, face-only models, overlays and debug
tools), the stock Android 15 `34.0` vendor SELinux policy could not be
integrated into Android 17 `202604`, and vendor-only linker closure still has
215 unresolved `NEEDED` references. See
`research/ulefone-gq5012bf1/campaigns/c54-vendor-selinux-integration/` and
`docs/integration/C54-VENDOR-SELINUX-INTEGRATION.md` in the LieppOS workspace.

## Superseded C47 local commits

The C47 source-import commits remain in the local development repositories but
are **not** the publication source:

| Repository | C47 local commit | Why not published |
| --- | --- | --- |
| `android_kernel_ulefone_gq5012bf1` | `7a75e19ebf5d7e4b3876c19f4e77cad385dff4f0` | Five-commit history contains the proprietary FPSGO private wrapper and four firmware/calibration-bearing Ulefone drivers |
| `android_kernel_modules_mediatek_mt6878` | `83adb6057cb20d71ab3cdac100c37910e70f5593` | Contains the FPSGO private wrapper and Ulefone-owned paths |
| `android_kernel_modules_ulefone_gq5012bf1` | `5fd5a2758d525b55bae4e354b32af4ce6c8c23e7` | Contains four drivers with embedded vendor firmware/calibration images |

The sanitized C48 publication branches are new root snapshots; the C47 branches
were not rewritten.

## Local development manifest

[`local-development.xml`](local-development.xml) records the three C48
publication-branch commits for local work only. It is **not** included by
`default.xml` and must not be published as an active manifest while the commits
are unreachable from the GitHub remotes. Use it as an explicit ledger, not as a
`repo sync` source from GitHub.

[`publication-candidate.xml`](publication-candidate.xml) is the exact intended
remote configuration after publication. It is also not active. It pins the
sanitized kernel, the MediaTek mirror and the partial Ulefone mirror. It does
**not** claim complete device support: the four withheld Ulefone drivers are
absent.

## C48 blockers

1. **Promotion into `default.xml` is not approved.** (C49 correction: the
   three C48 commits are reachable on the remote `publication/c48` branches and
   the manifest `main` is at `30600049`; verified with `git ls-remote`.) The
   remotes have no `main` branch, so `publication-candidate.xml` records
   `upstream="publication/c48"`. Activating the candidate still requires owner
   approval.
2. **Firmware/calibration redistribution is unresolved.** The FocalTech FT3680
   firmware, Hynitron CST8xx firmware, tiny-LCD logo/init arrays and SH366003
   AFI image have no documented redistribution grant. They are withheld from the
   publication branches.
3. **The research repository already contains a calibration blob.** The
   published `master` history includes
   `kernel/phase4-sh366003-recon/sinofs_afi_data.inc`. C48 did not add any new
   blob, but the owner should review this pre-existing exposure. Removing it
   would require a new commit; history rewrite is not proposed.
4. **Partial device support.** The published kernel/mirror set can build the
   core kernel, panel and `mtk-mmc`, but not the four withheld board modules.
   Do not present the manifest candidate as a complete device build.

## Promotion checklist

Before moving a row into `default.xml`:

- [ ] The remote has a real branch and the pinned commit is reachable remotely.
- [ ] The project path is unique and inside the single Android build root.
- [ ] The project does not duplicate source owned by another repository.
- [ ] Licence and provenance are recorded.
- [ ] Any proprietary content is isolated and not misrepresented as open source.
- [ ] No signing key, calibration data, NVRAM, or device-unique secret is present.
- [ ] The pinned SHA is recorded in `ACTIVE-BUILD-PROVENANCE.md`.
- [ ] Owner approval for publication has been recorded.
- [ ] The four withheld Ulefone drivers are either licensed for publication or
      explicitly documented as private-only.
