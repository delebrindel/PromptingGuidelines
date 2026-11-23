---
name: git-workflow
description: Stage, commit, branch, and push safely in repositories that may contain other people's or other agents' uncommitted work. Use when committing changes, writing commit messages, creating branches, pushing, or deciding what belongs in a commit.
---

# Git workflow

Only commit when the user asks for a commit in the current request. Otherwise verify the change, leave it in the working tree, and list the changed files.

## Before committing

1. Run `git status` and `git diff -- <paths>` for the files you changed. Read the diff before staging.
2. Stage explicit paths only: `git add -- path/to/file`. Never use `git add -A`, `git add .`, or `git commit -a` in a shared worktree.
3. Leave unrelated changes unstaged. Do not revert, stash, reformat, or delete them.
4. Keep generated and local files out: build output, `node_modules`, `.env`, local config, editor folders. Check `.gitignore` when unsure.
5. Run the narrowest check that covers the change, once. Record the result for the commit body.

## Commit message

- Subject: imperative mood, 72 characters or fewer. Match the repository's prefix style (`feat:`, `fix:`, `chore:`) if `git log --oneline -n 20` shows one.
- Body: explain why the change was needed. Wrap at 72 characters. Note side effects.
- Use `git commit -F <file>` for multi-line messages so the body formatting survives.

## Branches and history

- Work on the branch the user names. If none is named, use the current branch and say so.
- Create a new branch for large or risky changes, or when the user asks for review.
- Never rewrite shared history: no `git reset --hard`, no `git push --force`, no `git rebase` of pushed commits, no remote branch deletion without explicit approval.

## Pushing

- A push changes shared state. Ask for approval for each push, even if an earlier push was approved.
- Before pushing, show the commits (`git log --oneline origin/<branch>..HEAD`) and the remote.
- If the push is rejected, run `git fetch` and inspect the new commits. Do not force.

## Never on files you did not create

- `git checkout -- <path>`, `git restore`, `git stash`, `git clean`

## Checklist

- [ ] Diff reviewed
- [ ] Only intended paths staged
- [ ] Check ran and passed, or reported as not run
- [ ] Subject and body written
- [ ] Push approved before pushing
