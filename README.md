# GitSimple

**Your code and your GitHub issues, side by side, in one simple Windows app.**

Pick a repository, see what to do next, and do it: commit, push, open a pull request, move an issue across the board. No terminal needed.

This repository only holds GitSimple's installers and updates. The source code is private.

## GitSimple is invite-only

The app opens only for GitHub accounts on its invite list. If someone sent you this link, they have probably added you already. If the app says **"GitSimple is invite-only"** after you sign in, ask the person who invited you to check your GitHub username, then click **Check again**.

## Install

You need Windows 10 or 11 and [Git for Windows](https://git-scm.com/download/win). If Git is missing, the app tells you how to install it.

1. Open the [latest release](https://github.com/LandonThomp/GitSimple-releases/releases/latest) and download the file ending in **`_x64-setup.exe`** (under **Assets**).
2. Run it. It installs for your Windows user only, with no admin rights needed.
3. If Windows shows **"Windows protected your PC"**, click **More info**, then **Run anyway**. The installer isn't signed with a paid code-signing certificate, so Windows doesn't recognize it yet.

## First launch

1. Click **Sign in with GitHub**. Your browser opens GitHub, and the app shows a short code.
2. Enter the code on GitHub and approve. GitSimple asks for the `repo`, `read:org` and `project` scopes so it can work with your repositories, issues and Projects.
3. Add a repository: pick a folder that's already on your PC, or choose one of your GitHub repositories and clone it.

Your sign-in is stored in Windows Credential Manager, never in a file. You can sign out in **Settings, Account**.

## Updates

GitSimple checks for updates when it starts and every 6 hours, and shows a notification when a new version is out. Click **Update now**; the app restarts on the new version. Every update is checked against GitSimple's signing key before it installs.

To turn automatic checks off, go to **Settings, Updates**. You can still press **Check for updates** there at any time.

## Please don't share the installer

Send people to whoever invited you instead. The installer won't open for anyone who isn't on the invite list.
