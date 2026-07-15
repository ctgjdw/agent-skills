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

Cleans up messy Notion study notes into well-structured, consistent markdown.
Requires the [Notion MCP server](https://developers.notion.com/guides/mcp/get-started-with-mcp)
to be connected; the skill will guide you through installing it if it's missing.

**Use when:**

- "Clean up my notes"
- "Tidy this Notion page"
- "Reformat my study notes"
- "Organize my notes in Notion"
