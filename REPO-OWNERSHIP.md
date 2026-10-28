# LieppOS Repository and Path Ownership

**Version:** 1.0  
**Effective:** 2026-10-28  
**Canonical location:** `~/LieppOS/manifests/android_manifest/REPO-OWNERSHIP.md`  
**Companion:** [AI-POLICY.md](AI-POLICY.md)

This is the map of **existing** LieppOS repositories. It does not authorize new repositories, directory reorganizations, or overlapping Git checkout destinations. Paths below are relative to `/home/armol/LieppOS/` unless otherwise indicated.

## 1. Existing repositories

| # | GitHub repository (`LieppOS/…`) | Existing local path | Primary ownership |
|---:|---|---|---|
| 1 | `android_manifest` | `manifests/android_manifest/` | Android repo manifests, revision pins, project policy and ownership map |
| 2 | `android_device_ulefone_gq5012bf1` | `android/device/ulefone/gq5012bf1/` | Board/product config, device tree, init and device-level integration definitions |
| 3 | `android_kernel_ulefone_gq5012bf1` | `android/kernel/ulefone/gq5012bf1/` | Main coherent buildable GKI/kernel integration tree |
| 4 | `android_kernel_modules_mediatek_mt6878` | `android/kernel/modules/mediatek/mt6878/` | MediaTek module source mirror/ownership and lineage |
| 5 | `android_kernel_modules_ulefone_gq5012bf1` | `android/kernel/modules/ulefone/gq5012bf1/` | Ulefone board-specific module source mirror/ownership and lineage |
| 6 | `android_hardware_mediatek_mt6878` | `android/hardware/mediatek/mt6878/` | MediaTek Android userspace HAL source, interfaces and integrations |
| 7 | `android_hardware_ulefone_gq5012bf1` | `android/hardware/ulefone/gq5012bf1/` | Ulefone-specific HAL implementations and device services |
| 8 | `android_vendor_mediatek_mt6878` | `android/vendor/mediatek/mt6878/` | MediaTek vendor-side integrations and permitted vendor packaging |
| 9 | `android_vendor_ulefone_gq5012bf1` | `android/vendor/ulefone/gq5012bf1/` | Device proprietary-file extraction manifests, vendor configs and permitted packaging |
| 10 | `android_vendor_lieppos` | `android/vendor/lieppos/` | LieppOS OS customizations, build flavors, shared product features |
| 11 | `android_recovery_ulefone_gq5012bf1` | `android/recovery/ulefone/gq5012bf1/` | Device recovery integration source and build definitions |
| 12 | `android_tools_ulefone_gq5012bf1` | `tools/ulefone/gq5012bf1/` | Reusable device tools and validation utilities, including `hal-audit/` |
| 13 | `LieppOS-ulefone-gq5012bf1-research` | `research/ulefone-gq5012bf1/` | Canonical GQ5012BF1 hardware RE, source provenance, measurements and evidence |
| 14 | `OrangeFox-Ulefone-GQ5012BF1` | `recovery/ulefone/gq5012bf1/orangefox/` | OrangeFox-specific recovery sources/integration |
| 15 | `LieppOS.github.io` | `website/LieppOS.github.io/` | Project website and published documentation site |
| 16 | `SeklysMorka` | `apps/SeklysMorka/` | SeklysMorka Android application |
| 17 | `TinyDisplay` | `apps/TinyDisplay/` | TinyDisplay Android application |
| 18 | `NFC-ESP32-Android` | `apps/NFC-ESP32-Android/` | NFC/ESP32-related Android application |
| 19 | `scripts` | `legacy/scripts/` | Existing legacy LieppOS scripts; do not duplicate into new locations without a migration decision |
| 20 | `docs` | `legacy/docs/` | Existing legacy documentation; do not mistake it for workspace-level `docs/` |

**Important:** The directory names above describe logical ownership. An entry may currently be empty, local-only, or incompletely integrated. Do not infer implementation completeness or public availability from its name.

## 2. Workspace-level directories that are not independent Git repositories

These locations have existed as workspace coordination areas; do not silently turn them into additional repositories:

| Path | Function | Editing rule |
|---|---|---|
| `AGENTS.md` | Model-neutral entrypoint to the canonical policy | Workspace-level coordination; minimal pointer only |
| `docs/` | Workspace integration notes, provenance, roadmaps and campaign handoffs | Edit only when assigned; do not duplicate source-repository documentation |
| `inventory/` | Cross-repository subsystem inventory, including `subsystems.csv` | Shared artifact; only one agent writes at a time |
| Existing `out/` / `artifacts/` or other designated build-output locations | Disposable build outputs and selected release artifacts | Use existing paths as assigned; do not commit generated junk or create arbitrary alternatives |

Do not assume every example path already exists. Inspect actual workspace state before use; create no empty placeholders.

## 3. Source ownership boundaries

**Main GKI versus module mirrors:** The C48 main kernel source has a buildable integrated snapshot; the MediaTek and Ulefone repositories contain matching source subsets/mirrors. They are **not three independent competing owners of the same Android checkout path**. Before making a change, inspect the existing synchronization/provenance contract and select one authoritative source owner per file. Reflect approved changes into mirrors deterministically; do not hand-edit diverging versions of the same source.

**Kernel versus Android HAL:** Kernel drivers and Kbuild belong to kernel/module owners. Android AIDL/HIDL HAL implementations, services and their source belong to hardware owners. Device-level activation (device makefiles, VINTF integrations, init rc and device SELinux policy) belongs to the device repository unless current source proves a different existing owner. Vendor extraction recipes and proprietary dependencies belong to vendor repositories under appropriate redistribution restrictions.

**Research versus production:** Full hardware RE findings and proof belong to the research repo. Production C/C++, Kbuild, Android.bp and device configuration must remain in their appropriate source repository. Research folders marked `DONE` are not automatically deployable production components.

**Recovery variants:** Device recovery platform definitions and OrangeFox-specific changes have distinct owners. Do not overwrite one with the other.

**Legacy repositories:** `legacy/scripts` and `legacy/docs` are existing, separately versioned history. Do not create a third current copy by default. Proposed migration must identify the receiving owner and preserve history/links.

## 4. Parallel agent write ownership

No model has permanent ownership. The **current scoped assignment** decides who may write to a repository or subdirectory. For example:

| Concurrent task | Example writable boundary | Others |
|---|---|---|
| Kernel/source integration | Explicitly assigned kernel/module paths | Read-only |
| HAL/VINTF audit | `tools/ulefone/gq5012bf1/hal-audit/` | Read-only |
| Research integration | `research/ulefone-gq5012bf1/` when assigned and uncontested | Read-only |

A takeover after usage limits is **not** a new ownership category. Read Git and working-tree state, preserve uncommitted edits and continue on the existing task. If writable boundaries collide, stop the conflicting write and request a scope decision rather than creating another workspace.

## 5. Adding files or changing ownership

Before writing a new permanent file:

1. Classify it: kernel code, module source, Android HAL, vendor/device configuration, app, recovery, tool, research, documentation, or generated artifact.
2. Choose an existing repository from Section 1 and an existing appropriate subtree.
3. Search that owner for an existing implementation or equivalent document.
4. Check for shared/mirrored files and concurrent agent edits.
5. Create only the necessary new files in the assigned owner; update source ownership/provenance when required.

If there is **no legitimate existing owner**, report the architectural gap and propose a destination. Do not silently create an extra repository, directory tree, or new remote.

## 6. Research and restricted paths

Phone-specific verified findings belong to:

- Local: `/home/armol/LieppOS/research/ulefone-gq5012bf1/`
- GitHub: `LieppOS/LieppOS-ulefone-gq5012bf1-research`

Research publication remains **held** pending a separate owner decision on inherited sensitive history and AFI calibration exposure. A concurrent agent must hand off findings within its existing owned repository rather than writing into another agent's active research checkout.

Default protected path: `/home/armol/androido_dalykai/` (including symlink-resolved descendants). Do not access without new specific owner authorization. Historical external workspaces are not additional canonical source owners and are read-only unless separately approved.

## 7. No rule-enforcement tooling

This map is a **human- and AI-readable ownership contract**, not a requirement to create directory scanners, commit hooks, AI policy-check scripts or CI workflows. The owner has not requested such automation.
