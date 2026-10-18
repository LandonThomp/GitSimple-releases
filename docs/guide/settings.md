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
| Refresh interval · *Advanced* | 60 seconds | How often issues refresh while the window is focused; 0 = off. After a rate limit the app waits until it resets |

## Git

### Everyday

| Setting | Default | What it does |
| --- | --- | --- |
| Use GitSimple's GitHub sign-in for git | On | github.com https remotes use your GitSimple sign-in (passed over a pipe, never in arguments, environment or files). Off: Git Credential Manager and your own helpers. Other hosts always use yours |
| Pull strategy | Merge | Merge, Rebase, or "Only when nothing needs combining" (fast-forward only). A rebase pull with conflicts stays open: resolve in Changes, then continue or abort from the banner |
| Fetch automatically | 5 minutes | On window focus and every N minutes; 0 = off |
| Default clone folder | `%USERPROFILE%\source\repos` | Where **Clone to folder…** starts |
| Editor | First one found | What **Open in editor** starts: VS Code, VS Code Insiders, Cursor, VSCodium, Windsurf, Sublime Text or Notepad++. Pick with Browse or from the editors found; a path can't be typed in. Clear returns to "first one found" |
| Diff layout / context lines / wrap long lines | Unified, 3, off | Also switchable in the diff header |
| Syntax highlighting | On | Colours code by language in diffs, Files (content and blame) and the conflict resolver. Files over 512 KB or 20,000 lines are always shown plain; turn it off on a slow machine |

### Advanced

| Setting | Default | What it does |
| --- | --- | --- |
| Run repository git hooks on commit and push | On | Hooks run for commit, amend, merge, pull and push. Status, diff and log never run hooks |
| Refresh changes | 10 seconds | Status check while the window is focused; 0 = off |
| Network time limit | 30 minutes | Clone, fetch, pull and push stop after this |
| Commit message template | `{summary}\n\n{body}` | Placeholders `{summary}` (required), `{body}`, `{issue}` |
| Branch name template | `{type}/{issue}-{slug}` | Placeholders `{type}`, `{issue}`, `{key}`, `{slug}`, `{user}`; live preview |
| Branch type rules *(per workspace)* | `bug` gives `fix`, otherwise `feat` | Rules on issue type or label, first match wins |
| Commits that belong to an issue *(per workspace)* | Key in front of the summary | None, `Refs #N`, `Fixes #N`, or the key in front of the summary; live preview |
| Pull request title / body templates *(per workspace)* | `{title}` / `{commits}\n\n{closes}` | Placeholders `{title}`, `{issue}`, `{key}`, `{branch}`; body also `{commits}`, `{closes}`; live preview |
| Merge tool | None | Pick with Browse or from the tools found: VS Code, P4Merge, KDiff3, Beyond Compare, Meld, WinMerge, TortoiseGitMerge. A path can't be typed in |
| Default reset mode | Mixed | The mode reset and Undo start with. A hard reset always needs the branch name typed |
| Confirm before… | Cherry-pick, Revert, Pushing tags: on · Applying or popping a stash: off | Extra "are you sure" for medium-risk actions. Actions that can lose data always preview, whatever this says |
| Rebase defaults | Autostash on, autosquash off | "Set changes aside during a rebase (autostash)" and "Arrange fixup! and squash! commits (autosquash)" |
| Enable the Console | On | Show the Console view and allow its commands. Basic mode hides the view regardless |
| Path to git.exe (optional override) | Automatic | Pick with Browse: a local `git.exe` in a Git for Windows folder that answers `git --version`. Clear returns to automatic |
| Repository maintenance | – | **Run garbage collection** (`git gc`, never `--prune=now`) |

## Appearance

| Setting | Default | Notes |
| --- | --- | --- |
| Interface | Basic (new installs) | Basic or Advanced; see [Getting started](getting-started.md#basic-and-advanced-mode). "Reset to defaults" does not change it |
| Appearance | Dark | Dark, light, or follow the system |
| Theme family | Obsidian & Copper | Also Midnight Evergreen, Graphite & Blue, Carbon & Teal, Nordic Frost, Ink & Indigo. Each has dark and light |
| Text size, density, animations | | |

### Custom themes

**Import theme** loads your own family from a JSON file. **Export current** saves the selected family in the same format (or copies it as JSON).

Rules: `#RRGGBB` colours only, every key of `dark` and `light` required, family name up to 40 characters, at most 20 imported families, no name that matches a built-in. `semantic` is optional and ignored on import.

Code colours are optional: add `syntax` with `dark` and/or `light`, each with any of `keyword`, `string`, `comment`, `function`, `constant`, `parameter`, `punctuation`, `stringExpression`, `link`. Colours you leave out are derived from the theme (keywords in the accent, strings green, functions blue, constants amber, comments in the muted colour) and lightened or darkened until they are readable on the background. Export includes them.

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
      "syntax": { "dark": { "keyword": "#D78A57", "string": "#9CC48A" } }
    }
  ]
}
```

## Updates

| Setting | Default | What it does |
| --- | --- | --- |
| Check for updates automatically | On | Checks at launch and every 6 hours and shows a toast when a new version is out. Off: only **Check for updates** checks |

Updates come from the published releases of `LandonThomp/GitSimple-releases`. The app restarts after installing one.

## Supported hosts

GitHub now. GitLab, Bitbucket and Azure DevOps are planned.
