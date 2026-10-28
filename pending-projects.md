# Pending Android Projects

These repositories are **not** in `default.xml` because their GitHub remotes are
empty (no branch, no commit). They must not be added to an active manifest until
a real commit exists and the path mapping is reviewed.

| Repository | Intended Android path | State | Reason |
| --- | --- | --- | --- |
| `android_kernel_ulefone_gq5012bf1` | `kernel/ulefone/gq5012bf1` | `REMOTE_EMPTY` | External C45/C46 source is not importable while it contains private-key PEM files; remote has no commit |
| `android_kernel_modules_mediatek_mt6878` | `kernel/modules/mediatek/mt6878` | `REMOTE_EMPTY` | Source audit and split not complete |
| `android_kernel_modules_ulefone_gq5012bf1` | `kernel/modules/ulefone/gq5012bf1` | `REMOTE_EMPTY` | Board modules still live in the combined kernel tree |
| `android_hardware_mediatek_mt6878` | `hardware/mediatek/mt6878` | `REMOTE_EMPTY` | No verified HAL source imported |
| `android_hardware_ulefone_gq5012bf1` | `hardware/ulefone/gq5012bf1` | `REMOTE_EMPTY` | No verified board HAL source imported |
| `android_vendor_mediatek_mt6878` | `vendor/mediatek/mt6878` | `REMOTE_EMPTY` | Proprietary compatibility boundary unresolved |
| `android_vendor_ulefone_gq5012bf1` | `vendor/ulefone/gq5012bf1` | `REMOTE_EMPTY` | Proprietary compatibility boundary unresolved |
| `android_recovery_ulefone_gq5012bf1` | `recovery/ulefone/gq5012bf1` | `REMOTE_EMPTY` | OrangeFox is a separate verified project outside this path |
| `android_tools_ulefone_gq5012bf1` | `tools/ulefone/gq5012bf1` | `REMOTE_EMPTY` | Build/AVB tools still live in research and campaign trees |

The `android_manifest` repository itself is also empty on GitHub. The local
bootstrap manifest is a local commit and has not been pushed.

## Promotion checklist

Before moving a row into `default.xml`:

- [ ] The remote has a real branch and commit.
- [ ] The project path is unique and inside the single Android build root.
- [ ] The project does not duplicate source owned by another repository.
- [ ] Licence and provenance are recorded.
- [ ] Any proprietary content is isolated and not misrepresented as open source.
- [ ] No signing key, calibration data, NVRAM, or device-unique secret is present.
- [ ] The pinned SHA is recorded in `ACTIVE-BUILD-PROVENANCE.md`.
