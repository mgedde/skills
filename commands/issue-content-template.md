---
description: Use when someone suggests a specific template to add to the content library - a chart, table, grid or layout we should ship as a default template object - covers issue structure, images, and content-template labeling
---

# Template Suggestion → GitHub Issue

Turn a suggestion for a **specific new template** into a GitHub issue in the
content library backlog.

**Local config:** org-specific details (target repo, project board, label
taxonomy, issue types, library naming convention) live in
`~/.claude/issue.local.md` — read it before starting. If the file is missing,
ask the user for the specifics instead of guessing.

**Wrong skill?** This is for requests to *add a piece of content*. A request
about how the content library **behaves** — insert, sync, permissions, update
flows — is `issue-request` with the content-library product label.

## Keep it simple

This is a content request, not an engineering request. Someone is going to
build a template from this issue, so the issue only needs to say **what the
template is and what it should look like**.

Do not include:

- File paths, class or type names, or anything else from the codebase
- Whether the product can already express the shape — the label says it can
- Any "this is not a system change" / "not a capability gap" disclaimer
- "Problem" / "Desired capability" framing, or open questions for a PRD

If while writing you conclude the template genuinely cannot be built with what
exists today, say so to the user in chat. Don't grow the issue to argue it.

## Workflow

### 1. Name the artefact

The single most valuable thing this issue does is call the thing by its
industry name — cohort retention table, waterfall, Marimekko, BCG matrix,
football-field valuation, bridge chart. Whoever builds it can then look up a
hundred references. If the requester didn't name it and you recognise it, name
it; if you don't recognise it, describe the shape plainly and say so.

### 2. Write the issue body

Write the body to a scratch file and use `--body-file` (never inline
multi-paragraph bodies). Five short sections:

- **Background** — the requester's own words as a blockquote, with date and
  channel. Attach their screenshot (see Images).
- **What it is** — the industry name, then two or three plain sentences on what
  the artefact is and what people use it for.
- **Reference layout** — when the source is tabular, reproduce the shape as a
  markdown table with plausible dummy values, so the builder can see it. Skip
  when it isn't tabular.
- **What the template should look like** — a bullet list of the visual
  characteristics worth replicating: header treatment, shading, alignment,
  banding, how empty cells read. This is the part the builder works from, so be
  concrete and visual.
- **Proposed library name** — following the `Category | Variant` convention in
  the local config. Offer one, or two if the category is a genuine choice.
- **Source** — who asked, when, and where the reference came from.

### 3. Create, classify, report

```bash
gh issue create --repo <owner>/<repo> --title "<title>" --body-file <file> \
  --attach <image-path> --type Feature \
  --label content-template --label <content-library-label>
```

- **`content-template` and the content-library product label** — both, always.
- **Object-family label** — add the matching `product-*` label when the shape
  obviously belongs to one (a grid, a chart, a Gantt). Skip when unclear.
- **Issue type** — Feature.
- **Project board** — add to the triage board per the local config.
- **Do NOT add `ai-prd-ready`** — that's the user's manual sign-off.
- Verify labels exist (`gh label list --repo <owner>/<repo> --search <term>`).
- Report the issue URL back.

## Images

The reference screenshot is usually the whole point of the request, so attach
it. Save the image to a temporary local file and attach it:

```bash
gh issue create  --repo <owner>/<repo> --attach <image-path> ...
gh issue comment <n> --repo <owner>/<repo> --attach <image-path>
```

The format is `<file>#<alt text>` if you want alt text. Requires a recent gh
(confirmed on 2.100.0).

- Always target the private repo with `--repo <owner>/<repo>`.
- **Never** upload screenshots to public image hosts, gists, or public repos.
- Delete the temporary image file after the gh command completes.

## Titles

`Content Library template: <industry name> (<what makes it distinct>)` — e.g.
"Content Library template: cohort retention table (triangular heatmap grid)".
The prefix keeps template requests findable as a group.

## Common mistakes

| Mistake | Fix |
|---------|-----|
| Writing it like an engineering issue | Describe the template, not the system |
| Citing files, types or code | Leave all of it out |
| Arguing content gap vs capability gap | The label already says it's content |
| Not naming the artefact | Give it its industry name — that's the point |
| Vague visual description | Concrete bullets: header, shading, alignment, blanks |
| Losing the screenshot | `--attach` it; never a public host; delete the temp file |
| Paraphrasing the requester | Quote verbatim in a blockquote |
| Inline multi-line body | Write to scratch file, use `--body-file` |
| Missing `content-template` | It plus the content-library label, always |
| Adding `ai-prd-ready` | Never — that's the user's manual sign-off |
