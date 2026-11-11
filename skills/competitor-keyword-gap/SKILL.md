---
name: competitor-keyword-gap
description: Find keywords a competitor ranks for in Google that a site does not, by comparing CalmSEO ranked-keyword data for the competitor with the site's own Search Console queries, and turn the best gaps into a page plan. Use when the user asks for a competitor analysis, a keyword gap or content gap, or what keywords a competitor ranks for. Requires the CalmSEO MCP server.
argument-hint: "[competitor domain]"
---

# Competitor keyword gap

Compare what a competitor ranks for with what the site gets from Search Console, and find the gaps worth closing. The site's own data is free. Competitor rankings cost 5 CalmSEO credits per competitor for up to 100 keywords, or 20 for up to 1,000.

If the CalmSEO tools are not available, ask the user to connect the CalmSEO MCP server (https://mcp.calmseo.com/mcp) and stop.

Large results: when a result is too large for the chat, CalmSEO returns a short preview and a CSV download link. Download the CSV (for example with curl) and work from the full file. If you cannot download files, lower row_limit or limit and say the analysis covers only the top rows.

## 1. Pick the competitors

Use the domains the user named, up to three. If the user is not sure who competes in search, offer one search_serp call (depth 10, 1 credit) for the site's main keyword and pick the domains that rank. Search competitors are often different from business competitors.

## 2. Get the site's own queries (free)

- Call list_gsc_sites and pick the property.
- Call query_gsc_search_analytics with dimensions ["query"] and row_limit 25000 for the last 90 days (ending 3 days before today). Keep impressions and average position for each query.
- Call it with dimensions ["country"] to find the main country. Use it as location_name below.

## 3. Get the competitor's rankings (paid)

Before the first paid call, state the total, for example: "Ranked keywords for 2 competitors cost 10 CalmSEO credits (up to 100 keywords each), or 40 for up to 1,000 each." Ask the user to confirm and wait, unless they already approved it earlier in the conversation. Suggest starting with 100 per competitor and going deeper only if the first pass is useful.

Call get_ranked_keywords with target set to the competitor's domain (no https:// or www), the location, and the agreed limit. To study one competitor page, pass its URL as target instead.

If a paid call fails because the account is out of credits, stop and share the upgrade_url from the error. This workflow needs the competitor data.

## 4. Find the gaps

Match keywords after lowercasing and trimming. For each competitor keyword:

- Missing: the competitor ranks in the top 20 and the site has no impressions for it in Search Console.
- Weak: the site has impressions but ranks worse than position 20, or more than 10 positions behind the competitor.
- Covered: the site ranks close to or ahead of the competitor. Skip it.

Search Console hides rare queries, so "missing" means no recorded impressions, not proof the site never appears.

Drop keywords that do not fit the site's product or audience, branded keywords for the competitor (unless the user wants comparison pages), and keywords with almost no search volume.

## 5. Find the pages behind the gaps

Group the gap keywords by the competitor URL that ranks for them. Competitor pages that collect many gap keywords, or a lot of estimated traffic (etv), show which pages the site is missing: comparison pages, templates, integrations, glossaries, free tools, or guides.

## 6. Return the plan

First the competitor pages that matter most:

| Competitor page | Gap keywords | Combined volume | Est. traffic | Page type |
| --- | --- | --- | --- | --- |

Then the gaps, grouped by topic:

| Topic | Keywords (volume, difficulty) | Competitor rank | Site status | Action |
| --- | --- | --- | --- | --- |

Action is either "new page" or "improve <existing page>". End with the three gaps to close first and why: volume, fit with what the site sells, and difficulty compared with the site's strength.

If you are working in a project folder, offer to save it as `seo/competitor-gap-<domain>.md`. To write one of the pages, use the content-brief skill. To find the sites that link to these competitors but not to the site, use the backlink-gap skill.
