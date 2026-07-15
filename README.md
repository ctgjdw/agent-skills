# agent-skills

A collection of skills for AI coding agents. Skills are packaged instructions
that extend agent capabilities. Follows the [Agent Skills](https://agentskills.io/)
format and installs via [`npx skills`](https://github.com/vercel-labs/skills).

## Install

```bash
npx skills add ctgjdw/agent-skills
```

Install a specific skill:

```bash
npx skills add ctgjdw/agent-skills --skill clean-notion-study-notes
```

## Available Skills

### clean-notion-study-notes

Cleans up and reformats a Notion page of rough, pasted-in notes (from a
webpage, e-lecture, course, or article) into tidy, well-structured study
notes, edited in place via the [Notion MCP server](https://developers.notion.com/guides/mcp/get-started-with-mcp).
Preserves all images, diagrams, tables, and child pages/databases.

**Use when:**

- "Clean up my Notion page named 'X'"
- "Tidy up my notes"
- "Reformat/fix up my study notes"
- "Organize my notes in Notion"
