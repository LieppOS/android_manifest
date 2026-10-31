# LieppOS AI Development Policy

**Version:** 1.2 (main-only owner workflow, 2026-10-31)

**Effective:** 2026-10-28  
**Applies to:** Every AI agent, regardless of provider or model, creating, fixing, reviewing, reverse-engineering, integrating, testing, or documenting LieppOS software.  
**Canonical location:** `/home/armol/LieppOS-workspace/LieppOS_kernel/manifests/android_manifest/AI-POLICY.md`
**Companion:** [REPO-OWNERSHIP.md](REPO-OWNERSHIP.md) and the canonical workspace-root `AGENTS.md`.

## 1. Purpose and authority

LieppOS is developed primarily with AI assistance. AI agents may exercise engineering judgment **inside an explicitly assigned task and repository scope**. They must preserve project history, produce real working code, document evidence honestly, and protect the owner's phone and private information.

These are project rules, not permission to ignore higher-priority operating or safety requirements. A project-owner exception must identify the **path, operation, scope, and risk**. An exception for one action does not authorize unrelated actions. A previous campaign's approval does not automatically apply to a new one.

The **current on-disk evidence** and actual Git state take precedence over assumptions in an old prompt. Do not assume a task is unfinished just because its previous AI hit a usage limit.

## 2. Workspace structure and repository ownership

- Canonical workspace: `/home/armol/LieppOS-workspace/LieppOS_kernel/`. The old `/home/armol/LieppOS` path is obsolete; do not recreate it or aliases.
- Follow [REPO-OWNERSHIP.md](REPO-OWNERSHIP.md) for existing repository locations and roles.
- Before creating or editing a permanent file, identify its **single repository owner** and the assigned task scope.
- Put source in the source-owning repository; device research in the research repository; tools in the existing tools repository; build outputs in the task's existing designated output area.
- Never create arbitrary top-level folders, another full Android/kernel tree, extra GitHub repositories, nested Git projects, or redundant copies merely for convenience.
- Create a new directory **inside an owned repository** only when required by actual files and the existing project structure; do not create placeholder scaffolding.
- Preserve current file placement and public APIs unless change is necessary. Prefer incremental integration to wholesale reorganizations.
- When source is mirrored between the main kernel and module repositories, edit the canonical owner and synchronize through the documented process; never independently diverge equivalent copies. A build manifest must never checkout multiple projects over the same destination.

**Out-of-scope paths:** The directory `/home/armol/androido_dalykai/` and any path resolving inside it are forbidden by default. Do not traverse, search, stat, read, write, or follow symlinks into it. Access requires new, explicit, narrowly scoped authorization from the owner. External donor workspaces are read-only unless separately approved; do not alter their Git metadata or build outputs.

## 3. Initial inspection is mandatory

Before editing, establish:

1. The requested outcome and repository/paths authorized for modification.
2. Current Git branch, HEAD, remotes, staged/unstaged/untracked changes, and existing local commits (avoid unnecessary metadata writes in other agents' checkouts).
3. Relevant implementations, prior fixes, tests, documentation, and source provenance.
4. Whether another agent owns or is editing the same files/worktree.
5. Required build/test tooling, dependency and ABI boundaries, and available disk/resources.

Do not reimplement completed work, replace a driver merely because it was not found in a published mirror, or mistake a faulty Git pathspec for absent source. Preserve all unexpected existing changes and state any real blocker rather than destroying them.

## 4. Implementation quality

- Deliver correct, maintainable, minimal, reviewable changes; use the project's existing build and naming conventions.
- **No fake implementations:** no invented device registers, fabricated HALs, dummy kernel modules, blanket error suppression, bogus data, or placeholder headers presented as finished work.
- Do not silence Kbuild/modpost errors, relax SELinux, disable KCFI/CFI or AVB checks, manipulate verification outputs, or weaken device protections merely to pass a test.
- Treat missing dependencies as an evidence problem; inspect authentic source and include paths rather than inventing types or symbols.
- Keep production code and exploratory investigation separate. Explicitly mark partial/experimental work.
- No unrelated refactoring, mass renaming, formatting churn, duplicated helper frameworks, or speculative functionality during a focused fix.
- Verify the intended behavior and check regressions. Test failures are outcomes to report, not obstacles to hide.
- Do not claim source parity, successful builds, runtime function, stock compatibility, or reproducibility without direct supporting checks.

### Kernel and hardware-specific expectations

For relevant changes, preserve or explicitly validate GKI/KMI, symbol CRCs, module dependencies, vermagic, KCFI/CFI, kernel configuration, DT bindings, boot-stage load order, SELinux contracts, AVB chain, and the stock-anchor-slot safety model. Never assume that a clean compile proves that a module can initialize on GQ5012BF1.

## 5. Source provenance and publication

Maintain traceable origin for each imported or substantially reconstructed source group:

- **UPSTREAM_ORIGINAL:** direct upstream release (URL, commit, license).
- **UPSTREAM_MODIFIED:** upstream code plus documented LieppOS patches.
- **REVERSE_ENGINEERED:** source reconstructed from observed behavior or binary analysis; retain methodological evidence and legal review status.
- **INDEPENDENT:** independently authored source; record owner/author attribution and license decision.
- **UNKNOWN_ORIGIN:** cannot be published as confidently attributable source until resolved.

These labels describe *origin*, not performance or legal permission. An implementation can exist and build yet still be **publication-held**. Source availability, redistribution permission, compile status, ABI verification, and hardware functionality are separate facts.

Preserve applicable copyright notices, attribution, and license terms. Audit imported reachable Git history as well as the working tree before publication. Do not introduce secrets, private PEM files, calibration/NVRAM/NVDATA, personal identifiers, restricted vendor firmware, unauthorized blobs, or keys into public repositories. Never copy sensitive payloads just to prove they exist; use safe metadata and hashes where appropriate.

Published code must accurately disclose its proprietary userspace, firmware, TEE, modem, camera-tuning, and other remaining dependencies. **Do not call the entire hardware stack open source because kernel driver source is available.**

## 6. Git discipline and owner approvals

### Allowed within assigned scope

- Inspect source and Git history.
- Create/edit code, tests, and concise documentation.
- Work and create reviewed commits on `main` in the existing canonical checkout. Every LieppOS repository uses `main` as its sole working/default branch; do not create task branches, extra worktrees or clones without a fresh explicit owner exception.
- Run offline validation/builds in authorized paths, consistent with resource constraints.

### Standing authorization: commit, then push reviewed task work

The owner authorizes ordinary `git commit` followed immediately by a non-forced `git push` to `origin/main` for each completed, validated, publishable logical change in an existing LieppOS repository. `main` is explicitly assigned for normal development across all LieppOS repositories and is their GitHub default. Do not ask for separate routine commit/push approval and do not create a task branch instead.

Before committing, verify exclusive write ownership, `main`/HEAD, working/staged changes, existing fetch/push URLs and the entire outgoing range. Run relevant validation; review staged changes and reachable unpublished history for secrets, identifiers, calibration, firmware/blobs, licensing and publication holds. Stage only assigned files. Push only reviewed `main` with an explicit normal refspec, verify its remote SHA and report its URL. Do not blindly merge old experiments, rewrite history, push tags or interfere with another agent's work. Branch protection still applies; never bypass it.

If review, provenance, publication restrictions, authentication, network, remote divergence or branch protection blocks a push, retain the local commit and report **NOT PUSHED** with the specific reason. Never force-push, bypass checks, silently change remotes/visibility, or publish unsafe material to satisfy the routine workflow. The research hold in section 9 remains binding; the owner explicitly chose to retain it while publishing safe source work.

### Requires separate explicit scoped owner approval

- Pushes outside the standing authorization above, remote merges, tags/releases, repository settings changes, or publication of previously restricted material.
- Force-push, branch deletion, destructive reset/clean, history rewriting or discarding existing work.
- Changing repository visibility, permissions, remotes, or default branches.
- Deleting/reorganizing another project's files, moving canonical ownership, or creating new GitHub repositories.
- Phone access or device-side actions (including read-only ADB when not already authorized), flashing, switching slots, rebooting, recovery, bootloader, AVB or partition writes.
- Publication of material with unsettled copyright, licensing, privacy, firmware or calibration rights.

**Every permitted push must be exact:** determine the repository, verified remote, destination ref, full proposed commit and expected prior remote state before executing it. For routine pushes the standing authorization above supplies permission; extraordinary operations still require separate scoped owner approval. Force is **NO** unless separately and unmistakably authorized. Recheck the remote before push and verify the resulting SHA afterward. A prepared extraordinary-operation approval packet is not itself approval.

Do not blindly commit every pending change. Review completed work and integrate it on `main`; preserve unfinished work and uncertain deletions. Do not use task branches as a workaround. The owner's 2026-10-31 migration authorization permits local non-branch archival refs, removal of obsolete local/remote branch names and archival realignment of obsolete local mirror `main` refs; it does not permit force-pushes or publishing the archival refs. Routine destructive reset/clean, history rewriting, archive deletion and broad cleanup remain unauthorized. Never use `git push --mirror`.

## 7. Multiple AI agents and model handoff

- Ownership belongs to a **task, repository and path**, never permanently to DeepSeek, Claude, or Codex.
- Keep one writer per repository on canonical `main`. Parallel agents may write different repositories, not independent branches/worktrees of the same repository.
- Another agent's assigned tree is read-only until that agent finishes or pauses and ownership is coordinated. Do not change its branch, index, files, build output, locks or commits without an authorized takeover.
- Before taking over an interrupted task, inspect the actual repository state; continue from its existing partial work and commits rather than restarting.
- If paths overlap, stop the conflicting write; continue safe read-only analysis or prepare a patch/handoff in already approved ownership. Do not create a random workspace as a workaround.
- An agent that becomes unavailable must leave recoverable local commits or unchanged uncommitted work and a concise status report.

A handoff must include: objective, exact repository paths and branches, starting/current commits, uncommitted changes, files changed, tests and evidence, incomplete items, next concrete command/action, authorization boundaries, and whether the research findings have actually been integrated. Do not rely on hidden chat reasoning or fabricated state.

## 8. Tests, evidence and definitions

Choose **relevant** checks, not an automatic giant test suite:

- Simple edits: targeted checks, format/parse checks where relevant, and diff review.
- Kernel modules: real build/modpost, required dependency/ABI checks and a comparison against the stated baseline.
- Android integration: actual parsers/build checks, VINTF/SELinux/init checks when applicable.
- Publication: license/provenance/sensitive-content audit and remote-clone reproducibility where claimed.
- Hardware: only report hardware behavior supported by an explicitly authorized, captured device test.

Use precise result labels:

| Label | Meaning |
|---|---|
| `SOURCE_IDENTIFIED` | Relevant source is located; no build implied. |
| `SOURCE_IMPORTED` | Source is present in the assigned source tree; no build implied. |
| `BUILD_VERIFIED` | A recorded build succeeded for exact inputs. |
| `ABI_VERIFIED_OFFLINE` | Specified ABI tests passed; runtime not implied. |
| `BIT_IDENTICAL` | Exact byte/hash comparison passed for named artifacts. |
| `HARDWARE_VERIFIED` | A documented device test passed for that exact image/configuration. |
| `PARTIAL` / `BLOCKED` / `UNKNOWN` | Missing proof or dependency is stated explicitly. |

Do not equate `DONE` in a reverse-engineering folder with complete ROM support. A compile does not prove hardware operation. A matching function body does not prove whole-ELF parity unless the defined gate passes. Do not silently edit expected hashes or bypass negative tests.

## 9. Research, documentation and inventory

All newly **verified** information about Ulefone Armor 29 Pro Thermal / GQ5012BF1 must enter the canonical research repository:

`/home/armol/LieppOS-workspace/LieppOS_kernel/research/ulefone-gq5012bf1`
Remote: `LieppOS/LieppOS-ulefone-gq5012bf1-research`

Keep the publication hold until inherited sensitive history and AFI calibration exposure have a separately approved resolution. **Do not push held research.** Latest held work uses local `main`; public `main` retains only already-approved public history. Report this intentional local/public difference; never force-push or publish unsafe material to make the SHAs agree.

If another agent owns the research checkout, create only a concise research-ready handoff **inside your already assigned repository**, clearly mark it as not yet integrated, and identify where it should be applied. Do not create another research clone or overwrite a concurrent branch.

Keep one authoritative record per fact/artifact. Prefer updating an existing report to creating numbered copies. Source commits belong to source repositories; research findings, reproducible evidence and source maps belong to research. Maintain the canonical workspace's `inventory/subsystems.csv` only when assigned authority to edit it. Distinguish observations, inferences, hypotheses, and unresolved questions. Avoid documenting private absolute paths when relative paths suffice.

## 10. Workspace cleanliness and storage

- Every permanent file must be necessary and owned by an existing repository.
- Ignore generated outputs such as object files, module binaries, caches, `.pyc`, `.venv`, compiler temporary files and local build directories unless an artifact is specifically approved for versioning.
- Do not commit bulky evidence dumps or binary stock images to source repositories.
- Reuse already-designated staging/build locations. Do not leave experimental clones, orphan scripts, stray logs, nested `.git` directories, or redundant test fixtures in the project.
- Delete only disposable files **created by your own task** after confirming they are not needed for evidence or resumption; do not delete another agent's output even if it looks temporary.
- No wholesale cleanup, filesystem deduplication, permission changes or repository reorganization without separate authorization.
- If disk/resource limits threaten correctness, preserve existing work and report the blocker; do not start an uncontrolled heavyweight build.

## 11. Completion contract

A task is complete only when:

1. The requested implementation or explicitly bounded partial result exists in the proper repository.
2. Relevant actual tests/checks have been run and their outcomes are recorded honestly.
3. Original unrelated work and protected paths are unchanged.
4. The repository is not polluted by task-generated junk; unexpected dirty files are disclosed, never destroyed.
5. The appropriate research has been recorded or a clearly marked, unmerged handoff is provided because of active ownership.
6. Completed, validated, publishable changes are committed and pushed under section 6's standing authorization; blocked pushes remain recoverable locally and are explicitly reported.
7. The final report lists changed paths, tests, known limitations, remaining blockers, exact local/remote SHAs, push status and destination branch/URL, and next action.

A partial result with precise evidence is preferable to an invented success. No fixed number of reports, folders, commits or test cases is required. Small changes should stay small.

## 12. Deliberately **not** included

This policy does **not** establish a new automated AI-rule scanner, `lieppos-ai-check` command, pre-commit policy hook, mandatory policy CI, or GitHub Actions workflow. Do not create those without a future explicit owner request. Existing functional tests, ordinary build validation and human/AI review remain necessary.
