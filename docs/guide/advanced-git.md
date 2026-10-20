[User guide](README.md) › Advanced git

# Advanced git

Stashes, rewriting history, conflicts, undo, tags, remotes and the console. Most of these are in **Advanced mode**; the safety checks are the same in both modes.

## How GitSimple keeps you safe

Everything that can lose work:

1. shows a **preview** first: the exact commits, files or entries affected;
2. runs **once**;
3. is **re-checked** when you confirm. The plan is refused if HEAD, a ref, your uncommitted changes, the list of files, the stash list or the remote tag changed meanwhile.

Previews belong to the GitHub login that made them. Actions that discard data, rewrite or delete branches, or change a remote also ask in a Windows dialog right before they run.

Before an action that could lose uncommitted work runs, and only when you have uncommitted work, GitSimple saves it in a **snapshot** (see [Undo](#undo)): discard, hard reset (and Undo with Hard), pull, merge, abort merge, rebase, skip or abort of an operation in progress, delete untracked files, absorb into earlier commits, and restoring a snapshot. A dropped or cleared stash is kept the same way. The preview says so, and the result message confirms it.

## Stashes

*Advanced mode.* Put changes aside and bring them back later.

- The list shows message, branch, date and file count; open a stash for its files (untracked ones too) and their diffs.
- **New stash**: message, include untracked, keep index, or only selected files. Also **Stash…** in Changes.
- **Apply**, **Pop**, **Branch…**.
- **Drop** and **Clear all** are previews.
- A conflicting apply or pop goes to the [conflict resolver](#conflict-resolver).
- The branch-switch dialog offers **Stash changes and switch** and **Bring changes along**.

**Put-away changes (both modes).** Stashes is hidden in Basic, so the newest stash (the one **Stash…** or **Stash changes and switch** just made) also shows above the file list in Changes, in Changes when there is nothing to commit, and in Home's Status card:

- **Bring back** pops it (behind **Confirm before applying a stash**, as in Stashes). It waits, and says so, while files have conflicts or a merge, rebase or stash apply is open. If it clashes with your files, it stays on the list and the clashing files show in Changes.
- The title is the message you gave it. Without one, git's own "WIP on main: 1a2b3c4 subject" reads as **Unfinished work, after "subject"** (the last commit when you put the work away); the branch is shown next to it. Stashes shows the same titles.
- **Remove…** is the Drop preview.
- **See all in Stashes** opens the Stashes view, also in Basic (on Home only when there is more than one stash).

## History actions

Right-click a commit in History, or use its "…" menu.

| Action | Notes |
| --- | --- |
| **Undo this commit's changes (revert)** | Basic and Advanced. A merge commit asks which parent to keep |
| **Create branch here**, **Copy SHA** | Basic and Advanced |
| **Compare with current branch** | Basic and Advanced. A Shift-click range offers **Compare oldest and newest selected** (Advanced). See [Compare branches and commits](code.md#compare-branches-and-commits) |
| **Copy onto current branch (cherry-pick)** | One commit or a Shift-click range, optional `-x`. Dragging a commit onto a branch label does the same through a preview (see [History](code.md#history)) |
| **Move current branch to this commit (reset)…** | Soft, mixed or hard. The preview lists commits no branch will have any more and, for hard, the uncommitted changes that are lost. **Hard needs the branch name typed** |
| **Create tag here** | |
| **Rebase current branch interactively from here…** | Opens the [rebase editor](#interactive-rebase-editor) |
| **Rebase current branch onto here…** | |
| **Split this commit…** | Stops a rebase at the commit so its changes can be committed in pieces (see [Split a commit](#split-a-commit)) |
| **Find the first bad commit…** | Starts a [search for the commit that broke something](#find-the-first-bad-commit-bisect) with this commit as the good one |

On a branch row: **Rebase current branch onto this branch…** and **Interactive rebase onto this branch…**.

## Find the first bad commit (bisect)

When something worked in an older version and is broken now, git can find the commit that broke it by testing commits halfway in between (`git bisect`). Advanced mode: **Find a bad commit** in the History toolbar, or **Find the first bad commit…** on a commit (it becomes the good one).

1. Pick the **bad** commit (has the problem; HEAD by default) and an older **good** commit (before the problem). **Review** shows the range and about how many commits you will test.
2. **Start** checks out a commit in the middle (HEAD is detached). Uncommitted changes are put away in a stash named "Auto-stash before bisect" and saved in a snapshot first; bring them back from Put-away changes after you stop.
3. A banner on every Code view shows the commit to test. Build or run it, then click **Good** (no problem), **Bad** (has the problem) or **Skip** (can't be tested). Each click checks out the next commit.
4. When it is found, the banner names the first bad commit; **Show commit** opens it in History. If only skipped commits are left, it lists them: one of them is the first bad commit.
5. **Stop and go back to *branch*** is always in the banner: it ends the search and checks out the branch you started from, at any step. If that branch can't be checked out any more, Stop ends the search on the commit you were testing; switch to a branch in Branches. Stop also reminds you of the stash when it is still there.

A search started in the console with `bisect start` shows in the banner too. If it uses other words than good and bad (`--term-old` / `--term-new`) or `--no-checkout`, finish it in the console or stop it from the banner.

## Interactive rebase editor

1. Pick a base: a commit, or "onto" a branch.
2. Reorder by dragging (or Alt+Up / Alt+Down).
3. Choose per commit: pick, reword (edit inline), edit (stops so you can amend), squash, fixup, drop.
4. **Review** shows the commits before and after, and warns when commits that are already pushed would be rewritten.

Options: "Arrange fixup! and squash! commits (autosquash)" and "Set changes aside during a rebase (autostash)".

Merge commits in the range are refused: the editor works on straight lines of commits. Git runs without an interactive editor; GitSimple's own executable is git's sequence and message editor.

## Split a commit

*Advanced mode.* One commit became two things at once? **Split this commit…** on a commit in History takes it apart without retyping anything.

1. The preview is the [rebase editor](#interactive-rebase-editor)'s: the commits from this one up to the tip of the branch, with this one marked "stops for editing". It warns when a pushed commit is among them. Your working tree must be clean (commit or stash first).
2. **Start split** runs the rebase, which stops at the commit. The banner says so and offers **Open for splitting**: the commit's changes come back into Changes as uncommitted changes, and HEAD moves to the commit before it. (You can also change and amend the commit instead, as at any edit stop.)
3. In Changes, stage the lines for the first piece (whole files, hunks or single lines) and **Commit**; repeat for the next piece until nothing is left. Any number of commits is fine.
4. **Continue** in the banner replays the commits that came after it on top. Continue waits while something is still uncommitted; the banner says so.

Changed your mind halfway? **Abort** in the banner is the rebase abort: the branch goes back to exactly how it was, the pieces you already committed included. The commit itself is never lost: it stays in the reflog until the rebase ends.

## Absorb into earlier commits

*Advanced mode.* You fixed a typo in a line that one of your last commits added, and want the fix inside that commit rather than as a "fix typo" commit of its own. Stage the fix, then **Absorb into earlier commits…** above the staged list in Changes.

- Each staged change (hunk) goes to the newest **unpushed** commit that last changed those lines (`git blame`); a change that adds lines goes where its neighbouring lines came from. Only commits since the branch's upstream (or, without an upstream, any commit of the branch) are candidates: a change whose lines were last touched by a pushed commit, by a merge commit, by the repository's first commit, or by several commits **stays staged**, and the preview lists it with the reason. Files that are new, deleted, renamed or binary stay staged as a whole.
- The preview shows, per commit, the changes it gets ("line 12: +1 −1"), what stays staged, the branch before and after (every commit from the oldest target up gets a new id), the pushed warning when one of them is on a remote, and the backup: the branch's current tip is kept under `refs/gitsimple/backup/<time>/<branch>`, never pushed and not shown in History. Undo (Ctrl+Z) can bring it back too.
- **Absorb** asks once more (a Windows dialog), saves a snapshot of your uncommitted work, backs up the tip, makes one `fixup!` commit per target from exactly its changes and runs the rebase editor's rebase that folds them in (with autostash, so unstaged changes come along). Afterwards the changes that stayed staged are staged again, and unstaged changes are where they were.
- A conflict while folding in (rare: the changes touch lines a later commit changed nearby) stops the rebase like any other: the banner offers Continue and Abort. After an abort the `fixup!` commits are still on the branch; the backup ref has the tip from before. The changes that stayed staged come back unstaged when the rebase ends: stage them again.
- If a step fails before the rebase starts (a commit hook refusing a piece, say), GitSimple puts the branch and your staged changes back as they were and says so; the snapshot and the backup ref have them as well.

## Operation banner

While a rebase, cherry-pick, revert or merge is open, every Code view and Home show a banner with:

- "step N of M" and the commit being applied;
- **Continue** (needs the conflicts resolved), **Skip** and **Abort** (both previews);
- at a rebase stopped for editing: **Open for splitting** (see [Split a commit](#split-a-commit)), in both modes.

A stash apply with conflicts has no continue or abort in git: resolve the files and keep working.

## Conflict resolver

In Changes, select a conflicted file.

- **Base** and the two sides next to each other, and a **Result** editor you can edit by hand. When the result is longer than its box, a strip on its right marks where the conflict blocks still are; click a mark to scroll there (the cursor goes to that line).
- Per conflict: **Keep yours**, **Keep theirs**, **Keep both (yours first)**, **Keep both (theirs first)**.
- Whole file: **Use your whole file** / **Use their whole file**. These also handle binary files, delete/modify and add/add conflicts (a side that deleted the file deletes it).
- **Mark resolved** writes the result and stages it. CRLF and a missing final newline are kept. It is refused while a complete conflict block is left.
- **Open in merge tool** starts the program chosen in Settings, Git (VS Code, P4Merge, KDiff3, Beyond Compare or Meld are offered when found) with temp files, no shell. The button names the tool, for example **Open in VS Code**.
- **Unity files:** with Unity Smart Merge set in Settings, Git, scenes, prefabs and other Unity YAML assets (`.unity`, `.prefab`, `.asset`, `.mat`, `.controller`, `.anim` and more) open in UnityYAMLMerge instead (**Open in UnityYAMLMerge**). It merges what it can understand and hands the rest to the fallback tool in Unity's `mergespecfile.txt`. If conflict blocks are still left when it closes, the resolver says so and shows them, so you can finish by hand. Other files keep the general merge tool. The scene must be saved as text (Unity's default, Force Text); binary assets only get the whole-file choices.
- **Unity `.meta` files with two IDs:** when the two sides give the asset different IDs (GUIDs), the resolver shows **Two IDs for one asset** with how many files use each and which one to keep. See [A .meta conflict with two IDs](unity.md#a-meta-conflict-with-two-ids).

### Which side is which

The sides are named after the operation, with git's word in brackets in the pane titles and as a tooltip. The button labels above follow these names.

| Operation | Git's "ours" | Git's "theirs" |
| --- | --- | --- |
| Merge | Your version | Their version |
| Rebase | Upstream (the branch you rebase onto) | Your commit |
| Cherry-pick | Your version | Picked commit |
| Revert | Your version | Reverted change |
| Stash apply | Your version | Stash |

### Before and after

Merge, rebase, reset (and Undo) and force push previews show the branch **before** and **after**, side by side, newest commit first:

- commits the operation creates (a merge commit, rebased copies) are marked **new**;
- commits the branch loses are struck through (they stay in the reflog for a while; for a force push, they are what the remote branch loses);
- a merge also says what git expects: a fast-forward, a merge commit without conflicts, or the files that will conflict. The prediction looks at the commits only, not at your uncommitted changes, and needs git 2.38 or newer.

> **Rebase is the odd one out:** git calls the branch you rebase onto "ours" and your own commit "theirs". GitSimple names them for what they are.

## Undo

`Ctrl+Z` on a Code view or Home, or the **Undo** button (also called Recent actions).

- Lists recent actions from the reflog of the current branch (or HEAD).
- Undo is a preview of a reset to the entry before it, with the same loss listing as reset.
- Branch switches and the steps inside a rebase are marked as not undoable this way.
- It never resets by itself, and in a text box `Ctrl+Z` stays the normal text undo.

### Snapshots

**Show > Snapshots** in the same dialog lists the uncommitted work GitSimple saved, newest first: what it was saved before, the branch, when, and what wasn't saved.

- **Restore** is a preview: the files that change, and warnings when you are on another commit now. Restoring puts your files and your staged changes back exactly as they were. It saves your current uncommitted work in a new snapshot first, so a restore can be undone too. It doesn't move any branch: to also undo the commits of a pull or a reset, use the reflog list.
- A dropped stash shows as **Dropped stash**; **Put back** returns it to the stash list (your files don't change).
- What is not saved: ignored files, and untracked files over 50 MB (listed on the snapshot and in the result message). When the index had conflicts (an abort of a merge), the files are saved with their conflict markers but the staged state isn't.
- If a snapshot can't be taken (for example another program holds a file open), a Windows dialog asks whether to run the action without one.
- Snapshots are kept for 14 days, 20 at most (and up to 20 dropped stashes). They are commits in your repository under `refs/gitsimple/snapshots` that no branch points at and that are never pushed.

## Tags

*Advanced mode.*

- Annotated or lightweight; target, message, date; pushed or local only (**Check remote**).
- **New tag**, **Push** one or all (never overwrites a different remote tag).
- **Delete** locally, or **Delete on remote** (a preview that re-checks the remote when you confirm).

## Repository

*Advanced mode.* One view with tabs:

| Tab | What you can do |
| --- | --- |
| **Remotes** | List, add, rename, set URL, prune, remove (preview), fetch one. Also **Overwrite remote branch (force push)**: only `--force-with-lease` with `--force-if-includes`, with a preview of the remote commits that would be discarded. Never a plain `--force` |
| **Worktrees** | Add for a new or existing branch, remove (preview; a dirty worktree needs confirming), prune, open as a workspace |
| **LFS** | Shown when Git LFS is installed: tracked patterns (track, untrack, Unity and Unreal presets), the LFS cache and its clean-up (preview), locked files, Lock / Unlock, force-unlock (preview). See [Git LFS](#git-lfs) |
| **Submodules** | Status, update (`--init --recursive`) with progress |
| **Folders** | How this copy was cloned (partial, shallow, sparse), which folders are checked out, LFS downloads for some folders. See below |
| **Clean** | A preview of exactly what `git clean -n` lists (optionally folders and ignored files). Confirming deletes only that list |

Remote addresses must be `https://`, `ssh://` or `user@host:path`. Refused: `ext::`, `fd::`, `file://`, local paths, `http://`, values starting with `-`, and embedded passwords.

**Garbage collection** is in Settings, Git, Repository maintenance (`git gc` with progress, never `--prune=now`).

### Folders: big repositories

For a repository cloned with the big-repository options (see [Cloning a big repository](getting-started.md#cloning-a-big-repository)), or one you want to slim down:

- **This copy** shows whether it is a full, partial or shallow clone and a sparse checkout, with what that means for History, Blame and switching branches. A shallow clone has **Download the full history** (`git fetch --unshallow`).
- **Checked-out folders**: **Check out only some folders…** (or **Change folders…**) opens the repository's folder tree, read from git, so folders that aren't on the disk are listed too. Tick folders (a folder includes everything below it) and apply: git checks out exactly those folders plus the top-level files (`git sparse-checkout set --cone`). Removing a folder takes its files off the disk, except files you changed, which stay. **Don't download LFS files in the added folders yet** keeps their LFS files as pointers. **Check out every file** ends the sparse checkout. A sparse checkout made elsewhere with custom patterns (not folders) is shown but not edited here.
- **LFS files for some folders** (when Git LFS is installed): pick folders and **Download LFS files**, either **Only these folders** or **Everything except these folders** (`git lfs pull --include` / `--exclude`). The folders apply to that download only; your LFS settings are not changed. In a sparse checkout only checked-out folders are downloaded (git-lfs would otherwise also write files into folders you left out), and LFS files in the top-level folder are not part of "everything except".
- Skipped LFS files stay pointers only until git updates them: switching branches or pulling downloads the LFS files of the files it changes, as usual.

**Download the full history** shows progress and can be stopped. Changing folders and downloading LFS files show progress but can't be stopped, because they rewrite the files in your working copy while they download; they stop at the network time limit (Settings, Git). In a partial clone, GitSimple's GitHub sign-in is also used when git downloads older file contents for a view.

**Faster status.** In a repository with very many files, checking for changes can take seconds. When it does, Changes offers **Make it faster**, which turns on [Faster status for big repositories](settings.md#all-repositories-or-this-repository) for that repository: git watches the folder instead of reading every file each time.

Partial clones and sparse checkouts make git record a few settings of its own in the repository (for example `remote.origin.promisor` and `core.sparseCheckout`). These don't run programs, so they don't trigger **Review and trust…**. GitSimple itself still never edits your git configuration.

## Git LFS

Git LFS keeps big binary files (textures, audio, models, video) out of the history everyone downloads: git stores a small pointer and the file goes to the LFS server. GitSimple uses the `git-lfs` you install (`winget install --id GitHub.GitLFS -e --source winget`); without it the LFS tab only shows how to install it.

**Set up once per computer.** Run `git lfs install` in a terminal. It sets up the LFS filter in your git config, and in a repository it also adds the hook that uploads LFS files when you push. GitSimple never changes git config or installs hooks itself, so the LFS tab warns when pushes won't upload LFS files:

- there is no pre-push hook at all (in `core.hooksPath` when your repository sets one, as husky does, else in `.git/hooks`);
- the pre-push hook doesn't run `git lfs pre-push` ("may not upload LFS files"). With husky, add `git lfs pre-push "$@"` to `.husky/pre-push`. Hooks run by other managers (lefthook, pre-commit, older husky) aren't followed and get no warning;
- **Run repository git hooks** is off in Settings › Git.

### Tracking

**Repository › LFS › Tracked with LFS** lists the LFS lines of the `.gitattributes` file at the root of the repository, with a **lockable** tag where it applies.

- **Track**: type a pattern (`*.psd`) or a file (`Art/hero.fbx`), tick **Lockable** if nobody should edit those files without a lock, and press **Track**. GitSimple adds the line `git lfs track` would write (`*.psd filter=lfs diff=lfs merge=lfs -text`) at the end of `.gitattributes`, in the file's own line endings. Nothing else in the file changes.
- **×** next to a pattern stops tracking it (removes that line). Files already committed stay in LFS in those commits.
- **Unity preset…** and **Unreal preset…** preview the usual entries and add only the patterns your `.gitattributes` doesn't mention yet. Unity: textures, audio, video, models, fonts and packages in LFS; scenes, prefabs, assets, `.meta` and the other YAML files as `text eol=lf` (set Unity's Asset Serialization to Force Text). Unreal: `*.uasset` and `*.umap` in LFS and lockable, plus the usual binary types. The Unity preset doesn't add `merge=unityyamlmerge`; [Smart Merge during merges](unity.md#smart-merge-during-merges) does, from its own preview. In a Unity project (an `Assets` folder and `ProjectSettings/ProjectVersion.txt`) the tab suggests the Unity preset.

Tracking changes `.gitattributes`, which shows in Changes: commit it so your team stores the same files in LFS. Files that are already committed are not converted.

### Big files before a commit

The commit box's [safety net](code.md#safety-net-before-a-commit) warns when a file that is about to be committed is big:

- over **100 MB**: GitHub refuses the push, so the warning is red;
- over **50 MB**: GitHub warns, so GitSimple suggests LFS.

**Track \*.wav with LFS** (with the file's extension) adds the pattern and stages the file again, now as an LFS pointer, together with `.gitattributes` (and anything else you changed in `.gitattributes`). The file is staged as it is on disk, so a file that also has unstaged changes is left alone: stage it yourself when it is ready. A file whose type is already tracked but was staged before the pattern existed gets **Stage again for LFS**. If the file is still staged in full afterwards, the LFS filter isn't set up: run `git lfs install` and stage it again. With nothing staged, **Stage all and commit** also counts the big files it would stage. The warning never blocks the commit.

### Big files before a push

The [push preview's safety net](code.md#safety-net-before-a-push) checks the size of every file the pushed commits add or change, so a big file committed outside GitSimple shows up too, under the commit that added it. A file stored with LFS is a small pointer in the commit and is never listed. At 100 MB or more GitHub refuses the whole push: tracking the file with LFS now doesn't help, because the commit already holds the full file. When the commits that hold it aren't pushed yet, **Move into LFS…** right there replaces the push preview with the preview of rewriting them so they hold an LFS pointer instead (see [Moving committed files into LFS](#moving-committed-files-into-lfs)); push again afterwards. Otherwise take it out of that commit first (amend it, or use the [interactive rebase editor](#interactive-rebase-editor) to edit an older one), then track it with LFS. The same preview also lists conflict markers and possible secrets in the pushed commits. It never blocks the push.

### Moving committed files into LFS

Tracking a pattern only changes what you commit next. **Repository › LFS › Move committed files into LFS** also moves files that are already committed. Type one or more patterns (`*.psd *.wav`, a path such as `Art/big.fbx`, or `Art/**` for a whole folder; no spaces or commas inside a pattern) and choose:

- **From now on…**: the matching files git tracks are staged again as LFS pointers, and the patterns are added to `.gitattributes`. The preview lists the files and their sizes. Nothing is committed and history doesn't change: commit the result as usual. Older commits still hold the full files, so the repository doesn't get smaller, and pushing older unpushed commits still sends them in full. Files with their own uncommitted changes are left alone (stage them yourself), and `.gitattributes` must have no uncommitted changes. Files of 1 KB or less are left alone.
- **In unpushed commits…**: the commits of the current branch that no remote-tracking branch has are rewritten (`git lfs migrate import`), so they hold LFS pointers. Nothing you pushed or fetched changes, so no force push is needed: the next normal push sends the new commits. The preview shows how much of each file type moves, every commit that gets rewritten, before and after, any other local branch that moves with it (one whose tip is one of those commits), and the backup. It needs a clean working copy and no merge or rebase in progress. It is refused when a tag (or another ref that isn't a branch) points at one of those commits, because GitSimple never rewrites tags, and when a branch that would move is checked out in another worktree. Windows asks you to confirm.
- **Whole branch, then force push…**: every commit of the current branch is rewritten, pushed ones included, and the branch is force-pushed right after (`--force-with-lease` on the commit you last fetched, plus `--force-if-includes`). Use it when the big files are already on the server. The preview shows each replaced commit before and after, what the remote branch loses, the other branches that move, and the backup, and you type the branch name to confirm. Because it pushes, the preview also runs the [push safety net](code.md#safety-net-before-a-push) over the commits not on the server yet (the files the rewrite moves into LFS aren't sent in full, and it says so). When the branch's history changes GitHub Actions workflows and git pushes with GitSimple's sign-in, the preview warns that GitHub will likely refuse the force push: the rewrite re-sends those commits, and GitSimple's sign-in doesn't have the "workflow" permission (see [Safety net before a push](code.md#safety-net-before-a-push)). It is refused when the branch has no remote branch, when the remote branch has commits you haven't pulled, when some of its commits are also on another remote branch (the branch it was started from, say: the two would no longer share any history), when a local branch that would move along has its own remote branch, when a tag points at one of the commits, or when pushes wouldn't upload LFS files (no LFS pre-push hook, or "Run repository git hooks" off).

  **Everyone else must start over from the new commits.** Tell your team before you do it. Each teammate first pushes or saves any work they have on the old commits, then either clones again, or fetch and reset the branch to the remote one (`git fetch` then `git reset --hard origin/<branch>`, which discards their local commits on it). Branches started from the old commits must be rebased onto the new ones. Pull requests from this branch show the new commits. The old commits (and their big files) stay on GitHub until GitHub cleans them up, and in every clone that still has them.

After either rewrite the old commits are kept under `refs/gitsimple/backup/<time>/<branch>`, a ref nothing pushes, so you can reset back to them. They also keep the old full files in your `.git` folder until you delete that ref. The files in your working copy keep their content (GitSimple runs `git lfs checkout` after the rewrite). Other local branches that were started from the rewritten commits keep the old ones; rebase them if they need the change. Rewritten commits aren't signed: if the repository signs its commits, the preview says so, and a branch that must have signed commits on GitHub refuses the push. The backup refs, like Undo's snapshots, never show in History's "All branches".

The console doesn't run `git lfs migrate`.

### Locks

A lock tells your team you are editing a file nobody can merge (a scene, a prefab, a texture). Locks live on the LFS server (for GitHub, github.com), so taking and releasing one needs the network and your sign-in.

- **Repository › LFS › LFS locks** lists every lock with its owner and age. **Lock** takes one by path; **Unlock** releases yours; somebody else's lock has **Force unlock…**, a preview.
- In **Changes**, a changed file shows a padlock when it is locked (yours in the accent colour, someone else's in amber). Files stored in LFS or marked lockable get **Lock to edit** (an open padlock when you point at the row) while nobody holds them. Git LFS only unlocks a file with no uncommitted changes, so release your lock after you commit and push.
- In **Files**, locked files have a padlock and the file view has **Lock** / **Unlock**.
- **Home › My locks** lists the locks you hold and for how long, with **Unlock**. A file with uncommitted changes shows "commit first" instead.
- **After a push**: when the push sent changes to files you hold a lock on, and those files have no new changes, GitSimple asks "Unlock N files you just pushed?" with every file ticked. Untick what you are still working on. Nothing is unlocked unless you confirm.
- When you have changed a file somebody else has locked, Changes says who locked it and when. Their edit and yours can't be merged: ask them to push and unlock, or discard your changes to it.

GitSimple asks the LFS server for the locks every 5 minutes while the window has focus (only in repositories that use LFS), and **Refresh** asks at once. Your own locks show as soon as you take them.

Git LFS makes **lockable** files read-only in your working copy until you lock them (when its hooks are installed). Lock the file first, then open it in your editor or engine.

### Cleaning up the LFS cache

Git LFS keeps a copy of every LFS file you have downloaded or committed in the repository's `.git/lfs/objects` folder, so a project with big textures can fill the disk over time. **Repository › LFS › LFS cache on this computer** shows how big it is.

**Clean up…** asks Git LFS (`git lfs prune --dry-run`) which copies it no longer needs and shows them in a preview: how many files, how much space, and what Git LFS counted. Confirm and the copies are deleted (`git lfs prune`). Git LFS never deletes:

- the files of what is checked out, of your stashes and of your other worktrees;
- the files of recent branches and commits (by default the last 7 days of branches, plus 3 days; the `lfs.fetchrecent*` and `lfs.pruneoffsetdays` settings, which GitSimple doesn't change);
- the files of commits you haven't pushed yet (to `origin`, or the remote in `lfs.pruneremotetocheck`).

**Check the server first** is on by default and recommended: every file is looked up on the LFS server before its local copy goes, also a file only the reflog (an amended or reset commit), an [Undo](#undo) snapshot or a kept stash still refers to. A file the server doesn't have is kept, and the preview says how many. It needs the network and your sign-in, and Git LFS 3.4 or later. Untick it to clean up offline: then a file only the reflog, a snapshot or a kept stash refers to, from work you never pushed, is lost for good, and restoring that snapshot or stash can't bring the file back.

Running it checks again that the list is the same as in the preview, and Windows asks you to confirm. A deleted copy is downloaded again from the server when a checkout needs it. A custom `lfs.storage` folder isn't measured. The console doesn't run `git lfs prune`.

## The git console

*Advanced mode, and only while Settings, Git, Enable the Console is on.*

Type a git command and it runs in the repository through the same hardened runner as the rest of the app. A shell is never started.

- Arguments are split like a shell would (single and double quotes, `\ ` for a space), but nothing is expanded and pipes, redirects, `;`, `&&` and backticks are refused.
- Output streams in. **Stop** works for network commands. Up / Down walk the history, Ctrl+L clears.
- Commit messages never open an editor: a commit without `-m` fails at once.

Every command falls into one class:

| Class | Examples | What happens |
| --- | --- | --- |
| **read** | status, log, diff, show, blame, branch / tag / remote lists, `config --get`, `stash list`, `ls-remote` | Runs at once |
| **write** | add, commit (not `--amend`), switch, fetch, stash push / pop, cherry-pick, revert, `tag`, creating a branch | Runs at once |
| **confirm** | reset, clean, every push, pull, merge, rebase, `branch -d/-D`, `stash drop/clear`, overwriting `checkout`/`restore`, `rm`, `worktree remove`, `remote remove/prune`, `gc --prune=now`, `reflog expire`, `config` writes | A Windows dialog (shown by the app, not the page) lists the exact command and why. Runs only if you press Continue |
| **blocked** | Anything not on the allowlist (see below) | Never runs: "not available in the GitSimple console; use a terminal" |

<details>
<summary>What the console blocks</summary>

Unknown commands (including aliases, other letter case such as `STATUS`, and external `git-<name>` programs); options a command's table does not list (`--output`, `-O`, `--ext-diff`, `--textconv`, `--no-index`, `--upload-pack`, `--exec`, `-e`, …); global `-c` / `-C` / `--git-dir` / `--work-tree`; `ext::` / `fd::` / `file://` addresses; `clone`, `init`, `apply`, `am`, `archive`, `format-patch`, `bundle`, `bisect run`, `submodule foreach`, `credential*`, `hook`, `help`; `config` writes outside a short safe list; `worktree add` into system, start-up or key folders.

The allowlist is tested case by case, including the security review's attack lines and a fuzz test.

</details>

## "Review and trust…"

A repository's own `.git/config` can name programs that git runs: a cleaning `filter`, `core.sshCommand`, `remote.origin.uploadpack`, a `!` alias, credential helpers, proxies, URL rewrites, and more. GitSimple runs git by itself (status, auto-fetch), so a folder copied or cloned from elsewhere could run such a program without you typing anything.

When GitSimple finds settings like these:

1. It runs **no git command** in that repository and shows a red banner on the Code views.
2. **Review and trust…** opens a Windows dialog (shown by the app) listing every such setting and its value.
3. Press **Continue** only if you set them up or know where the repository came from.
4. GitSimple remembers exactly that set for the workspace. If the settings change, you are asked again.

Some standard values get a label in the dialog and in the banner's tooltip, so an ordinary game repository is easier to judge:

- `filter.lfs.clean = git-lfs clean -- %f`, `filter.lfs.smudge = git-lfs smudge -- %f` and `filter.lfs.process = git-lfs filter-process` (what `git lfs install --local` writes): `Standard Git LFS setup (runs git-lfs from PATH)`.
- A `merge.<name>.driver` that runs `UnityYAMLMerge.exe` from a Unity Editor's `Editor/Data/Tools` folder with the usual arguments (`merge -p %O %B %A %A`): `Unity Smart Merge (runs C:/.../UnityYAMLMerge.exe)`, with the full path.

Only exact values are labelled: anything added to them, such as `git-lfs smudge -- %f; calc`, is shown without a label. A label only explains the setting. These settings still run a program, so you still have to trust them like any other.

Auto-fetch never asks; it skips the repository until you trust it. The push preview always shows the address the push really goes to, and changing a remote's address in the console shows the old and the new one.

Two more things are reviewed the same way, each with a dialog of its own and a separate memory, so a change to one never pauses git: the [checks before a push](code.md#checks-before-a-push) (what they run and the files that decide it) and the repository's [team settings file](settings.md#team-settings-file) (**Use this repository's team settings?** lists each setting the file provides and the keys it has that GitSimple ignores).
