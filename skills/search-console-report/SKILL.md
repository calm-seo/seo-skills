---
name: search-console-report
description: Write a plain-English Google Search Console report for a site. Compares clicks, impressions, CTR, and position with the previous period, explains which pages and queries gained or lost the most and why, and ends with three actions for the coming weeks. Uses only free Search Console data from the CalmSEO MCP server. Use when the user asks how their site is doing in Google, for a weekly or monthly SEO report, or why search traffic went up or down.
argument-hint: "[site] [weekly|monthly]"
---

# Search Console report

Explain what changed in Google search for this site, why it most likely changed, and what to do next. Every step uses free CalmSEO Search Console tools. Do not call paid tools in this workflow.

If the CalmSEO tools are not available, ask the user to connect the CalmSEO MCP server (https://mcp.calmseo.com/mcp) and stop.

Large results: when a result is too large for the chat, CalmSEO returns a short preview and a CSV download link. Download the CSV (for example with curl) and work from the full file. If you cannot download files, lower row_limit and say the analysis covers only the top rows.

## 1. Pick the property and windows

- Call list_gsc_sites. If the user named a site, match it to a property. If several properties fit, ask which one.
- Search Console data is delayed by 2 to 3 days, so end the current window 3 days before today.
- Default: the 28 days ending on that date, compared with the 28 days before them. For a weekly report, use 7 days against the previous 7. For a monthly report, use 28 days.
- If the user asks about seasonality, or the change looks seasonal, also compare with the same window one year earlier. Search Console keeps about 16 months of data.

## 2. Get the totals

Call query_gsc_search_analytics with dimensions ["date"] for each window. Sum clicks and impressions. Compute CTR as clicks divided by impressions, and average position weighted by impressions. Report the change for each metric in absolute numbers and percent.

## 3. Find the biggest movers

For each window, call query_gsc_search_analytics with dimensions ["page"] and row_limit 1000, then with dimensions ["query"] and row_limit 1000. Join the windows on page and on query.

- Top 5 pages by clicks gained and top 5 by clicks lost.
- Top 5 queries by clicks gained and top 5 by clicks lost. Leave out branded queries, or report them separately.
- New queries: queries with impressions in the current window and none in the previous one, top 10 by impressions. These show demand the site is starting to reach.

Query rows add up to less than page rows because Google hides rare queries for privacy. Do not treat that gap as missing traffic.

## 4. Explain each big change

For each of the largest gains and losses (up to 5), call get_gsc_page_queries for both windows and decide which pattern fits:

- Impressions fell and position held: demand fell. Check the year-ago window before calling it a problem.
- Position fell: the page lost rankings. Suggest the content-refresh skill.
- CTR fell and position held: the result got less attractive or the SERP changed. Suggest the ctr-title-rewrite skill.
- The page lost almost all impressions: it may have dropped out of the index. Call inspect_gsc_urls for that URL and suggest the indexing-check skill if it is not indexed.
- Two of the site's pages trade places for the same query: suggest the keyword-cannibalization skill.

Only break down by device (dimensions ["device"]) or country (dimensions ["country"]) when it explains a change. Label every cause as likely, not certain, unless the data shows it directly.

## 5. Write the report

Keep it to about one screen:

```markdown
# Search Console report: <property>, <start> to <end>

| Metric | This period | Previous | Change |
| --- | --- | --- | --- |
| Clicks | | | |
| Impressions | | | |
| CTR | | | |
| Avg. position | | | |

## What changed
<two to four sentences in plain English>

## Biggest gains
<page or query, clicks before and after, likely cause>

## Biggest losses
<page or query, clicks before and after, likely cause>

## New queries to watch
<query, impressions, ranking page>

## Next actions
1. <specific action, the page it applies to, and the skill to run>
2. ...
3. ...
```

If you are working in a project folder, offer to save the report as `seo/reports/<end date>.md`. If an earlier report exists there, read it first and mention whether last time's actions moved anything.
