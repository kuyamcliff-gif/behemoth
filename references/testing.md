# Testing law: nothing ships on faith

"I ran it once and it worked" is not testing. This file sizes automated testing to the project tier from token-economy.md and makes it a gate, not an afterthought, in Phase 4 and Phase 6.

## What every tier gets, no exceptions

- The three critical journeys (signup, the core action, payment if present) are exercised end to end, by an automated test or by literally driving the flow and recording what was checked. Never claim a flow works without doing one of these.
- The authorization boundary is tested as an actual test, not a one-time manual check: take a real request from user A, swap the ID to user B's resource, confirm it is rejected. This belongs in the suite, not just in the Phase 5 memory.
- Automated accessibility check (axe-core or Lighthouse CI) run against the key pages before calling design done. A manual glance at contrast is not enough.

## Sizing by tier (from the Phase 0 classification)

- **S (landing page, single-purpose tool):** smoke test the one or two flows that matter. No test framework overhead required if the tool is truly this small.
- **M (multi-page site with auth and a database):** unit tests on business logic (anything touching money, permissions, or limits), integration tests on API routes, e2e on signup/signin/core action.
- **L (marketplace/SaaS with payments, tiers, admin):** all of the above, plus e2e on the payment flow with the provider's test mode, admin actions, and tier enforcement. Race-condition and idempotency tests on money-moving endpoints (see security.md).
- **XL (L plus mobile or heavy realtime or AI features):** all of the above, plus tests on the realtime/AI paths: the agent's escalation path, rate limits on expensive AI calls, and offline/reconnect behavior for realtime features.

## The pyramid shape

Most tests are fast unit tests on pure logic (pricing math, permission checks, validation). Fewer are integration tests hitting a real or in-memory database. Fewest are full end-to-end browser tests, reserved for the handful of journeys that must never break. Do not invert this: a suite of 200 slow e2e tests and zero unit tests is slower to run and harder to debug than the reverse.

## CI gate

Tests, lint, and the dependency/secret scan from security.md run on every push, before merge or deploy, not just on the developer's machine. A failing pipeline blocks the deploy. Set this up in Phase 3 alongside git init; it costs one config file (GitHub Actions or the platform's equivalent) and pays for itself the first time a regression is caught before a user sees it.

## Load and lightweight security testing (Phase 6)

- Load: a loop of concurrent requests against key endpoints (autocannon, k6) reveals N+1 queries and missing indexes that unit tests never catch.
- Security: an automated baseline scan (OWASP ZAP baseline mode) against the staging URL catches missing headers, obvious injection points, and misconfigurations in minutes. This is not a substitute for the Phase 5 checklist, it is a cheap second opinion.

## Visual regression (optional, L/XL design-heavy products only)

If the design has many pages and a distinctive visual language worth protecting, a visual regression tool (Percy, Chromatic) catches accidental layout breaks from unrelated changes. Mention the added cost and only add it if the user wants that level of protection; it is not a default.

## State plainly what was and was not tested

Every phase that touches testing ends with an honest one-line summary: what ran, what passed, what remains manual or unverified. Never imply full coverage that was not actually run.
