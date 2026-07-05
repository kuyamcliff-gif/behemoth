# Security law: the Phase 5 audit checklist

Roughly 40% of AI-generated code ships with vulnerabilities. Behemoth does not. Run this full list in Phase 5 and keep it in mind during Phase 4. Fix findings immediately.

## Threat model first, two minutes, not optional

Before running the checklist below, name this specific product's real attack surface out loud: what data would hurt someone if leaked (payment metadata, health info, private messages), who would want in (a competitor, a bored user, a bot farm scraping listings), and what they would gain (money, other users' data, free access). This turns the generic checklist into a targeted audit instead of a box-ticking exercise, and it decides which sections below matter most for this build.

## Software supply chain (OWASP 2025 added this as its own top-3 category)

Treat AI-generated code, including your own output, as an untrusted contribution that needs the same scrutiny as a third-party pull request, not a free pass because an assistant wrote it.

- [ ] Automated dependency scanning wired into CI (Dependabot, Renovate, or Snyk), not just a one-time `npm audit` at the end. Vulnerable packages get flagged on every push, not discovered at the next audit.
- [ ] Secret scanning with push protection enabled on the repo host (GitHub secret scanning, gitleaks in CI) so a leaked key is blocked before the commit lands, not found after.
- [ ] Lockfiles committed and CI installs from the lockfile only (`npm ci`, not `npm install`), so builds are reproducible and a compromised registry cannot silently swap a dependency.
- [ ] For anything going to production in a regulated niche (health, finance, government customers): generate a basic SBOM (CycloneDX or SPDX, most package managers have a one-command exporter) so the user can answer "what's in this" if ever asked.
- [ ] No dependency added without checking it is maintained and reasonably popular; a single-maintainer package with no updates in years is a supply chain risk, not just a preference.

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
- [ ] MFA/2FA offered (at minimum for admin and financial accounts); use the auth provider's built-in support rather than hand-rolling TOTP.
- [ ] OAuth/social login flows use PKCE and validate the `state` parameter; never trust a redirect callback without it.
- [ ] If hand-issuing tokens (JWT or similar): algorithm pinned server-side (reject `alg: none`, reject a client-supplied algorithm), signature always verified, short expiry with rotation, never used as the sole authorization check for a sensitive action without a server-side re-check.

## Business logic and race conditions

- [ ] Money and inventory changes happen inside a database transaction with proper locking; two simultaneous requests cannot both succeed at spending the same balance or claiming the last unit of stock.
- [ ] Payment and webhook endpoints are idempotent (an idempotency key or a check against an already-processed order ID); a retried request or a double-click never double-charges or double-fulfills.
- [ ] Coupon codes, referral bonuses, and free-tier limits are checked server-side against real usage history, not just a client-supplied flag.
- [ ] Negative, zero, or absurdly large quantities and amounts are rejected before they reach payment or inventory logic.

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

## Error handling (OWASP 2025 added this as its own category)

- [ ] Unexpected exceptions fail closed: a crash or timeout in a permission check denies the action, it never silently grants it or falls through to a default-allow path.
- [ ] Catch-all error handlers never swallow authorization or payment failures; they log and surface a safe error, they do not continue as if the check passed.
- [ ] Third-party API failures (payment provider down, email provider timeout) are handled explicitly; the code path is decided on purpose, not left to whatever the language does when an unhandled exception hits.

## Logging and alerting

- [ ] Security-relevant events are logged with enough detail to investigate later: failed logins, permission denials, payment failures, admin actions. Never log passwords, tokens, or full card numbers.
- [ ] Alerting is wired for spikes (login failure rate, 500 rate, payment decline rate), not just a dashboard nobody watches. Even a free-tier Sentry alert or an email on threshold breach counts.

## Responsible disclosure

- [ ] For any product handling user data or payments, add a `security.txt` file (RFC 9116, at `/.well-known/security.txt`) with a real contact for someone who finds a vulnerability to report it responsibly.

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
