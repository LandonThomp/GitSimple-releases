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

### Sprints

Creating, renaming and re-dating sprints is off until you allow it in Settings, Issues.

## The issue panel

Click an issue to open it.

- Edit the title and description. A change made on GitHub meanwhile is detected per field and offered as reload or overwrite.
- Comments with markdown.
- Pickers for status, priority, assignees, labels, sprint, estimate and milestone.
- Close and reopen with a reason.
- Relations: parent, sub-issues, blocked by, blocking, mentioned in.
- **Development**: linked branches, pull requests and commits (see [Issues and code together](code.md#issues-and-code-together)).

Links to other issues of the repository open inside the app; other links open in your browser (https only). GitHub's rendered markdown is shown sanitized. Images in a private repository are fetched with your sign-in (only for the active repository, at most 25 MB each).

## Project mode and label mode

A GitHub Project is optional. A workspace is in **project mode** when it has a Project number, otherwise in **label mode**.

| | Project mode | Label mode |
| --- | --- | --- |
| Status | The Project's status field; its options are the board columns | Open / closed, plus optional **status labels** (default prefix `status: `) as extra columns |
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
