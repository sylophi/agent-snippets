---
name: react-compiler-lint
description: Set up React Compiler lint enforcement — ban manual memoization imports (useMemo, useCallback, memo) and optionally run compiler diagnostics via oxlint jsPlugins. Use when setting up a new React project, adding oxlint/ESLint config, or when asked to "ban memoization imports" / "add the react compiler lint rule".
disable-model-invocation: true
---

# React Compiler lint setup

**Why this exists:** coding agents love to add `useMemo`/`useCallback`/`memo`
reflexively, even when React Compiler makes them redundant. Layer 1 makes
that a hard lint error. Layer 2 runs the compiler's own diagnostics so the
automatic memoization actually applies.

## When to apply

Only in projects that actually use React Compiler. Check for one of:
`babel-plugin-react-compiler` in dependencies, `reactCompiler: true` in
`next.config.*`, or `reactCompilerPreset` in a Vite config.

## Layer 1 — ban manual memoization imports

The compiler memoizes automatically; manual `useMemo`/`useCallback`/`memo`
is noise and can defeat compiler output. Enforced with the core
`no-restricted-imports` rule — no plugin or dependency needed.

Merge the rule from
<https://raw.githubusercontent.com/sylophi/agent-snippets/main/skills/react-compiler-lint/rule.json>
into the `rules` object of `.oxlintrc.json`. If a `no-restricted-imports` entry already exists, append
to its `paths` array instead of replacing it:

```json
"no-restricted-imports": [
  "error",
  {
    "paths": [
      {
        "name": "react",
        "importNames": ["useMemo", "useCallback", "memo"],
        "message": "React Compiler memoizes automatically. Remove the import. If you have a demonstrated bailout, opt out with `// oxlint-disable-next-line no-restricted-imports -- <reason>`."
      }
    ]
  }
]
```

### Escape hatch convention

Only for a *demonstrated* compiler bailout (verified in React DevTools or the
compiler playground), per line, with a reason:

```ts
// oxlint-disable-next-line no-restricted-imports -- compiler bails out on <X>, verified 2026-07
import { useMemo } from "react";
```

Never disable the rule file-wide or globally.

### Known gap

Only named imports are caught. `import * as React` + `React.useMemo(...)`
slips through (oxlint has no `no-restricted-syntax`). Don't write namespace
access to dodge the rule.

## Layer 2 — compiler diagnostics

Layer 1 stops manual memoization; it doesn't verify the compiler can
actually optimize your components. oxlint's `jsPlugins` (alpha, but working
as of oxlint 1.71+) can run the real `eslint-plugin-react-hooks` compiler
diagnostics: refs read during render, mutated props, setState in effects,
and other patterns that make the compiler bail out.

1. Install the plugin: `bun add -d eslint-plugin-react-hooks` (or
   `pnpm add -D`). Needs v6+; the compiler rules ship split (there is no
   single `react-compiler` rule in v7).
2. Add to `.oxlintrc.json`:

```json
"jsPlugins": [
  { "name": "react-hooks-js", "specifier": "eslint-plugin-react-hooks" }
]
```

3. Merge the `rules` from
   <https://raw.githubusercontent.com/sylophi/agent-snippets/main/skills/react-compiler-lint/diagnostics.json>
   into the config's `rules` object. They mirror the compiler-diagnostic subset of
   the plugin's `recommended-latest` preset. `rules-of-hooks` and
   `exhaustive-deps` are deliberately excluded — use oxlint's native
   equivalents if wanted; the JS versions are slow.

Notes:

- JS plugin rules run in Node: expect roughly ~1s added to lint runs (small
  project baseline 0.1s → 1.2s). Fine for CI/pre-commit; measure on big repos.
- `preserve-manual-memoization` is kept even though Layer 1 bans the imports:
  it validates the escape-hatch cases that opted out.
- Same convention for opt-outs:
  `// oxlint-disable-next-line react-hooks-js/<rule> -- <reason>`.

## ESLint equivalent

Layer 1's options object works in ESLint under the same rule name
(`no-restricted-imports`). Layer 2 is just
`eslint-plugin-react-hooks`' `recommended-latest` preset. Escape hatches use
`// eslint-disable-next-line ... -- <reason>`.

## Enforcement

Make sure lint runs somewhere enforcing (CI or pre-commit), not just the
editor.
