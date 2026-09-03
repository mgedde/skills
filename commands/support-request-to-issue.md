---
description: Use when a customer support request, feature request, or chat quote needs to become a GitHub issue - covers duplicate checking, grounding in code, issue structure, and labeling for the AI PRD process
---

# Support Request → GitHub Issue

Turn a customer support request (chat quote, email, ticket) into a well-grounded GitHub issue, or a comment on an existing one.

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

- **Background** — verbatim customer quotes as blockquotes, with date and channel. Never paraphrase away the customer's own words; include clarifying follow-up quotes if the request was misunderstood initially.
- Screenshot placeholder where the user will paste images: `<!-- PASTE SCREENSHOT HERE -->` (gh CLI cannot attach images; the user pastes them in the web UI afterwards).
- **Problem** — what's missing today, grounded in code. Explicitly distinguish from adjacent existing features when confusion is likely (e.g. connected scatter ≠ line chart).
- **Desired capability** — the envisioned behavior/modes, generalized from the customer's ask.
- **Current code context** — file paths, relevant types, existing concepts to build on.
- **Open questions (for the PRD)** — boundary semantics, UI, serialization/migration impact, scope boundaries, related prior art.
- **Source** — where the request came from.

### 4. Create, label, report

```bash
gh issue create --repo <owner>/<repo> --title "<title>" --label ai-prd-ready --body-file <file>
```

- Label `ai-prd-ready` = ready for the AI PRD/specing process. Verify the label exists first (`gh label list --repo <owner>/<repo> --search ai`). When commenting on an existing issue instead, add the label there (`gh issue edit <n> --add-label ai-prd-ready`).
- Report the issue/comment URL back.

## Titles

Feature-first, specific, with the common industry name if one exists: "Connected scatter / snail trail: optional connecting lines between scatter points" — searchable by both the internal framing and the term customers use.

## Follow-up comments

When the user dictates a comment ("add a comment: ..."), post it near-verbatim via `gh issue comment` — fix obvious typos, don't rewrite. Mention the fix when reporting back.

## Common mistakes

| Mistake | Fix |
|---------|-----|
| Creating a duplicate | Search multiple phrasings before creating; comment instead |
| Paraphrasing the customer | Quote verbatim in blockquotes |
| Ungrounded issue | Cite actual files/types; confirm the gap in code |
| Attaching images via CLI | Not possible — leave a paste placeholder |
| Inline multi-line body | Write to scratch file, use `--body-file` |
| Forgetting the label | `ai-prd-ready` on new AND existing issues touched |
