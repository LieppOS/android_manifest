# LieppOS Android Manifest

**Status: local bootstrap. The GitHub `android_manifest` repository is empty and
this commit has not been pushed.**

This repository is the pinned supplemental manifest for LieppOS Android platform
projects. It is not a complete LineageOS or AOSP manifest. A full checkout must
start from an upstream LineageOS base manifest and layer the LieppOS projects on
top; that upstream revision has not been selected or validated here.

## Active projects

`default.xml` contains only projects with real, reachable commits:

| Project | Path | Pinned revision |
| --- | --- | --- |
| `android_device_ulefone_gq5012bf1` | `device/ulefone/gq5012bf1` | `58e7e2d14f42cdee5988cd335229fae796706004` |
| `android_vendor_lieppos` | `vendor/lieppos` | `3f0dce0e0c0f4ced71dd0339147ecb454c2fc290` |

Remote definition:

```xml
<remote name="lieppos" fetch="ssh://git@github.com:LieppOS" />
```

This uses SSH, not HTTPS. Project `name` values are repository names under the
`LieppOS` organization.

## C48 publication candidate

`publication-candidate.xml` is the exact intended configuration after owner
approval and publication. It is **not active** and must not be used until every
revision is reachable from GitHub.

| Project | Path | Candidate revision | Scope |
| --- | --- | --- | --- |
| `android_kernel_ulefone_gq5012bf1` | `kernel/ulefone/gq5012bf1` | `d0feaaee4484e2fdd5cd87ebbf4c9e273a50b8b0` | Sanitized core snapshot |
| `android_kernel_modules_mediatek_mt6878` | `kernel/modules/mediatek/mt6878` | `82b5cbcbf4c54da198c01fa04c9dc6e92319e3af` | Sanitized MediaTek mirror |
| `android_kernel_modules_ulefone_gq5012bf1` | `kernel/modules/ulefone/gq5012bf1` | `0e00dda7f53fedd8b83092aca5c3b96542bf255f` | Partial board mirror |

The kernel and Ulefone mirrors are **partial**. The proprietary FPSGO private
wrapper is excluded. Four Ulefone board drivers that embed vendor firmware, logo
or calibration images are withheld pending a redistribution review:
`focaltech_touch`, `hynitron`, `tiny_lcd` and `sh366003`. The candidate can
reproduce the core kernel, panel and `mtk-mmc`, but not a complete device build.

## Branch and commit policy

- Every active project is pinned by commit SHA. No active entry relies on an
  assumed `main` branch.
- `android_vendor_lieppos` currently has only the remote branch `working` at
  `3f0dce0e0c0f4ced71dd0339147ecb454c2fc290`. A local `gki` branch was created
  in the workspace at that same commit and is checked out, but it has not been
  pushed. The manifest pins the reachable commit rather than a nonexistent
  remote `gki` branch.
- `android_device_ulefone_gq5012bf1` uses remote `master`.
- Do not add an empty repository to `default.xml`. Add it to
  `pending-projects.md` until a real commit exists and the path is reviewed.

## Pending projects

The nine Android platform projects that are not active are listed in
`pending-projects.md`. The three kernel/module repositories have published
sanitized C48 commits on their remote `publication/c48` branches (verified with
`git ls-remote` in C49); they are not yet promoted into `default.xml`:

| Repository | Publication commit | State |
| --- | --- | --- |
| `android_kernel_ulefone_gq5012bf1` | `d0feaaee4484e2fdd5cd87ebbf4c9e273a50b8b0` | `PUBLISHED_BRANCH_NOT_IN_DEFAULT` |
| `android_kernel_modules_mediatek_mt6878` | `82b5cbcbf4c54da198c01fa04c9dc6e92319e3af` | `PUBLISHED_BRANCH_NOT_IN_DEFAULT` |
| `android_kernel_modules_ulefone_gq5012bf1` | `0e00dda7f53fedd8b83092aca5c3b96542bf255f` | `PUBLISHED_BRANCH_NOT_IN_DEFAULT_PARTIAL` |

[`local-development.xml`](local-development.xml) is the local ledger. It pins
the **local-only** C49/C50 kernel (`27a31fbc`) and MediaTek mirror (`cbc7ea9e`)
commits that add the MediaTek vendor module sources and the independently
written gen4m TKIP MIC. It is intentionally not included by `default.xml` and is
not a remotely syncable manifest.

The C47 source import was a snapshot with replayed C45/C46 patch commits. Its
history is not published because it contains a non-redistributable FPSGO private
wrapper and firmware/calibration-bearing board drivers. The C48 branches are new
root snapshots; no authoritative history was rewritten. See
[`C48-MANIFEST-PUBLICATION-PLAN.md`](../../docs/integration/C48-MANIFEST-PUBLICATION-PLAN.md).

## Using the manifest later

1. Select and pin the upstream LineageOS base manifest revision.
2. Confirm this manifest has been pushed and its `default.xml` commit is known.
3. Initialize a disposable build checkout outside this ownership workspace.
4. Use `repo init` with the upstream base, then layer the LieppOS manifest.
5. Run `repo sync` only for the selected projects, not an unrestricted full-tree
   sync during bring-up.
6. Record the manifest commit and every resolved project SHA in the build
   provenance.

No `repo init`, `repo sync`, full Android checkout, or ROM build has been run.

## Validation

`default.xml`, `local-development.xml` and `publication-candidate.xml` must parse
as XML. Every active project revision must be reachable from its remote. A local
check:

```sh
python3 - <<'PY'
import xml.etree.ElementTree as ET
for name in ('default.xml', 'local-development.xml', 'publication-candidate.xml'):
    ET.parse(name)
    print(name, 'XML parse: PASS')
PY
```

Do not invent branch names or revisions to make an empty repository appear
syncable.
