---
name: shigomori-release
description: Publish a Shigoto no Mori release with user-facing notes written from the merged PRs. Use when asked for a new release, a patch release, or release notes for sylophi/shigoto-no-mori.
---

Releases are GitHub releases on `sylophi/shigoto-no-mori`. Publishing one starts the signed Mac build, which attaches the DMG and zip.

The steps keep a release safe, so stick to them. The writing guidance is a default, not a rule. If a release reads better another way, write it that way.

## Steps

1. **See what's unreleased.** `git fetch origin --tags`, then `git log --oneline <last tag>..origin/main`. Every commit is a squash merge ending in `(#N)`. Read each PR with `gh pr view N --json title,body`.

2. **Pick the version.** Minor (`v2.4.0` → `v2.5.0`) when something new shows up. Patch (`v2.4.0` → `v2.4.1`) when it's only fixes and small tweaks, or when the user asks for a patch.

3. **Draft the notes.** See "Writing the notes" below, and `examples.md` next to this file for real PRs and the notes that shipped for them.

4. **Preview or publish.** For a minor release, show the draft and wait for a go. Use placeholders like `*[image: sort menu]*` instead of raw `<img>` tags so it reads cleanly, and say which commit it would be tagged on. For a patch the user asked for, you can publish straight away and then show the notes that went out.

5. **Publish.** Check that `origin/main` hasn't moved since you drafted. If it has, the new PRs need notes too. Then:

   ```sh
   gh release create vX.Y.Z --target <full sha of origin/main> --title vX.Y.Z --notes-file <notes file>
   ```

   A full release must point at a commit on main, or the build refuses it.

6. **Watch the build.** Find the run with `gh run list --workflow release.yml --limit 1` and wait for it, e.g. `gh run watch <id> --exit-status`. It takes several minutes. When it passes, check that the release is marked Latest and has the DMG and zip.

7. **Report.** Give the release link, the commit it's tagged on, and the build run link. If anything changed from the approved draft, say what.

If a command gets interrupted, check `gh release view vX.Y.Z` before creating it again. It may already exist. Fix notes on a live release with `gh release edit vX.Y.Z --notes-file <file>`.

## Writing the notes

People read release notes on GitHub, or in the app's What's new while deciding whether to restart for an update. They skim, and they'll find the details themselves the first time they use the change. Write for that person.

Every choice follows from what they care about: what gets a section, what comes first, how much space it gets, and what's left out. The size of the PR or how much code changed doesn't matter. A one-line fix can lead a release, and a huge refactor can be one line under the hood.

A PR is written for a reviewer and lists everything it touched. A release note is a pointer, not a manual: what's new, where to find it, and why they'd want it. That's usually a sentence or two. Leave out what they'd find out the moment they use it, or wouldn't miss if it were gone. They'll see it work the first time they try it. Telling them a choice is remembered, that it's on by default, that it works everywhere they'd expect, or what happens in rare cases only slows down their skim.

Use the app's own words, the names and labels it shows, so they can find what you describe. Mention anything that would surprise them, like something removed or something they have to do.

When the draft is done, read it once more as that user and cut what they wouldn't miss.

**Shaping the release**

What users will notice and care about gets its own section, with what they'll care about most first. Smaller changes they'd still like to know about go under Smaller fixes. Under the hood is for changes they won't notice, like build, tests and refactors.

Group by what the user experiences, not by PR. One PR can become several sections, and several PRs can share one.

## Layout

```markdown
This release adds X, lets you Y, and Z.

**Short title from the user's side** ([#123](https://github.com/sylophi/shigoto-no-mori/pull/123))
One or two sentences.

<img alt="What the image shows" src="https://github.com/user-attachments/assets/..." width="640">

**Another feature** ([#124](https://github.com/sylophi/shigoto-no-mori/pull/124))
...

| Before | After |
| --- | --- |
| <img alt="Before" src="..." width="400"> | <img alt="After" src="..." width="400"> |

**Smaller fixes**
- One sentence. ([#125](https://github.com/sylophi/shigoto-no-mori/pull/125))

### Under the hood

- One sentence. ([#126](https://github.com/sylophi/shigoto-no-mori/pull/126))

**Full Changelog**: https://github.com/sylophi/shigoto-no-mori/compare/vA.B.C...vX.Y.Z
```

- The opening line is for minor releases. It names the top two or three sections in the same order. Patches start straight with the first section.
- Titles are short and say what changed for the user ("Sort a project's worktrees", "Hidden worktrees fade"), not the PR title ("Add per-project worktree sort").
- Link every PR. A section built from two PRs links both.
- Reuse the PR's images and videos, picking the ones that show the change best. Turn `![alt](url)` into `<img alt="..." src="..." width="640">`. Around 480 suits a small piece of UI, and around 400 suits a before/after table. A video is its bare URL on its own line.
- Leave out Smaller fixes or Under the hood when there's nothing for them. A single line is fine.
