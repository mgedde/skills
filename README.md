# mg — Personal Skills Plugin

A collection of Claude Code skills for planning, development workflow, and tooling.

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

### bump-version

Bumps the plugin version in `marketplace.json` after committing changes to this repo. Used internally before pushing.
