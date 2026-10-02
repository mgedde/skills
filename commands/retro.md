---
description: Use when the user asks for a retrospective, a retro, or "what should we change after this session" - reviews a coding session and proposes improvements to the agent's environment
disable-model-invocation: true
---

# Retro

The user has asked for a **retrospective**. You are suggesting improvements to the coding agent's **environment** so future runs go better. You are not reviewing the code that was written; you are reviewing what made the agent slow, wrong, or expensive.

## Steps

1. Read the primary sources for the session the user specifies. This may mean searching through session logs on this machine (`~/.claude/projects/<project>/`). If the user doesn't specify a session, default to the current one.

2. Look for candidates for improvement in these categories.

- **Navigation**: how easy was it for the agent to find the right files? Are there hidden dependencies between files? Would a **navigation pointer** make it easier? _Use when_ the session took a long time to find a piece of information.
- **Automated checks**: are there automated checks that could catch errors the agent made? Linting, typing, tests, filesystem linters? Read the repo's own check command first (its `package.json` or build-tool `lint`/`check` scripts, its CI workflow), so a check that already exists but sits unwired or silently broken is the finding, not a reinvention. A repo with no **guardrail** (no pre-commit hook and no CI job running its lint/typecheck/test command) is itself a finding: an un-linted repo is a standing missed opportunity, not a neutral default. _Use when_ the agent made a mistake an automated check could have caught, or the repo has no guardrail at all.
- **Coding standards**: should the **reviewer** (`mg:code-review`) be given a new rule to enforce? Should an existing rule be removed or clarified? Classify the violation first: a **mechanical** one (a fixed syntactic pattern, a banned API, an import shape, a file-location rule) gets a deterministic check, full stop: a custom rule in the repo's own linter, a new pre-commit hook, or a new CI job, whichever the repo's language and existing guardrail make cheapest. Default to building the check over writing the rule. Reserve `CODING_STANDARDS.md` for genuine **judgement calls** (cross-file consistency, "matches the surrounding style", anything no guardrail could ever substitute for). _Use when_ the reviewer failed to catch a mistake.
- **Steering files**: are there instructions in `CLAUDE.md` or `AGENTS.md` that should move to coding standards (or automated checks) instead? _Use when_ the steering file is particularly large, in the repo OR in the user's global `~/.claude/CLAUDE.md`.
- **Tool economy**: did the agent make expensive tool calls that could be streamlined? Is there any custom tooling (CLIs, MCP servers) that is particularly token-inefficient? _Use when_ the agent made an expensive tool call.
- **No-ops**: look for instructions in steering files that don't modify the agent's behaviour. _Use when_ the steering files are large and unwieldy.
- **Information access**: look for opportunities to increase the agent's access to information. Teeing dev server logs, read-only access to third-party services. _Use when_ a crucial piece of information was not available to the agent.

3. Present these candidates to the user, in order of severity. For each: what happened, which category, and the concrete change (file, rule, check). Don't apply anything until the user picks.

## Reference

### Implementation vs review

All work goes through two stages: implementation and review. The implementation agent has the most **context pressure**: it explores, writes code, and debugs failures.

The review agent has the least context pressure. It receives a diff, so no exploration is needed, and it rarely writes code.

So the **review agent** should be responsible for imposing coding standards, not the implementation agent. Rules belong in `CODING_STANDARDS.md` (read at review time), not in `CLAUDE.md` (read on every turn).

### Files

- `CLAUDE.md` / `AGENTS.md`: pushed into the context window of every agent working in the repo. Use incredibly sparingly, usually only for **navigation pointers** to other files.
- `CODING_STANDARDS.md`: read during review (`mg:code-review`), not implementation. Add **navigation pointers** to docs folders if the standards file grows past roughly 1,000 lines.
- Docs: reference files, pointed to by other files. Look for existing docs before writing new ones.
- Skills: use skills for docs the agent should reach for on its own (the description goes into the agent's context), or for user-invoked commands.

### Writing for agents

When you propose text for a steering file, standards file, or skill:

- A **pointer** (a `CLAUDE.md` line, a skill description) does two jobs: say what the material is, and name the condition for reaching it. Front-load the trigger word. Every always-loaded word costs on every turn.
- Inline what every path needs; push behind a pointer what only some paths reach.
- Every step ends on a checkable **completion criterion**. "Every modified model accounted for" beats "produce a change list".
- Prefer a deterministic check to a sentence of prose whenever the rule is mechanical.
