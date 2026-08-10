---
description: Commit, push, and open a PR with just a Description section
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
