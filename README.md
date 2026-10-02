# mg — Personal Skills Plugin

A collection of Claude Code skills for planning, development workflow, issue writing, and tooling.

Originally forked from [mattpocock/skills](https://github.com/mattpocock/skills) — **grill-me**, **git-guardrails**, **retro**, **code-review**, **pr**, **handoff**, **diagnosing-bugs**, **improve-codebase-architecture** and **codebase-design** are based on his work, adapted to this repo's conventions.

## Installation

This plugin is installed via a custom marketplace. Add the following to your Claude Code settings (`~/.claude/settings.json`):

```json
{
  "plugins": {
    "marketplaces": [
      "https://raw.githubusercontent.com/mgedde/skills/main/.claude-plugin/marketplace.json"
    ]
  }
}
```

Then install the plugin:

```
/plugin marketplace update
/plugin install mg
```

## Skills

### grill-me

Interview the user relentlessly about a plan or design until reaching shared understanding.

### git-guardrails-claude-code

Set up Claude Code hooks to block dangerous git commands (force-push, reset --hard, clean, branch -D, etc.) before they execute.

### git-feature-branching

Merge-based git feature branching workflow. Covers branch naming, developing on a feature branch, and merging back to trunk via PR or direct merge.

### issue-request

Turns a customer support request, feature request, or chat quote into a well-grounded GitHub issue — duplicate check first, grounded in the codebase with real file and type names, customer words quoted verbatim, screenshots attached with `gh --attach`, then labels, issue type, and project board.

### issue-content-template

Turns a suggestion for a *specific* content-library template — a chart, table, grid, or layout to ship as a default object — into a short content issue: names the artefact by its industry name, opens with the reference screenshot, reproduces the reference layout, and describes the visual characteristics to replicate. No code grounding; deliberately not an engineering issue.

Both issue skills ask clarifying questions in plain chat, one at a time, and read org-specific details — target repo, label taxonomy, issue types, project board, naming conventions — from `~/.claude/issue.local.md`, which lives outside this repo.

### bump-version

Bumps the plugin version in `marketplace.json` after committing changes to this repo. Used internally before pushing.

### code-review

Reviews the diff since a fixed point (defaults to trunk when on a feature branch) along two independent axes, each in its own sub-agent: **Standards** (the repo's `CODING_STANDARDS.md`, `CONTRIBUTING.md`, or `CLAUDE.md` rules, plus a fixed Fowler smell baseline) and **Spec** (the originating GitHub issue or spec file). Findings are reported side by side, never merged, so one axis can't mask the other.

### retro

Retrospective on a coding session. Reads the session and proposes improvements to the agent's *environment*, most severe first: navigation pointers, missing or unwired automated checks, new `CODING_STANDARDS.md` rules (or linter checks when the rule is mechanical), bloated steering files, token-expensive tooling, missing information access. User-invoked only.

### diagnosing-bugs

Disciplined diagnosis loop for hard bugs and performance regressions: build a tight feedback loop that goes red on *this* bug, reproduce and minimise, rank falsifiable hypotheses, instrument one variable at a time, write the regression test before the fix, clean up. Ships `scripts/hitl-loop.template.sh` for the human-in-the-loop fallback.

### pr

The shape of a PR body: a summary as the smallest visual that makes the change clear (pseudocode, call tree, file tree, diff sketch, Mermaid), before/after evidence that it works, and a merge-danger call (one-way or two-way door, plus blast radius). Based on Dex Horthy's `show-me`.

### handoff

Compacts the current conversation into a handoff document in the OS temp directory so a fresh session can pick the work up, including which skills the next agent should invoke. Takes an optional argument describing what the next session will focus on. User-invoked only.

### improve-codebase-architecture

Scans a codebase (weighted towards recent git hot spots) for shallow modules that could be deepened, renders the candidates as a self-contained HTML report in the temp directory with before/after diagrams and a recommendation strength, then grills through whichever one you pick using `grill-me`. User-invoked only. Lives under `skills/` because it carries `HTML-REPORT.md` alongside.

### codebase-design

The shared deep-module vocabulary (module, interface, depth, seam, adapter, leverage, locality) and principles (deletion test, interface is the test surface, two adapters make a seam real). Model-invocable, so it also fires on its own when designing or restructuring an interface. Ships `DEEPENING.md` (dependency categories and replace-don't-layer testing) and `DESIGN-IT-TWICE.md` (parallel sub-agents designing one interface several ways).

## Layout

Single-file skills live in `commands/`. Skills that carry reference files live in `skills/<name>/SKILL.md`. Claude Code picks up both; every skill is addressed as `/mg:<name>`.

## Coding standards convention

`code-review` and `retro` share one convention: a per-repo `CODING_STANDARDS.md` read at *review* time, never on every turn. It holds judgement calls only (naming, cross-file consistency, "matches the surrounding style"). Anything mechanical goes in a linter, pre-commit hook, or CI job instead. `retro` proposes entries; `code-review` enforces them.
