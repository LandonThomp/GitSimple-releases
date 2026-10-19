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
- optional **co-authors**: pick people who worked on it with you from the recent authors of the repository and its GitHub users (their GitHub noreply address, so GitHub credits their account; for a repository on a GitHub Enterprise Server, only the recent authors for now, and the picker says so), or type `Name <email>` for anyone else. Each gets a `Co-authored-by:` line at the end of the message (no need to type those lines). You are never added as your own co-author, and the same person is added once;
- **Commit**. With nothing staged the button reads **Stage all and commit**.

**Add to last commit (amend)** shows what it replaces and warns when that commit is already pushed. While amending, the picker shows the last commit's co-authors (not the description), so you can add or remove them; they are written back once. Co-authors picked for your next commit come back when you turn amend off. Co-authors are read from the message's final block of `Key: value` lines only; a line mixed in with other text stays in the description.

**Who commits:** under the message the box shows the name and email git will use (hover to see which configuration each comes from: this repository, your global config, ...). When the repository has an identity chosen under **Settings > Workspaces > Commit as** and git would use another one, the box says so; **Apply…** shows the change to the repository's `.git/config` (old and new `user.name` / `user.email`) and writes it when you confirm. Nothing else in your git configuration is changed. See [Identities](settings.md#identities).

**Signed commits:** GitSimple follows your git configuration. With `commit.gpgsign` on, the box says **Commits will be signed (SSH)** (or GPG, X.509) and git signs as usual with `gpg.format`, `user.signingkey` and `gpg.program` / `gpg.ssh.program`. There is no switch in GitSimple. git runs without a console window, so a signer can't ask for a passphrase in a terminal:

- **SSH keys** need ssh-agent (Windows: start the "OpenSSH Authentication Agent" service, then `ssh-add` your key once), or a key without a passphrase;
- **GPG keys** need a graphical pinentry (Gpg4win's), or a key gpg-agent has already unlocked.

When signing fails the commit isn't made and the error says why, with these hints. A signer that hangs is stopped after the network time limit (Settings > Git > Advanced; it also applies to commands that may run hooks).

**Drafts:** an unfinished summary, description and the co-authors picked are kept per repository and branch, on this computer. They come back when you return to Changes, switch back to the branch or restart GitSimple, and after a commit that failed (a hook rejected it, say). A successful commit clears the draft. A message that looks like it holds a secret (a token, a password, a URL with a password in it; the same rule as the [console](advanced-git.md#the-git-console)'s history) is never saved. While amending, the text comes from the last commit and isn't saved as a draft.

**Locks:** with Git LFS installed, changed files show a padlock when they are locked, and a warning names whoever else locked a file you changed. See [Git LFS](advanced-git.md#locks).

### Safety net before a commit

Above the commit button, GitSimple checks what is staged and warns. It never blocks the commit, and nothing leaves your computer.

- **Big files:** a staged file over 100 MB (GitHub refuses the push) or over 50 MB. **Track \*.ext with LFS** stores it with Git LFS instead (see [Big files before a commit](advanced-git.md#big-files-before-a-commit)). Without Git LFS installed, the warning says how to install it.
- **Conflict markers:** a staged text file that still has `<<<<<<<`, `=======` or `>>>>>>>` lines from a merge. A file needs a `<<<<<<<` or `>>>>>>>` line to count, because a lone `=======` is also how Markdown underlines a heading. Unity can't load a scene, prefab or asset with markers in it.
- **Possible secrets:** private key blocks (`-----BEGIN ... PRIVATE KEY-----`), GitHub tokens (`ghp_`, `gho_`, `ghu_`, `ghs_`, `ghr_`, `github_pat_`), AWS access key IDs (`AKIA...`, `ASIA...`), Google API keys (`AIza...`), and keystore files (`.keystore`, `.jks`, `.p12`, `.pfx`). Only these well-known shapes are checked, so most findings are real. A finding shows the first 4 characters and `••••`, never the whole value.

Only the lines the commit adds are checked, so a secret that is already in the history isn't reported again (remove it from the history and revoke it separately). Very large staged changes are checked up to 16 MB.

**Not a secret** hides that value wherever it appears; **Don't check this file** stops checking that file for secrets (it doesn't add it to `.gitignore`; for that, see [Ignore](#ignore-a-file-or-stop-tracking-it)). Both also apply to the [safety net before a push](#safety-net-before-a-push), and the other way round. GitSimple remembers only a fingerprint of the value (it can't be turned back into the value) and the file's path, in this computer's app storage.

**Right-click a file or folder** (see [Open in your editor, Explorer or a terminal](#open-in-your-editor-explorer-or-a-terminal)) to open it in your editor, show it in Explorer or open a terminal there.

### Ignore a file, or stop tracking it

Right-click a file in **New files** and open **Ignore**:

- **Write the rule to** (choose first; the menu stays open): the repository's `.gitignore`, the `.gitignore` in the file's folder, or `.git/info/exclude`, which only this copy of the repository uses and is never committed (worktrees of the repository share it).
- Then pick what to ignore: **This file**, **All .ext files** (with this file's extension), or **This folder**. A folder's own `.gitignore` can't ignore that folder, so **This folder** goes to the root `.gitignore` or `.git/info/exclude`.

A preview shows the exact line, the file it is added to (created when missing) and the new files it hides. **Ignore** adds the line at the end of the file in the file's own line endings; nothing else in it changes. The files leave **New files** right away. Commit the `.gitignore` so the whole team ignores them. Special characters in names (`*`, `?`, `[`) are escaped, so the rule only matches that name.

Ignoring doesn't affect files git already tracks. For those, right-click the file in **Changes** or **Ready to commit** and choose **Stop tracking (keep on disk)**. A preview lists the files; confirming runs `git rm --cached`: the files stay in your folder and show as new files, and the removal is staged. When you commit it, the files are removed from the repository, and everyone who pulls that commit has them deleted from their folder, so ignore them as well.

## Sync: check, pull, push

These buttons are in the header of every Code view, with progress and **Stop**:

| Button | What it does |
| --- | --- |
| **Check for new commits** | `git fetch`: looks for new commits without changing your files |
| **Pull** | Brings new commits into your branch |
| **Push** | Uploads your commits. A branch with no remote copy shows **Publish branch** instead |
| **Overwrite remote branch (force push)…** | Advanced mode, under the arrow next to Push. Uses a lease, previews the remote commits it would discard, never a plain force push |

Pull, push and anything that can lose work (discard, delete branch, merge, amend) first show a **preview** of exactly what will happen. A preview works once, and only if nothing changed since. A rejected push tells you to pull first.

### Safety net before a push

The push preview (and the force push preview, and the preview of [moving files into LFS in the whole branch](advanced-git.md#moving-committed-files-into-lfs), which force-pushes) runs the [commit safety net](#safety-net-before-a-commit) again, over the commits the push sends: the commits that are neither on your remote branch nor on any other remote branch. Commits you merged in from `origin/main`, say, are already on the server and aren't checked again. That catches what the commit check never saw: a commit made with `git commit` in a terminal or another tool, or a warning you dismissed and committed anyway.

- **Check before you push** lists the findings under the commit they came from (its short id and summary): conflict markers, possible secrets and keystore files, and files of 50 MB or more (red at 100 MB, where GitHub refuses the push).
- The rules are the same as at commit time, and only the lines each commit adds are checked, so something already on the remote isn't reported again. What the push sends is read up to 16 MB (the first 1000 commits); if there is more, the preview says part of it wasn't checked, and if the check fails it says so. A merge commit's own changes aren't checked, only the commits it brings in. In a [partial clone](advanced-git.md#folders-big-repositories) the check never downloads anything: if it needs file contents that aren't on your computer, it skips them and says so (with Git older than 2.44 it skips the added lines in every partial clone).
- **Not a secret** and **Don't check this file** work as at commit time and are shared with it.
- **GitHub Actions workflows:** when the commits change a file under `.github/workflows/` and git pushes with GitSimple's sign-in (an https remote, with **Use GitSimple's GitHub sign-in for git** on), the preview warns that GitHub will refuse the push: GitHub only accepts workflow changes from a sign-in with the "workflow" permission, which GitSimple doesn't ask for. Push those commits from a terminal, or turn that setting off in Settings > Git so git uses your own credentials (Git Credential Manager). A refused push says the same. A GitHub CLI sign-in with the permission (`gh auth refresh -s workflow`) also works while GitSimple has no sign-in of its own.
- It only warns: the push button stays enabled. A secret stays in the history even if a later commit deletes it, so revoke the key and fix the commit (amend, or rewrite it in [History](advanced-git.md#history-actions)) before you push. A file over 100 MB has to come out of the commits too; see [Big files](advanced-git.md#big-files-before-a-push).

## Branches

Local and remote branches with their upstream, ahead / behind and last commit. A branch whose remote copy is gone is marked "deleted on the remote".

- **Create** from HEAD or any branch or commit, **switch**, **rename**, **delete**.
- **Choose remote branch (upstream)…** (Advanced mode).
- **Bring into current branch (merge)**.
- **Compare with current** and **Compare with** the default branch open the [compare view](#compare-branches-and-commits) with that branch on the compare side.
- Switching with uncommitted changes asks before bringing them along.
- A merge, rebase, cherry-pick or revert that stops on conflicts lists the files in Changes and shows the operation banner (see [Advanced git](advanced-git.md#operation-banner)).

## History

- A paged commit list with a lane graph, branches and tags.
- Filters: text, author, path, all branches.
- A badge on a signed commit's details: **Signed** (verified), **Signed, key not trusted**, **Signed, expired**, **Signed, key revoked**, **Bad signature**, or **Signed, unverified** when git can't check it (no key, no gpg; for SSH signatures set `gpg.ssh.allowedSignersFile` to verify them). Hover it for the signer and key. The commit list itself never checks signatures (that would start gpg for every row).
- Commit details with per-file diffs. Drag the border between the commit list and the details to make them wider or narrower, and the borders under the commit message and the file list to make those taller or shorter (or focus a border and use the arrow keys); the sizes are remembered.
- **Copy SHA**, **Create branch here**, and the [advanced actions](advanced-git.md#history-actions) (right-click a commit, or its "…" menu).
- Right-click a commit for **Open terminal here** and **Reveal repository in Explorer** (the repository folder). Right-click a file of the commit to open the file as it is in your working folder (not as it was in that commit), show it in Explorer or open a terminal in its folder.
- A commit's parents are links: a click goes to that commit, even when the list hasn't loaded it yet (with filters on, it opens directly and the filters stay).
- Above a file's diff, **View file at this commit** opens the whole file as this commit left it (read-only, labelled with the commit), **Blame at this commit** shows the file's blame as this commit left it and **Blame parent** as it was just before (under its old name after a rename). Click a commit id in that blame to open the commit, and blame again from there to walk back through the file's history.

### Go to a commit

Paste a full or short commit id into the command palette (`Ctrl+K`) and pick **Open commit**; a branch or tag name works there too. In History's **Message** field, a commit id (7 or more hex digits) offers **Go to commit** (or press Enter) once git confirms it names a commit; a word that only looks like one, such as "defaced", stays a message search. In the Files view's **Blame** tab, click a commit id to open it in History (the small copy icon next to it copies the id).

History clears its text, author and path filters, then loads more of the list until the commit is in it, selects it and highlights its row in the graph. A commit the list can't reach (it is only on another branch) or that lies more than 5,000 commits back opens directly in the details pane, with a note; for another branch's commit, **Show all branches** turns on all branches and finds it in the list. An id or name that matches no commit says so; so does a short id that several commits start with (type more digits).

### Compare branches and commits

Open the compare view from History's **Compare** button, the palette (**Compare branches or commits…**), a branch's **Compare with current** / **Compare with** the default branch, or the commit menu: **Compare with current branch** for one commit (the commit on the compare side, the current branch as the base), **Compare oldest and newest selected** for a Shift-click range (Advanced; the oldest commit is the base, so its own changes are not in the diff).

Type or pick a branch, remote branch, tag or commit id on both sides (**Base** and **Compare**; the swap button turns them round). It shows:

- **Only in** each side: the commits one side has and the other doesn't, newest first (the newest 200 are listed, the count is complete). Click one to open it in History.
- How far the compare side is ahead of and behind the base, and the commit where they forked.
- The files that differ, and the diff of the selected file (with the same diff options as elsewhere).

Two modes, as in git: **A...B** (the default) shows what the compare side changed since it forked from the base, as a pull request shows it; **A..B** compares the two versions directly, so the base side's own changes show as removed. Two branches without common history can only be compared with **A..B**. The commit lists are the same in both modes.

## Files

- The tree at HEAD, loaded folder by folder, with status badges for changed files.
- Select a file for its content, **Blame** (grouped by commit, hover for the summary; click a commit id to [open it in History](#go-to-a-commit)) and its **History** (see below).

### A file's history

The **History** tab lists the commits that changed the file, newest first, and follows it across renames: a commit from before a rename shows the file's name in that commit ("as old/name.ts"), and the rename itself says "renamed from ...". Click a commit to see what it changed in the file.

- **View file at this commit** opens the whole file as that commit left it, read-only, with the commit id in its header (`At 1a2b3c4 · Read-only`). Close it to get back to the working tree. It isn't offered for the commit that deleted the file.
- **Compare two versions:** tick the boxes of two commits and click **Compare these versions**. The diff shows the file at the older commit against the file at the newer one, as one file even when it was renamed in between, as long as the two versions are still similar (git's rename detection: about half the content the same). A file that was renamed and largely rewritten shows as deleted and added instead. A third tick replaces the first one; **Back to the commit** returns to the single-commit diff.
- Both work in Basic and Advanced mode.

Limits:

- Renames are followed along one line of history (git's `--follow`). A rename made on a branch that was merged in may not be followed past the merge, so commits from before that rename can be missing.
- A merge commit is listed when it changed the file compared with its first parent, and its diff is against that parent.
- Files over 2 MB aren't shown in the file view or the diff (as everywhere); the list reaches back 10,000 commits.
- Right-click a file or folder to open it in your editor, show it in Explorer or open a terminal there.

Code is coloured by language (about 55 languages, picked from the file name or extension) in diffs, file content, blame and the conflict resolver. Colouring happens in the background, so a large file shows as plain text first and fills in; files over 512 KB or 20,000 lines, and lines over 2,000 characters, stay plain. A diff colours each block on its own, so a block that starts inside a long comment or string can be coloured wrongly at first. Turn it off in **Settings › Git › Syntax highlighting**; pick the colours in **Settings › Appearance › Code colours** (see [Code colours](settings.md#code-colours)).

Diffs everywhere show at most 2 MB per file ("too large", with the size, and an **Open in editor** button). Binary files are detected and shown as such. Unity assets, Unity package files and images also have a summary view (see [Unity files, package files and images](#unity-files-package-files-and-images)).

### Reading a diff

The diff header has the same controls everywhere (Changes, History, a file's history, Stashes and pull requests): additions and deletions, **whitespace** (the ¶ button), context lines, unified or side by side, and wrapping.

**Whitespace.** Choose **Show whitespace changes** (the default), **Ignore changes in amount of whitespace** (like `git diff -b`: re-indenting and trailing spaces don't count, but a space added where there was none does) or **Ignore all whitespace** (`git diff -w`). The choice is remembered and is also in **Settings › Git › Whitespace changes**. A file whose only changes are whitespace says so, with a button to show them. While whitespace is hidden, hunks and lines can't be staged, unstaged or discarded one by one (the diff on screen isn't the file's real diff, so its patch wouldn't apply); whole files still can. Pull request diffs come from GitHub without a whitespace option, so GitSimple hides the whitespace-only lines itself; the hunk headers keep GitHub's line ranges.

**Hidden lines.** The bar above each block says how many unchanged lines are hidden before it. Its buttons show 20 more lines below the previous block or above this one, or all of them; after the last block a bar shows the rest of the file. **Hide** (the fold icon) puts expanded lines away again. The lines are read from the file's new version (the working tree, the staged file, the commit or the stash) the first time you expand, so they take a moment; if the file changed since the diff was loaded, refresh first. Pull request diffs expand from the head commit when you have a clone that has fetched it. New and deleted files have nothing hidden. The arrow at the start of a block's bar folds the block itself; staging works the same with expanded or folded blocks (lines you selected in a block stay selected while it is folded).

**Find and go to line.** Press `Ctrl+F` to find in the diff, file or blame you last clicked in (or the one on screen; with several diffs on screen, as in a pull request, click the one you want first): **Enter** / `F3` for the next match, **Shift+Enter** / `Shift+F3` for the previous one, with match case, whole word and regular expression options. It searches all the lines, even in long files where only the visible part is drawn, but not inside folded blocks or hidden lines. `Ctrl+G` goes to a line number (in a diff, the new file's number; a line the diff doesn't show goes to the next line it does). `Esc` closes the bar.

In a changed line, the words that actually changed have a stronger background, in both unified and side by side views. A removed line is compared with the added line that replaced it (the first removed line of a block with the first added one, and so on). Lines that were rewritten rather than edited, lines over 1,000 characters and blocks where far more lines were added than removed (or the other way round) keep the plain line colour. A line whose only change is its line ending (CRLF and LF) shows as removed and added with nothing emphasised.

### Unity files, package files and images

Some files get a second view next to the raw diff. Switch between them with the **Summary** (or **Images**) and **Raw diff** tabs above the diff; each file opens on the summary. Staging, unstaging and discarding hunks or lines work in **Raw diff** as usual. The summaries appear in Changes, History (commit details and a file's history) and Stashes; a pull request's **Files changed** shows the image previews (see below).

**Unity scenes, prefabs and other assets** (`.unity`, `.prefab`, `.asset`, `.mat`, `.controller`, `.anim`, `.overrideController`, `.mask`, `.physicMaterial`, `.playable`, `.mixer`, `.spriteatlas`, `.lighting`, `.renderTexture` and similar):

- Changes are grouped by GameObject, shown with its place in the hierarchy (`Level/Enemies/Grunt`), then by component (Transform, Light, a script shown by its file name, and so on). Objects outside a GameObject, such as a material or an animator state, are listed by their own name.
- Each changed property shows its path, the old value and the new value. Added and removed components and GameObjects are marked as such.
- A prefab instance in a scene shows its overrides as `Overrides/<property> @<object>`.
- References to other assets show the asset's path instead of `{fileID: …, guid: …}`. The path comes from the matching `.meta` file in your working folder (new, untracked `.meta` files count). Assets of packages (TextMeshPro, uGUI, the Input System and so on) are named from the project's `Library/PackageCache` folder, which Unity fills on every computer that opens the project, for example "TextMeshProUGUI (com.unity.textmeshpro)". A Unity project in a subfolder of the repository works too. Without that folder (a fresh clone that Unity hasn't opened yet), a reference that isn't in the repository shows as "from a package or built-in asset", with the start of its GUID; with it, a reference found in neither place shows as "not in the project or its PackageCache, maybe a built-in package". In Unity 6, uGUI and TextMeshPro ship with the Editor rather than in the PackageCache, so their references show that way too. `{fileID: 0}` shows as "None", and a reference to an object in the same file shows that object's name.
- The summary needs the asset saved as text: in Unity, set **Edit › Project Settings › Editor › Asset Serialization** to **Force Text**. Binary assets, assets stored in Git LFS and versions over 8 MB keep the raw diff, with a note that says why. At most 2,000 property changes are listed; the rest are counted.

**Unity packages** (`Packages/manifest.json` and `Packages/packages-lock.json`): the packages added, removed, upgraded or downgraded, with the old and new versions, for example "upgraded com.unity.inputsystem 1.6.3 → 1.7.0". A change of source (a registry package that now comes from git, say) or of a git package's commit is noted next to it. In the lock file, packages that other packages pull in are listed separately. When something else in the file changed too (scoped registries, for example), the summary says so; the raw diff has the details.

**Images** (PNG, JPEG, GIF, WebP and BMP): the old and new image side by side, with the size in pixels, the format and the file size. When both versions can be shown, **Swipe** lays one over the other; drag the slider to compare. Images over 10 MB show their size without a preview.

**Images in a pull request.** In **Files changed**, an image opens on **Images** too: the version in the pull request's latest commit next to the version where it branched off the base branch (the same "before" GitHub uses, so changes merged into the base branch since don't show up). The images are read from your clone, so they need a clone that has fetched both the pull request's commits and the base branch; otherwise the view says to fetch. When the pull request's commits were pushed to the base branch directly, there is no "before" to read and the view says so. Without a clone, only GitHub's note ("Binary file…") shows. Comments, and the conversations on its lines, are on the **Raw diff** tab when the file has one (an LFS pointer, for example).

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
- head / base, conversation and reviews, files changed (see [Reading a diff](#reading-a-diff) for whitespace, hidden lines and find), commits, checks, labels, assignees, requested reviewers, linked issues;
- add comments (**Attach on GitHub** works as in the [issue panel](issues.md#the-issue-panel): it copies your comment and opens the pull request on GitHub, where you paste it and drag the file in);
- react to the description, comments and review comments (the same reactions as in the [issue panel](issues.md#the-issue-panel); not on your pending review comments, GitHub allows that only once the review is submitted).

### Checks: logs and re-runs

The **Checks** tab lists the checks of the latest commit. For a job that ran on GitHub Actions, and when you can push to the repository, a row also has:

- **Show log**: the job's log in the app, without color codes, scrolled to the end (where a failure is). A long log shows its last 5,000 lines first (**Show all lines** for the rest); a log over 25 MB shows its end. **Copy** copies what was loaded. Logs are there once a job has finished; GitHub deletes them after a while (90 days unless the repository says otherwise), and then the log says so.
- **Re-run** (the circular arrow): this job, the failed jobs of the workflow run (when the job failed) or all of its jobs. GitSimple asks first, because a re-run uses your GitHub Actions minutes. If GitHub refuses, for example because a run from a fork waits for a maintainer's approval, its reason is shown; approve the run on GitHub, then try again.

Other checks (other apps, commit statuses) open on GitHub with the arrow button. On a GitHub Enterprise Server re-runs work, but logs can't be shown in the app yet, so **Show log** isn't there: open the log on the server.

### Other pull requests that change the same files

An open pull request's detail warns when other open pull requests change some of the same files, with each one's number, title and shared files (click one to open it). The **Create pull request** dialog shows the same warning for the branch you are about to propose, and Home's **Status** card has a **Same files** line for the branch you are on (not the default branch). Use it to talk to the other author before you both edit the same file.

- "The same files" means the same path; whether the changed lines collide isn't checked. A renamed file counts with its old and its new name.
- A branch is compared from where it forked from the default branch (`origin/<default>`, else the local one), so commits on the default branch since then don't count. The branch's own pull request is left out.
- GitHub lists 100 files per pull request: when another pull request changes more, the warning says an overlap with it may be missing. Only the 100 most recently updated open pull requests are compared. When nothing was found but not everything could be compared, a note says so.
- A stacked branch (made from another feature branch) is compared with the default branch, so the parent branch's pull request shows up too. A pull request you opened from a fork isn't recognised as the branch's own.
- It works on a GitHub Enterprise Server too, through that server's sign-in.

### Review a pull request

In **Files changed**, point at a line and click the comment button in the gutter (side by side: next to the line number). Shift-click a second line of the same block and side for a range. A comment on a removed line goes on the old side.

- **Suggest a change** (lines of the new version only) inserts a `suggestion` block with the current text of the lines; edit it into what the lines should say, and the author can apply it on GitHub.
- Your first comment starts a **pending review**: you see it marked *Pending*, nobody else does yet. More comments and replies join it. It is kept on GitHub, so it survives closing the app.
- **Review** in the header (it shows how many comments are pending) finishes the review: **Comment**, **Approve** or **Request changes**, with an optional summary. Submitting posts the summary and every pending comment at once; it can't be taken back. You can't approve or request changes on your own pull request. **Discard pending review** deletes the pending comments instead.
- A pending comment has a delete button. Submitted comments are edited or deleted on GitHub.
- If new commits arrive while you write, the comment is refused and the files reload: comment again on the new diff.

Conversations show under their lines. **Resolve conversation** folds one; resolved conversations are hidden until you click **Show resolved** in the files toolbar. A conversation whose lines changed since (*Outdated*), or whose lines aren't in the shown diff, is listed above the file with the lines it was made on.

**Viewed** in a file's header marks it as reviewed and folds it; the toolbar counts viewed files. Only you see these marks, and they are the same as on github.com. A file that changes after you marked it shows **Changed since viewed**.

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
