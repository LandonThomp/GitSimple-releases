[User guide](README.md) › Settings

# Settings

Every setting, what it does, and its default.

## Finding a setting

The search box at the top of Settings finds a setting by its label or by git words, for example "autostash", "fast-forward", "cherry-pick" or "upstream".

- Arrow keys move through the results, `Enter` picks the first, `Esc` clears.
- Picking one opens its section, opens the Advanced group if it is inside one, and focuses it.

Settings marked **Advanced** below sit in a collapsed **Advanced** group in Basic mode.

## Issues

*Per workspace.*

| Setting | Default | What it does |
| --- | --- | --- |
| Mode and Project | Label mode | A Project owner (organization or user) and number turn on project mode |
| Field mapping | Auto-detect | Status, Priority, Sprint, Estimate: found by name, a chosen Project field, or none |
| Issue key prefix | `#` | `#42` or `GS-42`; also used by search |
| Closing statuses · *Advanced* | Done: completed · Canceled / Cancelled: not planned · Duplicate: duplicate | Which statuses close an issue when it is moved there, and with which reason |
| Status / priority label prefix · *Advanced* | `status: ` / `priority: ` | Label mode: labels starting with the prefix become columns / priorities. Empty turns it off |
| Hide the issue key at the start of titles · *Advanced* | Off | Shows "GS-42: Fix it" as "Fix it" (only the issue's own key) |
| Allow editing sprints · *Advanced* | Off | Create, rename and re-date sprints. GitHub replaces the whole sprint list on every edit; GitSimple always sends all sprints so none is lost |
| Start work when I create a branch for an issue · *Advanced* | Off | Moves the issue to a chosen status and assigns you |
| New pull requests start as drafts · *Advanced* | Off | Create pull request starts with Draft on; otherwise it starts with the reviewers below |
| Offer "Ready for review" · *Advanced* | On | The issue panel's Ready for review: marks the draft ready, asks the reviewers below and moves the issue to the chosen status (or leaves the status) |
| Reviewers to ask · *Advanced* | None | User names, comma separated, up to 15; added to the reviewers already asked |
| Offer "Clean up" after the merge · *Advanced* | On | One previewed step after a merge: close the issue as completed if GitHub didn't, delete the branch on GitHub and here, switch to the base branch and pull it. See [From issue to merge](code.md#from-issue-to-merge) |
| Refresh interval · *Advanced* | 60 seconds | How often issues refresh while the window is focused; 0 = off. After a rate limit the app waits until it resets |

## Git

### All repositories or this repository

**Applies to** at the top switches between **All repositories** (the settings below) and **This repository** (the repository open now). In This repository you can give that repository its own editor, faster status, pull strategy, automatic fetch interval, git hooks switch, rebase defaults (autostash, autosquash), default reset mode, commit message template and branch name template. Each row says **Same as all repositories** until you change it; then it says **Changed for this repository** and has **Reset**, which goes back to the all-repositories value. **Save** stores them with the repository; **Reset all to all repositories** clears every one. A changed setting applies only to that repository, and a later change in All repositories doesn't touch it. When the open repository has its own settings, All repositories says how many, with a link to them. They are part of a [settings export](#import-and-export), except the editor (a program on this computer) and faster status (it depends on this computer's git and disk).

The repository's **Editor** is chosen like the app-wide one: **Browse** (only the supported editors are accepted) or one of the editors found on this computer; a path can't be typed in. Browse and a found editor save it at once; **Reset** goes back to the editor for all repositories when you save. Use it for example to open a Unity project in Rider or Visual Studio while other repositories keep VS Code.

**Faster status for big repositories** (This repository, Advanced) makes git watch the repository's folder for changes (file system monitor) and remember folders without new files (untracked cache), so checking for changes doesn't read every file again. GitSimple passes this to its own git commands only; nothing is written to the repository's settings, and your terminal is unaffected. It needs Git 2.37 or later and a folder on this computer, not a network drive; the row says so when that isn't the case. When status is slow (about 1.5 seconds or more, three times running), **Changes** offers it with **Make it faster**; if status is still slow with it on, Changes says so, since it doesn't help on every disk. **Not now** hides the offer until GitSimple restarts. If you set `core.fsmonitor` to `true` in the repository yourself, GitSimple uses the watcher there too.

### Everyday

| Setting | Default | What it does |
| --- | --- | --- |
| Use GitSimple's GitHub sign-in for git | On | github.com https remotes use your GitSimple sign-in (passed over a pipe, never in arguments, environment or files). Off: Git Credential Manager and your own helpers. Other hosts always use yours |
| Fetch automatically | 5 minutes | On window focus and every N minutes; 0 = off. Also while minimised or in the tray when **New commits on the upstream** (Notifications) is on |
| Default clone folder | `%USERPROFILE%\source\repos` | Where **Clone to folder…** starts |
| Editor | First one found | What **Open in editor** starts: VS Code, VS Code Insiders, Cursor, VSCodium, Windsurf, Sublime Text, Notepad++, JetBrains Rider or Visual Studio (Rider and Visual Studio are found in their versioned install folders, newest first; Visual Studio opens a file without jumping to a line). A repository can have its own editor under **This repository**. Pick with Browse or from the editors found; a path can't be typed in. Clear returns to "first one found" |
| Diff layout / wrap long lines | Unified, off | Also switchable in the diff header |
| Syntax highlighting | On | Colours code by language in diffs, Files (content and blame) and the conflict resolver. Files over 512 KB or 20,000 lines are always shown plain; turn it off on a slow machine |

### Identities (collapsed in Basic mode until you have one)

The names and emails you commit as, for example a work and a personal one (**Settings > Git > Identities**, saved with the other Git settings). Each repository chooses one under **Settings > Workspaces > Commit as**; choosing saves at once and changes nothing in git yet. **Apply to repository…** (on the active workspace, or **Apply…** in the commit box) shows the old and new `user.name` and `user.email` and, when you confirm, writes them into that repository's `.git/config`. Your global configuration is never touched, and this is the only git configuration GitSimple writes. If `author.name` / `author.email` or the `GIT_AUTHOR_NAME` / `GIT_AUTHOR_EMAIL` environment variables are set, they win over `user.*` (as in git) and GitSimple says so instead of offering to apply. With no identity chosen, commits use whatever your git configuration says. Removing an identity clears it from the workspaces that used it (the repository's `.git/config` keeps what was written).

Signing is not a GitSimple setting: it follows `commit.gpgsign` in your git configuration (see [Changes](code.md#changes)).

### Advanced

| Setting | Default | What it does |
| --- | --- | --- |
| Pull strategy | Merge | Merge, Rebase, or "Only when nothing needs combining" (fast-forward only). A rebase pull with conflicts stays open: resolve in Changes, then continue or abort from the banner |
| Diff context lines | 3 | Unchanged lines shown around each change. Also in the diff header |
| Whitespace changes | Show | **Ignore amount** (`git diff -b`) or **Ignore all** (`git diff -w`). Also in the diff header. While whitespace is hidden, hunks and lines can't be staged one by one. See [Reading a diff](code.md#reading-a-diff) |
| Run repository git hooks on commit and push | On | Hooks run for commit, amend, merge, pull and push. Status, diff and log never run hooks |
| Refresh changes | 10 seconds | Status check while the window is focused; 0 = off |
| Network time limit | 30 minutes | Clone, fetch, pull and push stop after this |
| Commit message template | `{summary}\n\n{body}` | Placeholders `{summary}` (required), `{body}`, `{issue}` |
| Branch name template | `{type}/{issue}-{slug}` | Placeholders `{type}`, `{issue}`, `{key}`, `{slug}`, `{user}`; live preview |
| Branch type rules *(per workspace)* | `bug` gives `fix`, otherwise `feat` | Rules on issue type or label, first match wins |
| Commits that belong to an issue *(per workspace)* | Key in front of the summary | None, `Refs #N`, `Fixes #N`, or the key in front of the summary; live preview |
| Commit message helpers *(per workspace)* | All off | **Conventional Commits** (a type and scope picker; edit the types, `feat, fix, docs, ...` by default, and suggested scopes), **Check the message format** (soft hints; wrap width 72), **Recent summaries**. See [Changes](code.md#changes) |
| Safety net rules *(per workspace)* | None | Your own checks before a commit and a push; see [Safety net rules](#safety-net-rules) |
| Checks before push *(per workspace)* | None | Your tests or linters, run in the push preview; see [Checks before push](#checks-before-push) |
| Pull request title / body templates *(per workspace)* | `{title}` / `{commits}\n\n{closes}` | Placeholders `{title}`, `{issue}`, `{key}`, `{branch}`; body also `{commits}`, `{closes}`; live preview |
| Merge tool | None | Pick with Browse or from the tools found: VS Code, P4Merge, KDiff3, Beyond Compare, Meld, WinMerge, TortoiseGitMerge. A path can't be typed in |
| Unity Smart Merge | None | UnityYAMLMerge for Unity scenes, prefabs and other YAML assets (`.unity`, `.prefab`, `.asset`, `.mat`, `.controller`, `.anim` and more). Pick with Browse (`UnityYAMLMerge.exe` only) or from the Unity Editors installed with Unity Hub: the repository's Unity version comes first (from `ProjectSettings/ProjectVersion.txt`), then the newest. Other files keep the merge tool above; with this empty, Unity files use it too |
| Default reset mode | Mixed | The mode reset and Undo start with. A hard reset always needs the branch name typed |
| Confirm before… | Cherry-pick, Revert, Pushing tags: on · Applying or popping a stash: off | Extra "are you sure" for medium-risk actions. Actions that can lose data always preview, whatever this says |
| Rebase defaults | Autostash on, autosquash off | "Set changes aside during a rebase (autostash)" and "Arrange fixup! and squash! commits (autosquash)" |
| Enable the Console | On | Show the Console view and allow its commands. Basic mode hides the view regardless |
| Path to git.exe (optional override) | Automatic | Pick with Browse: a local `git.exe` in a Git for Windows folder that answers `git --version`. Clear returns to automatic |
| Repository maintenance | – | **Run garbage collection** (`git gc`, never `--prune=now`) |

### Safety net rules

Each repository can add its own checks to the [safety net](code.md#safety-net-before-a-commit), on top of the built-in ones. They run before a commit and in the push preview, only warn, and are kept in GitSimple's settings (never read from the repository, so a cloned repository can't add or turn off a rule). Save them with **Save rules for owner/repo**.

- **Secret patterns:** a name and a regular expression, for example `Payment key` and `pay_live_[0-9a-f]{16}`. Only the lines a commit adds are checked. The syntax is Rust's `regex` (no look-ahead or back-references; `\w` and `\d` match ASCII only). A pattern that isn't valid, that would match empty text, or that is too big to check quickly (`[a-z]{3000}`, say) is refused when you save, with the rule's name in the message. If the patterns take more than 5 seconds on a very large change, the rest isn't checked with them, and lines over 16 KB (a minified file, say) aren't read by them; the warning then says part wasn't checked.
- **File patterns:** a name and a pattern for files that shouldn't be committed: `*.env` (that name in any folder), `secrets/**` (everything under a folder at the root), `config/*.pem`. `*` stays within a folder, `**` crosses folders, `?` is one character, and `[` `]` are plain characters (not a set as in `.gitignore`); matching ignores case. A pattern needs a letter or digit (`*.*` would name every file).
- **Flag files from N MB:** staged or pushed files of at least this size get a warning (1 to 49 MB). GitHub itself warns from 50 MB and refuses 100 MB; empty means only those limits.

Findings of secret and file patterns can be dismissed like the built-in ones (**Not a secret**, **Don't check this file**); the size warning can't be dismissed, like the big-file warning. A repository's rules are part of a [settings export](#import-and-export).

### Checks before push

Programs GitSimple runs in the repository each time you open the push preview: see [Checks before a push](code.md#checks-before-a-push). Pick each check from a fixed list. There is no free command line and no shell:

| Check | Runs |
|---|---|
| npm run, pnpm run, yarn run | `npm run <script>` (and the same for pnpm and yarn) with the script name you type: letters, digits and `: _ . -`, starting with a letter or digit |
| cargo test | `cargo test` |
| cargo clippy | `cargo clippy --all-targets -- -D warnings` (a warning fails the check) |
| dotnet test | `dotnet test --nologo` |
| pytest | `pytest` |
| go test | `go test ./...` |

- Up to 8 checks, each at most once. **Time limit per check:** 1 to 60 minutes; empty means 10.
- The program is looked up on your `PATH` (`npm.cmd`, `cargo.exe`, ...). Folders inside the repository are skipped, so a repository can't bring its own `npm`. Not found: the check says so and the rest still run. A `pytest` installed only in a virtual environment inside the repository (`.venv`) isn't used; install it where `PATH` finds it.
- Checks run in the repository's root folder with your environment, minus git's own variables. They also get `CI=1` (test runners don't wait in watch mode), colours off, `GOTOOLCHAIN=local` (a `go.mod` can't download another Go), no MSBuild or dotnet build servers left running, and corepack doesn't write into `package.json`.
- They are kept in GitSimple's settings and they are **not** part of a [settings export](#import-and-export): set them up again on another computer, or share them through the [team settings file](#team-settings-file). Saving them doesn't trust them; the push preview asks for that.

### Team settings file

A `.gitsimple.json` file committed in the repository root shares four settings with everyone who uses GitSimple in that repository: the **branch name template**, the **commit style** (the commit message helpers), the **pull request title and text templates**, and the **checks before push**. On a fresh clone they apply with no setup beyond one review.

- **Review first.** When the open repository has the file, Settings > Git shows **This repository comes with team settings** with each setting it provides. Nothing from it is used until you press **Review and use them** and agree in the dialog that opens (a Windows dialog shown by the app, like [Review and trust…](advanced-git.md#review-and-trust)). If the file changes later, you are asked again; your own settings apply meanwhile. Git itself is never paused by this file.
- **What it can say.** Only the four settings, with the same rules as when you type them in Settings: a check is one of the fixed presets with a script name, a template may use only the known placeholders. The file can't name a program or a path. Keys GitSimple doesn't know are listed as ignored and do nothing. A file that can't be read says why in plain words and your own settings apply.
- **Team values win.** Once accepted, a setting the file provides shows **From the team file** in Settings and is read-only there. In Advanced mode each has **Use my own**, which keeps your own value for that setting in this repository; **Use the team's** goes back. Your own values are kept while the team's apply, so nothing is lost either way.
- **Share with the team** (Advanced) writes the settings in effect for the open repository into `.gitsimple.json` in its folder, after a preview of the file. Commit it like any other change; GitSimple never writes to git's own configuration. A file you wrote counts as reviewed for you.

```json
{
  "version": 1,
  "branchTemplate": "{type}/{issue}-{slug}",
  "commitStyle": { "conventional": true, "types": ["feat", "fix", "docs"], "scopes": [], "checks": true, "bodyWidth": 72, "recent": false },
  "pullRequest": { "title": "{title}", "body": "{commits}\n\n{closes}" },
  "pushChecks": { "checks": [{ "preset": "npmRun", "script": "test" }], "timeoutMinutes": 10 }
}
```

Every section is optional. The presets are `npmRun`, `pnpmRun`, `yarnRun` (with `script`), `cargoTest`, `cargoClippy`, `dotnetTest`, `pytest` and `goTest`. A [settings export](#import-and-export) contains the values in effect, team values included.

## Network

For company networks: a proxy, HTTPS inspection, or both. GitSimple and git each get these settings. Everything except **Test connection** sits in the collapsed **Proxy and certificates** group in Basic mode.

| Setting | Default | What it does |
| --- | --- | --- |
| Proxy | Windows settings | **Windows settings**: the proxy from Windows Settings > Network & internet > Proxy ("Use a proxy server"), for GitSimple and git. A setup script (automatic configuration) isn't read; choose Manual then. With no proxy in Windows, GitSimple uses the `HTTPS_PROXY` environment variable if there is one, and git keeps its own settings. A Windows proxy address with a user name and password is ignored. **Manual**: the address below. **No proxy**: connect directly, even when Windows or git name a proxy. **Leave as is**: GitSimple uses the `HTTPS_PROXY` environment variable or the Windows proxy, and git keeps its own `http.proxy` |
| Proxy address | Empty | Manual only: `http://host:port`. A proxy that asks for a user name and password isn't supported yet, and an address with one is refused |
| Don't use the proxy for | Empty | Manual only: host names, domains (`.corp.example`) or IP ranges (`10.0.0.0/8`), separated by commas |
| Extra trusted certificate | None | GitSimple always trusts the Windows certificate store. If your organization inspects HTTPS and its root certificate isn't installed in Windows, choose it with **Browse** (a PEM file: Base-64 text starting with `-----BEGIN CERTIFICATE-----`). It can't be typed in. With git's own certificates (below), git gets this file instead of its built-in list, so the file must hold every certificate git needs; behind an inspecting proxy that is usually just the company root |
| Certificates for git | Git's own setting | **Windows certificate store (Schannel)** makes git trust what Windows trusts, which is usually what a company network needs. The extra certificate above is then not passed to git |

**Test connection** reaches the GitHub API with GitSimple and github.com with git, using the saved settings, and shows which one failed and why. There is no setting that turns certificate checks off.

When something fails with a certificate error ("unknown issuer", "unable to get local issuer certificate"), install your organization's root certificate in Windows, or choose it here; for git, also try **Windows certificate store (Schannel)**.

## Appearance

| Setting | Default | Notes |
| --- | --- | --- |
| Interface | Basic (new installs) | Basic or Advanced; see [Getting started](getting-started.md#basic-and-advanced-mode). "Reset to defaults" does not change it |
| Theme | Calm | One card per theme, each with a small preview of the window: Calm, Windows, Obsidian & Copper, Midnight Evergreen, Graphite & Blue, Carbon & Teal, Nordic Frost, Ink & Indigo, High Contrast and themes you imported. Each has a dark and a light look. See [High Contrast](#high-contrast) |
| Follow Windows light and dark | Off (dark) | On: the light or dark look follows Windows. Off: a Dark / Light switch beside it picks one |
| Highlight colour | The theme's own colour | Replaces the theme's accent: orange, blue, teal, purple, pink or yellow. See [Highlight colour](#highlight-colour) |
| Show tips | On | A short tip on Home about saving, sharing and undoing work. Hiding the tip on Home turns this off |
| Code colours | From theme | Also GitHub, One, Solarized, Dracula, or one you imported. See [Code colours](#code-colours) |
| Diff colours | From theme | Also Colour-blind safe (blue added, orange removed). See [Diff colours](#diff-colours) |
| Interface font / Code font | Inter / Geist Mono | Any installed font, or a font file you imported. See [Fonts](#fonts) |
| Code font size / Code line height | 12.5 px / 1.6 | Size of code, and the space per line as a multiple of the size. See [Code text](#code-text) |
| Code ligatures | On | Fonts that have them join `=>`, `!==` and similar into one sign. Off shows every character on its own |
| Tab width | 8 | How many spaces a tab takes in code: 2, 4 or 8 |
| Text size, density, animations | | |

### Highlight colour

**Highlight colour** changes the one accent colour of the window: the main (filled) button on each screen, the bar beside the selected menu item, links and the focus ring. The first swatch, with a palette icon, keeps the theme's own colour; the others are orange, blue, teal, purple, pink and yellow. The chosen swatch has a ring and a check mark. The theme cards preview the colour you picked.

Text on the highlight colour is chosen automatically (the theme's text, the theme's background, black or white, whichever is readable at 4.5:1 or more), and where the colour is used as text or as the focus ring it is lightened or darkened until it stands out from the background. So every colour stays readable on every theme, light and dark, including High Contrast; a test checks each pair. **Reset to defaults** goes back to the theme's own colour.

### Custom themes

**Import theme** loads your own family from a JSON file. **Export current** saves the selected family in the same format (or copies it as JSON).

Rules: `#RRGGBB` colours only, every key of `dark` and `light` required, family name up to 40 characters, at most 20 imported families, no name that matches a built-in. Anything else is rejected with a message that names the field: an unknown key or variant (also in `syntax`, where older versions ignored it), a colour that isn't `#RRGGBB`, or a value of the wrong shape. Nothing from a rejected file is kept.

The `muted` colour is used for secondary text, so it is darkened (or lightened) toward the text colour until it has at least 4.5:1 contrast on the background, panels and hover rows (WCAG AA). A muted colour that is already readable is used as given.

Each family can also have four optional groups. Each has `dark` and/or `light` (no other variants), and each variant may set any of its keys:

| Group | Keys | Colours you leave out |
| --- | --- | --- |
| `syntax` (code colours) | `keyword`, `string`, `comment`, `function`, `constant`, `parameter`, `punctuation`, `stringExpression`, `link` | Derived from the theme (keywords in the accent, strings green, functions blue, constants amber, comments in the muted colour) and lightened or darkened until they are readable on the background |
| `semantic` (status colours: success, errors, warnings) | `success`, `error`, `warning`, `info`, `neutral` | The file's top-level `semantic`, or else the built-in ones |
| `diff` (diff colours) | `added`, `removed`, `addedEmphasis`, `removedEmphasis`, `conflict` | `added` = `success`, `removed` = `error`, `conflict` = `error`, each emphasis = its line colour |
| `surfaces` (the window's boxes and rows) | `titleBar`, `menu`, `panel`, `card`, `hover`, `selected` | `titleBar` = `background`, `menu` = the panel, `panel` = `surface`, `card` and `hover` = the panel with a little of the text colour mixed in, `selected` a little more |

The `diff` keys:

- `added` and `removed`: the + and − markers, the +N / −N counts, the A and D file letters, and (tinted to 12%) the background of added and removed lines.
- `addedEmphasis` and `removedEmphasis`: the changed words inside a changed line, tinted to 35%.
- `conflict`: conflicted files ("!"), conflict labels and the merge-conflict icon on pull requests.

`added`, `removed` and `conflict` are shown as text, so they are lightened or darkened until they have at least 4.5:1 contrast on the background (WCAG AA). The emphasis colours are only used as tinted backgrounds and are kept as given. A [diff colour preset](#diff-colours) other than From theme replaces a family's `diff`.

The `surfaces` keys:

- `titleBar`: the title bar. The window ground around the side menu and the content panel stays `background`.
- `menu`: the side menu box. `panel`: the content panel (and the card under the repository name). `card`: cards inside the panel.
- `hover`: a row under the pointer. `selected`: the page you are on in the side menu, and the selected row or tab in lists (History, Files, Stashes, the Issues view switcher, the command palette).

Every surface keeps the text colour readable on it (at least 4.5:1); one that doesn't is mixed toward `background` until it does. In dark variants the panel keeps a small step above `background` so it reads as raised, unless that would make the text unreadable (light variants use a shadow instead). `selected` is kept visibly apart from `hover`, so a row under a resting pointer never looks like the page you are on. The muted colour and the side menu's edge are fitted to `menu` and `titleBar` too; the selected row shows its name in the text colour, so pick `selected` with that in mind.

The top-level `semantic` is optional too. When present it needs every key of both variants, and it applies to each family in the file; keys a family sets in its own `semantic` win. **Export current** writes the family's `syntax`, `diff` and `surfaces` and puts the status colours in effect in the top-level `semantic`, so the file imports back unchanged under a new name.

```json
{
  "defaultTheme": "My Theme",
  "defaultAppearance": "dark",
  "appearanceOptions": ["system", "dark", "light"],
  "themes": [
    {
      "family": "My Theme",
      "dark":  { "background": "#171717", "surface": "#272421", "accent": "#D78A57", "text": "#F3EEE7", "muted": "#A59C91", "border": "#3B3632" },
      "light": { "background": "#F5F1E9", "surface": "#FFFCF6", "accent": "#A15A37", "text": "#302923", "muted": "#84766B", "border": "#DDD3C6" },
      "syntax": { "dark": { "keyword": "#D78A57", "string": "#9CC48A" } },
      "diff": {
        "dark":  { "added": "#56B4E9", "removed": "#E69F00", "conflict": "#CC79A7" },
        "light": { "added": "#0072B2", "removed": "#A85300", "conflict": "#A2477B" }
      },
      "surfaces": { "dark": { "menu": "#1F1D1B", "selected": "#3F3A35" } }
    }
  ],
  "semantic": {
    "dark":  { "success": "#58AE87", "error": "#E47777", "warning": "#D5A45C", "info": "#6C9DD9", "neutral": "#969BA5" },
    "light": { "success": "#237A50", "error": "#BD4141", "warning": "#98651F", "info": "#326EB1", "neutral": "#68717E" }
  }
}
```

### High Contrast

The **High Contrast** family meets WCAG AAA. Text, muted text, the accent, status colours and diff colours have at least 7:1 contrast on the background and on panels (text and muted text also on the title bar, side menu, cards, hover and selected rows), and its code colours at least 7:1 on the background. Borders have at least 3:1. This holds with Code colours and Diff colours set to From theme; a code colour theme or the Colour-blind safe preset is only guaranteed AA (at least 4.5:1 for diff colours). Dark is white on black with a yellow accent; light is black on white with a dark blue accent. A test checks these ratios.

### Diff colours

**Diff colours** sets the colours of added, removed and conflicted lines and files, separately from the theme family. They are used in diffs (Changes, History, stashes, pull requests), file lists and the conflict resolver. **From theme** uses the family's own `diff` colours, or else its status colours (green and red in the built-in families). **Colour-blind safe** uses blue for added, orange for removed and reddish purple for conflicts. These hues stay apart with red-green colour blindness, and they are readable on every built-in family. **Reset to defaults** goes back to From theme.

Colour is never the only signal. Changed lines keep their + and − markers in both the Inline and the Side by side layout, and files keep their status letters (A, M, D, R, U, !).

### Code colours

**Code colours** sets syntax highlighting separately from the theme family. **From theme** uses the family's own colours (its `syntax`, or derived ones). A code colour theme replaces them; each colour is lightened or darkened only where it would be hard to read on the family's background. **Reset to defaults** goes back to From theme and keeps your imports.

**Import code colours** loads your own from a JSON file. **Export current** saves the colours in effect (every key, both variants) in the same format, named "<name> (custom)" (numbered if that name is taken), ready to edit and import back.

Rules: `#RRGGBB` colours only, keys from the list in [Custom themes](#custom-themes), at least one colour per theme, name up to 40 characters and not already used (including "From theme"), at most 20 imported. Colours you leave out are derived from the family's palette, as for a family without `syntax`; a family's own `syntax` colours are not mixed in.

```json
{
  "syntaxThemes": [
    {
      "name": "My Code Colours",
      "dark":  { "keyword": "#FF7B72", "string": "#A5D6FF", "comment": "#8B949E" },
      "light": { "keyword": "#CF222E", "string": "#0A3069" }
    }
  ]
}
```

### Fonts

**Interface font** sets the font of menus, lists and text; headings keep their title font. **Code font** sets the font of diffs, Files, blame and other code; installed monospace fonts are listed first. The lists show the fonts in your Windows font folders (yours and all users'), including separate weights such as "Segoe UI Semibold". A sample line under each shows the choice. If a chosen font is uninstalled or its file is gone, the default font shows instead (and Settings says so for an imported file).

**Import font file** adds a `.ttf`, `.otf`, `.woff` or `.woff2` file (up to 10 MB, at most 10 files) to both lists, named after the file. GitSimple keeps its own copy; nothing is installed in Windows. Collections (`.ttc`) aren't supported. One file is one font: a variable font covers every weight, other fonts get a computed bold. Removing an imported font sets any setting that used it back to the default. **Reset to defaults** goes back to Inter and Geist Mono and keeps your imports.

### Code text

**Code font size**, **Code line height**, **Code ligatures** and **Tab width** apply to everything shown in the code font: diffs (Changes, History, pull requests), Files (content and blame), the Console output and the conflict resolver. The sample under **Code font** shows them as you change them.

- Size is in pixels (11 to 18); **Text size** still zooms it with the rest of the window.
- Line height is 1.2 to 2.0 times the size, rounded to a whole pixel and never under 16 px; the hint shows the result. Diffs and files use the same size and height (12.5 px and 20 px per line by default).
- Ligatures depend on the font: Cascadia Code, Fira Code and JetBrains Mono have them, many others have none, and then the setting changes nothing.
- **Reset to defaults** sets all four back.

## Keyboard shortcuts

Every shortcut, grouped by where it works, with its keys. **Change** records new keys; see [Keyboard shortcuts](shortcuts.md#change-a-shortcut) for the rules.

## Updates

| Setting | Default | What it does |
| --- | --- | --- |
| Check for updates automatically | On | Checks at launch and every 6 hours and shows a toast when a new version is out. Off: only **Check for updates** checks |

Updates come from the published releases of `LandonThomp/GitSimple-releases`. The app restarts after installing one.

## Account

The sign-in the app is open on (github.com, a server or GitLab; the label names it, such as **GitHub login**) and **Sign out**. Below it, **GitHub Enterprise Server** (collapsed in Basic mode until you add a server): the servers you added, each with its own **Sign in** / **Sign out**, and the trash button to remove one (refused while a repository on it is still added). **Add a server** asks for the server's address and the OAuth app client ID; see [Signing in to GitHub Enterprise Server](getting-started.md#signing-in-to-github-enterprise-server).

**Your accounts.** Every account you're signed in to, grouped by host (github.com first, then gitlab.com), with **Default** on the one repositories use unless you choose another. Under each login the row says where the account is: "GitHub · github.com", "GitLab · gitlab.com", or a server's address. Each row's **…** menu has **Make default** and **Sign out**. **Add another account** signs in one more: GitHub adds whichever account your browser is signed in to, so switch accounts on github.com first. An account that isn't on the GitSimple invite list isn't added (the dialog says so); signing in again as an account you already have adds nothing.

**GitLab.** **Add a GitLab account** signs in to gitlab.com (in the browser, or with **Use an access token**); once you have one, gitlab.com is listed with your other accounts, with **Add another account** under it. **Add a GitLab server** asks for the server's address and, optionally, the app ID your administrator gives you (GitLab 17.9 or newer). Without an app ID the server signs in with access tokens only ("Access token only"). Each GitLab server is listed with **Sign in** / **Sign out** and the trash button. A GitLab account that isn't on the GitSimple invite list (a gitlab.com username, or a server that isn't listed) isn't added, also when it would be the first on that GitLab; nothing of it is kept. An account signed in with an access token shows when its token runs out, a week before; see [Signing in to GitLab](getting-started.md#signing-in-to-gitlab).

**Which account a repository uses.** When you're signed in to more than one account on the same GitHub or GitLab (a work and a personal github.com account, say), each repository uses one of them for its pushes, pulls, issues and pull requests: **Settings > Workspaces > GitHub account** (**GitLab account** for a GitLab repository). Adding a repository picks for you: your default account when it can see the repository, otherwise the first of your other accounts that can. The choice is saved at once. If a repository's account is signed out, a red bar at the top says so, with **Sign in again** and a button to use your default account instead.

## Notifications

See [Inbox](inbox.md) for what these do.

| Setting | Default | What it does |
| --- | --- | --- |
| Review requested | On | A Windows notification when someone asks you, or a team you're on, to review a pull request |
| Mentions | On | A Windows notification when you or a team you're on is @mentioned |
| Assigned | On | A Windows notification when an issue or pull request is assigned to you |
| CI finished | On | A Windows notification when a GitHub Actions run you started has finished |
| New commits on the upstream | On | A Windows notification when the automatic fetch (or the tray's Fetch all) finds new commits on the upstream of the branch you're on. The automatic fetch then also runs while GitSimple is minimised or in the tray |
| Keep running in the tray | Off | Closing the window hides GitSimple to the tray (next to the clock) instead of quitting, so notifications keep arriving. The tray menu also has your recent repositories and **Fetch all repositories**. Quit from the tray icon's menu |
| Start with Windows | Off | Opens GitSimple minimized when you sign in to Windows (in the tray when the setting above is on) |

Windows notifications only appear while GitSimple isn't the window you're using. These settings are for the whole app, not per repository.

## Import and export

Moves your setup to another computer, or shares a team setup, as one JSON file.

**Export settings** saves: appearance (theme, imported themes and code colours, fonts, diff options, Basic or Advanced), git settings, your repositories with their issue, branch and commit message settings and their own safety-net rules, saved issue views and filters, changed keyboard shortcuts, and the update and notification switches.

What is never in the file:

- Your sign-in and tokens. The token stays in Windows Credential Manager, and nothing in the export reads it.
- Program paths: git.exe, the merge tools and the editor. Choose them again with **Browse** on the other computer.
- Commit identities (names and emails) and which identity each repository commits as. An import leaves the ones on this computer as they are.
- Imported font files. A setting that uses one goes back to the default font; import the font file there and choose it again.
- Settings > Network and the servers you added (GitHub Enterprise Server and GitLab).
- A repository's [checks before push](#checks-before-push), and which checks you trusted: they run programs, so they stay on the computer where you set them up.
- Local folders, unless you switch on **Include local folders** (where each repository and new clones are on this computer). Leave it off when you share the file with others.

**Import settings…** reads such a file and shows what would change, part by part: git settings, repositories, saved issue views, keyboard shortcuts, appearance, and updates and notifications, with the names of the settings that change. Parts that are already the same are greyed out. Untick what you don't want, then **Import**. Repositories that aren't here yet are added (from their local folder when the file has one and it holds that repository, otherwise without a folder; choose its local folder later in Settings > Workspaces). Only folders on a drive letter are used: network shares in a file are refused. A repository on a GitHub Enterprise Server you haven't added in Settings > Account is skipped, and so is a GitLab repository that isn't here yet (add it from GitLab first; an import then updates its settings).

The file is checked like a theme file: a value of the wrong type or out of range is refused with its name, and nothing is applied. A repository's issue and branch settings are checked when they are saved; one that fails is listed in the message at the end, and the other parts are still imported. Settings this version doesn't know are ignored, and a file from a newer version is refused.

## Supported hosts

GitHub (github.com), GitHub Enterprise Server (issues in label mode), and GitLab: gitlab.com and GitLab servers your company runs, 16.0 or newer (issues in label mode; see [GitLab](gitlab.md)). Bitbucket and Azure DevOps are planned.
