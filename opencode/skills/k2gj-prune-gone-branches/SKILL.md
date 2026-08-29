---
name: k2gj-prune-gone-branches
description:
  Identifies and safely removes local Git branches whose configured remote
  upstream no longer exists. Use when cleaning up local branches after remote
  branches have been deleted, while preserving branches with valid upstreams and
  branches that Git refuses to delete safely.
---

# K2GJ Prune Gone Branches

## Objective

Clean up local Git branches whose configured remote upstream has been deleted.

The skill refreshes remote references, identifies local branches reported by Git
as having a `[gone]` upstream, and removes those branches using Git's safe
branch deletion mechanism.

## Inputs & Parameters

The skill requires:

- A valid Git repository as the working directory.
- Permission to access the configured Git remotes.

No branch names, remote names, or additional parameters are required for the
standard cleanup operation.

## Rules & Scope

- Operate only within a valid Git repository.
- Refresh remote references before identifying obsolete branches.
- Treat only branches reported by Git with `[gone]` as cleanup candidates.
- Do not remove branches whose configured upstream still exists.
- Do not remove branches solely because they appear old, inactive, merged, or
  unused.
- Use `git branch -d` for deletion.
- Never replace `git branch -d` with `git branch -D` automatically.
- Do not create, rename, modify, or push branches.
- Do not intentionally modify project files.
- Limit the operation to remote-reference cleanup and deletion of eligible local
  branches.

## Workflow

### 1. Refresh Remote References

Run:

```bash
git fetch --prune
```

This updates remote-tracking references and removes local references to remote
branches that no longer exist.

If this command fails, stop the workflow before attempting any branch deletion.

### 2. Identify Gone Branches

Run:

```bash
git branch -vv | awk '/: gone]/{print $1}'
```

Use the resulting branch names as the candidates for local cleanup.

The `[gone]` marker is the authoritative criterion for determining that the
configured upstream reference is no longer available.

### 3. Delete Eligible Branches

Delete the identified branches using:

```bash
git branch -d
```

The complete cleanup operation is:

```bash
git fetch --prune && git branch -vv | awk '/: gone]/{print $1}' | xargs -r git branch -d
```

The `xargs -r` option prevents `git branch -d` from being invoked when no
`[gone]` branches are found.

## Skill-Specific Sections

### Remote Reference Pruning

Run `git fetch --prune` before inspecting local branch tracking information.

This ensures that stale remote-tracking references are removed before Git
determines which configured upstreams are no longer available.

### Gone Upstream Detection

Use the `[gone]` status reported by `git branch -vv` as the selection criterion.

Do not infer that a branch is obsolete from its name, commit age, merge status,
or apparent inactivity. A branch is a cleanup candidate only when its configured
upstream is reported as gone.

### Safe Branch Deletion

Use `git branch -d` rather than `git branch -D`.

The safe deletion mode allows Git to reject deletion when the branch contains
commits that have not been sufficiently merged. A rejected deletion must not be
overridden automatically.

### Empty Candidate Set

If no branches are reported as `[gone]`, treat the operation as successfully
completed.

Do not invoke branch deletion when there are no candidates.

## Evidence & Analysis

Use Git's command output as the primary evidence for all cleanup decisions.

- `git fetch --prune` establishes the current state of remote-tracking
  references.
- `git branch -vv` establishes the tracking relationship and `[gone]` status of
  local branches.
- `git branch -d` determines whether each candidate can be safely deleted.

Do not use assumptions or external criteria to classify a branch as obsolete.

If Git reports conflicting or unexpected information, preserve the affected
branch rather than making a destructive assumption.

## Output Format

Return a concise execution summary containing:

- Branches successfully removed.
- Branches that Git refused to delete.
- A statement that no `[gone]` branches were found when applicable.
- Any errors encountered during the workflow.

Do not report a branch as removed unless Git confirms its deletion.

## Failure & Fallback

- If the working directory is not a Git repository, stop and report the problem.
- If `git fetch --prune` fails, stop without deleting branches.
- If branch detection fails, do not construct an alternative deletion list
  automatically.
- If `git branch -d` rejects a branch, leave it unchanged and report the
  rejection.
- Never use `git branch -D` as an automatic fallback.
- If no `[gone]` branches exist, finish successfully without performing
  deletions.

## Final Verification

Before considering the task complete:

- Confirm that `git fetch --prune` completed successfully.
- Confirm which local branches were identified as `[gone]`.
- Confirm which candidate branches were actually deleted.
- Confirm that branches rejected by `git branch -d` remain intact.
- Ensure no branch was deleted using `git branch -D`.
- Ensure the final summary does not claim deletion of an unconfirmed branch.
- Ensure no branch outside the `[gone]` candidate set was intentionally removed.
