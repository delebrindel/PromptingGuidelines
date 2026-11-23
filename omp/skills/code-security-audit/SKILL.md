---
name: code-security-audit
description: Evidence-based security audit of a repository or a diff. Covers secrets, injection, authentication and sessions, authorization, input and output handling, file handling, configuration and headers, and dependencies. Use when asked for a security review, vulnerability scan, or pre-release security check.
---

# Code security audit

Report only what you can show from the code, configuration, or a reproduction. Mark everything else as inference.

## Rules

- Do not run attacks against systems you do not own or against production data.
- Do not modify code during the audit unless the user asks for fixes.
- Do not run `npm audit fix --force`, `composer update`, or any command that changes lockfiles. Use read-only audits (`npm audit`, `composer audit`).
- Never print secret values. Show the file and line, and redact the value.

## Scope

1. Confirm the target: whole repository, a branch, or a diff (`git diff <base>...HEAD`).
2. Inventory: languages, entry points (routes, API handlers, CLI commands, cron jobs), auth boundaries, data stores, external calls.
3. Read configuration: environment handling, debug flags, CORS, security headers, cookie settings, TLS.

For a large repository, delegate the inventory to a read-only `scout` subagent and keep the findings in the main context.

## Method

Use `grep` and `glob` to find risky patterns, then `read` the code path to confirm. For call sites of a suspicious function, use `xd://lsp` references rather than text search. A grep hit alone is not a finding.

| Category | Look for |
|---|---|
| Secrets | Hardcoded keys, tokens, passwords, connection strings, private keys in source, config, or history |
| Injection | String-built SQL, shell commands with user input, `eval`, template injection, unsafe `innerHTML` or `dangerouslySetInnerHTML` |
| Authentication | Weak password hashing, no rate limit on login, predictable tokens, session fixation |
| Sessions and cookies | Missing `HttpOnly`, `Secure`, or `SameSite`; session IDs in URLs; no session regeneration on login |
| Authorization | Object IDs accepted without ownership checks; admin routes without role checks; client-supplied roles trusted |
| Input and output | Missing server-side validation, unescaped output (XSS), open redirects, mass assignment |
| CSRF | State-changing requests without a token or same-site protection |
| Files | Paths built from user input, uploads checked only by extension, uploads served from writable directories |
| Network | SSRF through user-supplied URLs, disabled certificate checks, permissive CORS with credentials |
| Deserialization | `unserialize` or equivalent on untrusted input |
| Configuration | Debug mode on, verbose errors shown to users, missing security headers, directory listing |
| Dependencies | Known vulnerable versions reported by a read-only audit |

For a web UI behind login, reproduce suspected issues with the `browser` namespace on a local instance you control. Close the tab afterward.

## Severity

- **Critical**: remotely exploitable without authentication, leading to data loss or full compromise.
- **High**: low-effort exploit leading to data exposure, account takeover, or privilege escalation.
- **Medium**: needs specific conditions or a logged-in user; limited impact.
- **Low**: defense-in-depth gap with no shown exploit path.
- **Info**: observation, not a vulnerability.

## Report format

Order findings by severity. For each:

- **Title** and severity
- **Location**: file and line
- **Evidence**: the code path, and the reproduction if one was run
- **Impact**: what an attacker gains
- **Fix**: the change at the source, not a local workaround
- **Status**: Verified, or Inference (with what would confirm it)

End with: scope covered, categories with no findings, and what was not checked.

## Checklist

- [ ] Scope and inventory recorded
- [ ] Every category checked or marked not applicable
- [ ] Every finding has a file location and evidence
- [ ] No secret values in the output
- [ ] No files modified and no lockfiles changed
