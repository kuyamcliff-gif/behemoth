# Behemoth

A Claude skill that builds anything properly: websites, apps, games, SaaS, bots, tools. Research first, ask everything upfront, design for scale from day one, test before claiming, write like a human, hand over full ownership.

## Why this exists

Most AI builders take your prompt and run. You get a purple gradient hero, filler copy, code that works for 10 users and dies at 10,000, and no idea what was actually built. Behemoth treats you as a partner instead:

- **Researches your market first.** Names real competitors, what they do well, where your gap is, before a line of code exists.
- **Asks every question upfront**, in organized batches. Pages, features, what signed-in users see vs visitors, payments, rule enforcement, design direction, fonts with examples, the works. Then plays back a plain-English summary you approve before anything gets built.
- **Builds in professional phases**: research, interview, architecture, scaffold, feature-by-feature build, security audit, scale check, deployment, handoff, maintenance plan. Git checkpoint after every working feature, so "go back to before the cart" actually works.
- **Designs for scale from day one.** Stateless app layer, correct database choice, indexes, caching, CDN, rate limits. Ends with an honest statement of exactly what traffic level the setup handles and what to upgrade when you outgrow it.
- **Never claims something works without testing it.** Asks for test credentials and verifies signup, signin, payments, and admin flows end to end, with automated tests sized to the project so regressions get caught in CI, not by users. Runs a full security audit, including a supply chain check (dependency and secret scanning wired into CI, not a one-time scan) and a threat model tailored to what this specific product actually protects, before anything deploys.
- **Keeps working after launch.** Monitoring, alerting, an incident-response runbook, a verified (not just assumed) backup restore, and automated dependency updates are set up before handoff, so the product does not quietly rot or go dark the week after launch.
- **Writes like a human.** Every page sounds like it belongs to your product and your audience. Built-in detection for all known AI writing patterns, and the em dash never appears anywhere, not even in code comments.
- **No generic AI design.** Researches real successful sites in your exact niche, picks a distinctive direction, uses real brand icons, real or approved images, and sourced open-source interactive 3D when you want it.
- **Minimizes token cost.** A three-file memory system (plan, context, tasks) means changing one button never requires re-reading your whole project. Cache-friendly habits, subagent delegation, and honest cost estimates per model before starting.
- **Full ownership.** Ends with a plain-English handoff doc: what everything does, what every service costs, how to change things yourself, how to roll back.

Also reviews existing projects: point it at what you have and it tells you what is wrong, ranked by impact, with options.

## Install

**Claude Code:** copy the `behemoth` folder into `~/.claude/skills/` (personal) or `.claude/skills/` in your repo (project-level).

**Claude.ai:** zip the `behemoth` folder and upload it in Settings, Capabilities, Skills.

**Claude API:** upload via the Skills API endpoints.

Then just describe what you want to build. "Make a website for my auntie who sells clothes" is enough; Behemoth takes it from there.

## Structure

```
behemoth/
├── SKILL.md                    core workflow and rules
└── references/
    ├── stages.md               the ten phases in detail
    ├── interview.md            the complete question bank
    ├── design-rules.md         anti-generic design law and asset sourcing
    ├── humanize.md             AI writing pattern detection and voice rules
    ├── scale.md                architecture from 100 users to millions
    ├── security.md             the pre-deploy audit checklist, supply chain, threat modeling
    ├── testing.md              the test pyramid, CI gating, load and accessibility tooling
    ├── operations.md           monitoring, incident response, backups, dependency freshness
    ├── token-economy.md        cost minimization habits and estimation
    └── legal-pages.md          terms, privacy, and enforcement pages done right
```

Progressive disclosure keeps it cheap: only the name and description load until the skill triggers, and reference files load only when their phase needs them.

## License

MIT. Build things.
