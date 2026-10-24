[User guide](README.md) › GitLab

# GitLab

GitSimple works with GitLab projects much as it does with GitHub repositories: Changes, Branches, History and Files are the same, and issues, the board, merge requests and pipelines read and write GitLab. This page sums up what is different and links to the details.

## What you need

- **gitlab.com**, or **a GitLab your company runs** (self-managed): GitLab 16.0 or newer, 17.9 or newer to sign in through the browser. Its address is just `https://` and the server's name (`https://gitlab.example.com`), with no port or folder.
- **An invite.** GitSimple is invite-only: your gitlab.com username, or your company's GitLab server, must be on the invite list. GitLab's list is separate from GitHub's.
- **The Free tier is enough.** GitSimple uses what GitLab Free offers. Where your edition has more (approvals, say), it is read but not required.

## Signing in

Choose **GitLab** on the first sign-in step for gitlab.com, or **A company server** and then **GitLab** for your company's. Sign in through the browser with a short code, or with a personal access token that has the **api** scope (on gitlab.com, a **Legacy token**: the fine-grained form behind **Add new token** has no scopes) when the browser page is blocked, the server has no app ID for GitSimple, or it runs a GitLab older than 17.9. A token doesn't renew itself: a week before it runs out, a yellow bar reminds you to make a new one.

Step by step: [Signing in to GitLab](getting-started.md#signing-in-to-gitlab). More accounts and servers: [Settings, Account](settings.md#account).

## Adding a project

**Add a repository**, **From GitHub or GitLab**, then choose gitlab.com or your server. The list shows every project you are a member of, including projects in subgroups (`group/sub/project`). See [Adding a repository](getting-started.md#adding-a-repository).

## Issues and the board

A GitLab project is always in **label mode**: status and priority are labels.

- **Scoped labels.** Columns are labels named `status::To do`, priorities `priority::high`. Change the prefixes in **Settings, Issues** if your project uses other names.
- **Board order** follows the project's first issue board in GitLab. GitSimple only reads that board.
- **Closing** is a plain Close: GitLab has no close reasons.
- **Not on GitLab:** sprints, estimates, sub-issues, issue types and issue templates.

Details: [Issues on GitLab](issues.md#issues-on-gitlab).

## Merge requests and pipelines

**Pull requests** shows the project's merge requests, and the most visible places say "merge request" and number them `!12` (issues stay `#12`). Draft is GitLab's `Draft:` prefix, the merge follows the project's own rules and method, and review comments are posted at once (there is no pending review). The **Checks** tab lists the jobs of the newest pipeline, with **Show log** and **Re-run** for a job or the failed jobs.

Details: [Merge requests on GitLab](code.md#merge-requests-on-gitlab) and [GitLab pipelines](code.md#checks-logs-and-re-runs).

## Home

**Waiting on you** and **All repositories** include GitLab merge requests and issues, asked with your GitLab sign-in. GitLab's lists carry no check result or approval state, and each list reads the 50 most recently updated items. See [All repositories](getting-started.md#all-repositories).

## Not on GitLab yet

- **Inbox.** It shows github.com notifications only, and it is hidden while GitSimple is open on a GitLab sign-in only.
- **Pull request extras:** syncing a fork, the files-viewed marks, the "other pull requests that change the same files" warning, and added and removed line counts in the list.
- **Settings import** updates GitLab projects you already added, but doesn't add new ones: add them from GitLab first.

## A GitLab your company runs

- Your administrator registers an instance-wide OAuth app for GitSimple with the device flow turned on (GitLab 17.9 or newer) and gives you its app ID. Without one, sign in with an access token (GitLab 16.0 or newer).
- Behind a company proxy or HTTPS inspection, **Settings, Network** applies to GitLab too. See [Network](settings.md#network).
- **Not tested on a real server yet.** A company GitLab uses the same features as gitlab.com, but it hasn't been tried against a real self-managed server yet, so please report anything that doesn't work.
