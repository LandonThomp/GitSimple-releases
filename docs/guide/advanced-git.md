[User guide](README.md) › Advanced git

# Advanced git

Stashes, rewriting history, conflicts, undo, tags, remotes and the console. Most of these are in **Advanced mode**; the safety checks are the same in both modes.

## How GitSimple keeps you safe

Everything that can lose work:

1. shows a **preview** first: the exact commits, files or entries affected;
2. runs **once**;
3. is **re-checked** when you confirm. The plan is refused if HEAD, a ref, your uncommitted changes, the list of files, the stash list or the remote tag changed meanwhile.

Previews belong to the GitHub login that made them. Actions that discard data, rewrite or delete branches, or change a remote also ask in a Windows dialog right before they run.

## Stashes

*Advanced mode.* Put changes aside and bring them back later.

- The list shows message, branch, date and file count; open a stash for its files (untracked ones too) and their diffs.
- **New stash**: message, include untracked, keep index, or only selected files. Also **Stash…** in Changes.
- **Apply**, **Pop**, **Branch…**.
- **Drop** and **Clear all** are previews.
- A conflicting apply or pop goes to the [conflict resolver](#conflict-resolver).
- The branch-switch dialog offers **Stash changes and switch** and **Bring changes along**.

## History actions

Right-click a commit in History, or use its "…" menu.

| Action | Notes |
| --- | --- |
| **Undo this commit's changes (revert)** | Basic and Advanced. A merge commit asks which parent to keep |
| **Create branch here**, **Copy SHA** | Basic and Advanced |
| **Copy onto current branch (cherry-pick)** | One commit or a Shift-click range, optional `-x` |
| **Move current branch to this commit (reset)…** | Soft, mixed or hard. The preview lists commits no branch will have any more and, for hard, the uncommitted changes that are lost. **Hard needs the branch name typed** |
| **Create tag here** | |
| **Rebase current branch interactively from here…** | Opens the [rebase editor](#interactive-rebase-editor) |
| **Rebase current branch onto here…** | |

On a branch row: **Rebase current branch onto this branch…** and **Interactive rebase onto this branch…**.

## Interactive rebase editor

1. Pick a base: a commit, or "onto" a branch.
2. Reorder by dragging (or Alt+Up / Alt+Down).
3. Choose per commit: pick, reword (edit inline), edit (stops so you can amend), squash, fixup, drop.
4. **Review** shows the commits before and after, and warns when commits that are already pushed would be rewritten.

Options: "Arrange fixup! and squash! commits (autosquash)" and "Set changes aside during a rebase (autostash)".

Merge commits in the range are refused: the editor works on straight lines of commits. Git runs without an interactive editor; GitSimple's own executable is git's sequence and message editor.

## Operation banner

While a rebase, cherry-pick, revert or merge is open, every Code view and Home show a banner with:

- "step N of M" and the commit being applied;
- **Continue** (needs the conflicts resolved), **Skip** and **Abort** (both previews).

A stash apply with conflicts has no continue or abort in git: resolve the files and keep working.

## Conflict resolver

In Changes, select a conflicted file.

- **Base** and the two sides next to each other, and a **Result** editor you can edit by hand.
- Per conflict: **Keep yours**, **Keep theirs**, **Keep both (yours first)**, **Keep both (theirs first)**.
- Whole file: **Use your whole file** / **Use their whole file**. These also handle binary files, delete/modify and add/add conflicts (a side that deleted the file deletes it).
- **Mark resolved** writes the result and stages it. CRLF and a missing final newline are kept. It is refused while a complete conflict block is left.
- **Open in merge tool** starts the program chosen in Settings, Git (VS Code, P4Merge, KDiff3, Beyond Compare or Meld are offered when found) with temp files, no shell.

### Which side is which

The sides are named after the operation, with git's word in brackets in the pane titles and as a tooltip. The button labels above follow these names.

| Operation | Git's "ours" | Git's "theirs" |
| --- | --- | --- |
| Merge | Your version | Their version |
| Rebase | Upstream (the branch you rebase onto) | Your commit |
| Cherry-pick | Your version | Picked commit |
| Revert | Your version | Reverted change |
| Stash apply | Your version | Stash |

> **Rebase is the odd one out:** git calls the branch you rebase onto "ours" and your own commit "theirs". GitSimple names them for what they are.

## Undo

`Ctrl+Z` on a Code view or Home, or the **Undo** button (also called Recent actions).

- Lists recent actions from the reflog of the current branch (or HEAD).
- Undo is a preview of a reset to the entry before it, with the same loss listing as reset.
- Branch switches and the steps inside a rebase are marked as not undoable this way.
- It never resets by itself, and in a text box `Ctrl+Z` stays the normal text undo.

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
| **LFS** | Shown when Git LFS is installed: locked files, Lock / Unlock, force-unlock (preview). Lock badges appear in Files |
| **Submodules** | Status, update (`--init --recursive`) with progress |
| **Clean** | A preview of exactly what `git clean -n` lists (optionally folders and ignored files). Confirming deletes only that list |

Remote addresses must be `https://`, `ssh://` or `user@host:path`. Refused: `ext::`, `fd::`, `file://`, local paths, `http://`, values starting with `-`, and embedded passwords.

**Garbage collection** is in Settings, Git, Repository maintenance (`git gc` with progress, never `--prune=now`).

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

Auto-fetch never asks; it skips the repository until you trust it. The push preview always shows the address the push really goes to, and changing a remote's address in the console shows the old and the new one.
