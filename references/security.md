# Security law: the Phase 5 audit checklist

Roughly 40% of AI-generated code ships with vulnerabilities. Behemoth does not. Run this full list in Phase 5 and keep it in mind during Phase 4. Fix findings immediately.

## Secrets and configuration

- [ ] No API key, password, token, or connection string anywhere in the codebase, comments, or git history. All secrets in env vars. If a secret ever touched a commit, rotate it and scrub history.
- [ ] .env in .gitignore and .claudeignore. .env.example exists with placeholder names only.
- [ ] Debug flags, verbose error output, and framework debug pages off in production. Stack traces never reach the user.
- [ ] No leftover test routes, seed endpoints, or commented-out backdoors.

## Authentication and sessions

- [ ] Passwords hashed with bcrypt/argon2 via the auth provider or framework, never hand-rolled, never logged.
- [ ] Rate limiting on login, signup, password reset, and OTP endpoints (block brute force).
- [ ] Session tokens httpOnly, secure, sameSite. Reasonable expiry. Logout actually invalidates.
- [ ] Password reset tokens single-use and expiring. Reset flow does not reveal whether an email exists.
- [ ] Email/phone verification actually gates the actions it is supposed to gate.

## Authorization (the one AI code fails most)

- [ ] Every server route checks who is asking, not just whether they are signed in. User A must not read or modify user B's data by changing an ID in the URL or payload (test this literally: take a real request, swap the ID, confirm rejection).
- [ ] Admin routes verify the admin role server-side. Hiding the button in the UI is not authorization.
- [ ] If using Supabase or similar: Row Level Security enabled on every table, policies written and tested. Service-role keys used only server-side, never shipped to the client.
- [ ] Tier limits (free vs pro) enforced on the server, not just in the UI.

## Input handling

- [ ] All database access through parameterized queries or the ORM. Zero string-concatenated SQL.
- [ ] All user-rendered content escaped or sanitized (XSS). Rich text goes through a sanitizer with an allowlist.
- [ ] File uploads: type checked by content (not extension), size limited, stored in object storage under generated names, never executed, served from a separate domain or with safe content-type headers.
- [ ] Server-side validation on every input, regardless of client validation. Reject unexpected fields (no mass assignment).
- [ ] CSRF protection on state-changing form endpoints (framework default or tokens); safe methods stay safe.

## APIs and communication

- [ ] HTTPS everywhere; HTTP redirects to HTTPS.
- [ ] CORS restricted to the real frontend origin(s), not "*", on any authenticated API.
- [ ] Webhooks verify signatures (Stripe, payment providers, etc.). Payment amounts verified server-side against the order, never trusted from the client.
- [ ] Internal endpoints and cron/queue triggers require a secret or are network-isolated.
- [ ] Security headers set: Content-Security-Policy (at least a basic one), X-Content-Type-Options, X-Frame-Options or frame-ancestors, Referrer-Policy.

## Dependencies and platform

- [ ] Run the ecosystem audit (npm audit / pip-audit) and resolve high/critical findings.
- [ ] No abandoned or suspicious packages; prefer well-maintained libraries checked during Phase 2.
- [ ] Lockfiles committed so deploys are reproducible.

## Data care

- [ ] Collect only the data the product needs (this also keeps the Privacy page honest).
- [ ] Anything sensitive at rest (tokens for third-party accounts, etc.) encrypted or stored by the provider, not in plaintext columns.
- [ ] Logs do not contain passwords, tokens, or full card numbers. Payment card data never touches the server (provider-hosted fields only).
- [ ] Account deletion actually deletes or anonymizes what the Privacy page says it does.
- [ ] Database backups enabled on the managed provider; note the restore procedure in HANDOFF.md.

## The product's AI agent (if present)

- [ ] System prompt does not contain secrets.
- [ ] The agent cannot be prompt-injected into performing forbidden actions; its tool access is limited to exactly the allowed operations, enforced server-side (the model asking is not authorization).
- [ ] User content passed to the agent is treated as untrusted data, not instructions.
- [ ] Escalation path tested end to end.

## Closing the audit

Summarize for the user in plain terms: what was checked, what was found, what was fixed. One short paragraph, no drama, no jargon. Commit as "security: audit pass, fixes applied".
