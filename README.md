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

Ten GitHub repositories are currently empty. Nine are Android platform projects
and are listed in `pending-projects.md`; the tenth is this manifest repository
itself. They are intentionally absent from `default.xml`.

The kernel source is staged locally in
`android/kernel/ulefone/gq5012bf1` from the verified C46 worktree, but its GitHub
repository has no pushed commit. It cannot be an active manifest project until
that history is published with owner approval.

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
