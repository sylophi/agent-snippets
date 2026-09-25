---
name: spaghetti-wall
description: Used when the user asks to try out experiments.
disable-model-invocation: true
---

Make worktrees with `/sm-new-worktree`, then send a subagent down each trying out/experimenting with new features or changes that you think might be helpful for the project.

When done, provide the user with proof (images, diagrams, whatever you feel is appropriate).

Rename the branches with `/sm-rename-branch` as soon as you can. The branch names should be prefixed `exp/`.

If the user did not specify the number of worktrees, choose as many as you want.

Do not ask the user what they want to see. The point is to try out different things and see what works.

Note that there may already be existing experiments (branches/worktrees prefixed with `exp/`). Ensure that your experiments do not overlap conceptually.
