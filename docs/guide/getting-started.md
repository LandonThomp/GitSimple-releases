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

### Signing in to GitHub Enterprise Server

If your company runs its own GitHub (GitHub Enterprise Server), you can use its repositories too, with or without a github.com account.

1. Your administrator registers an OAuth app for GitSimple on the server, with **Enable Device Flow** turned on, and gives you its **client ID**.
2. On the sign-in screen click **GitHub Enterprise Server** (or, once signed in, go to Settings, Account, **Add a server**). Enter the server's address (`https://github.example.com`) and the client ID, then **Add server**. GitSimple asks you to confirm in a Windows dialog, because it lets the app contact that server.
3. Click **Sign in to github.example.com** and enter the code on the server, as for github.com.

| | |
| --- | --- |
| One sign-in per server | Each server has its own token in Windows Credential Manager. Your github.com sign-in is never sent to a server, and a server's sign-in never leaves that server (git included). |
| Fallback | `gh auth token --hostname <server>` when nothing is stored. |
| Invite-only | The whole server is invited: everyone it signs in may use GitSimple once the server is on the invite list. |
| Issues | Label mode only for now: GitHub Projects on a server come later. |
| Features the server doesn't offer | Hidden, not shown as errors or greyed out: in a repository on a server there is no Project choice in Settings, Issues (label mode only), no **Sprints** in the sidebar, no **Show log** on a check (re-runs work), and the **Inbox** and its settings are hidden while the app is open on the server's sign-in (notifications come from github.com). |
| Not supported yet | GitHub Enterprise Cloud with data residency (`*.ghe.com`), servers on a port other than 443. |

Repositories on the server are added like any other: from a local clone whose `origin` is on the server, or under **From GitHub** after choosing the server.

## Adding a repository

Use **Add a repository**, then either:

- **Local folder**: a clone that is already on your PC, or
- **From GitHub**: pick one of your repositories, then **Clone to folder…**.

A repository added without a clone still has its issue views. The Code views show **Clone this repository / Locate existing clone** until there is one.

### Switching from the taskbar

Right-click GitSimple on the taskbar (or in the Start menu) to see **Recent repositories**, the ones you opened last first. Picking one opens GitSimple on that repository; if GitSimple is already running, it switches the open window instead of starting a second one. The list follows the repositories you open, add, rename and remove. Windows decides how many entries it shows, and hides the list when recent items are switched off (Settings > Personalization > Start, the switch that mentions Jump Lists). An entry you remove with **Remove from this list** comes back the next time you open that repository.

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

It shows, top to bottom:

- the active branch in the header, with how far ahead / behind it is;
- the **Next step** card: one button for what to do next (see below). Under it, only the lines that say something about the branch: **Put-away changes** (the newest stash, with **Bring back**; see [Stashes](advanced-git.md#stashes)), an operation in progress, **Same files** when other open pull requests change files this branch changed (see [Pull requests](code.md#other-pull-requests-that-change-the-same-files)), the branch's pull request, and uncommitted changes when the next step is something else;
- **My locks**, when you hold Git LFS locks in this repository: each file, how long you've held it, and **Unlock** (a file with uncommitted changes says "commit first": Git LFS only unlocks a file with none). See [Git LFS](advanced-git.md#locks);
- **Waiting on you**, across every repository you added (works without a clone): pull requests waiting for your review, your pull requests with changes requested, failed checks, or approved and ready to merge, and issues assigned to you. Most urgent first; the first 5 show, **Show all** shows the rest. Click one to open it (GitSimple switches repository first when needed). When nothing waits on you, it says you're all caught up;
- in Advanced mode, **My open issues** and **Recently merged** (pull requests merged in the last week);
- at the bottom, **Read the README** (it opens [Docs](#docs)) and **Other repositories**, which unfolds [All repositories](#all-repositories) when you have added more than one;
- the trust banner ("Review and trust…") and the operation banner when they apply.

`Ctrl+Z` opens Recent actions (undo) here too.

### Docs

**Docs** shows the repository's own documentation, read from your clone: the `README.md` in the top folder and the Markdown pages of a `docs` (or `Docs`) folder, up to three folders deep. Open it with **Read the README** on Home, or type "docs" in the command palette (`Ctrl+K`).

- The pages are listed on the left, README first, then the docs folder's pages and each subfolder's.
- Links between pages, and to headings (`#section`), open in Docs. https links open in your browser. A link to another file in the repository opens Files; links that leave the repository do nothing.
- Pictures stored in the repository show. Pictures from other websites (such as status badges) and HTML in the page show their description or are left out.
- **Open in editor** opens the page in your editor to change it.
- The list follows the files committed to the branch; a page shows its text as it is in your working folder.

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

### All repositories

Click **Other repositories** at the bottom of Home to unfold **All repositories**. It lists every repository you added, one row each:

- its next step, by the same rules as the Next step button (for example **Commit 3 changes** or **Pull 2 new commits**). For a repository that isn't the active one, GitSimple doesn't know its default branch, so it never suggests **Create pull request** there; it does show **Open pull request #N** when you have an open pull request for that branch;
- **Review trust** instead of a step when the clone's own git settings can run programs: GitSimple doesn't run git there until you open it and review them (see ["Review and trust…"](advanced-git.md#review-and-trust));
- **Folder not found** when the clone's folder is gone, **Clone this repository** when there is no clone;
- counts of pull requests waiting for your review, your open pull requests (with the worst check result) and open issues assigned to you. The arrow on the right shows them; GitHub's open / closed state only.

Repositories with something to do come first. The first 20 are shown, **Show more** shows the rest.

Click a row to switch to that repository: it opens where its step is done (Changes, Branches, the pull request, or Home). Click an issue or pull request to switch and open it.

The list refreshes when you come back to the window (GitHub at most once a minute) and with **Refresh**. Each GitHub is asked with its own sign-in: repositories on a GitHub Enterprise Server use that server's sign-in, never github.com's. When you aren't signed in to a server (or it was removed from Settings, Account), a line says so and its repositories show their local state only.

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
| Commit box | **Co-authors** and **Add to last commit (amend)** (they stay while a draft has co-authors or an amend is on) |
| Branches | The **Remote** list starts folded; click its heading to open it. A filter shows matching remote branches anyway |
| Inbox | Notifications from repositories you only watch are folded under a count at the bottom |
| Changes | **Write the rule to** `.git/info/exclude` in the Ignore menu (the `.gitignore` choices stay) |
| Board | **Add column** in label mode (it creates a status label GitSimple can't delete again) |
| Issues and Board | **Group**, **Display**, saved views and the Board's **Columns** menu (search, whose issues, open/closed, **Filters** and **Sort** stay). They show anyway while a grouping, display choice or hidden column set in Advanced is in effect, so you can undo it |
| Settings | The **Advanced** groups start collapsed (the Issues one opens by itself when a field inside it has an error): in Git, the pull strategy, diff context and whitespace, and **Identities** (open once you have one); in Account, **GitHub Enterprise Server** (open once you added a server); in Network, the proxy and certificates. The settings search still finds and opens them |

### Hidden is not disabled

- Nothing is turned off. A link from a banner or another view still opens a hidden view.
- Recovery is always there in both modes: the operation banner, the conflict resolver, Undo / Recent actions and every preview.
- What Basic lets you put away comes back in Basic: stashes made with **Stash…** or **Stash changes and switch** show as **Put-away changes** in Changes and on Home.
- Switching to Basic while on a hidden view takes you to Home.
