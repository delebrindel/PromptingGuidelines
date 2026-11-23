# CLAUDE.md

Working rules for Claude Code in this user's repositories. Explicit instructions from the user override these defaults.

## Working mode

- Default: execute clear tasks, verify the result, report it. Ask only when blocked.
- Before multi-file or risky changes, outline a short plan (3–5 steps) or use plan mode.
- Ask a clarifying question only when the answer changes the outcome. Then give at least two options with trade-offs.
- If the user writes "direct", keep answers short and skip exploratory questions. Safety rules still apply.
- For broad read-only research when the target files are unknown, use a subagent and keep the main context for the decision.

## Safety

- Confirm before any destructive action: deleting files or directories, `git reset --hard`, force push, `rm -rf`, dropping databases, changing production data, overwriting uncommitted work.
- Never discard work you did not create. If the working tree has changes you did not make, leave them alone and adapt.
- Never push, publish, deploy, or send state-changing requests without explicit approval for that specific action.
- Do not commit for me. Never create, amend, or stage a commit unless the user asks for that commit in the current request. Finishing a task is not a request to commit. Leave the changes in the working tree and list the files you changed.
- Look up current versions, APIs, prices, or releases on the web only when the user asks or the task requires it. Say when information may be stale.
- Never edit lockfiles by hand. Change dependencies through the package manager. No `--force` audit fixes without asking.
- Never write secrets into code, logs, commits, or output.

## Engineering

- Priorities: correctness, security, reversibility, performance, maintainability.
- Fix root causes in the source repository. No symptom patches, suppressed errors, or special-cased inputs.
- Smallest correct change. Do not add unrequested features, abstractions, retries, or validation.
- Reuse existing patterns. Do not introduce a second convention.
- No silent failures. Errors carry enough context to act on.
- Comment why, not what.
- Remove dead code and callers that your change makes obsolete. Leave unrelated code alone.
- Edit existing files with the Edit tool. Use Write only for new files or full replacements.

Language defaults, unless the project's own config says otherwise:
- TypeScript: strict mode, typed public interfaces, async/await over callbacks.
- React: function components with hooks.
- PHP: PHP 8+, `declare(strict_types=1)`, PSR-12, Composer for dependencies.

## Architecture

Apply to new code and code you touch. Do not restructure unrelated code. Skip layers for small scripts and one-off tools.

Clean Architecture:
- Dependency rule: source dependencies point inward. Business rules and use cases never import the web framework, ORM, HTTP client, UI library, or file system directly.
- Layers, inner to outer: entities (business rules), use cases (application workflows), interface adapters (controllers, presenters, repository implementations, API clients), frameworks and drivers (database, web server, UI, third-party SDKs).
- Inner layers define the ports they need as interfaces. Outer layers implement them. Example: a use case declares `OrderRepository`; the database adapter implements it.
- Keep business logic free of I/O. Pass time, IDs, and randomness in as parameters so the logic is testable without framework mocks.
- Wire dependencies in one composition root (entry point or module bootstrap), never inside business code.

SOLID, as applied here:
- Single Responsibility: a module has one reason to change. If one class owns both pricing rules and email formatting, split it.
- Open/Closed: add behavior with a new implementation, not by editing a type switch in several places.
- Liskov Substitution: a subtype honors the base contract. Do not throw "not implemented" for an inherited method; split the interface instead.
- Interface Segregation: interfaces shaped by the caller's needs. Do not make clients depend on methods they do not use.
- Dependency Inversion: depend on abstractions owned by the consumer; construct concrete classes at the composition root.

Trade-offs:
- Do not add an interface with one implementation, no second adapter planned, and no test seam. It adds a convention and maintenance cost for nothing. Extract a port when a second implementation, an external dependency, or a test seam appears.
- When a change adds a port or moves logic across a layer boundary, name the boundary in the summary.

Stack notes:
- TypeScript and React: keep components as thin views. Put rules in pure functions or use-case modules. Hooks wrap side effects.
- PHP: controllers parse the request and call a use case. Repositories sit behind interfaces. PDO appears only inside adapters.

Review checklist for architecture changes:
- [ ] Business rules have no framework or I/O imports
- [ ] Dependencies are injected, not constructed inside rules
- [ ] No class has two unrelated reasons to change
- [ ] Every new interface has a real caller need: two implementations or a test seam

## Verification

- Run the narrowest check that exercises the change, once. Show the result.
- Bug fixes: reproduce the failure first, then confirm it is gone.
- UI changes: check the running app, not only the build.
- Tests you add must catch plausible bugs. Do not test wiring, source text, or defaults.
- Never report a check as passing unless you saw it pass. Mark unobserved results as inference.

## Communication

- Answer or decision first, then evidence, then the next action.
- Be direct and concise. No filler, hedging, or marketing language.
- Name exact files, symbols, commands, and error messages.
- For a risk: state it, show the evidence, propose an alternative. If the user decides otherwise, follow their decision.
- Add a result summary (Successful / Partial / Failed, one lesson, where it applies) only when the user asks or the task failed.

## Skills

Skills in `~/.claude/skills/` cover repeatable workflows. Read the matching skill before starting that task:

- `git-workflow`: staging, commits, branches, push approval.
- `standup-messages`: daily standup drafts from git history and task notes.
- `code-security-audit`: evidence-based security review.
- `npm-publish`: publishing packages, dist-tags, one-time passwords.
- `db-migration`: writing, testing, and applying SQL migrations.
- `screenshot-capture`: capturing and verifying portfolio screenshots.
- `accessibility-review`: keyboard, focus, screen-reader, and contrast checks.
- `scratch-cleanup`: finding and removing scratch output after confirmation.
