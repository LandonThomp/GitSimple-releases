[User guide](README.md) › Inbox

# Inbox

Your GitHub notifications, from every repository, in GitSimple. They come from your github.com sign-in; notifications from a GitHub Enterprise Server aren't shown yet, and while the app is open on a server's sign-in only (no github.com sign-in) the Inbox isn't in the sidebar, the palette or Settings. Open it from **Inbox** in the sidebar (under Home), with `G` then `N`, or from the command palette. The number on **Inbox** is how many are unread ("50+" when there are more than GitSimple fetched).

## Reading the list

Notifications are grouped by repository, the most recent first. Each row shows the title, the issue or pull request number, why you got it (Review requested, Mentioned, Assigned, CI finished, Comment, Your activity, ...) and when.

- **Unread** (the default) shows what you haven't read. **All** also shows read ones (the newest 50).
- Click a notification to open it. It is marked read, as visiting it on GitHub would.
  - An issue or pull request of a repository you have added (marked **Added**) opens in GitSimple: the app switches to that repository and opens the issue panel or the pull request.
  - Anything else (another repository, a release, a CI run, a commit, a discussion) opens on github.com in your browser.
- Hover a row and click the check mark to mark it read without opening it.
- **Mark all as read** marks everything up to the newest notification you can see; one that arrives after that stays unread.

GitSimple asks GitHub for notifications about once a minute (as often as GitHub allows), also while the window is minimised or hidden in the tray. Unchanged answers don't count against your GitHub rate limit. Your existing sign-in is enough; no extra permission is asked for.

## Windows notifications

When something arrives while you're not using GitSimple (it's in the background, minimised or in the tray), Windows shows a notification for:

- a review request (for you or a team you're on),
- a mention (of you or a team you're on),
- an issue or pull request assigned to you,
- a finished GitHub Actions run you started (CI finished).

Each one has its own switch in [Settings, Notifications](settings.md#notifications); all are on at first. While GitSimple is the window you're using, only the Inbox badge changes. Click a Windows notification to bring GitSimple back with that notification open. If many arrive at once you get three and a summary ("4 more notifications").

Notifications that were already waiting when GitSimple started don't pop up; they are in the Inbox.

## Keeping GitSimple running in the tray

By default, closing the window quits GitSimple, and notifications stop until you start it again. Turn on **Keep running in the tray** in [Settings, Notifications](settings.md#notifications) to keep it running instead: closing the window hides it, and the GitSimple icon next to the clock has **Open GitSimple**, **Inbox** and **Quit GitSimple**. A click on the icon opens the window; hovering it shows the unread count. Starting GitSimple again (Start menu, taskbar) also brings the window back; only one GitSimple runs at a time.

Updating GitSimple works the same with the tray on: the app quits for the installer and starts again.
