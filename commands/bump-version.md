---
description: Use after committing changes to this plugin repo, before pushing - bumps the plugin version in marketplace.json
---

# Bump Plugin Version

After committing changes to skills or scripts in this repo, bump the version in `.claude-plugin/marketplace.json` before pushing.

## Rules

- **Patch** (e.g. 1.3.0 → 1.3.1): Default for most changes — typo fixes, wording tweaks, small adjustments to existing skills or scripts.
- **Minor** (e.g. 1.3.0 → 1.4.0): New skills, new scripts, significant reworks of existing skills, or breaking changes to hook scripts.

Both `metadata.version` and the plugin entry `version` in `marketplace.json` must match.

## Steps

1. Read `.claude-plugin/marketplace.json` to get the current version.
2. Determine patch or minor based on the commits since last push (use `git log origin/main..HEAD` if needed).
3. Update both version fields in `marketplace.json`.
4. Commit with message: `Bump plugin version to X.Y.Z`
5. Proceed with push.
