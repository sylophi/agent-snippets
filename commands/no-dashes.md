---
description: Ban em dashes and middle dots from produced artifacts, and scrub the ones already written
---

For the remainder of this session, keep these characters out of the artifacts
you produce:

- em dash `—`
- en dash `–`
- middle dot / interpunct `·`
- bullet `•` used inline in a sentence

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
sentence that leans on an em dash is usually two sentences.

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
