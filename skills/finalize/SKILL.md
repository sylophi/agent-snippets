---
name: finalize
description: Review the branch and make the PR. Runs /simplify, /code-review, /fast-deslop, and /easy-pr in order. Use when the work on a branch is done and ready to be cleaned up and shipped.
---

Do /simplify, then /code-review, then /fast-deslop, then /easy-pr

If your cwd is not the worktree, pass the branch to /code-review, or the review may run in the wrong place.
Don't run /easy-pr if there is already a PR open for this branch.
