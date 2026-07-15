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

### planning-trips

Structured, collaborative process for turning a vague trip idea ("plan a trip
from Singapore to Osaka for 10D9N") into a complete, durable set of
trip-planning documents (`CLAUDE.md` + `00_overview.md` through
`05_bucketlist.md`). Asks one upfront questionnaire, then iterates with
research, enforcing strict source-verification and uncertainty-labeling
rules for every operational fact (hours, prices, dates, addresses).

**Use when:**

- "Plan a trip from X to Y for ND"
- "Help me plan a 2-week Italy vacation"
- "Let's plan a Hokkaido honeymoon"

**Not for:** single-recommendation questions ("best ramen in Kyoto") or
on-the-go edits to an already-planned trip.
