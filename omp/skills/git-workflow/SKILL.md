---
name: git-workflow
description: Stage, commit, branch, and push safely in shared worktrees that may contain other people's or other agents' uncommitted work. Use when committing changes, writing commit messages, creating branches, pushing, or deciding what belongs in a commit.
---

# Git workflow

Only commit when the user asks for a commit in the current request. Otherwise verify the change, leave it in the working tree, and list the changed files.

## Before committing

1. Run `git status` and `git diff -- <paths>` for your changes. Read the diff before committing.
2. Commit only through the eval `commit` namespace. It selects only the lines you and your subagents wrote, and it never touches other agents' staged or unstaged work.
3. Do not use `git add`, `git commit`, `git stash`, `git reset`, `git checkout -- <path>`, or `git restore` in a shared worktree.
4. Keep generated and local files out of commits: build output, `node_modules`, `.env`, local config, `.vscode/`. Check `.gitignore` when unsure.
5. Run the narrowest check that covers the change, once. Record the result for the commit body.

## Commit call

```javascript
const d = await commit.draft({ repo: "<absolute repo path>" });
await d.take("<path/to/file>");
await d.commit("<subject>", ["<body line>", "<body line>"]);
```

- Subject: imperative mood, 72 characters or fewer. Match the repository's prefix style (`feat:`, `fix:`, `chore:`) if `git log --oneline -n 20` shows one.
- Body lines: explain why the change was needed. Note side effects. Keep lines under 72 characters.
- Take each path explicitly. Do not take whole directories you did not change.

## Branches and history

- Work on the branch the user names. If none is named, use the current branch and say so.
- Create a new branch for large or risky changes, or when the user asks for review.
- Never rewrite shared history: no reset with `--hard`, no force push, no rebase of pushed commits, no remote branch deletion without explicit approval.

## Pushing

- A push changes shared state. Ask for approval for each push, even if an earlier push was approved.
- Before pushing, show the commits (`git log --oneline origin/<branch>..HEAD`) and the remote.
- If the push is rejected, fetch and inspect the new commits. Do not force.

## Checklist

- [ ] Diff reviewed
- [ ] Only intended paths taken into the commit
- [ ] Check ran and passed, or reported as not run
- [ ] Subject and body written
- [ ] Push approved before pushing
