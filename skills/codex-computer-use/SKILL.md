---
name: codex-computer-use
description: Delegate browser and desktop actions to Codex when Claude Code needs computer-use tools.
metadata:
  lichen-harnesses: claude
---

Run a computer-use task:

```sh
codex exec --dangerously-bypass-approvals-and-sandbox -C "$PWD" "Use computer use to <task>"
```

Use `-i <file>` to attach screenshots, `-o <file>` to save the result,
and `--json` to capture events and the session ID.

Continue a session:

```sh
codex exec resume --dangerously-bypass-approvals-and-sandbox <session-id> "<follow-up>"
```

See `codex exec --help` for other options. Requires computer-use tools
configured for the Codex CLI.
