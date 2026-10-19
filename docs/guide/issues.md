[User guide](README.md) › Issues

# Issues

A GitHub issue tracker for the active repository. The issue views never need a local clone; they read and write GitHub with your GitSimple sign-in.

## The views

| View | What it shows |
| --- | --- |
| **Overview** | The active sprint (when a Project has a sprint field), recently updated issues, your open issues, open issues by priority and by type |
| **Issues** | A list you can group, sort, filter and search (see below) |
| **Board** | One column per status. Drag a card to change its status |
| **Sprints** | Active, Upcoming (with a planning pool) and History. Needs a Project with an iteration field. Hidden in Basic mode |

### Issues list

- **Group** by status, priority, assignee or sprint, and **sort**.
- **Filter** by status, priority, labels, assignee, milestone, sprint, type, open / closed.
- **Search** by text, `#42`, or your key prefix such as `GS-42`.
- **My issues / All** quick toggle.
- Select several for **bulk actions**. A bulk change reports how many issues were updated and why the others failed.
- **Display options**: which fields show (key, labels, assignee avatars, priority, estimate, sprint) and the density.
- **Save view** keeps the current filter, sort, grouping and scope under a name. Saved views appear in the sidebar under Issues (per workspace), where you can rename or delete them.

### Board

- Columns follow the order of the status options; all of them show by default.
- Moving a card into a closing status closes the issue; moving it out reopens it.
- Hide or collapse columns. The choice is remembered per workspace.
- In label mode, **Add column** (after the last column) creates a status: type a name such as `In progress` and GitSimple creates the label `status: In progress` on GitHub (with your prefix). It needs write access to the repository and is hidden when the status label prefix is empty. With a Project, add an option to the Project's status field on GitHub instead. Advanced mode only: GitSimple can't delete a status label again, so Basic doesn't offer it.

### Sprints

Creating, renaming and re-dating sprints is off until you allow it in Settings, Issues.

## The issue panel

Click an issue to open it.

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
  - The heading counts how many are done. Hover a sub-issue and click **x** to remove it from the list (the issue itself stays).
  - When every sub-issue is closed and the issue is still open, the panel offers **Close #N as completed**. Nothing closes on its own.
- **Development**: linked branches, pull requests and commits (see [Issues and code together](code.md#issues-and-code-together)).

Links to other issues of the repository open inside the app; other links open in your browser (https only). GitHub's rendered markdown is shown sanitized. Images in a private repository are fetched with your sign-in (only for the active repository, at most 25 MB each).

## Creating an issue

**New issue** (shortcut C, the toolbar button, or a board column's **+**) opens a dialog with a title, a description and pickers for status, priority, sprint, estimate, labels, assignees, milestone and type.

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
