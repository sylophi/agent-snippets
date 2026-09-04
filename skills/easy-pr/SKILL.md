---
name: easy-pr
description: Commit, push, and open a PR whose body is just a brief Description section. Use when asked to open a quick or easy PR without a test plan or other boilerplate sections.
---

Create a pull request for the current branch. Follow these steps exactly:

1. Run `git status` and `git diff` (including untracked files) to understand all changes.
2. Stage and commit all changes with a concise commit message describing what changed.
3. Check the current branch name. If it looks like a random/nonsense animal name, rename it to something short and descriptive that fits the actual changes (e.g. "fix-stale-session-cleanup"). Use `git branch -m <new-name>`.
4. Push the branch to the remote with `git push -u origin HEAD`.
5. Create the PR using `gh pr create`. The PR body should only contain a brief, simple, concise `## Description` section. No test plan, keep things high level.

Do NOT add any of the following to the PR body:
- Test plan or testing steps
- Co-authored-by lines
- Generated-by lines
- Any other sections beyond the summary

If you have any images of the final result, you may add them to the PR. Do not add too many.

## Title Examples

=== Bad ===
`Add the sm-teardown skill for removing a worktree without merging`

=== Good ===
`Add sm-teardown skill`


## Description Examples

=== Bad ===
```markdown
## Description

Deleting a branch from Manage Branches ran `git branch -D` unconditionally, so unmerged work could be discarded with no warning. The delete now tries git's safe `-d` first. When git refuses because the branch has unmerged commits, the confirm dialog swaps into a force-delete stage that explains what will be lost (and why squash-merged branches trip the check even though their changes landed) before running `-D`.

Supporting changes:
- `git` is spawned with `LC_ALL=C` so the stderr detection is locale-stable.
- The not-merged refusal crosses IPC as a shared marker error, following the `isEntityGoneError` pattern.
- New `branch-delete-states` seed fixture covering merged, unmerged, squash-merged, and checked-out branches, plus a `--only=<names>` seed flag for recreating single fixtures inside an existing seed tree.
```

=== Good ===
```markdown
## Description

Branch deletion in Manage Branches ran `git branch -D` unconditionally, so unmerged work could be lost without warning. It now tries the safe `-d` first. If git refuses, the confirm dialog swaps into a force-delete stage that explains what will be lost before running `-D`.

Also adds a `branch-delete-states` seed fixture and an `--only` flag for reseeding single fixtures.
```

=== Bad ===
```markdown
## Description

Adding a project through the app (direct path entry, the native folder picker, or a scan) now lands on that project's primary checkout instead of staying wherever the app happened to be. Bulk scan adds navigate to the first project that was added, and a bare repo with no checkout falls back to the new-worktree page. Also extracts a shared `worktreesQueryOptions` so the worktree-list query is defined once, and makes `useAddProject` await the projects-list refresh so the destination route can't flash "Worktree not found".
```

=== Good ===
```markdown
## Description

Adding a project through the app now selects that project's primary checkout instead of staying on the current view. Bulk scan adds land on the first added project, and a bare repo falls back to the new-worktree page.
```

=== Bad ===
```markdown
## Description

`sm land` fast-forwarded the primary checkout to the repo default branch after every merge, with no idea what the PR actually merged into. Landing a PR based on a long-lived branch pulled unrelated commits into main and then reported `primary checkout caught up (origin/main)`, plus `primaryCaughtUp: true` in the JSON, even though the primary branch did not have the landed work.

`prSummary` now carries `baseRefName`, and the catch-up is skipped with a reason when the base is not the primary branch, so land reports `skipped primary catch-up: PR merged into v2, not the primary branch main`. An unreadable base keeps the old behavior, since the default base is the common case.
```

=== Good ===
```markdown
## Description

`sm land` always pulled the primary branch after merging a PR, even when the PR merged somewhere else. Landing a PR based on a long-lived branch pulled unrelated commits into main and then reported "primary checkout caught up", although main did not have the landed work.

It now checks which branch the PR merged into, and skips the pull with a reason when that is not the primary branch.
```

=== Bad ===
```markdown
## Description

`catchUpPrimary` returned early with `skipped primary catch-up: PR merged into v2, not the primary branch main` whenever `pr.BaseRefName != pt.localPrimary`, so the `v2` worktree was never pulled and an agent ran `git pull --ff-only origin v2` by hand, which fast-forwarded `main` in the primary checkout because `--ff-only` only forbids merge commits.

`catchUpPrimary` is replaced by `catchUpBase`, which resolves `cmp.Or(prBase, pt.localPrimary)`, finds the checkout via the new `checkoutOn` helper in `context.go`, and calls `ffPull(path, pt.remote, base)`. `ffPull` now reads `git symbolic-ref --short HEAD` and returns an error when it is not `base`. `primaryTarget` gains a `remote` field, the JSON document replaces `primaryCaughtUp` with a `caughtUp` object and `catchUpSkipped`, the `--help` text is updated, and `TestCatchUpBasePullsTheBaseBranchCheckout` covers the v2 pull, the refused pull on main, the unchecked-out base, and the empty-base fallback.
```

=== Good ===
```markdown
## Description

`sm land` skipped its catch-up when a PR merged into a branch other than the primary one. That left the base branch's worktree stale and invited a by-hand fast-forward, which once moved main onto v2 in the primary checkout.

It now pulls whichever checkout has the PR's base branch out, and the pull refuses unless that checkout really is on the branch being advanced.
```
