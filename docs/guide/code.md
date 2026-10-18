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

**Locks:** with Git LFS installed, changed files show a padlock when they are locked, and a warning names whoever else locked a file you changed. See [Git LFS](advanced-git.md#locks).

### Safety net before a commit

Above the commit button, GitSimple checks what is staged and warns. It never blocks the commit, and nothing leaves your computer.

- **Big files:** a staged file over 100 MB (GitHub refuses the push) or over 50 MB. **Track \*.ext with LFS** stores it with Git LFS instead (see [Big files before a commit](advanced-git.md#big-files-before-a-commit)). Without Git LFS installed, the warning says how to install it.
- **Conflict markers:** a staged text file that still has `<<<<<<<`, `=======` or `>>>>>>>` lines from a merge. A file needs a `<<<<<<<` or `>>>>>>>` line to count, because a lone `=======` is also how Markdown underlines a heading. Unity can't load a scene, prefab or asset with markers in it.
- **Possible secrets:** private key blocks (`-----BEGIN ... PRIVATE KEY-----`), GitHub tokens (`ghp_`, `gho_`, `ghu_`, `ghs_`, `ghr_`, `github_pat_`), AWS access key IDs (`AKIA...`, `ASIA...`), Google API keys (`AIza...`), and keystore files (`.keystore`, `.jks`, `.p12`, `.pfx`). Only these well-known shapes are checked, so most findings are real. A finding shows the first 4 characters and `••••`, never the whole value.

Only the lines the commit adds are checked, so a secret that is already in the history isn't reported again (remove it from the history and revoke it separately). Very large staged changes are checked up to 16 MB.

**Not a secret** hides that value wherever it appears; **Ignore file** stops checking that file for secrets. GitSimple remembers only a fingerprint of the value (it can't be turned back into the value) and the file's path, in this computer's app storage.

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

Code is coloured by language (about 55 languages, picked from the file name or extension) in diffs, file content, blame and the conflict resolver. Colouring happens in the background, so a large file shows as plain text first and fills in; files over 512 KB or 20,000 lines, and lines over 2,000 characters, stay plain. A diff colours each block on its own, so a block that starts inside a long comment or string can be coloured wrongly at first. Turn it off in **Settings › Git › Syntax highlighting**; pick the colours in **Settings › Appearance › Code colours** (see [Code colours](settings.md#code-colours)).

Diffs everywhere show at most 2 MB per file ("too large", with the size, and an **Open in editor** button). Binary files are detected and shown as such. Unity assets, Unity package files and images also have a summary view (see [Unity files, package files and images](#unity-files-package-files-and-images)).

In a changed line, the words that actually changed have a stronger background, in both unified and side by side views. A removed line is compared with the added line that replaced it (the first removed line of a block with the first added one, and so on). Lines that were rewritten rather than edited, lines over 1,000 characters and blocks where far more lines were added than removed (or the other way round) keep the plain line colour. A line whose only change is its line ending (CRLF and LF) shows as removed and added with nothing emphasised.

### Unity files, package files and images

Some files get a second view next to the raw diff. Switch between them with the **Summary** (or **Images**) and **Raw diff** tabs above the diff; each file opens on the summary. Staging, unstaging and discarding hunks or lines work in **Raw diff** as usual. The summaries appear in Changes, History (commit details and a file's history) and Stashes.

**Unity scenes, prefabs and other assets** (`.unity`, `.prefab`, `.asset`, `.mat`, `.controller`, `.anim`, `.overrideController`, `.mask`, `.physicMaterial`, `.playable`, `.mixer`, `.spriteatlas`, `.lighting`, `.renderTexture` and similar):

- Changes are grouped by GameObject, shown with its place in the hierarchy (`Level/Enemies/Grunt`), then by component (Transform, Light, a script shown by its file name, and so on). Objects outside a GameObject, such as a material or an animator state, are listed by their own name.
- Each changed property shows its path, the old value and the new value. Added and removed components and GameObjects are marked as such.
- A prefab instance in a scene shows its overrides as `Overrides/<property> @<object>`.
- References to other assets show the asset's path instead of `{fileID: …, guid: …}`. The path comes from the matching `.meta` file in your working folder (new, untracked `.meta` files count). Assets of packages (TextMeshPro, uGUI, the Input System and so on) are named from the project's `Library/PackageCache` folder, which Unity fills on every computer that opens the project, for example "TextMeshProUGUI (com.unity.textmeshpro)". A Unity project in a subfolder of the repository works too. Without that folder (a fresh clone that Unity hasn't opened yet), a reference that isn't in the repository shows as "from a package or built-in asset", with the start of its GUID; with it, a reference found in neither place shows as "not in the project or its PackageCache, maybe a built-in package". In Unity 6, uGUI and TextMeshPro ship with the Editor rather than in the PackageCache, so their references show that way too. `{fileID: 0}` shows as "None", and a reference to an object in the same file shows that object's name.
- The summary needs the asset saved as text: in Unity, set **Edit › Project Settings › Editor › Asset Serialization** to **Force Text**. Binary assets, assets stored in Git LFS and versions over 8 MB keep the raw diff, with a note that says why. At most 2,000 property changes are listed; the rest are counted.

**Unity packages** (`Packages/manifest.json` and `Packages/packages-lock.json`): the packages added, removed, upgraded or downgraded, with the old and new versions, for example "upgraded com.unity.inputsystem 1.6.3 → 1.7.0". A change of source (a registry package that now comes from git, say) or of a git package's commit is noted next to it. In the lock file, packages that other packages pull in are listed separately. When something else in the file changed too (scoped registries, for example), the summary says so; the raw diff has the details.

**Images** (PNG, JPEG, GIF, WebP and BMP): the old and new image side by side, with the size in pixels, the format and the file size. When both versions can be shown, **Swipe** lays one over the other; drag the slider to compare. Images over 10 MB show their size without a preview.

**Other binary files** (`.psd`, `.tga`, `.fbx`, `.wav` and the like) show their old and new file sizes.

**Git LFS:** for a file stored in Git LFS the size shown is the real file's size, and an image whose content is on this computer (in the local LFS store) is previewed. If the file in your working folder is only an LFS pointer because its content hasn't been downloaded, the view says so; run `git lfs pull` to get it.

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
