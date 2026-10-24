[User guide](README.md) › Issues

# Issues

An issue tracker for the active repository's GitHub or GitLab issues. The issue views never need a local clone; they read and write the host with your GitSimple sign-in.

## The views

| View | What it shows |
| --- | --- |
| **Overview** | Tiles: how many issues are open, closed this week and assigned to nobody, the active sprint (when a Project has a sprint field) with its progress and a count per status, your open issues, recently updated issues, open issues by priority and by type. Click **Open**, **Nobody assigned** or a bar to open the list with that filter |
| **Issues** | A list you can group, sort, filter and search (see below) |
| **Board** | One column per status. Drag a card to change its status |
| **Sprints** | Active, Upcoming (with a planning pool) and History. Needs a Project with an iteration field. Hidden in Basic mode |
| **Insights** | A sprint's burndown, the median cycle time of closed issues and the median time to first review and to merge of merged pull requests (see [Insights](#insights)). Hidden in Basic mode |

### Issues list

- **Group** by status, priority, assignee or sprint, and **sort**.
- **Filter** by status, priority, labels, assignee, milestone, sprint, type, open / closed. Status, priority and labels have their own dashed buttons in the toolbar while they're unused; **Filters** holds all of them. Each active filter shows as a chip ("Status: Todo") with an **x** to remove it.
- **Search** by text, `#42`, or your key prefix such as `GS-42`.
- **My issues / All** quick toggle.
- Select several for **bulk actions**. A bulk change reports how many issues were updated and why the others failed.
- **Display options**: which fields show (key, labels, assignee avatars, priority, estimate, sprint) and the density. A row shows its first label; **+N** lists the others.
- **New issue** is at the top right. While an issue is open in the panel it is an outlined button, because the panel's own button (such as **Start work**) is then the main one.
- **Save view** keeps the current filter, sort, grouping and scope under a name. Saved views appear in the sidebar under Issues (per workspace), where you can rename or delete them.

### Board

- Columns follow the order of the status options; all of them show by default. Cards assigned to you have an outline in the accent colour.
- **New issue** at the bottom of a column creates an issue in that column's status.
- Moving a card into a closing status closes the issue; moving it out reopens it.
- Hide or collapse columns. The choice is remembered per workspace.
- In label mode, **Add column** (after the last column) creates a status: type a name such as `In progress` and GitSimple creates the label `status: In progress` on GitHub (with your prefix). It needs write access to the repository and is hidden when the status label prefix is empty. With a Project, add an option to the Project's status field on GitHub instead. Advanced mode only: GitSimple can't delete a status label again, so Basic doesn't offer it.

### Sprints

Creating, renaming and re-dating sprints is off until you allow it in Settings, Issues.

## The issue panel

Click an issue to open it. The panel shows, top to bottom: the issue's number (click to copy), **Close** or **Reopen**, **Open on GitHub**, expand and close; the title; the progress card ("Ready to work on it?" with **Start work** before any work, then the stages); the properties as a two-column grid; the description; relations and development; and the comment box, with the thread folded behind "N comments · Show".

- Edit the title and description. A change made on GitHub meanwhile is detected per field and offered as reload or overwrite.
- Tick or untick a task-list box (`- [ ] item`) in the description to save the description with just that box changed. Boxes in comments are read-only.
- Comments with markdown.
- **Attach on GitHub** under the comment box adds a file (an image, a log) to a comment. GitHub doesn't let apps upload files, so the button copies what you wrote, keeps it in the box, and opens the issue on GitHub: paste it into the comment box there and drag the file in. Clear the draft here once you've posted it.
- **Reactions** under the description and each comment: GitHub's eight emoji with how many people chose each. Yours are highlighted; click one to add or take back yours, or the smiley to pick another. On a locked issue they are shown but can't be changed.
- Pickers for status, priority, assignees, labels, sprint, estimate and milestone.
- Close and reopen with a reason.
- Relations: parent, sub-issues, blocked by, blocking, mentioned in.
- **Sub-issues** break an issue into parts that must be finished first, such as "Server enhancements" with one sub-issue per change:
  - **Add** attaches an existing open issue; **New** creates an issue with just a title and attaches it. An issue can have only one parent: GitHub refuses one that already has a parent, and an issue that is above this one.
  - The heading counts how many are done ("1 of 3 done"). Hover a sub-issue and click **x** to remove it from the list (the issue itself stays).
  - When every sub-issue is closed and the issue is still open, the panel offers **Close #N as completed**. Nothing closes on its own.
- **Development**: linked branches, pull requests and commits (see [Issues and code together](code.md#issues-and-code-together)).
- The **progress strip** at the top (Start, Work, Review, Merged) offers the next step: branch, commit, push, **Review my changes** (your own look at the branch before asking others, see [Review your own changes first](code.md#review-your-own-changes-first)), pull request, ready for review, clean up. Each stage can be turned off in Settings, Issues.

Links to other issues of the repository open inside the app; other links open in your browser (https only). GitHub's rendered markdown is shown sanitized. Images in a private repository are fetched with your sign-in (only for the active repository, at most 25 MB each).

## Creating an issue

**New issue** (shortcut C, the button at the top right of Overview, Issues and Board, or **New issue** at the bottom of a board column) opens a dialog with a title, a description and pickers for status, priority, sprint, estimate, labels, assignees, milestone and type.

### Issue templates and issue forms

When the repository has issue templates in `.github/ISSUE_TEMPLATE/` on its default branch, the dialog first shows a chooser. GitSimple reads the templates over the GitHub API, so this works without a local clone. A repository without templates opens straight on a blank issue, as before.

- **Templates and forms** are listed in GitHub's order: forms (`.yml`, `.yaml`) before Markdown templates (`.md`), each alphabetically by file name. Prefix file names with numbers (`1-bug.yml`) to set the order.
- **Markdown templates** prefill the title, description, labels, assignees and issue type from their front matter (`title`, `labels`, `assignees`, `type`; `name` and `about` are required, as on GitHub).
- **Issue forms** replace the description with the form's fields: text (`markdown`), single-line inputs, text areas, dropdowns (single or multiple choice), checkboxes and uploads. The form's title, labels, assignees and type are preselected. **Create issue** writes the answers into the description the way GitHub does: a `### Label` heading per field, `_No response_` for an empty answer, `- [X]` for a ticked box, and a code block for a text area with `render`. Markdown text is not part of the issue.
- **Required answers** (`required`, `min_length`, required checkboxes) are checked before the issue is created; the dialog moves to the first field that needs an answer. GitHub enforces them only in public repositories; GitSimple enforces them everywhere.
- **Uploads**: files can't be uploaded from GitSimple. Paste links to the files, or attach them on GitHub after creating the issue.
- Labels, assignees and the issue type a template names are applied only when the repository has them, like on GitHub. A template's `projects` are not applied.
- `config.yml`: `blank_issues_enabled: false` hides **Blank issue** for people with read or triage access; people with write access still see it, marked **Maintainers only**, as on GitHub. Each of the `contact_links` is shown with its description and opens in your browser.
- Use the arrow next to the dialog title to go back to the chooser. What you typed is replaced only when you pick another template.
- The blank issue shows while the templates load. If you start typing before they arrive, the dialog stays on the blank issue.
- Only `.github/ISSUE_TEMPLATE/` of the repository itself is read: templates inherited from the owner's `.github` repository and a single legacy `ISSUE_TEMPLATE.md` are not offered.
- A template file that GitHub would not offer (invalid YAML, a missing `name`, `about` or `description`, a form without a field that is not markdown, duplicate labels or ids, a dropdown option `None`, and so on) is left out too. **N template files can't be used** under the chooser lists each one with the reason. Files larger than 256 KB and templates after the first 30 are skipped the same way.
- Templates are read again when the dialog opens after five minutes. If they can't be read (for example, during a rate limit), the dialog opens on a blank issue.

## Project mode and label mode

A GitHub Project is optional. A workspace is in **project mode** when it has a Project number, otherwise in **label mode**.

| | Project mode | Label mode |
| --- | --- | --- |
| Status | The Project's status field; its options are the board columns | Open / closed, plus optional **status labels** (default prefix `status: `) as extra columns. **Add column** on the Board creates one |
| Priority | The Project's priority field | **Priority labels** (default prefix `priority: `), ordered urgent to low when the names are common ones |
| Sprints, estimates | The Project's iteration and number fields | Not available ("Link a GitHub Project in Settings to enable sprints") |
| Closing | A status marked as closing closes the issue with its reason | The same for status labels, plus the built-in Closed column |

### Issues on GitLab

A GitLab repository is always in **label mode**: GitLab has no Projects like GitHub's, so status and priority live in labels.

- **Scoped labels.** A new GitLab workspace reads labels named `status::To do` (the column) and `priority::high`, the way GitLab's scoped labels are written. Change the prefixes in **Settings, Issues** if your project names them differently. On GitLab Free a scoped label is an ordinary label with `::` in its name, so GitSimple removes the old status label itself when an issue moves.
- **Board order.** The columns follow your project's first issue board in GitLab (**Plan, Issue boards**): the status labels that are lists on that board, left to right. Status labels that aren't on the board come after them. Open and Closed stay at the ends. If the board can't be read, the usual workflow order is used. GitSimple only reads the board; it never changes it.
- **Closing.** A status that closes (**Done** by default; see Settings, Issues) closes the issue in the same step as it changes the label. GitLab has no close reasons, so closing is a plain Close.
- **Add column** creates the label `status::<name>` in the project. It needs at least the Reporter role.
- **Milestones, assignees and comments** are the project's own. Assignees must be members with at least the Reporter role. System notes ("changed the description", "added label") aren't shown as comments.
- **Reactions** use GitLab's award emoji. GitHub's eight reactions map to 👍 👎 😄 🎉 😕 ❤️ 🚀 👀; other emoji on an issue aren't shown.
- **Images** uploaded to an issue or committed to the repository load in the issue text. Images need GitLab 17.4 or later for uploads in a private project; otherwise they may not load.
- **Not available on GitLab:** sprints, estimates, sub-issues, issue types and issue templates.

### Link a GitHub Project

1. On GitHub, create a Project (an organization's or your own). Note its number in the URL: `github.com/orgs/acme/projects/3` is number 3. It does not have to be linked to the repository.
2. In GitSimple open **Settings, Issues**, choose **GitHub Project**, and pick it from the list (projects linked to the repository first), or type its owner and number.
3. Check the **Field mapping**. Status, Priority, Sprint and Estimate are found by name (Sprint also matches Iteration). Choose another field, or **None** to hide the feature.
4. The sign-in needs the `project` scope. If Settings, Account lists it as missing, sign out and sign in again.

GitSimple never creates, renames or deletes Projects, fields or field options.

## Caching and errors

- Issues are cached in the app's browser storage for up to 24 hours, so the views draw at once on launch. Only issue titles and text of the repositories you added; never your token.
- Signing out, signing in as someone else, or a rejected sign-in clears the cache. Changing the mode or field mapping never shows data read the old way.
- Errors are explained where they happen:
  - a **rate limit** shows when requests resume;
  - an organization that restricts third-party apps (or requires SAML) means an owner must approve GitSimple;
  - a missing `project` scope tells you to sign in again.

## Insights

**Insights** (Advanced mode; in the Issues group of the sidebar) is one page of numbers about how work flows, computed from the issues and pull requests GitSimple already shows. Nothing is written to GitHub.

- **Burndown** of one sprint: how much of its work was still open at the end of each day, next to the even pace that would finish on the last day. Pick the sprint at the top right; the page opens on the active one. Work is counted in estimate points when the sprint has estimates (issues without one count as 0, and the page says how many), otherwise in issues. **Show the days as a table** lists the same numbers. Only today's issues are counted: an issue taken out of the sprint is not known, so the scope is what the sprint holds now. Without a GitHub Project there is no burndown.
- **Cycle time**: the median time from created to closed, over the last 30 issues closed as completed (issues closed as duplicate or not planned are left out). Issues closed in the last 90 days are read for it even when the list does not hold them.
- **Time to first review**: the median time from a pull request being opened to its first review by someone other than its author, over the last 30 merged pull requests that got one.
- **Time to merge**: the median time from opened to merged, over the last 30 merged pull requests.

Each number has one sentence under it saying what it covers. With fewer than 3 samples the page says **Not enough data** and what it would need. Cycle time counts from when an issue was created: status changes (such as moving to In progress) are not read from GitHub, so a "started to done" time is not shown. The windows (the last 30, the last 90 days) are fixed.
