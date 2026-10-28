# LieppOS Android Manifest

**Status: local bootstrap. The GitHub `android_manifest` repository is empty and
this commit has not been pushed.**

This repository is the pinned supplemental manifest for LieppOS Android platform
projects. It is not a complete LineageOS or AOSP manifest. A full checkout must
start from an upstream LineageOS base manifest and layer the LieppOS projects on
top; that upstream revision has not been selected or validated here.

## Active projects

`default.xml` contains only projects with real commits:

| Project | Path | Pinned revision |
| --- | --- | --- |
| `android_device_ulefone_gq5012bf1` | `device/ulefone/gq5012bf1` | `58e7e2d14f42cdee5988cd335229fae796706004` |
| `android_vendor_lieppos` | `vendor/lieppos` | `3f0dce0e0c0f4ced71dd0339147ecb454c2fc290` |

Remote definition:

```xml
<remote name="lieppos" fetch="ssh://git@github.com/LieppOS" />
```

This uses SSH, not HTTPS. Project `name` values are repository names under the
`LieppOS` organization.

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
`pending-projects.md`. Seven remain empty on GitHub. Three now have local
commits but are still not active because their remotes are empty and the
commits are unpushed:

| Repository | Local commit | State |
| --- | --- | --- |
| `android_kernel_ulefone_gq5012bf1` | `c3bd65dcf7e2bf437f01783e120c8e7477cd788a` | `LOCAL_COMMIT_UNPUSHED` |
| `android_kernel_modules_mediatek_mt6878` | `83adb6057cb20d71ab3cdac100c37910e70f5593` | `LOCAL_COMMIT_UNPUSHED` |
| `android_kernel_modules_ulefone_gq5012bf1` | `5fd5a2758d525b55bae4e354b32af4ce6c8c23e7` | `LOCAL_COMMIT_UNPUSHED` |

[`local-development.xml`](local-development.xml) is a local ledger for those
commits. It is intentionally not included by `default.xml` and is not a
remotely syncable manifest.

The C47 kernel import is a source snapshot with replayed C45/C46 patch commits;
both private-key PEM files were excluded. It cannot be an active manifest
project until it is reviewed, published with owner approval, and the pinned
commit is verified reachable from GitHub. See
[`C47-RESULTS.md`](../../docs/integration/C47-RESULTS.md).

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

`default.xml` must parse as XML. Every active project revision must be reachable
from its remote. A local check:

```sh
python3 - <<'PY'
import xml.etree.ElementTree as ET
ET.parse('default.xml')
print('XML parse: PASS')
PY
```

Do not invent branch names or revisions to make an empty repository appear
syncable.
