# mg — Personal Skills Plugin

A collection of Claude Code skills for planning, development workflow, issue writing, and tooling.

Originally forked from [mattpocock/skills](https://github.com/mattpocock/skills) — the **grill-me** and **git-guardrails** skills are based on his work.

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
