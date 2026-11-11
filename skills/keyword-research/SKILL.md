---
name: keyword-research
description: Build a keyword map for a site or topic. Expands seed keywords into ideas with search volume and difficulty, groups them by search intent, checks which ones the site already ranks for in Search Console, and assigns each group to an existing page or a new one. Use when the user asks for keyword research, what to write about, content ideas, topic clusters, or a keyword strategy. Requires the CalmSEO MCP server.
argument-hint: "[site or seed keywords]"
---

# Keyword research

Turn a few seed keywords into a prioritized keyword map, where each group of keywords belongs to one page. Search Console steps are free. The keyword data usually costs 15 to 25 CalmSEO credits, plus 1 credit for each SERP check.

If the CalmSEO tools are not available, ask the user to connect the CalmSEO MCP server (https://mcp.calmseo.com/mcp) and stop.

Large results: when a result is too large for the chat, CalmSEO returns a short preview and a CSV download link. Download the CSV (for example with curl) and work from the full file. If you cannot download files, lower row_limit and say the analysis covers only the top rows.

## 1. Understand the business

Read the site first: the homepage, pricing, and product or docs pages, from the source code if it is open in this session, otherwise with a web fetch tool. Note what the product does, who buys it, and which pages exist.

Agree on 3 to 10 seed keywords with the user. Good seeds name the problem or the product category, not the brand.

## 2. Check what the site already gets (free)

If the site is in Search Console (list_gsc_sites):

- Call query_gsc_search_analytics with dimensions ["query", "page"] and row_limit 1000 for the last 90 days (ending 3 days before today).
- Call it with dimensions ["country"] to find the main country. Use it as location_name below. Otherwise default to "United States" and say so.

This shows which topics the site already ranks for and how strong it is. A site with only a few thousand impressions should aim at lower-difficulty keywords first.

## 3. Expand the seeds (paid)

Before the first paid call, state the total, for example: "Keyword ideas for these seeds cost 15 CalmSEO credits for up to 50 ideas, or 25 for up to 250. Checking the SERP for 3 unclear groups adds 3 credits." Ask the user to confirm and wait, unless they already approved it earlier in the conversation.

Call find_keyword_ideas with the seeds, the location, limit 50 (or 250 for a broad topic), and include_serp_info true. Each idea comes with search volume, keyword difficulty, CPC, and the SERP features shown.

If a paid call fails because the account is out of credits, continue with the Search Console data and share the upgrade_url from the error.

## 4. Filter

Drop ideas that:

- Do not fit what the product does or who buys it.
- Name a competitor's brand, unless the user wants comparison or alternative pages.
- Have no search volume and no impressions in Search Console.
- Are far above the site's strength. For a newer or smaller site, prefer keyword difficulty under about 30.

## 5. Group by intent

One group is one page. Keywords belong in the same group when the same page could rank for all of them: same intent, same kind of result. Name each group by its main keyword, the one with the most volume that still fits.

When two keywords could go either way, call search_serp with depth 10 for each (1 credit each). If their top 10 results share about four or more URLs, they belong on one page.

## 6. Map groups to pages

For each group, find the existing page that already gets impressions for it (from step 2) or that the site's structure suggests. If none fits, mark it as a new page.

Prioritize by search volume, fit with what the product sells, and how winnable it is: difficulty compared with the site's strength, and whether the site already has impressions nearby.

## 7. Return the keyword map

| Priority | Group (main keyword) | Other keywords | Volume | Difficulty | Intent | Target page | Status |
| --- | --- | --- | --- | --- | --- | --- | --- |

Status is "ranks" (position 10 or better), "close" (11 to 30), "weak" (worse than 30), or "new page". Below the table, recommend the first three pages to build or improve and why.

If you are working in a project folder, offer to save the map as `seo/keyword-map.md`. To write a page for one group, use the content-brief skill.
