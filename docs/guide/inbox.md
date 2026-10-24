[User guide](README.md) › Inbox

# Inbox

Your GitHub notifications, from every repository, in GitSimple. They come from your github.com sign-in; notifications from a GitHub Enterprise Server or GitLab aren't shown yet, and while the app is open on a server's or GitLab's sign-in only (no github.com sign-in) the Inbox isn't in the sidebar, the palette or Settings. Open it from **Inbox** in the sidebar (under Home), with `G` then `N`, or from the command palette. The number on **Inbox** is how many are unread ("50+" when there are more than GitSimple fetched).

## Reading the list

Notifications are grouped by repository, the most recent first. Each row shows the title, the issue or pull request number, why you got it in plain words (Review requested, Mentioned, Assigned, Checks finished (CI), You commented, You opened this, ...) and when.

- **Unread** (the default, with the count) shows what you haven't read. **All** also shows read ones (the newest 50).
- In Basic mode, notifications that are only activity you follow (repositories you watch, threads you subscribed to) are folded under **N more you follow** at the bottom; click it to see them. Advanced mode shows one list.
- Click a notification to open it. It is marked read, as visiting it on GitHub would.
  - An issue or pull request of a repository you have added (marked **In GitSimple**) opens in GitSimple: the app switches to that repository and opens the issue panel or the pull request.
  - Anything else (another repository, a release, a CI run, a commit, a discussion) opens on github.com in your browser.
- Click the check mark at the end of a row to mark it read without opening it. Hover a row's title to see it in full.
- **Mark all as read** marks everything up to the newest notification in the list, folded ones included; one that arrives after that stays unread.
- **Several GitHub accounts.** Signed in to more than one github.com account? The Inbox shows all of their notifications in one list, and each row names the account it is for. The menu next to **Unread** shows one account's notifications only; **Mark all as read** then marks only that account's. Marking a notification read always does it as the account it names.

GitSimple asks GitHub for notifications about once a minute (as often as GitHub allows), also while the window is minimised or hidden in the tray. Unchanged answers don't count against your GitHub rate limit. Your existing sign-in is enough; no extra permission is asked for.

## Windows notifications

When something arrives while you're not using GitSimple (it's in the background, minimised or in the tray), Windows shows a notification for:

- a review request (for you or a team you're on),
- a mention (of you or a team you're on),
- an issue or pull request assigned to you,
- a finished GitHub Actions run you started (CI finished),
- new commits on the upstream of the open repository's current branch, found by the automatic fetch (Settings > Git, **Fetch automatically**) or by the tray's **Fetch all repositories** ("3 new commits on origin/main"; Fetch all sums up several repositories in one notification). With this one on, the automatic fetch also runs while GitSimple is minimised or in the tray, without ever asking for a sign-in. Click it to bring GitSimple back.

Each one has its own switch in [Settings, Notifications](settings.md#notifications); all are on at first. While GitSimple is the window you're using, only the Inbox badge changes. Click a Windows notification to bring GitSimple back with that notification open. If many arrive at once you get three and a summary ("4 more notifications").

Notifications that were already waiting when GitSimple started don't pop up; they are in the Inbox.

## Keeping GitSimple running in the tray

By default, closing the window quits GitSimple, and notifications stop until you start it again. Turn on **Keep running in the tray** in [Settings, Notifications](settings.md#notifications) to keep it running instead: closing the window hides it, and the GitSimple icon next to the clock has **Open GitSimple**, **Inbox**, **Recent repositories** (the five you opened last; picking one opens GitSimple on it), **Fetch all repositories** (fetches every repository that has a local clone, one after another, and says how it went when it's done, with a Windows notification while the window is hidden; a repository that would need you to sign in is skipped instead of asking) and **Quit GitSimple**. A click on the icon opens the window; hovering it shows the unread count. Starting GitSimple again (Start menu, taskbar) also brings the window back; only one GitSimple runs at a time.

**Start with Windows** (same settings page, off by default) opens GitSimple when you sign in to Windows: minimized, or straight to the tray when Keep running in the tray is on. It adds GitSimple to your own Windows startup list (no admin rights); turning it off in Windows' **Settings > Apps > Startup** shows here as off too, and turning it on here again also turns it back on there.

Updating GitSimple works the same with the tray on: the app quits for the installer and starts again.
