---
name: backlink-gap
description: Find link building prospects for a site. Uses CalmSEO backlink data to list sites that link to its competitors but not to it, qualifies them by relevance, rank, dofollow links, and spam, checks which competitor pages earn links, and returns a prioritized prospect list with what to pitch each one. Use when the user asks for link building ideas, a backlink gap or link gap analysis, who links to a competitor, or how to get more backlinks. Requires the CalmSEO MCP server.
argument-hint: "[competitor domains]"
---

# Backlink gap

Find the sites that link to the site's competitors but not to the site, keep the ones worth pitching, and say what to pitch each one. The gap list costs 5 CalmSEO credits for up to 100 linking sites, or 15 for up to 1,000. Optional backlink summaries cost 5 credits each, and backlink lists cost 5 for up to 100 rows. A run with two competitors costs from 5 credits for the gap list alone to about 30 with the optional checks.

If the CalmSEO tools are not available, ask the user to connect the CalmSEO MCP server (https://mcp.calmseo.com/mcp) and stop.

Large results: when a result is too large for the chat, CalmSEO returns a short preview and a CSV download link. Download the CSV (for example with curl) and work from the full file. If you cannot download files, lower limit and say the analysis covers only the top rows.

Rank in these results is a 0 to 100 score of how strong a site's backlink profile is. It is not Domain Rating or Domain Authority, so do not call it DR or DA.

## 1. Pick the competitors

Use the domains the user named. If the competitor-keyword-gap skill already ran in this project, reuse its competitors (see `seo/competitor-gap-*.md`). Otherwise, find who competes in search:

- If the site is in Search Console (list_gsc_sites), call query_gsc_search_analytics with dimensions ["query"] for the last 90 days (ending 3 days before today), free, and take the top non-branded queries.
- Offer search_serp calls (depth 10, 1 credit each) for one to three of those queries, and pick domains that rank more than once.
- To check whether a business competitor also competes in search, offer get_ranked_keywords with limit 100 (5 credits). If it ranks for few of the site's topics, its links matter less.

Leave out giant sites (Wikipedia, Amazon, YouTube, Reddit, large news publishers). Their backlinks have little to do with the site's niche. Prefer competitors a bit stronger than the site, not ten times stronger.

How many to use: find_backlink_gaps returns sites that link to every competitor listed. Two or three competitors give a shorter list of sites that link to the whole category, which are the most likely to link to the site too. One competitor gives the broadest list. Start with two. If that returns fewer than about 20 sites, run each competitor on its own.

## 2. Confirm the cost

Before the first paid call, state the total, for example: "The backlink gap for 2 competitors costs 5 CalmSEO credits for up to 100 linking sites, or 15 for up to 1,000. Backlink summaries for your site and both competitors add 15, and a backlink list for one competitor adds 5." Ask the user to confirm and wait, unless they already approved it earlier in the conversation. Suggest starting with 100 rows and going deeper only if the first pass is useful.

If a paid call fails because the account is out of credits, stop and share the upgrade_url from the error. This workflow needs the backlink data.

## 3. Compare profiles (paid, optional)

Call get_backlink_summary for the site and each competitor (5 credits each). Compare rank, referring domains, the nofollow share of referring domains, backlinks spam score, and the top linking countries. This shows how far behind the site is and which prospects are realistic: a site with rank 20 rarely wins a link from a rank 80 publication with a cold email.

Skip this step if the user wants to save credits.

## 4. Find the gap (paid)

Call find_backlink_gaps with:

- target: the site's domain, without https:// or www. Sites that already link to it are left out.
- competitors: the agreed domains.
- dofollow_only: true when the goal is rankings. Leave it false when referral traffic or brand mentions also count.
- limit: the agreed limit, usually 100.

Each result has the linking domain, its rank, and how many backlinks it sends to each competitor. "Not linking to the site" means no link was found in the backlink index, so a few prospects may already link to the site.

## 5. Qualify the prospects

Go through the list and drop or flag sites that will not help:

- Relevance: the site should cover the same topic, industry, or audience. Judge from the domain name, and if you have a web fetch tool, read the homepage of the top 30 or so. Drop unrelated sites even when their rank is high.
- Link pattern: one to a few links to each competitor usually means an editorial mention, such as a list, review, comparison, or resource page. Hundreds or thousands of links usually mean a sitewide footer or blogroll link, a directory, or a platform where users post links. Those are a different tactic, not outreach.
- Rank: higher is stronger, but relevant sites in the middle of the range are often easier to win. Do not keep only the top of the list.
- Spam: drop link farms, scraper and "website worth" sites, auto-generated directories, and sites in unrelated languages. If a borderline prospect matters, get_backlink_summary on it shows its spam score (5 credits). Only spend that on a few.
- Platforms: social networks, forums, Q&A sites, and app or plugin marketplaces can still be worth it, but as places to take part or list a product, not to pitch. Group them separately.

## 6. See what earns competitor links (paid, optional)

Call get_backlinks for the strongest competitor (5 credits for up to 100 rows) with mode one_per_domain, status live, sort rank, and the same dofollow_only setting. Then:

- Match its linking domains with the qualified prospects. The url_from and anchor show the exact page and wording that link to the competitor, which tells you what to pitch: get added to a list, offer an update to a comparison, or suggest a resource.
- Group the backlinks by url_to. Competitor pages that collect links from many sites (free tools, studies and statistics, templates, glossaries, integration pages) are the linkable assets the site may be missing.
- To see the competitor's current link building, run it again with sort newest (another 5 credits). Recent links show what it is doing right now.

## 7. Return the prospect list

Rank prospects as high (relevant, links to two or more competitors, and the linking page is one the site fits on), medium (relevant, but the fit or the page is less clear), or low (weak relevance or a long shot). Then return:

| Priority | Prospect | Rank | Links to competitors | Linking page (if known) | Type | Pitch |
| --- | --- | --- | --- | --- | --- | --- |

Type is list or roundup, review or comparison, resource page, directory, partner or integration, guest post, news, or community. Pitch is one sentence on why this site belongs on that page.

Then the linkable assets:

| Competitor page | Linking sites | What it is | Site's equivalent |
| --- | --- | --- | --- |

End with the ten prospects to contact first and why, and the one asset worth building if the site has no equivalent. To plan that page, use the content-brief skill.

Do not recommend buying links or link exchanges. They break Google's spam policies. If the user asks, draft short outreach emails for the top prospects, each specific to the linking page. Never send email for the user.

If you are working in a project folder, offer to save the list as `seo/backlink-gap-<domain>.md`. CalmSEO does not track backlink history over time, so to see which prospects linked, run get_backlinks for the site in 2 to 3 months with sort newest and compare with the saved file.
