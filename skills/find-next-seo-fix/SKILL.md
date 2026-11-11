---
name: find-next-seo-fix
description: Find the one SEO change most likely to bring a site more Google clicks. Uses free Google Search Console data to pick a page and target query, runs a few paid CalmSEO checks, and returns the exact title, meta description, H1, and content changes with a plan to re-check the result. Use when the user asks for their next SEO fix, what to improve for more search traffic, or an SEO review of their site. Requires the CalmSEO MCP server.
argument-hint: "[site or page URL]"
---

# Find my next SEO fix

Find the one change on this site most likely to bring more Google clicks in the next few weeks, back it with data, and give the user an exact fix to ship. Use the CalmSEO MCP tools. Search Console tools are free. Paid tools spend CalmSEO credits, so use them only where they change the decision and say why each one is worth it.

If the CalmSEO tools are not available, ask the user to connect the CalmSEO MCP server (https://mcp.calmseo.com/mcp) and stop.

## 1. Review past fixes

If you are working in a project folder and `seo-fixes.md` exists at its root, read it first. For each entry whose re-check date has passed, run the check it describes and report what moved (clicks, impressions, CTR, and position for the page and target query). Skip pages changed in the last 4 weeks when choosing a new fix.

## 2. Pick the property and date window

- Call list_gsc_sites. If the user named a site or page, match it to a property. If several properties fit, ask which one.
- Search Console data is delayed by 2 to 3 days, so end the window 3 days before today. Use the 28 days ending on that date, and compare with the 28 days before them.
- If the property has fewer than about 1,000 impressions in that window, use 90-day windows instead.
- If the property has no impressions, say so and stop.

## 3. Find candidates (free)

Use get_gsc_top_pages, get_gsc_page_queries, and query_gsc_search_analytics (dimensions ["page", "query"], row_limit 1000 or more) to find:

- Pages with meaningful impressions at an average position of about 4 to 20.
- Pages with a low CTR compared with other pages on the same site at a similar position.
- Queries at positions 5 to 30 that an existing page could win with better targeting.
- Pages that lost clicks or position compared with the previous window.

Leave out branded queries. If the user named a page, focus on that page and its queries.

## 4. Choose one page and one target query

Rank candidates by impressions and room to move up. Pick one page and one primary target query. Explain the choice in two or three sentences, and list up to two runners-up in one line each.

## 5. Check the choice with paid tools

Before the first paid call, state the expected total once, for example: "The paid checks use about 21 CalmSEO credits (15 for search volume, 1 for the live SERP, 5 for the page audit), or 16 if I read the page from your source code." Ask the user to confirm and wait for their answer before any paid call, unless they already approved these paid checks earlier in the conversation.

For location_name, use the country that sends the page most of its impressions (query_gsc_search_analytics with dimensions ["country"] and a page filter, free).

- get_keyword_search_volume: one batched call with the target query and the top candidate queries for the page (up to 20). Volume shows which wording to use in the title and H1. 15 credits.
- search_serp with depth 10 for the target query. Note the search intent, the kinds of pages that rank, and what the top titles promise. 1 credit.
- audit_page for the page URL to read its current title, meta description, H1, canonical, and status code. Skip this when you can read the page's source in the current project, and say so. 5 credits.

Before recommending content changes, read the page's current body content: from the source code if it is open in this session, otherwise with a web fetch tool if you have one. audit_page does not return the body text.

If a paid call fails because the account is out of credits, continue with the free data. Tell the user what that step would have added, and share the upgrade_url from the error.

## 6. Give the fix

Return:

- The page and target query, with the evidence: top queries, impressions, clicks, average position, CTR, search volume, and SERP notes.
- The new title tag (exact text, about 60 characters or fewer).
- The new meta description (exact text, about 155 characters or fewer).
- The new H1 (exact text).
- Specific content changes: sections to add or rewrite, questions to answer, and terms from the page's queries that it does not cover yet. If you could not read the page's body, base these on the queries only and say that they are unverified against the current page.
- Internal links: two or three pages on the same site that should link to this page (prefer pages with Search Console traffic for related queries), with suggested anchor text.
- The expected effect: which metric should change for which query, and why. Do not promise specific numbers.

If the site's source code is open in this session, offer to apply the edit. Make changes only after the user agrees.

## 7. Plan the re-check

- Re-check date: about 4 weeks after the change ships (2 weeks at the earliest).
- The check: query_gsc_search_analytics on the property with dimensions ["query"] and a page filter equal to the page URL. Compare clicks, impressions, CTR, and position for the page and the target query across equal-length windows before and after the change date.
- If you are working in a project folder, ask whether to add an entry to `seo-fixes.md` at the project root (create it if needed). Use this format:

```markdown
## YYYY-MM-DD: <page path>

- Property: <site_url>
- Page: <page URL>
- Target query: <query>
- Change: <what changed>
- Shipped: <date, or "not yet">
- Baseline (<start> to <end>): <clicks> clicks, <impressions> impressions, <CTR> CTR, position <position>
- Re-check on or after: <date>
- Check: query_gsc_search_analytics, dimensions ["query"], page equals <page URL>, equal windows before and after the ship date
```

The next run of this workflow reads this file first and reports what moved.
