# Subagent workflow: how Phase 4 actually runs

Phase 4 in stages.md describes the feature loop at the level the user sees. This file is the mechanical detail for how that loop uses the personas in `agents/` so each step actually runs in its own isolated context instead of one thread narrating every role.

## Git worktree per feature

Before dev starts a feature, create (or reuse) an isolated git worktree on its own branch, using the environment's worktree tools if available, or plain `git worktree add`. Work happens there, not directly on the trunk the user is watching. This means a feature that goes sideways never leaves the main tree in a broken state, and "go back to before the cart" is a worktree discard, not an archaeology project. Merge to the trunk only when the feature's two-stage review (below) both pass.

## The hard refusal gate

Do not write product code before the user has explicitly approved the Phase 1 playback and, when architect ran, the Phase 2 architecture.md. This is not a soft preference: if asked to skip ahead ("just start building, skip the questions"), say plainly that skipping means guessing at requirements that are expensive to unwind later, offer the fastest legitimate path (a shorter S-tier interview per token-economy.md, not zero interview), and proceed only once the user has actually chosen to accept that tradeoff. The gate exists to prevent silent scope-guessing, not to be bureaucratic; keep the explanation to one sentence and move on the moment the user decides.

## Per-task, bite-sized checkpoints

scrum-master breaks each feature into tasks completable in roughly 2 to 5 minutes of focused work, each with an exact file scope and a concrete "done" test. Tasks flagged sensitive (money, permissions, auth, anything a compliance pack from references/compliance-packs.md touches) get a quick human checkpoint before dev starts them; routine tasks (styling a component, adding a form field) run straight through to the two-stage review without stopping the user for each one. This keeps the checkpoint discipline proportional to actual risk instead of interrupting the user every few minutes on low-stakes work, which SKILL.md rule 6 already warns against.

## TDD as a hard gate, not a suggestion

dev writes the test before the implementation, runs it, and confirms it fails for the stated reason (red) before writing a single line of the real implementation. Only after a genuine red result does implementation begin; run again and confirm green before the task is marked done. A task is not complete if the test was written after the code, or if it was never actually run.

## Two-stage review, two separate passes

Once dev reports a task done, qa runs twice, not once:

1. **Spec compliance pass:** does this actually do what the task said, does the test test the right thing, are error/empty states handled. qa re-runs the suite itself rather than trusting dev's report.
2. **Code quality pass:** reuse, simplification, obvious inefficiency, and for sensitive tasks the relevant references/security.md items, checked here rather than deferred entirely to the Phase 5 audit.

A fail on either pass sends the task back to dev with the specific findings; qa does not silently fix it in this role, the loop stays two-sided.

## The debugger loop is separate from feature work

When a test fails unexpectedly or a runtime error appears mid-task, dev stops and hands off to the debugger persona rather than guessing repeatedly at the same bug inside the feature-implementation context. Debugger reproduces first, finds the actual root cause, applies the smallest correct fix, and hands control back to dev (or directly to qa if the fix is complete) once the original failure and the full local suite both pass.

## Merging out of the worktree

Only after both qa passes succeed: merge the feature branch, update context.md and tasks.md, commit on the trunk with the human-readable message stages.md Phase 4 already specifies, and remove the worktree. The trunk only ever receives reviewed, green, tested work.
