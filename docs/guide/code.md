[User guide](README.md) › Code

# Code: changes, branches, history, pull requests

The everyday git work. The Code views need a local clone, except **Pull requests**.

## Changes

Four lists: **Conflicted**, **Ready to commit** (staged), **Changes** and **New files** (untracked). Show them flat or by folder; select several with Ctrl / Shift. Drag the border between the lists and the diff to make the file list wider or narrower (or focus it and use Left / Right); the width is remembered.

**Choose what goes in the commit:**

- a file, a selection, or everything;
- a single block of changes (a *hunk*), or **Discard this block**;
- single **lines**: tick the boxes in the diff gutter, Shift-click for a range.

**Write the commit:**

- a summary (a hint appears past 72 characters) and an optional description;
- an optional issue key (the workspace's prefix, `#` or `GS-`, is added);
- **Commit**. With nothing staged the button reads **Stage all and commit**.

**Add to last commit (amend)** shows what it replaces and warns when that commit is already pushed.

**Right-click a file or folder** (see [Open in your editor, Explorer or a terminal](#open-in-your-editor-explorer-or-a-terminal)) to open it in your editor, show it in Explorer or open a terminal there.

## Sync: check, pull, push

These buttons are in the header of every Code view, with progress and **Stop**:

| Button | What it does |
| --- | --- |
| **Check for new commits** | `git fetch`: looks for new commits without changing your files |
| **Pull** | Brings new commits into your branch |
| **Push** | Uploads your commits. A branch with no remote copy shows **Publish branch** instead |
| **Overwrite remote branch (force push)…** | Advanced mode, under the arrow next to Push. Uses a lease, previews the remote commits it would discard, never a plain force push |

Pull, push and anything that can lose work (discard, delete branch, merge, amend) first show a **preview** of exactly what will happen. A preview works once, and only if nothing changed since. A rejected push tells you to pull first.

## Branches

Local and remote branches with their upstream, ahead / behind and last commit. A branch whose remote copy is gone is marked "deleted on the remote".

- **Create** from HEAD or any branch or commit, **switch**, **rename**, **delete**.
- **Choose remote branch (upstream)…** (Advanced mode).
- **Bring into current branch (merge)**.
- Switching with uncommitted changes asks before bringing them along.
- A merge, rebase, cherry-pick or revert that stops on conflicts lists the files in Changes and shows the operation banner (see [Advanced git](advanced-git.md#operation-banner)).

## History

- A paged commit list with a lane graph, branches and tags.
- Filters: text, author, path, all branches.
- Commit details with per-file diffs.
- **Copy SHA**, **Create branch here**, and the [advanced actions](advanced-git.md#history-actions) (right-click a commit, or its "…" menu).
- Right-click a commit for **Open terminal here** and **Reveal repository in Explorer** (the repository folder). Right-click a file of the commit to open the file as it is in your working folder (not as it was in that commit), show it in Explorer or open a terminal in its folder.

## Files

- The tree at HEAD, loaded folder by folder, with status badges for changed files.
- Select a file for its content, **Blame** (grouped by commit, hover for the summary) and its **History** (follows renames).
- Right-click a file or folder to open it in your editor, show it in Explorer or open a terminal there.

Code is coloured by language (about 55 languages, picked from the file name or extension) in diffs, file content, blame and the conflict resolver. Colouring happens in the background, so a large file shows as plain text first and fills in; files over 512 KB or 20,000 lines, and lines over 2,000 characters, stay plain. A diff colours each block on its own, so a block that starts inside a long comment or string can be coloured wrongly at first. Turn it off in **Settings › Git › Syntax highlighting**; the colours follow the theme (see [Custom themes](settings.md#custom-themes)).

Diffs everywhere show at most 2 MB per file ("too large", with the size, and an **Open in editor** button). Binary files are detected and shown as such.

In a changed line, the words that actually changed have a stronger background, in both unified and side by side views. A removed line is compared with the added line that replaced it (the first removed line of a block with the first added one, and so on). Lines that were rewritten rather than edited, lines over 1,000 characters and blocks where far more lines were added than removed (or the other way round) keep the plain line colour. A line whose only change is its line ending (CRLF and LF) shows as removed and added with nothing emphasised.

### Open in your editor, Explorer or a terminal

Three actions work on a file or folder of the repository, from the right-click menu in Changes, History and Files, and on the repository folder from the command palette (Ctrl+K: **Open repository in editor**, **Reveal repository in Explorer**, **Open terminal here**):

- **Open in editor** starts the editor chosen in [Settings > Git > Editor](settings.md#everyday) (the first supported editor found when none is chosen). The file must exist in your working folder, so the entry is hidden for deleted files.
- **Reveal in Explorer** opens the parent folder with the file or folder selected (for the repository folder, its parent with the repository selected). If the file is gone, its folder is selected instead.
- **Open terminal here** opens Windows Terminal, or Windows PowerShell when Windows Terminal isn't installed, in the folder.

Only paths inside the repository are accepted, and a link or junction that leads out of it is refused. A repository on a network share can't use these actions.

## Issues and code together

### Create a branch from an issue

In the issue panel click the branch icon (or press `B`, or use the palette).

- The name comes from the branch template, `{type}/{issue}-{slug}` by default. The slug is ASCII, lowercase, at most 50 characters. `{type}` is `fix` for issues labelled or typed `bug`, else `feat` (configurable).
- Choose the base (default: the repository's default branch, freshly fetched) and whether to check out the new branch.
- **Link on GitHub** makes the branch appear in the issue's Development section.
- Without a local clone the branch is created on GitHub only, and the app offers to clone.
- Optional **Start work** (Settings, Issues) moves the issue to a status and assigns you.

### Commits that mention issues

- The commit box guesses the issue from the branch name (only if that issue exists) and shows it as a chip you can change or remove.
- Settings, Git chooses how a commit refers to it (per workspace): nothing, a `Refs #N` trailer, `Fixes #N`, or the key in front of the summary (default). A `{issue}` in the commit template always wins.
- History shows issue chips for `#N`, `owner/repo#N` and the key prefix. Click one to open the issue.
- The issue panel's **Development** section lists linked branches, linked pull requests and recent commits that mention the issue.

## Pull requests

**Pull requests** sits between Branches and History and works without a clone.

**The list:** open / closed / merged, author, "review requested from me", search; checks, review decision, draft and conflict state. Refreshed like issues (focused window only, backs off on rate limits).

**The detail pane:**

- edit the title and text (a change made on GitHub meanwhile is offered as reload or overwrite);
- head / base, conversation and reviews, files changed, commits, checks, labels, assignees, requested reviewers, linked issues;
- add comments.

**Actions:**

| Action | How it works |
| --- | --- |
| **Check out locally** | Fetches the head and switches to it (a tracking branch, or a read-only `pr/N` branch for forks). Asks first when you have uncommitted changes |
| **Mark ready / Convert to draft** | Runs directly |
| **Close / Reopen**, **Delete branch** | Preview first |
| **Merge** | Preview first. Offers only the methods the repository allows, an editable commit title and message, and "Delete branch after merge" (on GitHub, and the local branch when it is checked out, clean and at the merged commit) |

A merge is refused while the pull request is a draft, conflicted or not open. Failing checks or unmet branch protection need **Merge anyway**, which only exists when GitHub lets you bypass protection. GitHub's own refusals are shown.

### Create a pull request

From the toolbar, Branches or the issue panel:

- base, title and body from the workspace templates (default: the issue title or the first commit summary; the commit list plus `Closes #N`);
- draft toggle, reviewers, labels, assignees;
- an unpublished branch is pushed first, through the usual push preview.

The toolbar shows the current branch's pull request (number, state, checks).
