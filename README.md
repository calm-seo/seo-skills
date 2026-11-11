<p align="center">
  <img src="assets/logo.png" width="72" height="72" alt="CalmSEO" />
</p>

<h1 align="center">SEO Skills for Claude Code, Cursor, and Codex</h1>

<p align="center">
  <strong>Agent skills that do SEO with your real Google Search Console data, not guesses.</strong><br />
  Find your next SEO fix, rewrite low-CTR titles, refresh decaying pages, fix cannibalization and indexing, and plan content from live SERPs and keyword data.
</p>

<p align="center">
  <a href="https://agentskills.io"><img src="https://img.shields.io/badge/format-SKILL.md-2563eb?style=flat-square" alt="Agent Skills format" /></a>
  <a href="https://calmseo.com"><img src="https://img.shields.io/badge/MCP-calmseo.com-10b981?style=flat-square" alt="CalmSEO MCP server" /></a>
  <a href="LICENSE"><img src="https://img.shields.io/badge/license-MIT-71717a?style=flat-square" alt="MIT License" /></a>
</p>

---

Most SEO prompts ask an AI to guess. These skills make your agent look first: which pages get impressions, which queries slipped, what Google ranks today, and how many people search for it. Then they end in a concrete change, usually an edit to your codebase that you approve.

Each skill is a plain Markdown `SKILL.md` that runs on the [CalmSEO MCP server](https://calmseo.com). Search Console tools are free. Live SERP, keyword, and audit tools use CalmSEO credits, and every skill tells you the cost and waits for your OK before spending any.

## Skills

| Skill | What it does | Cost |
| --- | --- | --- |
| [`find-next-seo-fix`](skills/find-next-seo-fix/SKILL.md) | Picks the one page and query most likely to bring more clicks, then gives the exact title, meta description, H1, and content changes | About 16 to 21 credits |
| [`search-console-report`](skills/search-console-report/SKILL.md) | Plain-English report of what changed in Google, why, and the next three actions | Free |
| [`ctr-title-rewrite`](skills/ctr-title-rewrite/SKILL.md) | Finds pages with low CTR for their position and rewrites their titles and meta descriptions in your code | Free, plus 1 credit per SERP check |
| [`content-refresh`](skills/content-refresh/SKILL.md) | Finds a page losing traffic, checks what outranks it, and rewrites the weak sections | About 1 to 2 credits |
| [`keyword-cannibalization`](skills/keyword-cannibalization/SKILL.md) | Finds queries where your own pages compete, and merges, redirects, or re-targets them | Free |
| [`indexing-check`](skills/indexing-check/SKILL.md) | Inspects important URLs with the URL Inspection API and fixes noindex, canonical, robots.txt, and sitemap problems | Free |
| [`keyword-research`](skills/keyword-research/SKILL.md) | Expands seeds into a keyword map grouped by intent and mapped to existing or new pages | About 15 to 25 credits |
| [`content-brief`](skills/content-brief/SKILL.md) | Writes a brief for one keyword from the live SERP: intent, angle, outline, titles, and internal links | About 16 credits |
| [`competitor-keyword-gap`](skills/competitor-keyword-gap/SKILL.md) | Finds keywords a competitor ranks for that you do not, and the pages to build | 5 to 20 credits per competitor |

New CalmSEO accounts start with free trial credits, enough for a full `find-next-seo-fix` run.

## Install

### Claude Code (skills and MCP server in one step)

```text
/plugin marketplace add calm-seo/seo-skills
/plugin install calmseo@calmseo
```

Then run `/mcp`, pick `calmseo`, and sign in. Skills show up as `/calmseo:find-next-seo-fix`, `/calmseo:search-console-report`, and so on, or just ask in plain English ("why did my traffic drop last month?").

### Any agent with the skills CLI

Works with Claude Code, Cursor, Codex, OpenCode, Windsurf, and other agents that read `SKILL.md`:

```bash
# All skills
npx skills add calm-seo/seo-skills

# One skill
npx skills add calm-seo/seo-skills --skill find-next-seo-fix
```

Then connect the MCP server if you have not already:

| Agent | Connect CalmSEO |
| --- | --- |
| Claude Code | `claude mcp add calmseo --transport http https://mcp.calmseo.com/mcp`, then `/mcp` to sign in |
| Codex | `codex mcp add calmseo --url https://mcp.calmseo.com/mcp`, then `codex mcp login calmseo` |
| Cursor | [One-click install](https://calmseo.com/connect-google-search-console-to-cursor) |
| Claude.ai, ChatGPT | Add a custom connector with the URL `https://mcp.calmseo.com/mcp` |

Finally, connect Google Search Console in your [CalmSEO dashboard](https://calmseo.com/app). CalmSEO asks Google for read-only access.

## Workflows

Skills work on their own, but they also hand off to each other:

1. **Weekly check-in:** `search-console-report` explains what moved and points each problem at the right skill.
2. **More clicks from pages you already have:** `find-next-seo-fix`, or `ctr-title-rewrite` for many pages at once. Both log each change in `seo-fixes.md` with a baseline and a re-check date, and the next run reports what moved.
3. **Win back lost traffic:** `content-refresh`, with `indexing-check` and `keyword-cannibalization` when the cause is technical.
4. **Plan new content:** `keyword-research` or `competitor-keyword-gap` builds the map, and `content-brief` turns one row into a page.

## Using the skills without an agent

Every `SKILL.md` is readable Markdown. You can paste one into any chat app that has the CalmSEO connector, or read it as a checklist. The tool names map to the [CalmSEO MCP tools](https://calmseo.com/docs).

## FAQ

**Do I need a CalmSEO account?**
Yes. The skills call CalmSEO tools to read your Search Console data and run SERP and keyword checks. Search Console tools are free.

**What does a paid call cost?**
A live SERP is 1 credit, a page audit is 5, search volume for up to 1,000 keywords is 15, keyword ideas are 15 to 50, and a competitor's ranked keywords are 5 to 20. See [calmseo.com](https://calmseo.com) for plans.

**Will the agent change my site without asking?**
No. Every skill presents its changes first and edits files only after you agree.

**How is this different from a generic SEO prompt?**
A generic prompt guesses what to fix. These skills start from your own clicks, impressions, and positions, check the live SERP before recommending a title or a page, and plan a re-check so you can see whether the change worked.

## License

MIT. Built by [CalmSEO](https://calmseo.com).
