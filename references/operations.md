# Operations law: what keeps it alive after Phase 7

Launch is not the finish line. A product with no monitoring, no dependency freshness, and an untested backup is one incident away from disaster, and nobody finds out until it is too late. This is short by design; do the minimum below for every product, more for L/XL.

## CI/CD

- Every push runs lint, tests, and the dependency/secret scan (see testing.md and security.md) before deploy. Set this up in Phase 3, not as a Phase 7 afterthought.
- L/XL tier: a staging environment that mirrors production configuration (same env var names, a similar data shape) sits between a merge and a production deploy. "It worked in staging" only means something if staging actually resembles production.
- Rollback is always the git tag from Phase 7 plus the platform's redeploy-previous-version button. Confirm this actually works once, do not assume it.

## Monitoring and alerting

- Error tracking (Sentry or equivalent) wired in from Phase 4, with an actual alert threshold configured, not just a dashboard nobody opens.
- Uptime monitoring on the production URL (UptimeRobot, Better Uptime, or the host's own) hitting a real health-check endpoint, with a notification channel the user actually checks (email or a phone alert, not a silent log).
- For products with real paying users, a one-page public status page is worth the fifteen minutes it takes to set up; it turns "is it down for everyone" into a link instead of a support ticket flood.

## Incident response (goes in HANDOFF.md)

A short runbook, three to five lines: what to check first when something breaks (error tracker, then host status, then database), who to contact for each external service if it is the one failing, and the rollback command. This is not a corporate playbook, it is "if the site is down at 2am, here is the order to check things in."

## Backups and recovery

- Confirm the managed provider's automated backup is actually enabled, not just assumed to be on by default.
- Restore it once, to a scratch database, before calling this done. "Backups enabled" and "backups that restore" are different claims; only make the second one if it was actually tested.
- State the honest recovery expectation in HANDOFF.md: roughly how much data could be lost between backups (RPO) and roughly how long a restore takes (RTO), in plain terms, not jargon.

## Dependency freshness

Automated dependency update PRs (Dependabot or Renovate) configured before handoff, so the project does not start rotting the day after launch. Note in HANDOFF.md that these PRs need a human glance and a merge, they are not fully autonomous.

## What goes in the weekly glance (Phase 9)

Three numbers: error rate, response time, database size, and where to see each (usually one dashboard). State what a bad number looks like for each, in plain terms, so the user recognizes trouble without needing to ask.
