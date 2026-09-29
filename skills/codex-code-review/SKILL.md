---
name: codex-code-review
description: Review a branch with the Codex CLI when asked for an independent code review.
---

Choose the review base from the branch's PR target or parent branch. Do not assume `main`.

Run the branch review from the repository with GPT-6.1 Sol at high reasoning effort:

```sh
codex exec review --base <chosen-base> -m gpt-6.1-sol -c 'model_reasoning_effort="high"'
```
