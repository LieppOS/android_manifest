# LieppOS Android Manifest

## Main-only development

The owner-directed workflow is **`main`, and only `main`, across all 20 existing
LieppOS repositories**. It is their working and GitHub default branch. Work in
existing canonical checkouts; do not create task branches or extra worktrees.
Keep one writer per repository. See [AI-POLICY.md](AI-POLICY.md),
[REPO-OWNERSHIP.md](REPO-OWNERSHIP.md) and the canonical workspace `AGENTS.md`.

The workspace is `/home/armol/LieppOS-workspace/LieppOS_kernel/`. Old branch tips
are preserved in each existing repository under local, non-branch archival refs:

```sh
git for-each-ref refs/archive/main-only-20261031/
```

These are recovery records, not development branches. Do not push them or run
`git push --mirror`. Historical extra worktrees are recovery material, not places
to resume development. Pending/private work must not be discarded to tidy Git.

## Active supplemental manifest

This is **not a complete LineageOS/AOSP platform manifest**. `default.xml` keeps
its existing two active project destinations, following their published `main`
branches rather than stale campaign snapshots:

| Project | Android path | Revision |
| --- | --- | --- |
| `android_device_ulefone_gq5012bf1` | `device/ulefone/gq5012bf1` | `main` |
| `android_vendor_lieppos` | `vendor/lieppos` | `main` |

```xml
<remote name="lieppos" fetch="ssh://git@github.com/LieppOS" />
```

Changing branch conventions does not activate additional Android projects or
establish a successful combined ROM build. Select an upstream platform manifest
and record every resolved project SHA when performing an actual reproducible
build. Do not sync an unrestricted platform tree merely to validate these edits.

## Historical inputs and incomplete projects

- `publication-candidate.xml` remains **inactive**. Its C48 commit pins are kept
  for historical reproduction; `upstream="main"` names their published lineage.
- `local-development.xml` remains an **inactive historical C49–C54 input ledger**.
  Its old publication-status commentary describes that campaign's state, not
  the current main-only repository layout.
- [pending-projects.md](pending-projects.md) records historical integration and
  implementation limits. Having a `main` branch is not proof of implementation.
  The empty MediaTek vendor and recovery repositories received initialization-only
  commits; no fake source or placeholder implementation was added. They remain
  excluded from the active manifest.
- Private vendor payloads, stock-derived camera tables, firmware and calibration
  remain subject to their existing publication restrictions. Available kernel
  source does not make the full hardware stack open source or self-contained.
- Latest held research stays on **local `main`**. Public research `main` retains
  only previously public history; the owner explicitly kept the publication hold.
  Do not upload held data or force-push to conceal this intentional difference.

## Validation boundary

Manifest XML must parse, paths must remain disjoint, active `main` refs must
exist remotely, and historical pins must remain reachable from their appropriate
published `main` lineages. Git migration does not authorize ADB, flashing,
installation, power changes or other device operations. Host/source checks are
not a fresh full Android/kernel build or hardware validation.
