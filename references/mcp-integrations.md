# MCP integrations: real project data instead of Claude's own guess

When the user's environment has MCP servers connected, use them to ground planning in the user's actual project data instead of researching or assuming from scratch. Check what is connected before falling back to web research or asking the user.

## Check before you guess

At Phase 0 and Phase 2, and again whenever a review starts, check for connected tools (a connectors/MCP tool list, or simply try the relevant server) before assuming none exist. If a relevant one is not connected, tell the user plainly what connecting it would get them, once, and move on with the best available information if they decline; do not block the build on it.

## Known integrations and what they replace

- **GitHub:** pulls real repository state, issues, and PR history instead of the analyst or project-auditor persona guessing project structure from a README alone. Already the default for any repo-backed project in this skill.
- **Jira / Atlassian:** pulls the real backlog and existing tickets into Phase 1 planning, so tasks.md reflects what the team already tracks instead of a list invented from the interview alone. Useful when the user already runs their team through Jira; do not suggest introducing it to a solo user who has never used it.
- **Figma:** pulls actual design files and design tokens (real colors, type scale, spacing) into Phase 2/Batch G instead of the ux persona proposing a palette from research alone. If the user has an existing Figma file, this is strictly better than guessing their brand from scratch.
- **SonarQube (or an equivalent code-quality/security scanner):** pulls real, already-measured code quality and security metrics for an existing project into the project-auditor's review instead of a fresh manual read of the whole codebase. Prefer this over reinventing static analysis by hand when it is already connected.

## The rule

An MCP integration replaces a guess with a fact; it never replaces the user's actual answer to a Phase 1 question. If Figma says the brand color is blue but the user says they want to rebrand, the user's answer wins. Cite which integration a fact came from when presenting it, the same way a web source gets cited.
