# Behemoth

A Claude skill that builds anything properly: websites, web apps, iOS/Android apps, games, SaaS, bots, tools. Research first, ask everything upfront, design for scale and security from day one, test before claiming, write like a human, get found by search and AI, hand over full ownership.

## Why this exists

Most AI builders take your prompt and run. You get a purple gradient hero, filler copy, code that works for 10 users and dies at 10,000, a security hole nobody checked for, and no idea what was actually built. Behemoth treats you as a partner instead:

- **Researches your market first.** Names real competitors, what they do well, where your gap is, before a line of code exists.
- **Asks every question upfront**, in organized batches. Pages, features, what signed-in users see vs visitors, payments, rule enforcement, design direction, fonts with examples, the works. Then plays back a plain-English summary you approve before anything gets built.
- **Builds in professional phases**: research, interview, architecture, scaffold, feature-by-feature build, security audit, scale check, deployment, handoff, maintenance plan. Git checkpoint after every working feature, so "go back to before the cart" actually works.
- **Designs for scale from day one.** Stateless app layer, correct database choice, indexes, caching, CDN, rate limits. Ends with an honest statement of exactly what traffic level the setup handles and what to upgrade when you outgrow it.
- **Never claims something works without testing it.** Asks for test credentials and verifies signup, signin, payments, and admin flows end to end, with automated tests sized to the project so regressions get caught in CI, not by users.
- **Takes security seriously, not as a checkbox.** A full audit covering authentication, authorization, input handling, business logic and race conditions, and the software supply chain (dependency and secret scanning wired into CI, not a one-time scan at the end), starting from a threat model of what this specific product actually protects.
- **Keeps working after launch.** Monitoring, alerting, an incident-response runbook, a verified (not just assumed) backup restore, and automated dependency updates are set up before handoff, so the product does not quietly rot or go dark the week after launch.
- **Gets found.** Technical SEO built into every page as it is built, not bolted on later, plus generative engine optimization so AI answer engines (ChatGPT, Perplexity, AI Overviews, Claude) can find and cite it too.
- **Ships mobile properly.** When the build is an iOS or Android app, follows the real submission lifecycle: stack choice, TestFlight/closed testing, store metadata and privacy labels, the actual rejection reasons stores cite most, honest review-time expectations.
- **Writes like a human.** Every page sounds like it belongs to your product and your audience. Detects and removes every known AI writing pattern, and the em dash never appears anywhere, not even in code comments.
- **No generic AI design.** Researches real successful sites in your exact niche, picks a distinctive direction, uses component foundations like shadcn/ui for structure but never ships them unstyled, uses real brand icons, real or approved images, and sourced open-source interactive 3D when you want it.
- **Minimizes token cost.** A three-file memory system (plan, context, tasks), the same idea behind spec-driven development tools like GitHub's Spec Kit, means changing one button never requires re-reading your whole project. Planning depth scales with project size. Split-surface planning lets you run the research/interview/architecture phases on a flat-rate Claude.ai or Cowork plan, then hand the approved plan to metered Claude Code for the actual build, the same cost-splitting trick behind BMAD-METHOD's web bundles.
- **Real persona subagents, not one voice narrating every role.** `agents/` ships an analyst, PM, architect, UX designer, scrum master, dev, QA, debugger, security officer, devops, L3 support, and project auditor, each an isolated subagent with its own job, mirroring what BMAD-METHOD and great_cto's SDLC subagent teams do. Dev and QA run a genuine two-stage review (spec compliance, then code quality) instead of the same model grading its own work, each feature builds in its own isolated git worktree, and TDD (red, then green) is a hard gate, not a suggestion, the same discipline behind Superpowers' engineering-grade workflow.
- **Party mode for real disagreements.** When a decision has a genuine tradeoff (SQL vs NoSQL for an odd data shape, monolith vs an early service split, security vs usability), Behemoth dispatches multiple relevant personas on the same question and presents their actual disagreement instead of one silent pick.
- **Pulls real project data when it is connected.** GitHub, Jira/Atlassian, Figma, and SonarQube integrations (when available) replace a guess with the user's actual backlog, design tokens, or code-quality metrics instead of researching from scratch every time.
- **Compliance packs that attach themselves.** Payments, health data, minors, GDPR/CCPA, an embedded AI agent, or an automated-decision feature each pull in their own extra checklist during the security audit automatically, based on what the product actually does, not left for the user to know to ask about by name.
- **Full ownership.** Ends with a plain-English handoff doc: what everything does, what every service costs, how to change things yourself, how to roll back, how it stays alive after you stop reading this repo.

Also reviews existing projects: point it at what you have and it tells you what is wrong, ranked by impact, with options.

## Honest scope

This skill is thorough, not magic. It cannot guarantee a search ranking, an app store approval, or that nothing will ever break; what it guarantees is that the technical foundation, the security posture, and the process are done right, so the things within its control are not what causes a failure later. Any claim it makes to you about what was tested or verified is meant literally, if something was not checked, it will say so.

One more honest note: Behemoth is a skill, a set of instructions and persona subagents that run inside whatever Claude surface you use. It is not a standalone autonomous sandbox runtime like OpenHands or Devin, it uses the execution environment (terminal, file system, browser) that Claude Code, Cowork, or the Agent SDK already provides rather than shipping a separate one. It also does not maintain a cross-project memory service the way Devin's Wiki/Search does; context.md is per-project by design, for the token-economy reasons in that file. If you specifically need a self-hosted, always-on autonomous engineering runtime independent of any Claude surface, that is a different category of tool than a skill.

## Install and use, on every Claude surface

The skill is the same everywhere; only the install step differs.

**Claude Code (CLI/terminal, and the VS Code/JetBrains extensions):** copy the `behemoth` folder into `~/.claude/skills/` for every project on your machine, or into `.claude/skills/` inside one repo for that project only. Claude Code auto-loads it; the skill activates when you describe something to build. To get the real isolated persona subagents (not just one thread narrating every role), also copy `behemoth/agents/*.md` into `~/.claude/agents/` (personal) or `.claude/agents/` (project); Claude Code will then dispatch `analyst`, `pm`, `architect`, `ux`, `scrum-master`, `dev`, `qa`, `debugger`, `security-officer`, `devops`, `l3-support`, and `project-auditor` by name.

**Claude.ai (web/desktop app):** zip the `behemoth` folder and upload it under Settings, Capabilities, Skills. It then activates in any conversation on that account. Claude.ai does not run custom subagents the way Claude Code does, so personas are followed as instructions in the main conversation instead of dispatched as isolated subagents; this is exactly where split-surface planning helps, do Phases 0 to 2 here, then move to Claude Code for Phase 3 onward.

**Claude Cowork:** Cowork uses the same skills system as Claude.ai; install it the same way (Settings, Capabilities, Skills). Cowork is aimed at non-developers automating file and desktop workflows, but "build me a website" or "review my project" inside Cowork triggers the same Behemoth process; it will ask the same upfront questions in plain language.

**Claude Code on the web / GitHub-connected sessions:** commit the `behemoth` folder to `.claude/skills/` and `behemoth/agents/*.md` to `.claude/agents/` in the repository the session works from; both load automatically the same way local Claude Code does.

**Claude API / Agent SDK (building your own app on Claude):** upload the skill via the Skills API endpoints, or point a Claude Agent SDK session's skills directory at this folder; the SDK also supports defining subagents programmatically, so `agents/*.md` can be loaded the same way for isolated persona dispatch. Useful if you are building a product that itself uses Claude to build things for its own users.

Once installed anywhere, just describe what you want. "Make a website for my auntie who sells clothes" or "build me an iOS app like X but for Y" is enough; Behemoth takes it from there; asks what it needs to ask, once, in batches; and does not start writing product code before you approve the plan.

## Structure

```
behemoth/
├── SKILL.md                    core workflow and rules
├── agents/                     one subagent definition per persona (copy to .claude/agents/)
│   ├── analyst.md              Phase 0 research and feasibility
│   ├── pm.md                   Phase 1 interview batches and playback
│   ├── architect.md            Phase 2 stack and architecture.md
│   ├── ux.md                   design research and direction proposals
│   ├── scrum-master.md         turns plan/architecture into tasks.md
│   ├── dev.md                  implements one task at a time, TDD-first
│   ├── qa.md                   two-stage review: spec compliance, then code quality
│   ├── debugger.md             dedicated runtime-error diagnosis loop
│   ├── security-officer.md     Phase 5 audit plus compliance packs
│   ├── devops.md                CI/CD, deployment, monitoring, operations
│   ├── l3-support.md           post-launch incident triage
│   └── project-auditor.md      the existing-project review flow
└── references/
    ├── stages.md               the ten phases in detail, with the owning persona per phase
    ├── interview.md            the complete question bank
    ├── design-rules.md         anti-generic design law, component foundations, asset sourcing
    ├── humanize.md             AI writing pattern detection, voice rules, and why they work
    ├── scale.md                architecture from 100 users to millions
    ├── security.md             the pre-deploy audit checklist, supply chain, threat modeling
    ├── testing.md              the test pyramid, CI gating, load and accessibility tooling
    ├── operations.md           monitoring, incident response, backups, dependency freshness
    ├── seo.md                  technical SEO and generative engine optimization
    ├── mobile.md               iOS/Android lifecycle and store submission
    ├── token-economy.md        cost minimization, planning depth by tier, split-surface planning
    ├── legal-pages.md          terms, privacy, accessibility, and enforcement pages done right
    ├── subagent-workflow.md    worktrees, the hard refusal gate, TDD gate, two-stage review
    ├── party-mode.md           multi-persona debate for genuinely contested decisions
    ├── mcp-integrations.md     using GitHub/Jira/Figma/SonarQube data instead of guessing
    └── compliance-packs.md     archetype-specific compliance checklists that auto-attach
```

Progressive disclosure keeps it cheap: only the name and description load until the skill triggers, reference files load only when their phase needs them, and persona subagents load only when dispatched.

## License

MIT. Build things.
