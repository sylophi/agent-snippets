---
name: finalize
description: Generalized instructions for creating a PR. Can be used when the work on a branch is done and ready to be cleaned up and shipped.
metadata:
  lichen-harnesses: claude
---

Test for edge cases, until you are confident.
Do /simplify, then /code-review, then /fast-deslop, then /easy-pr

If your cwd is not the worktree, pass the branch to /code-review, or the review may run in the wrong place.
Don't run /easy-pr if there is already a PR open for this branch.
