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

### 1. Ask first — but only what changes the issue

Usually a screenshot and a quote are enough and you should just write. When
something is genuinely load-bearing and you can't settle it yourself, ask —
**in plain chat, never with the question tool, and one question at a time**.
Ask it, wait for the answer, then decide whether the next one is still needed;
the first answer usually settles the rest. Worth asking:

- **Which artefact** — the screenshot shows several things, or you can't tell
  which part of it is the request
- **One or many** — is this one template, or a family of variants
- **The category slot** — the `Category | Variant` choice is a real fork, not
  something you can pick sensibly
- **A missing reference** — the quote points at an image or link you don't have

Don't ask about: anything you can name or read off the screenshot yourself,
anything the local config already answers, or whether to go ahead and create
the issue — create it. And don't stack questions into a list or a
multiple-choice prompt; one plain question, then the next if it survives.

### 2. Name the artefact

The single most valuable thing this issue does is call the thing by its
industry name — cohort retention table, waterfall, Marimekko, BCG matrix,
football-field valuation, bridge chart. Whoever builds it can then look up a
hundred references. If the requester didn't name it and you recognise it, name
it; if you don't recognise it, describe the shape plainly and say so.

### 3. Write the issue body

Write the body to a scratch file and use `--body-file` (never inline
multi-paragraph bodies).

**Open with the screenshot.** Whoever builds this should see the artefact
before reading a word about it, so the body's first line is the image
reference — see Images for how to pin it there. Then five short sections:

- **Background** — the requester's own words as a blockquote, with date and
  channel.
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

### 4. Create, classify, report

```bash
gh issue create --repo <owner>/<repo> --title "<title>" --body-file <file> \
  --attach ./reference.png --type Feature \
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
it — and show it at the **top** of the issue, above Background.

`gh` appends an attachment to the end of the body *unless* the body already
references that file by its local path, in which case the reference is
rewritten in place. So reference it yourself, as the body's first line:

```markdown
![Cohort retention table shared by the requester](./reference.png)
```

Save the image next to the body scratch file and pass the identical path to
`--attach` — if the two strings don't match, gh uploads the image and dumps it
at the bottom of the issue instead:

```bash
gh issue create --repo <owner>/<repo> --body-file <file> --attach ./reference.png ...
gh issue comment <n> --repo <owner>/<repo> --attach ./reference.png
```

Alt text written in the body wins; the `<file>#<alt text>` form on `--attach`
only applies when the body doesn't reference the file. Requires a recent gh
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
| Screenshot stuck at the bottom | Reference the same path in the body's first line |
| Paraphrasing the requester | Quote verbatim in a blockquote |
| Batching questions into a list | One at a time — the first answer often kills the rest |
| Using the question / multiple-choice tool | Ask in plain chat |
| Guessing at an ambiguous screenshot | Ask which artefact is meant |
| Asking permission to create the issue | Don't — create it and report the URL |
| Inline multi-line body | Write to scratch file, use `--body-file` |
| Missing `content-template` | It plus the content-library label, always |
| Adding `ai-prd-ready` | Never — that's the user's manual sign-off |
