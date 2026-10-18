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

## Home

**Home** is at the top of the sidebar (or `G` then `D`). A new install opens here; an existing install keeps the view it was on.

It shows:

- the active branch, with how far ahead / behind it is;
- a **Status** card: uncommitted changes, an operation in progress, the branch's pull request;
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
