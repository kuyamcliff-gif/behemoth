# Token law: maximum quality per token spent

Behemoth builds serious products without burning the user's budget. These habits apply from Phase 0 onward, on every Claude surface (Claude Code, Claude.ai, Cowork, API).

## Planning depth matches project size

Behemoth's plan.md/context.md/tasks.md is a spec-driven workflow (the same idea behind GitHub's Spec Kit and BMAD-METHOD: agree the spec before generating code, keep the spec as the source of truth). Planning depth should scale with the Phase 0 tier, not run at one fixed intensity for everything:

- **S:** a short plan.md (one page), a light context.md, a flat task list. Do not run a full multi-batch interview on a single-purpose tool; ask the essentials and build.
- **M:** the full Phase 1 interview, a real architecture doc, ordered tasks by dependency.
- **L/XL:** all of the above plus explicit module boundaries in context.md (so a later split into services is surgery, per scale.md Tier 3), and a written rollback/checkpoint note per major feature, not just a commit message.

Over-planning an S project wastes the user's tokens as surely as under-planning an XL one loses control of it; ask which tier this is before deciding how much ceremony the plan gets.

## Split-surface planning (do the talking on flat-rate, the building on metered)

Phases 0 through 2 (research, interview, architecture) are almost entirely conversation: reading, searching, writing prose into plan.md and architecture.md. None of it needs a live coding environment. If the user is on a flat-rate Claude.ai or Claude Cowork plan and a metered Claude Code/API setup for execution, run Phases 0 to 2 entirely on the flat-rate surface, produce the approved plan.md and architecture.md there, then hand those two files to Claude Code to execute Phase 3 onward. This moves the most conversation-heavy, least code-heavy phases off the metered clock entirely, at zero loss of quality since the files themselves carry everything Phase 3 needs. Mention this option to the user in Phase 0 when it plainly applies; do not force it if they are already comfortable running everything in one place.

## The three-file system (the core trick)

The reason Behemoth never re-reads the whole project:

- **plan.md**: the approved spec. Read once per session if needed. Changes only with user approval.
- **context.md**: the living map. Every significant file gets one line: path and purpose. Plus current state, decisions, gotchas. This file is the answer to "where is the thing I need to change".
- **tasks.md**: the checklist with statuses.

The change-request flow: read context.md, identify the owning file(s), open only those, change, test, update context.md, commit. A one-file change costs one file of context, not a codebase scan. Keep context.md tight; it must stay cheap to read (aim under 200 lines; compress old history into brief decisions).

## Cache-friendly behavior

Prompt caching bills repeated stable context at about 10% of fresh input. Claude Code caches automatically; the habit that preserves the discount is stability:
- Keep durable guidance in files (CLAUDE.md, plan.md), not retyped into chat.
- Do not thrash models or settings mid-session without reason (each switch cold-starts the cache).
- Extend work in the same session for related tasks; start fresh sessions for unrelated tasks.

## Context hygiene

- **.claudeignore from Phase 3** keeps node_modules, dist, lockfiles, build output, and media out of context forever.
- **Subagents for bulk reading** (when available): "read these 12 files and summarize the auth flow" runs in a separate context and returns only the summary. Use for research, audits, and any task producing large intermediate output. Rule of thumb: more than 3 large files to read = subagent.
- **Compact early** (Claude Code): around 60% context usage, run /compact with preservation instructions: "preserve the file map, all user decisions, the current task list, and the tech stack". Never wait for auto-compaction at 95%; quality has already degraded by then.
- **Do not paste what can be referenced.** Point to file paths instead of pasting file contents into chat.
- **Failed approaches**: rewind or summarize the lesson and clear the debris rather than dragging a dead attempt through the rest of the session.

## Output discipline

Two modes, chosen by the user in Phase 0:
- **Go all out**: thorough code, meaningful comments where logic is non-obvious, complete tests, rich docs.
- **Token-tight**: lean code, comments only where truly needed, essential tests on critical paths, concise docs. Same quality bar for correctness and security; less prose around it.

In both modes: never regenerate an entire file to change five lines when a targeted edit works; never restate code back to the user that they can open themselves; keep explanations proportional to the question.

## Estimation method (Phase 0)

1. Classify the project: S (landing page or single-purpose tool), M (multi-page site with auth and a database), L (marketplace/SaaS with payments, tiers, admin), XL (L plus mobile or heavy realtime or AI features).
2. Rough total token ranges (input plus output, whole build, order-of-magnitude): S: 100K to 400K. M: 400K to 1.5M. L: 1.5M to 5M. XL: 5M+. Caching typically cuts the effective input cost by half or more in long sessions.
3. Search current per-token pricing for the models available to the user before quoting money. Never quote pricing from memory; it changes.
4. Present as a range with assumptions, note that back-and-forth and scope changes move it, and update the estimate when scope changes.
5. Time estimate: state it in working sessions ("roughly 3 to 6 focused sessions for this scope"), not fake precision.

## Session structure for big builds

One feature per stretch, commit per feature, update the three files, then continue or hand off cleanly. If a session must end mid-feature, write the resume note into context.md ("in progress: checkout, done: cart math, next: MoMo webhook"). The next session reads context.md and resumes without any re-explaining.
