[User guide](README.md) › Unity projects

# Unity projects

GitSimple knows when a repository is a Unity project and checks the things that most often break one for the rest of the team: `.meta` files, GUIDs, project settings, the editor version and `.gitignore`. Every check only warns. Nothing blocks a commit, and GitSimple never changes your project settings.

A repository counts as a Unity project when a folder holds an `Assets` folder and `ProjectSettings/ProjectVersion.txt`: the repository's root folder, or a folder up to three levels below it (for example `Game`, next to your tools). Hidden folders, generated or dependency folders (`Library`, `node_modules`, `Build`, ...) and nested repositories or submodules are not searched. When a repository holds several Unity projects, GitSimple checks the first one it finds (the nearest, then alphabetical) and says which others it saw on Home.

## Why .meta files matter

Unity gives every asset (and every folder) under `Assets` a `.meta` file with a GUID. Scenes, prefabs and materials refer to other assets by that GUID, not by name. So an asset and its `.meta` have to be committed, moved and deleted together:

- an asset without its `.meta`: everyone else's Unity makes a new GUID, and every reference to the asset breaks;
- a `.meta` without its asset: Unity deletes the `.meta` again on the next machine;
- two `.meta` files with the same GUID (usually a file copied in Explorer): Unity gives one of them a new GUID.

The safest habit: create, move, rename and delete assets inside Unity, which keeps the pairs together.

Native plugin folders (`.bundle`, `.framework`, `.plugin`, `.xcframework`) are one asset: they have one `.meta` next to the folder and none inside.

## Checks before you commit

In **Changes**, a yellow box above **Commit** lists problems in what is staged:

| Warning | What to do |
| --- | --- |
| An asset (or a new folder) is staged without its `.meta` | **Stage N files** stages the `.meta`. If there is no `.meta` at all, open the project in Unity so it makes one, and check that `.gitignore` doesn't ignore `.meta` files |
| A `.meta` is staged without its asset | **Stage N files** stages the asset (or the new folder's files; a big new folder is staged as the folder). If the asset doesn't exist, leave the `.meta` out |
| An asset is deleted but its `.meta` stays, or the other way round | **Stage N files** stages the other deletion. If the other file is still there, delete the asset in Unity |
| A `.meta` belongs to an empty folder | Git doesn't keep empty folders, so the `.meta` appears and disappears on other machines. **Unstage N empty folders** takes it out of the commit |
| Two `.meta` files have the same GUID | Delete the copy and duplicate it in Unity (Ctrl+D) instead, or delete the copy's `.meta` so Unity makes a new one |
| Deleting an asset breaks references | The listed scenes, prefabs, materials or settings still use the asset's GUID. Remove those references in Unity first, or keep the asset |

One click stages at most 500 files (fewer when the paths are very long); when there are more, the button says "Stage 500 of N files" and the rest stays listed for the next click.

On Windows a file renamed only in upper and lower case is the same file, so such a rename of an asset or its `.meta` is not reported as a missing or left-over `.meta`.

The reference check looks at Unity's text files (scenes, prefabs, materials, animations, `.asset` files, the build settings and similar). It can only see references in projects saved as text (see below). Files stored in Git LFS are read from the LFS store on your computer (up to 200 files and 64 MB in all; files over 8 MB, usually baked lighting or terrain data, are skipped). One that isn't downloaded can't be checked, and the box then says the check is incomplete. In a very large change some checks may be cut short; the box says so too.

## A .meta conflict with two IDs

When the same asset was added on two branches separately (two people imported the same texture, say), each Unity gave it its own ID (GUID), and merging stops on its `.meta` file. Scenes, prefabs and materials find an asset by this ID, so this isn't something to merge line by line: only one ID can stay. The [conflict resolver](advanced-git.md#conflict-resolver) then shows **Two IDs for one asset**:

- both IDs, with how many files use each and the first few of them (the same search as the reference check above, including files stored in Git LFS);
- which one to keep: the ID other files use. If both are used, keep the one more files use, then pick the asset again in Unity where a reference shows as missing;
- **Use your whole file** / **Use their whole file** to keep one. The import settings of that side come along; change them in Unity afterwards if needed.

When both sides have the same ID and only import settings differ, it is a normal text conflict. `.meta` files never go to UnityYAMLMerge: Unity's Smart Merge documents nothing for them, and it can't choose an ID either.

## Project settings

On **Home**, GitSimple notes once per repository (**Dismiss** hides it) when a setting makes git work harder:

- **Asset Serialization** should be **Force Text**, so scenes and prefabs are text that git can show, compare and merge.
- **Version Control** should be **Visible Meta Files**, so the `.meta` files are real files that get committed.

Change them in Unity: **Edit > Project Settings > Editor**, then commit the changed files in `ProjectSettings`.

## Unity editor version

When the incoming commits of a pull or a merge change the project's `ProjectSettings/ProjectVersion.txt`, its preview says so: "This changes the Unity editor version from 2021.3.5f1 to 2022.3.10f1". Everyone on the team needs that Unity version to open the project afterwards. A version change that only your own branch made is not mentioned. The pull preview compares with the commits already fetched; **Check for new commits** first to see the latest.

Switching branches has no preview, so after a switch to a branch that uses another Unity version a notice says so: "This branch uses Unity 2022.3.10f1 (was 2021.3.5f1)".

## Recommended .gitignore

Unity's generated folders (`Library`, `Temp`, `Obj`, `Logs`, `UserSettings`, `Build`, `Builds`, ...) and IDE files (`*.csproj`, `*.sln`, `.vs`, ...) don't belong in git. When the project has no `.gitignore`, or one that doesn't cover them, **Home** offers **Add to .gitignore…**:

- the preview lists the exact lines;
- they are added to the end of `.gitignore` (or a new one), with the file's own line endings; nothing already in it changes;
- the lines go into the `.gitignore` of the Unity project's folder (for a project in `Game`, `Game/.gitignore`), where Unity's own template puts them;
- only entries that the repository's ignore files don't cover yet are added, whatever spelling or file already covers them. Your personal excludes (`core.excludesFile`) don't count, because your teammates don't have them; `.git/info/exclude` does count, since git gives no way to leave it out;
- the result is an ordinary change in **Changes**: commit it so the whole team gets it.

Files that are already committed stay in git; ignoring them only stops new ones from being added.

## More for Unity teams

- **Smart Merge for scene and prefab conflicts:** set UnityYAMLMerge in [Settings, Git](settings.md), then use the [conflict resolver](advanced-git.md#conflict-resolver). To have it run during the merge itself, see [Smart Merge during merges](#smart-merge-during-merges).
- **Readable diffs:** scenes, prefabs and other Unity assets get a **Summary** grouped by GameObject and component, package changes are summarised, and images show side by side. See [Unity files, package files and images](code.md#unity-files-package-files-and-images).
- **Git LFS and locks:** the Unity `.gitattributes` preset, lock-first editing and **My locks** on Home. See [Git LFS](advanced-git.md#git-lfs).
- **Safety net:** big files, leftover conflict markers and likely secrets are flagged before a commit. See [Safety net before a commit](code.md#safety-net-before-a-commit).
- **Big projects:** partial, shallow and sparse clones. See [Cloning a big repository](getting-started.md#cloning-a-big-repository).

## Smart Merge during merges

With UnityYAMLMerge set in [Settings, Git](settings.md), GitSimple can let it merge scenes, prefabs and the other Unity YAML assets while git merges, so many scene conflicts never reach you.

1. **Repository › LFS › Unity Smart Merge in merges › Use automatically…** previews an edit of the root `.gitattributes`: `merge=unityyamlmerge` is added to the line of each Unity YAML type (`*.unity`, `*.prefab`, `*.asset`, `*.mat`, `*.controller`, `*.anim` and the rest UnityYAMLMerge understands), or a new line is added for a type the file doesn't mention. Types stored in Git LFS (the merge driver would only see pointer files) and types with their own merge setting (`merge=...`, `-merge`, `binary`) are left alone, and the preview lists them. GitSimple also asks git which of your tracked Unity files are in LFS, so a broader rule such as `Assets/** filter=lfs` counts too.
2. Commit `.gitattributes` so your team gets it.

From then on, GitSimple's merge, pull, rebase, cherry-pick, revert and stash apply / pop pass UnityYAMLMerge to git as the merge driver for those files (`merge -p %O %B %A %A`, Unity's documented form). What it can't resolve stops as a normal conflict and opens in the [conflict resolver](advanced-git.md#conflict-resolver). A result that still contains conflict markers is treated as a conflict too, never committed.

GitSimple never writes the driver to git config: it is defined for each of those commands only, and the UnityYAMLMerge path is checked again every time (the file name, a fixed local drive, and only plain path characters, because git runs the driver through a shell). So:

- a merge in a terminal or in another git client merges these files as text, as before, unless you set up Smart Merge there yourself;
- with UnityYAMLMerge not set (or moved), GitSimple's merges are text merges too;
- the console doesn't use the driver.

UnityYAMLMerge may start the fallback tool named in its `mergespecfile.txt` for a conflict it can't resolve. GitSimple waits for it like for any merge step (up to the time limit in Settings, Git). Not tested with a real Unity install yet.

