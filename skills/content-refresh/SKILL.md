---
name: content-refresh
description: Refresh a page that is losing Google traffic. Compares two Search Console periods to find the decaying page and the queries it lost, checks what now ranks above it, and rewrites the weak sections in place while keeping the author's voice. Use when the user says a post or page is declining, traffic dropped, rankings slipped, or asks to update or refresh old content. Requires the CalmSEO MCP server.
argument-hint: "[site or page URL]"
---

# Refresh a decaying page

Find a page that is losing Google clicks, work out why, and update it so it can win back its rankings. Search Console steps are free. The SERP check costs about 1 to 2 CalmSEO credits.

If the CalmSEO tools are not available, ask the user to connect the CalmSEO MCP server (https://mcp.calmseo.com/mcp) and stop.

Large results: when a result is too large for the chat, CalmSEO returns a short preview and a CSV download link. Download the CSV (for example with curl) and work from the full file. If you cannot download files, lower row_limit and say the analysis covers only the top rows.

## 1. Pick the property and windows

- Call list_gsc_sites and pick the property. If several fit, ask which one.
- End the current window 3 days before today. Compare the last 28 days with the 28 days before them. If the property has fewer than about 1,000 impressions in 28 days, use 90-day windows.
- If at least 13 months of data exist, also compare with the same window a year earlier, so a seasonal dip is not mistaken for decay.

## 2. Find the decaying page (free)

If the user named a page, use it. Otherwise call query_gsc_search_analytics with dimensions ["page"] and row_limit 1000 for both windows, and rank pages by clicks lost. Prefer pages where average position got worse, since those are the ones a refresh can fix. A page that only lost impressions at a steady position is losing demand, not rankings. Show the top three and pick one, or ask the user.

## 3. Diagnose the loss (free)

- Call get_gsc_page_queries for both windows. For each query that lost clicks, note whether position, impressions, or CTR changed.
- Call inspect_gsc_urls for the page. If it is not indexed, or Google chose a different canonical, stop and fix that first (see the indexing-check skill).
- For the top two or three lost queries, call query_gsc_search_analytics with dimensions ["page"] and a query filter equal to that query. If another page on the same site now takes those impressions, this is cannibalization. Suggest the keyword-cannibalization skill instead of a refresh.

## 4. Check what outranks it (paid)

Before the first paid call, state the total, for example: "Checking the live SERP for the 2 main lost queries costs 2 CalmSEO credits (search_serp, depth 10)." Ask the user to confirm and wait, unless they already approved it earlier in the conversation.

- For location_name, use the country that sends the page most impressions (query_gsc_search_analytics with dimensions ["country"] and a page filter, free).
- Call search_serp with depth 10 for the one or two queries that lost the most clicks.
- Note the search intent, the kinds of pages that rank, how fresh they are, and what the top titles promise.

If a paid call fails because the account is out of credits, continue with the free data and share the upgrade_url from the error.

## 5. Compare the content

- Read the page's current content from the source code if it is open in this session, otherwise with a web fetch tool.
- If you have a web fetch tool, read the top three ranking pages. Do not copy them.
- List the gaps:
  - Stale facts: old years, prices, product versions, screenshots, statistics, broken links.
  - Missing subtopics or questions that most of the top pages answer.
  - Format: a table, steps, a calculator, or a comparison that the top pages use and this one does not.
  - Proof: first-hand experience, data, examples, or screenshots this site can add and the others cannot.
  - Intent shift: the SERP now wants a different kind of page. Say so plainly. A refresh may not be enough.
  - A slow opening: the answer to the main query should appear in the first few sentences.

## 6. Plan the refresh

Return:

- The page, the lost queries, and the evidence: clicks, impressions, CTR, and position in both windows, plus SERP notes.
- What to update, add, cut, and restructure, section by section.
- A new title tag and meta description, only if the intent or wording has shifted.
- Two or three internal links from pages on the same site with Search Console traffic for related queries, with anchor text.

Keep the URL. Do not change the published date to look fresh without a real update. If the page shows an "updated" date, change it only for a substantial update.

## 7. Apply and log

If the source code is open in this session, offer to make the edits. Keep the author's voice and structure where it works, and rewrite only the weak sections. Make changes only after the user agrees.

Re-check about 4 weeks after the change ships (2 weeks at the earliest) with get_gsc_page_queries over equal windows before and after the ship date. If you are working in a project folder, ask whether to log it in `seo-fixes.md` at the project root (create it if needed):

```markdown
## YYYY-MM-DD: <page path>

- Property: <site_url>
- Page: <page URL>
- Target query: <main lost query>
- Change: content refresh (<short summary>)
- Shipped: <date, or "not yet">
- Baseline (<start> to <end>): <clicks> clicks, <impressions> impressions, <CTR> CTR, position <position>
- Re-check on or after: <date>
- Check: get_gsc_page_queries for the page, equal windows before and after the ship date
```
