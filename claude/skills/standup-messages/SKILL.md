---
name: standup-messages
description: Draft a daily standup message (yesterday, today, blockers) from git history, pull requests, and task notes. Use when asked for a standup, status update, or progress summary for a day, week, or sprint.
---

# Standup messages

## Gather evidence first

Collect facts before writing. Do not infer work that has no record.

1. Identify the repositories and the reporting window. Default: since the previous working day (skip weekends on Monday). Ask only if the repositories are unknown.
2. Commits by the user, per repository, using Bash:
   `git -C <repo> log --since="<date>" --author="<name or email>" --oneline --all`
3. Pull requests, if `gh` is installed. If not, call the GitHub REST API with `curl`.
4. Task notes, TODO lists, or tickets the user points to.
5. Uncommitted changes (`git -C <repo> status --short`). Report them as in progress, not done.

## Structure

Three sections, 1–3 bullets each.

**Yesterday**
- Completed work, stated as outcomes, with the repository or ticket name.

**Today**
- Planned work, based on evidence (open branches, uncommitted changes, open tickets).

**Blockers**
- What is blocked, what is needed, and from whom. Write "None" when there are none.

## Style

- Plain text, ready to paste into chat. No headings larger than bold text.
- Specific nouns: feature, module, or file names, and ticket IDs. Avoid vague wording such as "worked on stuff".
- One line per item. Keep the message under 120 words unless the user asks for more.
- Mark uncertainty explicitly, for example "(not verified: no commit yet)".

## Example

```
Yesterday
- Shipped the contact form validation in the marketing site (commit abc1234).
- Reviewed the auth middleware PR.

Today
- Finish the image compression step for the gallery.
- Start the migration script for the orders table.

Blockers
- Waiting on the staging database credentials from ops.
```

## Checklist

- [ ] Every item traces to a commit, PR, ticket, or uncommitted change
- [ ] Blockers name the missing input and its owner
- [ ] Under 120 words, or the length the user asked for
