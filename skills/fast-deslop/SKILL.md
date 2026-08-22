---
name: fast-deslop
description: Use when asked to quickly strip AI-sounding punctuation from code, docs, commits, or PRs.
---

Write like a normal human would. Keep em dashes and other telltale punctuation out of committed artifacts and clean up what this session already wrote. 

The characters below are the usual offenders, either because they are not on
a normal keyboard or because nobody reaches for them when typing casually.
For the remainder of this session, keep them out of the artifacts you
produce:

- em dash `—`
- en dash `–`
- middle dot / interpunct `·`
- bullet `•` used inline in a sentence
- semicolon `;` used in prose

**Scope: artifacts only.** This covers anything that outlives the
conversation. Code and code comments, UI copy and other user-facing strings,
documentation and README prose, config and schema comments, commit messages,
and PR titles and bodies.

**Not your own voice.** Your chat responses to the user and your thinking are
explicitly out of scope. Write those however you normally would, em dashes
included. The rule is about what gets committed, not about how you talk.

Do not simply swap in a hyphen or ellipsis. Rewrite the sentence so it does
not need the character in the first place. Use a period and a new sentence, a
colon, a comma, or parentheses, whichever the sentence actually calls for. A
sentence that leans on an em dash or a semicolon is usually two sentences.

The semicolon ban is a prose rule, not a syntax rule. Semicolons that are
part of a programming language's grammar, a CSS declaration, an HTML entity
like `&amp;`, or any other machine-read syntax are untouchable. The ban
applies only where a semicolon joins two clauses of human-readable text:
comments, docs, commit messages, UI copy, PR bodies.

Then clean up the artifacts already written in this session:

1. Run `git diff HEAD` and `git diff main...HEAD` (or against the upstream
   branch) to find the lines this session added.
2. Search those added lines for the characters above and rewrite each one.
3. If a PR has already been opened from this work, check its title and body
   too and update them with `gh pr edit`.
4. Run whatever checks the repo uses (typecheck, lint, format) and commit the
   cleanup.

Two exceptions. Do not touch lines that already existed before this session's
work, even if they contain these characters: that is someone else's prose and
it is not yours to rewrite. And do not touch a character that is load-bearing
data rather than prose, such as a test fixture, a parser case, a snapshot, or
a string that is deliberately exercising the character. When you are unsure
whether an occurrence is prose or data, leave it and say so instead of
guessing.
