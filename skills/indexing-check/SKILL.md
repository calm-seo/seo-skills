---
name: indexing-check
description: Check which important pages Google has not indexed and why, using the Search Console URL Inspection API through the CalmSEO MCP server, then fix the causes in the codebase (noindex tags, canonicals, robots.txt, sitemaps, redirects, thin or duplicate pages). Use when the user asks whether pages are indexed, why a page is not on Google, about "Crawled - currently not indexed" or "Discovered - currently not indexed", or after a launch or site migration.
argument-hint: "[site or URLs]"
---

# Check indexing

Find the pages Google has not indexed, group them by cause, and fix what the site controls. Every step uses free CalmSEO Search Console tools.

If the CalmSEO tools are not available, ask the user to connect the CalmSEO MCP server (https://mcp.calmseo.com/mcp) and stop.

Large results: when a result is too large for the chat, CalmSEO returns a short preview and a CSV download link. Download the CSV (for example with curl) and work from the full file. If you cannot download files, lower row_limit and say the analysis covers only the top rows.

## 1. Pick the property

Call list_gsc_sites. If the user named a site or URLs, match them to a property. If several fit, ask which one.

## 2. Build the URL list

If the user gave URLs, use those. Otherwise:

- Call list_gsc_sitemaps. Note each sitemap's errors, warnings, and last download date.
- Collect URLs from the sitemaps with a web fetch tool if you have one, or from the project's routes and content files if the source code is open.
- Call query_gsc_search_analytics with dimensions ["page"], row_limit 25000, over the last 90 days (ending 3 days before today). Sitemap URLs with no impressions at all are the main suspects.

Pick up to about 100 URLs, in this order: pages the user named, money pages (home, pricing, product, signup), recently published pages, and sitemap URLs with no impressions. Google allows 2,000 inspections per property per day, so tell the user before inspecting more than a few hundred.

## 3. Inspect (free)

Call inspect_gsc_urls with up to 25 URLs per call, repeating for the rest. If a result has stopped_early, send its remaining_urls in the next call. Results reflect Google's last crawl, not a live test.

## 4. Group by cause and fix

For each URL that is not indexed, use coverage_state, indexing_state, robots_txt_state, page_fetch_state, the canonicals, and in_sitemap to find the cause:

- Excluded by a noindex tag: find the noindex in the code or CMS. Remove it if it is a mistake.
- Blocked by robots.txt: find the rule. Remove it if the page should rank.
- Google chose a different canonical than the one declared: the pages look like duplicates. Merge them, make them clearly different, or point the canonical at the version Google picked.
- Duplicate with no declared canonical: add a canonical tag.
- Alternate page with a proper canonical: usually fine. Make sure the canonical target is the one you want.
- Page with a redirect, or Not found (404) or Soft 404, listed in the sitemap: put the final URL in the sitemap, or fix or remove the page.
- Crawled, currently not indexed: Google saw it and passed. Usually thin, duplicate, or low-value content. Improve it, merge it into a stronger page, or add internal links from pages that get traffic.
- Discovered, currently not indexed: Google knows the URL but has not crawled it. Add internal links from strong pages, make sure it is in the sitemap, and check that the server responds quickly.
- URL is unknown to Google: nothing links to it and no sitemap lists it. Add it to the sitemap and link to it.
- Indexed but in_sitemap is false for a page that should be listed: add it to the sitemap. A false value does not prove the URL is missing from every sitemap, so check the file.

## 5. Report

```markdown
# Indexing check: <property>, <date>

Inspected <n> URLs: <n> indexed, <n> not indexed.

## <cause> (<n> URLs)
- <URL>: <coverage state>, last crawl <date>
- Fix: <what to change, and where in the codebase>
```

Lead with the groups that affect the most important pages.

## 6. Apply and follow up

If the source code is open in this session, offer to make the fixes: robots.txt, meta robots tags, canonical tags, sitemap generation, redirects, and internal links. Make changes only after the user agrees.

This tool cannot request indexing. After a fix ships, share the inspection_result_link for the most important URLs so the user can click "Request indexing" in Search Console. Re-inspect those URLs after a week or two. Google recrawls on its own schedule.
