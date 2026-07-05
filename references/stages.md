# The ten phases in full

Follow these in order. Each phase ends with a checkpoint the user can see.

## Personas: who actually does the work

Each phase below names which persona from `agents/` owns it, when that persona subagent is available (Claude Code with `agents/` installed, or an equivalent subagent mechanism). Dispatch the persona to do the research/drafting/building/reviewing work in its own isolated context, then relay its output to the user yourself; personas cannot talk to the user directly. On a surface with no subagent support, read the persona's file as instructions for how you should approach that phase yourself; nothing in this skill requires subagents to function, they just make each phase's work isolated and reviewable instead of one thread doing everything.

## Phase 0: research and feasibility

Goal: the user makes an informed go/no-go decision before anyone spends effort. Persona: `analyst`.

1. Restate the idea in one sentence and confirm you understood it.
2. Dispatch `analyst` (or do this research directly) to search the web for the top existing products doing the same or similar thing. Present the top 3: what each does well, what users complain about, what it costs. Cite sources.
3. State the gap this project can fill, or say honestly that the space is crowded and why the user might still win (niche, region, price, community).
4. Estimate build effort in three numbers: rough calendar time at normal pace, rough token cost range per model tier, and the number of distinct features. Use the method in token-economy.md. Search for current model pricing first.
5. Ask one question: "Go all out (thorough code, full tests, rich docs) or manage tokens tightly (lean output, essentials only)?" Respect the answer for the whole project.
6. Flag anything infeasible now (for example: "an iOS app cannot be published from this environment; I can build it fully but you will need a Mac plus an Apple developer account for the store step").

## Phase 1: the full interview

Goal: zero questions during the build. Everything is asked here, in organized batches. Persona: `pm` drafts the batches and the playback; you ask the user directly, personas cannot.

Use references/interview.md as the question bank, or have `pm` draft each batch from it. Batch questions by topic (identity, pages, users and state, money, safety, design, extras) so the user answers 5 to 8 related questions at a time, not 40 at once and not 1 every minute. If the interface has an option-picker tool, use it. For the design batch specifically, dispatch `ux` to research real references in the niche before presenting options.

Skip questions already answered by context (their earlier messages, their connected tools, their region). Check references/mcp-integrations.md first, a connected Figma or Jira may already answer some of this. Never ask what you already know.

End with the playback (`pm` drafts it, you present it): a plain-English description of the entire product as you understood it, page by page, flow by flow, including what happens to rule-breakers, what the AI agent may and may not do, and what the design will feel like. The user must approve this playback before Phase 2. This is a hard gate, not a formality: see references/subagent-workflow.md's refusal-gate section for what to do if asked to skip it. Save the approved playback into plan.md verbatim in Phase 3.

## Phase 2: architecture

Goal: the right stack for this project's real scale target, explained so the user could repeat the reasoning to a friend. Persona: `architect`, with `ux` for design direction.

1. Dispatch `architect` (or read references/scale.md yourself). If mobile is in scope per Phase 1, also read references/mobile.md and decide the stack (cross-platform vs native vs PWA) here, not mid-build.
2. Choose: framework, database (SQL vs NoSQL based on the data shape), auth provider, file storage, caching layer, hosting, CDN, payment provider, email/SMS provider. For each: one plain sentence of why, and roughly what it costs at their expected scale.
3. Prefer what the user already has. If Supabase is connected, use it. If they mentioned a Hetzner VPS, deploy there. Introducing a new paid service requires telling the user its cost and getting a yes.
4. Offer options where real choice exists (for example email verification: Resend vs Supabase built-in vs SES) with a one-line tradeoff each. Let the user pick. If a choice has genuine, non-obvious tradeoffs, consider references/party-mode.md instead of picking silently.
5. Write architecture.md: the choices above with reasoning, a module-boundary diagram (even a monolith gets one, per scale.md Tier 3), and the honest scale statement template. Present it in plain English. Get approval; this is the second half of the Phase 1/2 hard gate, see references/subagent-workflow.md.

## Phase 3: scaffolding

Persona: `scrum-master` for tasks.md, `devops` for the CI pipeline.

1. Initialize git.
2. Create the project structure for the chosen stack.
3. Write .claudeignore excluding: node_modules, dist, build, .next, .nuxt, out, coverage, __pycache__, venv, *.lock (package-lock.json, yarn.lock, pnpm-lock.yaml), .env (never read secrets into context), large media directories.
4. Create the four project files:
   - plan.md: the approved playback from Phase 1. This file changes only with explicit user approval.
   - architecture.md: the approved architecture from Phase 2, including its diagram. Same approval rule.
   - context.md: living map of the project. For every significant file: path, one-line purpose. Plus: current state, decisions made, gotchas discovered. Update after every work session.
   - tasks.md: dispatch `scrum-master` to turn plan.md and architecture.md into the full bite-sized, ordered task checklist per references/subagent-workflow.md, with sensitivity flags for tasks needing a human checkpoint. Mark items as todo / in progress / done / verified.
5. Set up environment variable files with named placeholders and a comment on where each value comes from.
6. Dispatch `devops` to set up the CI pipeline (GitHub Actions or the platform's equivalent): lint, tests, and a dependency/secret scan run on every push (see testing.md and security.md). This is one config file now; it is expensive to retrofit later.
7. First commit: "scaffold: project structure, plan, and task list".

## Phase 4: core build

Personas: `dev` builds, `qa` reviews twice, `debugger` handles runtime errors, per the full mechanics in references/subagent-workflow.md. The loop, per task in tasks.md:
1. Mark the task in progress in tasks.md. If flagged sensitive by `scrum-master`, get the quick human checkpoint before dev starts.
2. Dispatch `dev` in its isolated worktree: test first (red), implement, test again (green), handle error and empty states, not just the happy path. If discoverability matters for this page (per references/seo.md), meta tags/structured data/heading hierarchy go in now, not a later pass. Content follows references/humanize.md.
3. If dev hits a runtime error it cannot resolve immediately, dispatch `debugger` to reproduce, find the root cause, and fix it, then hand back to dev or straight to qa.
4. Dispatch `qa` for the spec-compliance pass, then again for the code-quality pass. A fail on either sends the task back to dev with specifics; do not merge on a partial pass.
5. Grep the changed files for the em dash and en dash characters. Remove any found.
6. Merge the worktree, update context.md (what was added, where it lives).
7. Commit with a message a human would write: "add checkout flow with MoMo and card options".
8. Mark the task verified. Next task.

Build order rule: foundation before decoration. Auth and data models before pages that need them. Payments before the pages that sell. The AI agent last, once there is real product data for it to know.

If a task turns out to need something not in plan.md or architecture.md, stop and ask. Do not silently expand scope.

## Phase 5: security audit

Persona: `security-officer`. Start with the two-minute threat model in references/security.md: name this product's real attack surface before running the checklist. Detect the product's archetype from plan.md and attach every applicable pack from references/compliance-packs.md, not just the generic checklist. Then run every applicable item against the whole codebase, including the software supply chain section (dependency and secret scanning wired into CI, not just a one-time audit). Fix findings immediately; they do not go in a report for later. Then commit: "security: audit pass, fixes applied". Summarize for the user in plain terms what was found, what packs applied and why, and what was fixed.

## Phase 6: scale and load check

1. Verify per references/scale.md: caching active, indexes on queried columns, assets on CDN, images optimized, no N+1 query patterns, rate limiting on auth and expensive endpoints.
2. Run the load and lightweight security testing from references/testing.md: a loop of concurrent requests against key endpoints (say what was and was not tested), plus an automated baseline scan (OWASP ZAP or similar) against staging if available.
2b. If discoverability matters, verify the references/seo.md checklist against the deployed site: sitemap.xml reachable, robots.txt correct, structured data validates, Core Web Vitals actually measured (not assumed) on the live URL.
3. Write the honest scale statement into HANDOFF.md later: "This setup comfortably handles roughly X concurrent users. The first thing to struggle will be Y. When you see Z symptom, upgrade W. Approximate cost of that upgrade: ...". Never imply infinite scale.

## Phase 7: deployment

Persona: `devops`.

1. Deploy to the chosen platform. Set every environment variable. Wire the domain. Confirm SSL.
2. Hit the live URL. Run through the critical flows in production: signup, signin, the core action, payment in test mode, admin access.
3. If the user provided test credentials for third-party services, use them to verify integrations end to end. If not, list exactly which flows remain unverified and how the user can verify them in five minutes.
4. For a mobile build, this phase is store submission, not just a deploy command: run the full references/mobile.md pre-submission checklist, submit to TestFlight/closed testing first, then production. Set expectations honestly on review time, it is Apple's and Google's clock, not something to promise a date against.
5. Commit and tag: "v1.0.0".

## Phase 8: handoff package

Persona: `devops` for the operational content. Follow references/operations.md for the monitoring, incident-response, backup, and dependency-freshness content. Write HANDOFF.md covering, in plain English:
- What was built, page by page, in two lines each.
- Every external service: what it does, where its dashboard is, what it costs, which env var holds its key.
- Credentials hygiene: which values must never be shared or committed.
- How to make simple changes themselves (edit this file for the homepage text, run this command to deploy).
- How to roll back: every feature has a git checkpoint; the command to return to any of them.
- The scale statement from Phase 6.
- Monitoring, incident response, and backup restore confirmation per references/operations.md, including the honest recovery expectation (RPO/RTO in plain terms).
- Where the legal pages are and the one thing they must update (their real contact address and jurisdiction).

## Phase 9: maintenance guidance

Persona: `l3-support` becomes the standing escalation point after this phase closes, for any future bug report or incident. Tell the user, briefly, per references/operations.md:
- The three numbers to glance at weekly (error rate, response time, database size) and where to see them.
- What the first real bug report will probably look like and how to hand it to Claude efficiently (paste the error, name the page, mention what the user was doing); this is exactly what `l3-support` triages first.
- The upgrade triggers from the scale statement.
- That dependency-update PRs (Dependabot/Renovate) will show up periodically and need a human glance and merge.

## Existing projects: the review flow

Persona: `project-auditor`. When reviewing rather than building:
1. Dispatch `project-auditor` to map the project from its structure and README, reading individual files only as needed.
2. Audit in this order: security (worst first, with compliance packs per references/compliance-packs.md), broken or fragile flows, scale traps, missing legal pages, AI-slop design and writing, missing state-awareness, missing SEO/GEO and CI/testing/operations discipline.
3. Present findings ranked by impact, each with: what is wrong, why it matters in plain terms, what fixing it involves, rough effort.
4. Let the user choose. Then run the normal phase discipline on the chosen work: agree, build, verify, commit, document.
