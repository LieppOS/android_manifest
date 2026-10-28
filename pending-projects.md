# Pending Android Projects

These repositories are **not** in `default.xml`. Either their GitHub remotes are
still empty, or they contain local commits that have not been published and are
therefore not remotely resolvable.

| Repository | Intended Android path | State | Local commit | Reason |
| --- | --- | --- | --- | --- |
| `android_kernel_ulefone_gq5012bf1` | `kernel/ulefone/gq5012bf1` | `LOCAL_COMMIT_UNPUSHED` | `c3bd65dcf7e2bf437f01783e120c8e7477cd788a` | C47 imported the verified C44 source snapshot and replayed the C45/C46 patches, excluding both private-key PEM files; the remote is still empty |
| `android_kernel_modules_mediatek_mt6878` | `kernel/modules/mediatek/mt6878` | `LOCAL_COMMIT_UNPUSHED` | `83adb6057cb20d71ab3cdac100c37910e70f5593` | C47 imported a verified MediaTek source mirror with per-file hashes; the remote is still empty |
| `android_kernel_modules_ulefone_gq5012bf1` | `kernel/modules/ulefone/gq5012bf1` | `LOCAL_COMMIT_UNPUSHED` | `5fd5a2758d525b55bae4e354b32af4ce6c8c23e7` | C47 imported a verified Ulefone board source mirror with per-file hashes; the remote is still empty |
| `android_hardware_mediatek_mt6878` | `hardware/mediatek/mt6878` | `REMOTE_EMPTY` | — | No verified HAL source imported |
| `android_hardware_ulefone_gq5012bf1` | `hardware/ulefone/gq5012bf1` | `REMOTE_EMPTY` | — | No verified board HAL source imported |
| `android_vendor_mediatek_mt6878` | `vendor/mediatek/mt6878` | `REMOTE_EMPTY` | — | Proprietary compatibility boundary unresolved |
| `android_vendor_ulefone_gq5012bf1` | `vendor/ulefone/gq5012bf1` | `REMOTE_EMPTY` | — | Proprietary compatibility boundary unresolved |
| `android_recovery_ulefone_gq5012bf1` | `recovery/ulefone/gq5012bf1` | `REMOTE_EMPTY` | — | OrangeFox is a separate verified project outside this path |
| `android_tools_ulefone_gq5012bf1` | `tools/ulefone/gq5012bf1` | `REMOTE_EMPTY` | — | Build/AVB tools still live in research and campaign trees |

The `android_manifest` repository itself is also empty on GitHub. The local
bootstrap manifest and `local-development.xml` are local commits and have not
been pushed.

## Local development manifest

[`local-development.xml`](local-development.xml) records the three C47 kernel
commits for local work only. It is **not** included by `default.xml` and must not
be published as an active manifest while the commits are unreachable from the
GitHub remotes. Use it as an explicit ledger, not as a `repo sync` source from
GitHub.

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
