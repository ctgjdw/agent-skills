---
name: clean-notion-study-notes
description: >-
  Clean up and reformat a Notion page that holds rough notes pasted from a
  webpage, e-lecture, course, or article — turning a messy copy-paste dump into
  tidy, well-structured study notes IN PLACE. Use this whenever the user asks to
  "clean up", "tidy", "reformat", "fix up", or "organize" a Notion page/notes by
  name (e.g. "clean up my Notion page named 'Helm'"), especially for revision or
  study notes. The page name is the argument. ALWAYS preserve existing images,
  diagrams, and tables. Trigger even if the user doesn't say the word "skill".
---

# Clean Notion Notes

Transform a Notion page of rough, pasted-in notes into clean, well-structured
notes that read well on revision — editing the page **in place** via the Notion
MCP server. The page to clean is identified by name, passed as the argument
(referred to below as `<PAGE_NAME>`).

## Why this exists

Notes copy-pasted from a webpage or e-lecture arrive with broken structure:
emphasis sentences pasted as `#` headings, inconsistent heading levels, mangled
list nesting, and code/YAML/shell crammed into a single line of inline code.
The content is good; the formatting fights the reader. The goal is to fix the
*presentation* so the user can revise efficiently — **never** to rewrite,
summarize away, or drop the substance.

## Hard constraints

- **Edit in place.** Update the existing page; do not create a new page or copy.
- **Preserve all images, diagrams, and tables.** Reproduce their markdown
  exactly (image URLs, table structure, column widths). Never delete them. If
  you reorganize, keep each image/table near the content it illustrates.
- **Preserve meaning.** Keep all the information. You may tighten wording and
  remove pure duplication, but don't cut concepts or invent new ones.
- **Preserve child pages/databases.** Never drop a `<page>` or `<database>`
  block — removing it deletes the child. Use `<mention-page>` if you only need a
  reference.

## Workflow

### 1. Read the Notion-flavored Markdown spec

Before editing, read the MCP resource `notion://docs/enhanced-markdown-spec`
(via the resource-reading interface — do NOT fetch it as a URL). This is the
source of truth for callouts, code blocks, tables, and image syntax. Don't guess
syntax.

### 2. Find the page

Search the workspace for `<PAGE_NAME>` with `notion-search` (query_type
`internal`). Pick the page whose title matches. If several plausibly match, or
none do, ask the user to confirm rather than guessing.

### 3. Fetch the full content

`notion-fetch` the page by id. Read the whole `<content>` block carefully and
identify the formatting problems (see Cleanup checklist). Note every image,
table, and code block so you can preserve them.

### 4. Rewrite the content

Build the cleaned markdown, then apply it with `notion-update-page` using the
`replace_content` command (`new_str` = full cleaned page). `replace_content`
refuses if it would delete a child page/database, which is a useful safety net —
if it errors, keep the `<page>`/`<database>` blocks intact and retry.

### 5. Verify and fix auto-conversions

`notion-fetch` again. Notion silently rewrites some things — check for and fix:
- **Auto-linkified filenames** like `README.md` → `http://README.md`. Wrap such
  filenames in inline code (`` `README.md` ``) so Notion leaves them alone.
- Code blocks given the wrong language label (cosmetic; fix if it bothers you).
- Lost emphasis or broken callouts.

Apply small fixes with the `update_content` command (search-and-replace via
`content_updates`), which is cheaper and safer than re-replacing everything.

## Cleanup checklist

Apply these transformations; they cover the usual copy-paste damage:

- **Demote fake headings.** Bold sentences pasted as `#`/`##` headings (often a
  concluding takeaway) are not section titles. Convert them to a normal
  paragraph, **bold** text, or — for tips/warnings/definitions — a callout.
- **Normalize the heading hierarchy.** Use `##` for main sections and `###` for
  subsections, consistently and in order. The page title is already the H1.
- **Fix list nesting.** Flatten the bogus nested-empty-bullet structure that
  pasting produces; make sibling items real siblings. Use numbered lists for
  sequences/steps, bullets otherwise. Every list item must contain inline text
  (no empty items).
- **Promote code to fenced blocks.** Move directory trees, YAML, and shell
  commands out of inline ``` `code…<br>…` ``` into proper fenced blocks with a
  language tag (```` ```yaml ````, ```` ```bash ````, etc.). Keep code content
  literal — don't escape characters inside fences.
- **Use callouts for labeled asides.** "Key Concept", "Best Practice",
  "Note", "Important", warnings → `<callout>` with a fitting icon and a soft
  background color (e.g. `gray_bg`, `green_bg`, `yellow_bg`, `blue_bg`). This
  makes them scannable on revision.
- **Tidy inline formatting.** Convert pasted artifacts: stray escaped delimiters,
  zero-width characters inside URLs/words, `*term*` used for code → `` `term` ``.
- **Add a table of contents** (`<table_of_contents/>`) near the top for long
  pages, and a short summary callout if the page lacks an intro.
- **Add a Source link** at the bottom if the page properties contain a source
  URL (e.g. a `userDefined:URL` property), so the original is one click away.

## Style

Aim for notes a student would actually want to revise from: clear section
headers, short scannable paragraphs, lists for enumerations, callouts for the
things worth remembering, and clean code blocks. Tighten verbose prose, but keep
the page's voice and all its facts. When in doubt about whether a change alters
meaning, preserve the original.
