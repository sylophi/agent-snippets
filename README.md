# agent-snippets

A personal collection of instruction snippets for coding agents: skills and
slash commands.

## Skills

`skills/` holds skills usable by any agent that supports the format. Install
with [Vercel skills](https://github.com/vercel-labs/skills) (skills.sh). The
installer lets you pick which ones to include:

```sh
npx skills add https://github.com/sylophi/agent-snippets
```

Note: finalize invokes deslop and easy-pr, so install all three together.
stitch-images needs ImageMagick on the machine the agent runs on.

## Slash commands

Symlink entries from `commands/` into your agent's corresponding directory
(for Claude Code: `~/.claude/commands/`).
