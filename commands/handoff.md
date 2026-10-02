---
description: Use when the user wants to hand the current conversation to a fresh session or another agent, says "handoff", "hand off", or "write this up for the next session"
argument-hint: "What will the next session be used for?"
disable-model-invocation: true
---

# Handoff

Write a handoff document summarising the current conversation so a fresh agent can continue the work.

**Where to save:** the temporary directory of the user's OS (`$TMPDIR` or `/tmp` on Unix, `$env:TEMP` on Windows), not the current workspace. Name it `handoff-<repo>-<yyyy-mm-dd>.md`. Print the full path at the end so the user can paste it into the next session.

**What to include:**

- The goal, where the work stands, and what's next, in that order.
- Decisions made and why, especially ones that were argued over.
- Open questions and known blockers.
- A **suggested skills** section naming which skills the next agent should invoke with the Skill tool (use full names such as `mg:code-review` or `superpowers:test-driven-development`).

**What to leave out:**

- Content already captured in other artifacts (specs, plans, ADRs, issues, commits, diffs). Reference them by path or URL instead.
- Anything sensitive: redact API keys, passwords, tokens, and personally identifiable information.

If the user passed arguments, treat them as a description of what the next session will focus on and tailor the document accordingly.
