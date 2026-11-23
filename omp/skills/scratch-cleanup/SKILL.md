---
name: scratch-cleanup
description: Find, classify, and remove scratch files, logs, previews, and temporary shot folders left by our work, only after explicit confirmation. Use when cleaning up after a task, when the drive root or a repo has leftover output, or when asked to remove temporary files.
---

# Scratch cleanup

Delete only files you can name and tie to a task. The drive root and the user folders hold personal data and installers.

## Where scratch goes

- Write new scratch output to `D:/scratch/<task>/`. Create the folder when you need it.
- Do not write logs, previews, or debug images at the drive root, inside a repo, or inside a user folder.

## Inventory

1. Start from known paths: `D:/scratch/` and the leftovers recorded for this task. Do not glob the drive root.
2. Check each path exists. Record its size and modified date.
3. Classify each item:
   - **scratch**: logs, previews, debug images, and build output from our runs. Safe to delete after confirmation.
   - **shot folder**: screenshot output. Confirm ownership first. It may hold the only copy of a capture.
   - **backup**: SQL dumps and git bundles. Never delete without explicit confirmation. Say whether another copy exists.
   - **unknown**: anything you cannot tie to this work. Leave it and ask.
4. Folders that contain an app or repo copy are not scratch. They need their own decision.

## Confirm

- Show the list grouped by class, with paths and sizes.
- Ask for confirmation of the exact list with `ask`. Preselect only scratch items. Leave backups and shot folders unselected.
- Do not assume approval from earlier messages.

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
