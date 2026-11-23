---
name: scratch-cleanup
description: Find, classify, and remove scratch files, logs, previews, and temporary shot folders left by a task, only after explicit confirmation. Use when cleaning up after a task, when the filesystem root or a repo has leftover output, or when asked to remove temporary files.
---

# Scratch cleanup

Delete only files you can name and tie to a task. The filesystem root and the home and user folders hold personal data and installers.

## Where scratch goes

- Write new scratch output to a dedicated folder outside any repo, for example `<scratch-root>/<task>/`. Create the folder when you need it.
- Do not write logs, previews, or debug images at the filesystem root, inside a repo, or inside a home or user folder.

## Inventory

1. Start from known paths: the scratch folder and the leftovers recorded for this task. Do not glob the filesystem root or home folders.
2. Check each path exists. Record its size and modified date.
3. Classify each item:
   - **scratch**: logs, previews, debug images, and build output from our runs. Safe to delete after confirmation.
   - **shot folder**: screenshot output. Confirm ownership first. It may hold the only copy of a capture.
   - **backup**: SQL dumps and git bundles. Never delete without explicit confirmation. Say whether another copy exists.
   - **unknown**: anything you cannot tie to this work. Leave it and ask.
4. Folders that contain an app or repo copy are not scratch. They need their own decision.

## Confirm

- Show the list grouped by class, with paths and sizes.
- Ask for confirmation of the exact list. Preselect only scratch items. Leave backups and shot folders unselected.
- Use the question or form tool available in the harness. Do not assume approval from earlier messages.

## Delete

- Delete only confirmed paths, by exact path. No wildcards over the drive root and no recursive delete of folders you did not list.
- Remove an empty scratch folder only after its contents are gone.
- Re-list the confirmed paths afterward. Report anything that remains.

## Rules

- Never delete user folders, installers, or files you did not create.
- Never delete the only copy of a backup or bundle without explicit approval.
- Never delete files inside a git repository as scratch cleanup. Use the repo's own workflow.

## Checklist

- [ ] Items checked to exist, sizes recorded
- [ ] Each item classified; unknowns left alone
- [ ] Exact list confirmed by the user
- [ ] Only confirmed paths deleted
- [ ] Post-delete listing shows them gone
