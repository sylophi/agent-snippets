# From PR to release note

Real PRs and the notes that shipped for them. PR bodies are shortened, and images are shown as `[image]`.

These show the judgment behind each note, not phrasings to reuse. Write each new note to fit its own change.

## Features

### #446 Show one project at a time in sidebar

PR:

> The sidebar's tree listed every project unfolded, so with many projects their names got lost among the worktree rows. It now has two levels: a list of projects, and one project on its own with a Projects button to go back. The tree goes into the project of whatever page is open.
>
> The project launcher and the saved per-project folds are removed, since the list of projects does both jobs.

Release note:

```markdown
**One project at a time in the sidebar** ([#446](https://github.com/sylophi/shigoto-no-mori/pull/446))
With lots of projects, their names got lost among the worktree rows. The sidebar now starts with a list of your projects. Pick one to see just its worktrees, and use the Projects button to go back. This replaces the project launcher and per-project folding.

[image]
```

The PR's problem becomes the first sentence. "Saved per-project folds" becomes "per-project folding", and the note still says what's gone.

### #455 Add per-project worktree sort

PR:

> Inside a project, the tree's toolbar now has a sort menu for its worktrees: by name (the default), most recently active, or most recently created. The pick is saved per project as a shared setting, so it holds on every device. Every device's worktrees sort together, with the primary checkouts kept on top and PR stacks still grouped.
>
> The CLI now reports a `createdAt` for each linked worktree, read from when git added it.

Release note:

```markdown
**Sort a project's worktrees** ([#455](https://github.com/sylophi/shigoto-no-mori/pull/455))
A new sort menu inside a project orders its worktrees by name, most recently active, or most recently created.

[image]
```

That the pick is saved and shared across devices is what you'd expect, so it's left out. So is the `createdAt` field, which is plumbing for the feature.

### #459 Add Visitors collection to Village life

PR:

> Adds Visitors, a collection of every villager who has moved into a worktree. It is a section of Settings while Village life is on.
>
> Like the rest of Village life it lives in the app alone. The app counts every villager worktree it sees on any device it shows, once each, and keeps the album in its own storage. Villagers are collected like stickers in an album, by rarity, with silhouettes for the ones still to meet. Each card shows how often they came and flips over to their details. The villager met most is your best friend.

Release note:

```markdown
**Visitors** ([#459](https://github.com/sylophi/shigoto-no-mori/pull/459))
With Village life on, Settings has a new Visitors album of every villager who's moved into one of your worktrees. They're collected like stickers, sorted by rarity, with silhouettes for the ones you haven't met yet. Each card shows how often they've come and flips over for their details. The villager you've met most is your best friend.

[image]

[image]
```

How it counts and where it stores the album is dropped. The fun details stay, because they're what the user will see.

### #460 Show port forwards on the Ports button and sidebar

PR:

> Nothing showed that this machine was forwarding a peer worktree's ports. Now the Ports button's icon turns green and the worktree's sidebar row gets a green Ports mark. Hovering either lists the mappings.
>
> Each forward records the worktree it was switched on from in its Ports dialog, so the marks need no extra port reads. Forwards started from the devices page carry no worktree and mark nothing.

Release note:

```markdown
**See your port forwards** ([#460](https://github.com/sylophi/shigoto-no-mori/pull/460))
When you're forwarding another device's worktree ports, the Ports button turns green and the worktree's sidebar row gets a green Ports mark. Hover either to see the ports.

| Before | After |
| --- | --- |
| [image] | [image] |
```

"Peer worktree" becomes "another device's worktree". The second paragraph is about how it works, so it's gone.

### #423 Enable auto-merge where the repo allows it

PR:

> Merging a PR that was still waiting on its checks or a review failed, even on repos where GitHub could have merged it on its own later. On a repo with auto-merge enabled, `sm merge` and `sm land` now enable auto-merge for such a PR instead, and GitHub merges it once the requirements are met. `sm land` stops before the cleanup until then, and picks it up on the next run.
>
> The app's merge button does the same: it reads "Squash and merge when ready", shows an armed auto-merge, and can disable it.

Release note:

```markdown
**Merge when ready** ([#423](https://github.com/sylophi/shigoto-no-mori/pull/423))
On repos with auto-merge enabled, a PR still waiting on checks or a review no longer fails to merge. The button reads "Squash and merge when ready", and GitHub merges it once it can. `sm merge` and `sm land` do the same, and `sm land` finishes the cleanup on its next run.

[image]
```

The PR leads with the CLI. The note leads with the app, quotes the button label, and keeps the CLI to one sentence at the end.

### #444 Keep worktrees on an external project's drive

PR:

> Adds a device setting, "Keep worktrees on the project's drive". With it on, a project that lives on an external drive keeps its Managed worktrees in `/Volumes/<drive>/.sm/worktrees/<project>/` instead of the data folder. Existing worktrees move from the project's Worktree location page.
>
> `sm worktrees move` can now move a worktree to another volume. Git refuses that, so it copies the checkout, repairs git's link and removes the original.

Release note:

```markdown
**Keep worktrees on an external drive** ([#444](https://github.com/sylophi/shigoto-no-mori/pull/444))
A new setting, "Keep worktrees on the project's drive", keeps worktrees for projects on an external drive on that same drive. You can move existing ones from the project's worktree location page. `sm worktrees move` can now move a worktree to another drive too.

[image]
```

The exact folder path and how the move works around git are left out. "Volume" becomes "drive".

### #388 Clone carry-over copies with clonefile on APFS

PR:

> Copy-mode carry-over entries (node_modules, build caches) were copied byte for byte with `cp -R`. On macOS they now land as one `clonefile(2)` call, which shares blocks with the source until either side writes. An 855 MiB node_modules takes about a second instead of 24, and no extra disk.
>
> Where a clone is not possible (another volume, a non-APFS disk, off macOS) it falls back to the same `cp -R -P` as before, after clearing any partial clone.

Release note:

```markdown
**Faster carry-over on Mac** ([#388](https://github.com/sylophi/shigoto-no-mori/pull/388))
Carry-over folders like `node_modules` are now cloned instead of copied byte by byte. An 855 MB `node_modules` takes about a second instead of 24, and uses no extra disk space until one side changes.
```

The numbers stay, since they're what makes it worth reading. The system call, the `cp` flags and the fallback for other disks go.

### #428 Let a device with command access off ask for a mirror

PR:

> "Mirror here" and `sm worktrees mirror --from` were refused whenever the asking device had command access off, because the peer's send, stream and git calls into that device are all gated on its switch. The ask now leaves an invitation on the asking device that admits exactly those calls, for that one peer and that one copy, whatever the switch says. Landed invitations persist so a relaunch keeps serving, and they go with the copy.
>
> The ask itself is one local orchestrator, `mirror:startFrom`, used by the dialog and the CLI alike. The calls a mirror may make under an invitation are tagged on the contracts, so the admitted surface cannot drift from the calls.

Release note:

```markdown
**Mirror here without allowing control** ([#428](https://github.com/sylophi/shigoto-no-mori/pull/428))
"Mirror here" and `sm worktrees mirror --from` now work even when this device has "Allow control from other devices" off. The other device only gets access for that one mirror.
```

A dense PR can shrink to two sentences. "Command access" becomes the setting's real label, and the invitation mechanism becomes "only gets access for that one mirror".

### #417 Turn the ⌘K palette into a jump box

PR:

> The ⌘K palette is now a jump box. Beside the fuzzy list, a pane shows what the highlighted worktree offers: open, changes, its PR, its launch tools (⌘1 to ⌘9 now launch for the highlighted row while the palette is open), a safe push or pull, its scripts, and copy path. Matched letters are marked, merged and shelved work sinks, PRs are searchable by number or title, and a query that names a project lists it as a row.
>
> When a query isn't an existing branch, the list offers to create a worktree on it in the project on screen, with ⇥ to pick another project or open the full form.

First draft title: **⌘K is now a jump box**. The user pointed out that "jump box" isn't a term people know. What shipped:

```markdown
**Do more from ⌘K** ([#417](https://github.com/sylophi/shigoto-no-mori/pull/417))
Next to the list, a new pane shows what you can do with the highlighted worktree: open it, see its changes or PR, launch tools, push or pull, run scripts, or copy its path. ⌘1 to ⌘9 launch tools for the highlighted row. You can search PRs by number or title, and type a new branch name to create a worktree for it.

[image]

[image]
```

A long list of small touches gets cut down to the ones a user would reach for.

## Fixes that get their own section

A fix users will notice in their day-to-day can get a section like any feature.

### #452 End the mirror when its copy is deleted

PR:

> A mirror runs on the device holding the original, so deleting the copy on its own device left the session there, halted on a folder that no longer exists. Stopping it was refused because the pair never read as synced, and a forced stop then failed to delete the copy that was already gone.
>
> The device running the mirror now ends it when the peer announces the copy's removal. Stop also ends a session whose copy the peer no longer lists...

Release note:

```markdown
**Deleting a mirror's copy ends the mirror** ([#452](https://github.com/sylophi/shigoto-no-mori/pull/452))
Deleting a mirror's copy now ends the mirror on the other device too. Before, the mirror got stuck there and couldn't be stopped.

[image]
```

A fix is the new behavior first, then what used to go wrong, in the user's terms.

### #456 Fade hidden worktrees like shelved ones

PR:

> Hidden worktrees (those matching a hidden prefix) now fade in the sidebar the way shelved ones do, in both the tree and the inbox. The fade only checked the worktree's own `shelved` flag, and hidden isn't stored on the worktree, so those rows never faded. Each row now carries the fold it was filed behind, and the fade reads that.

Release note:

```markdown
**Hidden worktrees fade** ([#456](https://github.com/sylophi/shigoto-no-mori/pull/456))
Hidden worktrees now fade in the sidebar and inbox, like shelved ones.

[image]
```

One sentence is enough. Why it was broken doesn't matter to the user.

## Smaller fixes

### #438 Raise default window height

PR:

> The main window now opens at 920×720 instead of 920×600, so more of the page is visible without resizing. 720 still fits within the usable screen area of a 1280×800 display.

```markdown
- The main window opens taller by default, so you see more without resizing. ([#438](https://github.com/sylophi/shigoto-no-mori/pull/438))
```

### #448 Open the app on the list of projects

PR:

> Opening the app landed inside the first registered project instead of on the list of projects. The "/" route redirected to the first worktree... The redirect is gone, so a fresh window rests on "Nothing selected" next to the list.
>
> Cancel on the New worktree, Tidy and convert pages now goes back to the page it came from...

```markdown
- The app now opens on your list of projects instead of jumping into the first one. ([#448](https://github.com/sylophi/shigoto-no-mori/pull/448))
- Cancel on New worktree, Tidy and convert goes back to the page you came from. ([#448](https://github.com/sylophi/shigoto-no-mori/pull/448))
```

Two unrelated changes from one PR become two bullets.

### #434 Paint the web page canvas with the theme

PR:

> The page's html and body are transparent so the desktop window's vibrancy shows through, which left the web client's canvas white. That white showed when overscrolling on macOS, and around iOS Safari 26's toolbars...

```markdown
- In the web client, overscrolling and iOS Safari's toolbars now show the theme's color instead of white. ([#434](https://github.com/sylophi/shigoto-no-mori/pull/434))
```

## Under the hood

### #418 Fix dirty release builds

PR:

> Release builds reported their commit as `-dirty` because the committed third-party license notices had fallen behind the lockfile... This regenerates the notices, records license paths relative to each package, and makes `licenses:check` compare against the committed files instead of rewriting them. That check now runs in PR CI...

```markdown
- Release builds no longer report their commit as "dirty", and license notices are checked in CI. ([#418](https://github.com/sylophi/shigoto-no-mori/pull/418))
```

### #426 Pool the web client and UI lab dev ports

PR:

> The web client and UI lab dev servers had fixed ports, so they couldn't run in two worktrees at once. They now get per-worktree ports from port-pool, like the renderer.

```markdown
- The web client and UI lab dev servers get their own ports per worktree, so several can run at once. ([#426](https://github.com/sylophi/shigoto-no-mori/pull/426))
```

### #415 Convert mjs and cjs files to TypeScript

PR:

> Every `.mjs` and `.cjs` file is now TypeScript: the test proofs and their helpers, the build and dev scripts, the lab harnesses, the oxlint plugin and the Astro config...

```markdown
- The remaining JavaScript scripts, tests and tools are now TypeScript and get type-checked with the app. ([#415](https://github.com/sylophi/shigoto-no-mori/pull/415))
```

### #462 Rebuild the packaged app's environment from the login shell

PR:

> A packaged launch on macOS starts with whatever environment its launcher had. From Finder that is launchd's stripped set, but from a terminal or an agent running `open` it is the whole session, agent markers and tokens included, and every dev server the app starts then inherits it. The app now rebuilds its environment once at startup... Every launch looks the same however it was started, and `.zshrc` exports reach every child.

This went out as a smaller fix, but nothing changes on screen and most users won't notice it. It fits better under the hood:

```markdown
- Scripts and dev servers started by the app get the same environment however the app was opened, built from your login shell. ([#462](https://github.com/sylophi/shigoto-no-mori/pull/462))
```

## Lessons from review

Changes the user asked for on drafts:

- **Cut how-to filler.** The palette note first ended with "Pick one of each in Appearance from a row of swatches. Your picks stay put even while Doubutsu is off." The user said it wasn't needed. The screenshot already shows the picker.
- **No made-up terms.** "⌘K is now a jump box" became "Do more from ⌘K".
- **Split bundled polish.** "A round of Doubutsu polish" became separate sections: accent colors, toasts, village news cards, popups without lines, and the leaf wallpaper.
- **Order by what users notice.** Two new palettes and a mirror fix went ahead of the in-app changelog. A new dark mode went first, ahead of villager faces.
- **Give noticeable fixes room.** Three smaller fixes that users would see every day (a stuck mirror, faded hidden worktrees, the device filter moving) became their own sections.
- **Cut the expected and the unimportant.** A v2.20.0 draft said the diff wrap button "works in both layouts and stays on across diffs and restarts", that a link "opens the app first if it isn't running", and that grouping by owner changes nothing "if all your projects have the same owner". None of it was needed. It also described the face that pops in when village news grows, which the video already shows.

## A whole release

v2.18.0, a minor release:

```markdown
This release fills out the new one-project sidebar: richer worktree rows, a sort menu, a header that stays put, and smooth slides between views. ⌘K can also find any project now.

**Richer worktree rows** ([#453](https://github.com/sylophi/shigoto-no-mori/pull/453))
Inside a project, worktrees now show the same rows as the inbox: the branch with all its status pills and PR number, over the worktree's name, device and villager face.

[image]

**Sort a project's worktrees** ([#455](https://github.com/sylophi/shigoto-no-mori/pull/455))
A new sort menu inside a project orders its worktrees by name, most recently active, or most recently created.

[image]

**Find any project with ⌘K** ([#454](https://github.com/sylophi/shigoto-no-mori/pull/454))
⌘K now finds every project on every device, even ones with no worktrees yet. Pick one without worktrees to start a new one.

[image]

**The project header stays put** ([#457](https://github.com/sylophi/shigoto-no-mori/pull/457))
Inside a project, its header stays at the top of the sidebar as you scroll through the worktrees.

[image]

**Sidebar slides between views** ([#458](https://github.com/sylophi/shigoto-no-mori/pull/458))
Every change in the sidebar now slides in from the side it came from: switching between inbox and projects, opening Settings or a diff, and coming back. (The video is slowed down.)

[video]

**Deleting a mirror's copy ends the mirror** ([#452](https://github.com/sylophi/shigoto-no-mori/pull/452))
Deleting a mirror's copy now ends the mirror on the other device too. Before, the mirror got stuck there and couldn't be stopped.

[image]

**Hidden worktrees fade** ([#456](https://github.com/sylophi/shigoto-no-mori/pull/456))
Hidden worktrees now fade in the sidebar and inbox, like shelved ones.

[image]

**Device filter on top** ([#451](https://github.com/sylophi/shigoto-no-mori/pull/451))
The device filter now sits at the top of the sidebar, above the toolbar.

[image]

**Full Changelog**: https://github.com/sylophi/shigoto-no-mori/compare/v2.17.1...v2.18.0
```

v2.14.1, a patch with an under-the-hood part:

```markdown
**Palettes in pairs** ([#414](https://github.com/sylophi/shigoto-no-mori/pull/414))
Each light Doubutsu palette now sits above its dark twin in Appearance: Snow & Charcoal, Meadow & Forest, Cream & Wood, Sky & Midnight, and Sakura & Cocoa. The defaults stay the same.

[image]

### Under the hood

- Stricter type checks on list and lookup access across the app and the hub. ([#413](https://github.com/sylophi/shigoto-no-mori/pull/413))
- New tests for the changes page's discard, restore, amend and undo, and for mirror file patterns. Leftover support for pre-v2.10 devices is gone, since they can't connect anymore. ([#412](https://github.com/sylophi/shigoto-no-mori/pull/412))

**Full Changelog**: https://github.com/sylophi/shigoto-no-mori/compare/v2.14.0...v2.14.1
```
