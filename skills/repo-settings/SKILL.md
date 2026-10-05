---
name: repo-settings
description: Apply preferred GitHub repo settings. Use when creating a GitHub repo or when asked to update an existing repo's settings.
---

Use these settings when creating a repo or when asked to update an existing repo:

```sh
gh repo edit OWNER/REPO \
  --enable-merge-commit=false \
  --enable-rebase-merge=false \
  --enable-squash-merge \
  --squash-merge-commit-message=pr-title-description \
  --delete-branch-on-merge
```
