---
name: clean-notion-study-notes
description: Clean up messy Notion study notes into well-structured, consistent markdown via the Notion MCP. Use when asked to "clean up my notes", "tidy this Notion page", "reformat my study notes", or "organize my notes in Notion".
metadata:
  author: ctgjdw
  version: "1.0.0"
  argument-hint: <notion-page-url-or-title>
---

# Clean Notion Study Notes

Fetch a Notion page's study notes, clean up their structure and formatting, and write the result back to the same page.

## Requirements

This skill requires the **Notion MCP server**. Before doing anything else, check whether it is
connected.

### Checking for Notion MCP

Look for tools named `notion-search`, `notion-fetch`, or `notion-update-page`. If none of these
tools are available, the Notion MCP is not connected. Stop and guide the user through setup
instead of attempting the task.

### Guiding the user to install Notion MCP

If the tools are missing, tell the user to run:

```bash
claude mcp add --transport http notion https://mcp.notion.com/mcp
```

Then have them authenticate by running `/mcp` inside Claude Code and completing the OAuth flow
in the browser. Notion MCP only supports user-based OAuth (no bearer tokens), so this step
cannot be automated - a human must approve it.

Once `/mcp` shows `notion` as connected, re-run this skill.

## Workflow

1. **Locate the page.** If the user gave a URL, use `notion-fetch` directly. If they gave a
   title or description, use `notion-search` first to find the right page, and confirm the
   match with the user before proceeding if more than one plausible result comes back.
2. **Fetch the current content** with `notion-fetch`.
3. **Clean the notes.** Apply these rules:
   - Normalize heading levels into a logical hierarchy (one H1 topic, H2 sections, H3 subsections).
   - Merge fragmented bullet points and fix inconsistent list nesting.
   - Deduplicate repeated content.
   - Fix obvious typos and inconsistent terminology, without changing the meaning of the notes.
   - Convert ad-hoc emphasis (random bold/italic/caps) into a consistent scheme: bold for key
     terms, italics for asides.
   - Preserve all original information. Do not summarize away content, only restructure and
     tidy it.
4. **Show a short before/after summary** of what changed (e.g. "merged 3 duplicate sections,
   fixed heading levels, cleaned up 12 bullet points") and ask the user to confirm before writing
   back, unless they've already indicated they want it applied directly.
5. **Write the cleaned notes back** using `notion-update-page`, replacing the page content.

## Notes

- Never fabricate content that wasn't in the original notes.
- If the page is very large, work section by section rather than attempting one giant rewrite.
- If `notion-update-page` fails, report the exact error to the user rather than retrying blindly.
