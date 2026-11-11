---
name: ctr-title-rewrite
description: Rewrite title tags and meta descriptions for pages that get Google impressions but too few clicks. Finds pages whose CTR is low for their position in Search Console, checks what the live SERP rewards, and edits the titles in the codebase. Use when the user asks to improve CTR, rewrite titles or meta descriptions, or fix pages that rank but nobody clicks. Requires the CalmSEO MCP server.
argument-hint: "[site or page URL]"
---

# Rewrite titles for more clicks

Find pages that already show up in Google but get fewer clicks than they should for their position, and write better title tags and meta descriptions for them. Search Console steps are free. The optional SERP checks cost 1 CalmSEO credit each.

If the CalmSEO tools are not available, ask the user to connect the CalmSEO MCP server (https://mcp.calmseo.com/mcp) and stop.

Large results: when a result is too large for the chat, CalmSEO returns a short preview and a CSV download link. Download the CSV (for example with curl) and work from the full file. If you cannot download files, lower row_limit and say the analysis covers only the top rows.

## 1. Pick the property and window

- Call list_gsc_sites and pick the property. If several fit, ask which one.
- Use the 28 days ending 3 days before today. If the property has fewer than about 1,000 impressions in that window, use 90 days.
- If the user named one page, skip to step 3 for that page.

## 2. Find low-CTR pages (free)

Call query_gsc_search_analytics with dimensions ["page"] and row_limit 1000.

Build the site's own CTR curve instead of using industry averages. Group pages by average position (1 to 3, 4 to 6, 7 to 10, 11 to 20) and take the median CTR of each group, counting only pages with at least 100 impressions.

A page is a candidate when:

- It has at least 100 impressions in the window (or is in the top quarter of the site by impressions).
- Its average position is 20 or better.
- Its CTR is below about half the median for its position group.

Score each candidate by missed clicks: impressions times (group median CTR minus its CTR). Take the top 10.

## 3. Learn what each page is found for (free)

For each candidate, call get_gsc_page_queries. Note the two or three queries with the most impressions, their positions, and their wording. A title that does not use the words people actually search is the most common reason for low CTR.

Leave out pages where most impressions come from branded queries or from queries the page does not answer. Those need a different page, not a new title.

## 4. Read the current titles

Read the current title tag, meta description, and H1 from the source code when it is open in this session. Otherwise use a web fetch tool if you have one. Use audit_page (5 credits per page) only when neither works, and say so.

## 5. Check the SERP (paid, optional)

For the top three to five candidates, a live SERP shows what Google rewards for the main query. Before the first paid call, state the total, for example: "Checking the live SERP for 5 queries costs 5 CalmSEO credits (search_serp, depth 10)." Ask the user to confirm and wait for their answer, unless they already approved these checks earlier in the conversation.

For location_name, use the country that sends the page most impressions (query_gsc_search_analytics with dimensions ["country"] and a page filter, free).

From each SERP, note what the top titles promise (a year, a number, "free", a format like "template" or "calculator", a comparison) and whether the page type matches the site's page. If the SERP is mostly a different kind of page, say that a title change alone will not fix it.

If a paid call fails because the account is out of credits, continue without the SERP check and share the upgrade_url from the error.

## 6. Write the new titles

For each page:

- Put the main query, in the words people search, near the front.
- Keep the title to about 60 characters or fewer so Google does not cut it off.
- Make one specific promise that matches the intent: what the reader gets, for whom, or how fast.
- Add the brand at the end only if there is room. Do not repeat the same title pattern across every page.
- Write a meta description of about 155 characters or fewer that answers the query directly and gives a reason to click. Google may rewrite it, so the title matters more.
- Keep the H1 on the same topic as the title. It does not need to be identical.
- Do not stuff keywords or promise anything the page does not deliver.

## 7. Present and apply

Return a table:

| Page | Main query | Position | CTR (site median) | Current title | New title | New meta description | Why |
| --- | --- | --- | --- | --- | --- | --- | --- |

If the source code is open in this session, offer to apply the changes. Find where each title is set (framework metadata, frontmatter, a head component, or a CMS field) and edit it there. Make changes only after the user agrees.

## 8. Plan the re-check

CTR responds faster than rankings. Re-check 2 to 4 weeks after the change ships: call get_gsc_page_queries for each page over equal windows before and after the ship date and compare CTR at similar positions.

If you are working in a project folder, ask whether to log each change in `seo-fixes.md` at the project root (create it if needed):

```markdown
## YYYY-MM-DD: <page path>

- Property: <site_url>
- Page: <page URL>
- Target query: <query>
- Change: title and meta description rewrite
- Shipped: <date, or "not yet">
- Baseline (<start> to <end>): <clicks> clicks, <impressions> impressions, <CTR> CTR, position <position>
- Re-check on or after: <date>
- Check: get_gsc_page_queries for the page, equal windows before and after the ship date
```
