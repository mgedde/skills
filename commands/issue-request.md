---
description: Use when a customer support request, feature request, or chat quote needs to become a GitHub issue - covers duplicate checking, grounding in code, issue structure, and labeling for the AI PRD process
---

# Support Request → GitHub Issue

Turn a customer support request (chat quote, email, ticket) into a well-grounded GitHub issue, or a comment on an existing one.

**Local config:** org-specific details (target repo, project board, label taxonomy, issue types) live in `~/.claude/issue.local.md` — read it before starting. If the file is missing, ask the user for the specifics instead of guessing.

**Related skill:** a request for a *specific template* to add to the content library is `issue-content-template`, not this one.

## Clarifying questions

Default to writing the issue. When something load-bearing is genuinely unclear — which behaviour the customer actually means, how far the scope reaches, whether this is one issue or several — ask before writing, **in plain chat, never with the question tool, and one question at a time**: ask it, wait for the answer, then decide whether the next one is still needed. Don't stack questions into a list or a multiple-choice prompt.

Don't ask what the code, the existing issues, or the local config can answer, and don't ask permission to create the issue — create it and report the URL. Real unknowns that don't block writing belong in the issue's **Open questions (for the PRD)** section instead of in chat.

## Workflow

### 1. Check for existing issues FIRST

Search several phrasings — synonyms and spellings matter (colored/coloured, quadrant/section/area):

```bash
gh search issues --repo <owner>/<repo> "<terms>" --limit 20 --json number,title,state,url
```

- **Match found (open):** comment on it instead of creating a new issue. Read it first (`gh issue view <n> --json title,body,labels,comments`).
- **Feature spans multiple existing issues:** comment on each, cross-link them, and suggest they be specced together.
- **Closed issues** can still be worth cross-referencing as prior art.

### 2. Ground the request in the codebase

Find the relevant types/files and confirm the gap is real. Use targeted searches (Glob on type names beats repo-wide grep in big repos). The issue should cite concrete file paths and member names — this gives the later PRD/spec work a starting point.

### 3. Write the issue body

Write the body to a scratch file and use `--body-file` (never inline multi-paragraph bodies). Structure:

- **Screenshot** (when the request comes with one) — preferably the body's first line, above Background, so the reader sees what the customer saw before reading about it. Attached via `--attach`; see [Images](#images) for how to pin it at the top rather than letting gh dump it at the bottom.
- **Background** — verbatim customer quotes as blockquotes, with date and channel. Never paraphrase away the customer's own words; include clarifying follow-up quotes if the request was misunderstood initially.
- **Problem** — what's missing today, grounded in code. Explicitly distinguish from adjacent existing features when confusion is likely (e.g. connected scatter ≠ line chart).
- **Desired capability** — the envisioned behavior/modes, generalized from the customer's ask.
- **Current code context** — file paths, relevant types, existing concepts to build on.
- **Open questions (for the PRD)** — boundary semantics, UI, serialization/migration impact, scope boundaries, related prior art.
- **Source** — where the request came from.

### 4. Create, classify, report

```bash
gh issue create --repo <owner>/<repo> --title "<title>" --body-file <file>
```

Add `--attach <path>` for any screenshot, and `--type <name>` when the type is
clear (see [Images](#images) and the local config).

Then classify and file it (specifics in the local config):

- **Product-area labels** — apply the matching area label(s) from the local config's taxonomy when the issue clearly relates to a specific part of the product; skip when cross-cutting or ambiguous. Verify labels exist (`gh label list --repo <owner>/<repo> --search <term>`).
- **Issue type** — set the type (e.g. Bug/Feature) when it clearly fits, using the command in the local config.
- **Project board** — add the issue to the triage board per the local config.
- **Do NOT add `ai-prd-ready`** — the user adds it manually after reviewing the issue. Same for comments on existing issues: never add or change that label.
- Report the issue/comment URL back.

## Images

Screenshots are attached by the CLI, not pasted afterwards, and they
preferably sit towards the **top** of the issue, above Background — a reader
should see what the customer saw before reading the quotes about it.

`gh` appends an attachment to the end of the body *unless* the body already
references that file by its local path, in which case the reference is
rewritten in place. So reference it yourself, as the body's first line:

```markdown
![Customer's chart with the missing connecting lines](./screenshot.png)
```

Save the image next to the body scratch file and pass the identical path to
`--attach` — if the two strings don't match, gh uploads the image and dumps it
at the bottom of the issue instead:

```bash
gh issue create  --repo <owner>/<repo> --body-file <file> --attach ./screenshot.png ...
gh issue comment <n> --repo <owner>/<repo> --attach ./screenshot.png
```

Alt text written in the body wins; the `<file>#<alt text>` form on `--attach`
only applies when the body doesn't reference the file. Requires a recent gh
(confirmed on 2.100.0).

- Always target the private repo with `--repo <owner>/<repo>`.
- **Never** upload screenshots to public image hosts, gists, or public repos.
- Delete the temporary image file after the gh command completes.

## Titles

Feature-first, specific, with the common industry name if one exists: "Connected scatter / snail trail: optional connecting lines between scatter points" — searchable by both the internal framing and the term customers use.

## Follow-up comments

When the user dictates a comment ("add a comment: ..."), post it near-verbatim via `gh issue comment` — fix obvious typos, don't rewrite. Mention the fix when reporting back.

## Common mistakes

| Mistake | Fix |
|---------|-----|
| Creating a duplicate | Search multiple phrasings before creating; comment instead |
| Paraphrasing the customer | Quote verbatim in blockquotes |
| Batching clarifying questions | One at a time in plain chat — never the question tool |
| Asking permission to create the issue | Don't — create it and report the URL |
| Ungrounded issue | Cite actual files/types; confirm the gap in code |
| Leaving a paste placeholder for images | `--attach <path>` works now; never a public host; delete the temp file |
| Screenshot stuck at the bottom | Reference the same path in the body's first line, above Background |
| Inline multi-line body | Write to scratch file, use `--body-file` |
| Adding `ai-prd-ready` | Never — that's the user's manual sign-off |
| Skipping classification | Product-area label, issue type, and project board per local config |
| Hardcoding org specifics here | They live in `~/.claude/issue.local.md`, not this public skill |
