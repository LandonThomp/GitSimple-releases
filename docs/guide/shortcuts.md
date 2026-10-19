[User guide](README.md) › Keyboard shortcuts

# Keyboard shortcuts

`Ctrl+K` opens the command palette from anywhere. The tables below are the default keys; you can change them (see [Change a shortcut](#change-a-shortcut)), and Settings, Keyboard shortcuts always shows the current ones.

## Go to a view

Press `G`, then a letter.

| `G` then | Code views |
| --- | --- |
| `D` | Home |
| `C` | Changes |
| `B` | Branches |
| `P` | Pull requests |
| `H` | History |
| `F` | Files |
| `S` | Stashes *(Advanced)* |
| `T` | Tags *(Advanced)* |
| `Y` | Repository *(Advanced)* |
| `X` | Console *(Advanced, while the Console is on)* |

| `G` then | Issue views |
| --- | --- |
| `O` | Overview |
| `I` | Issues |
| `M` | My issues |
| `K` | Board |
| `R` | Sprints *(Advanced)* |

| `G` then | Other |
| --- | --- |
| `N` | [Inbox](inbox.md) (GitHub notifications) |

## Code

| Keys | Action |
| --- | --- |
| `Ctrl+Enter` | Commit (in Changes) |
| `Ctrl+Shift+P` | Push |
| `Ctrl+Shift+L` | Pull |
| `F5` or `Ctrl+R` | Refresh |
| `Space` | Stage / unstage the selected file |
| `Ctrl+Z` | Recent actions (undo). Never resets by itself; in a text box it is the normal text undo |
| `Ctrl+F` | Find in the diff, file or blame you last clicked in. `Enter` / `Shift+Enter` in the box for the next / previous match, `Esc` to close |
| `F3` / `Shift+F3` | Next / previous match |
| `Ctrl+G` | Go to line (in a diff, the new file's line number) |
| `F7` / `Shift+F7` | Next / previous change in a diff |
| `Ctrl+L` | Clear the Console's screen |
| `Ctrl+P` | Find a file by name (in Files) |

## Issues

| Keys | Action |
| --- | --- |
| `C` | New issue |
| `/` | Search |
| `J` / `K` or arrow keys | Move through issues |
| `Enter` / `Esc` | Open / close the issue panel |
| `B` | Create a branch for the open issue |
| `S` / `P` / `A` / `L` | Status / priority / assignee / label picker for the open issue |
| `Ctrl+B` / `Ctrl+I` / `Ctrl+K` | Bold / italic / link, in the issue editor |
| `Ctrl+Enter` | Submit, in a comment box or the new-issue dialog |

## Change a shortcut

In **Settings, Keyboard shortcuts**, click **Change** next to a shortcut and press the new keys (`Esc` cancels). The change works at once, and hints and tooltips show the new keys.

- **Go to a view:** `G` stays the first key; you choose the letter after it.
- **Keys already in use:** if another shortcut uses the keys where this one works, GitSimple says which one and offers **Use it here instead**, which takes the keys from the other shortcut (it is then left without keys until you give it new ones). A key may mean different things in different places: `Ctrl+K` is the command palette, but in the text editor it inserts a link.
- **Plain keys** (a letter, `Space`) can be used for any shortcut, but they only work outside text boxes, where they would otherwise be typed. Text editor shortcuts need `Ctrl` or `Alt` (or a function key).
- **Can't be used:** `Esc`, `Tab`, `Enter`, the arrow keys, `Backspace`, `Ctrl+C` / `Ctrl+V` / `Ctrl+X` / `Ctrl+A`, `Alt+F4`, and `G` on its own (it starts the go-to keys). `Esc`, the arrow keys and `Enter` for moving through lists can't be changed.
- **No shortcut** removes a shortcut's keys. The reset button next to a changed shortcut brings its default keys back; **Reset all shortcuts** does that for every one.
