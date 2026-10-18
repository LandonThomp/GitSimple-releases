[User guide](README.md) › Getting started

# Getting started

Sign in, add a repository, and let **Home** tell you what to do next.

## Signing in

GitSimple uses GitHub's **device flow**:

1. Click **Sign in with GitHub**. Your browser opens `github.com/login/device` and the app shows a short code.
2. Enter the code on GitHub and approve the requested scopes: `repo`, `read:org`, `project`.
3. The app notices the approval and continues.

| | |
| --- | --- |
| Where the token lives | **Windows Credential Manager** only. Never in a file, browser storage or a log. |
| Client secret | None. The OAuth client ID is public. |
| Fallback | If nothing is stored and the GitHub CLI is signed in, GitSimple uses `gh auth token --hostname github.com`. |
| Sign out | Settings, Account. |

> **Invite-only.** After sign-in, GitSimple checks your GitHub username against its invite list. If you're not on it, you see "GitSimple is invite-only": ask the person who shared the app to add you, then click **Check again**, or **Use another account**.

> **Organization repositories missing?** An owner may need to approve GitSimple under the organization's Settings, Third-party access.

## Adding a repository

Use **Add a repository**, then either:

- **Local folder**: a clone that is already on your PC, or
- **From GitHub**: pick one of your repositories, then **Clone to folder…**.

A repository added without a clone still has its issue views. The Code views show **Clone this repository / Locate existing clone** until there is one.

### Cloning a big repository

Game projects and monorepos can be too large to download whole. **Clone to folder…** has **Options for big repositories** (closed by default; with every option off you get a normal full clone):

| Option | What you get | The trade-off |
| --- | --- | --- |
| **Partial clone** | The files of the commit you check out, and the rest of the history without file contents (`--filter=blob:none`) | Older file versions are downloaded when a view needs them, so History, Blame and switching branches need the network and can be slower |
| **Shallow clone** | Only the last N commits of every branch (`--depth`) | History, Blame and Undo stop at the oldest downloaded commit, and merging with an older branch may fail. **Download the full history** fixes that later |
| **Only some folders** | A sparse checkout: the top-level files plus the folders you list, one per line (for example `Assets/Characters`) | The other folders aren't on your disk until you add them. Commits, branches and pushes still cover the whole repository |
| **Skip LFS files for now** | LFS files (textures, models, audio) stay small pointer files | A tool that opens a pointer file sees text, not the asset, until you download the LFS files of that folder |

You can combine them. Everything after the clone (changing folders, downloading LFS files for some folders, the full history) is in **Repository**, **Folders** in Advanced mode; see [Folders: big repositories](advanced-git.md#folders-big-repositories).

## Home

**Home** is at the top of the sidebar (or `G` then `D`). A new install opens here; an existing install keeps the view it was on.

It shows:

- the active branch, with how far ahead / behind it is;
- a **Status** card: uncommitted changes, an operation in progress, the branch's pull request;
- **My locks**, when you hold Git LFS locks in this repository: each file, how long you've held it, and **Unlock** (a file with uncommitted changes says "commit first": Git LFS only unlocks a file with none). See [Git LFS](advanced-git.md#locks);
- **My open issues** (works without a clone);
- the trust banner ("Review and trust…") and the operation banner when they apply.

`Ctrl+Z` opens Recent actions (undo) here too.

### How the next step is chosen

The **Next step** button shows the first rule that matches:

| # | When | Next step |
| --- | --- | --- |
| 1 | No local clone | **Clone this repository** |
| 2 | A merge, rebase, cherry-pick, revert or stash apply is open | **Finish the merge** (or rebase, …) |
| 3 | Files have conflicts | **Resolve N files with conflicts** |
| 4 | Not on a branch (detached HEAD) | **Switch to a branch** |
| 5 | Uncommitted changes | **Commit N changes** |
| 6 | Behind the upstream | **Pull N new commits** |
| 7 | Ahead of the upstream | **Push N commits** |
| 8 | The branch has no upstream yet | **Publish branch** |
| 9 | Published, not the default branch, no open pull request (none, or only a closed one) | **Create pull request** |
| 9 | …with an open pull request | **Open pull request #N** |
| 10 | Anything else | **All caught up** |

Notes:

- A stash apply that stopped on conflicts skips rule 2 and goes to rule 3.
- Rule 9 waits until the pull request lookup has finished.
- Pull and Push show their usual preview first. The other steps take you to Changes, Branches or the pull request.

## Basic and Advanced mode

GitSimple starts in **Basic**. Switch in **Settings, Appearance, Interface**, or with **Switch to Advanced mode / Switch to Basic mode** in the command palette (`Ctrl+K`).

- A new install starts in **Basic**.
- Installs from before this option are set to **Advanced**, so nothing disappears.
- It is one setting for the whole app, stored next to the other appearance settings (not a secret).

### What Basic hides

| Where | Hidden in Basic |
| --- | --- |
| Views (sidebar, palette, `G` shortcuts, shortcut list) | Stashes, Tags, Repository, Console, Sprints |
| History | Cherry-pick, reset, both rebases, Create tag, selecting several commits |
| Branch row | **Choose remote branch (upstream)…**, **Rebase current branch onto this branch…**, **Interactive rebase onto this branch…** |
| Toolbar | The force push menu next to Push |
| Settings | The **Advanced** groups start collapsed (the Issues one opens by itself when a field inside it has an error) |

### Hidden is not disabled

- Nothing is turned off. A link from a banner or another view still opens a hidden view.
- Recovery is always there in both modes: the operation banner, the conflict resolver, Undo / Recent actions and every preview.
- Switching to Basic while on a hidden view takes you to Home.
