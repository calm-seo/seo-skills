---
name: keyword-cannibalization
description: Find queries where two or more of a site's own pages compete in Google, using free Search Console data through the CalmSEO MCP server, and decide for each one whether to merge, redirect, re-target, or leave it alone. Use when the user asks about keyword cannibalization, pages competing with each other, duplicate content, or rankings that keep swapping between URLs.
argument-hint: "[site]"
---

# Fix keyword cannibalization

Find queries where several pages on the same site split Google's attention, separate real problems from harmless overlap, and fix the real ones. Every step uses free CalmSEO Search Console tools.

If the CalmSEO tools are not available, ask the user to connect the CalmSEO MCP server (https://mcp.calmseo.com/mcp) and stop.

Large results: when a result is too large for the chat, CalmSEO returns a short preview and a CSV download link. Download the CSV (for example with curl) and work from the full file. If you cannot download files, lower row_limit and say the analysis covers only the top rows.

## 1. Pick the property and window

- Call list_gsc_sites and pick the property. If several fit, ask which one.
- Use the 90 days ending 3 days before today. Cannibalization needs more data than most checks.

## 2. Pull query and page pairs (free)

Call query_gsc_search_analytics with dimensions ["query", "page"] and row_limit 25000. If it returns exactly 25000 rows, call it again with start_row 25000 until fewer rows come back.

Group rows by query. Keep a query when:

- It has at least 100 impressions in the window.
- Two or more pages each get at least 10% of its impressions.

Leave out branded queries and URLs that differ only by a fragment (#...) or a tracking parameter.

## 3. Confirm the swapping (free)

For the 10 queries with the most impressions, call query_gsc_search_analytics with dimensions ["date", "page"] and a query filter equal to the query. Real cannibalization shows pages taking turns: one page ranks on some days, the other on the rest, and neither holds a strong position.

## 4. Separate problems from overlap

Not a problem, leave it alone:

- Both pages rank in the top 5 at the same time. The site takes two spots.
- The pages answer different intents, for example a pricing page and a how-to guide that both get some impressions for a broad query.
- One page gets nearly all the clicks and the other only shows up rarely.

A real problem:

- The pages take turns and neither ranks better than about position 8.
- Both pages target the same intent with similar titles and content.
- The site has a strong page that a weaker, newer, or thinner page keeps displacing.

## 5. Pick a fix for each real case

Choose the page to keep: usually the one with more clicks, more internal links, and the closest match to the intent. Then pick one fix:

- Merge and redirect: move the useful parts of the weaker page into the stronger one and 301 redirect the weaker URL. Best when the pages cover the same topic.
- Re-target: give the weaker page a different main query, with a new title, H1, and angle. Best when both pages deserve to exist.
- Canonical: point near-duplicate pages, such as parameter or print versions, at the main URL.
- Noindex: for thin tag, filter, or archive pages that should not rank at all.
- Internal links: point links with the query's wording at the page you keep, and remove links that send that wording to the other page.

## 6. Present and apply

Return a table:

| Query | Impressions | Pages (clicks, position) | Problem? | Keep | Fix |
| --- | --- | --- | --- | --- | --- |

If the source code is open in this session, offer to make the changes: redirects in the project's redirect config, canonical tags, noindex tags, title and content edits, and internal links. Make changes only after the user agrees, and never delete content without asking.

Re-check about 4 weeks after the change ships with the same query filter and dimensions ["page"]. The kept page should hold more of the impressions and a better position. If you are working in a project folder, ask whether to log each fix in `seo-fixes.md` at the project root, with the property, the query, the pages, the change, the ship date, a baseline, and the re-check date.
