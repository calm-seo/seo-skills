---
name: content-brief
description: Write a content brief for one target keyword from the live Google SERP. Covers search intent, the format and angle that rank, the subtopics and questions to cover, title and meta description options, and internal links, after checking Search Console for an existing page that should be improved instead. Use when the user asks for a content brief or outline, or wants to write a page or blog post that ranks for a keyword. Requires the CalmSEO MCP server.
argument-hint: "[keyword]"
---

# Content brief

Write a brief that someone can use to write one page that ranks for one keyword. The Search Console check is free. The live SERP and search volume cost about 16 CalmSEO credits.

If the CalmSEO tools are not available, ask the user to connect the CalmSEO MCP server (https://mcp.calmseo.com/mcp) and stop.

## 1. Confirm the target

Get the target keyword and the site. If the user has a keyword map (for example `seo/keyword-map.md`), read it and use the group's other keywords too.

## 2. Check for an existing page (free)

If the site is in Search Console (list_gsc_sites), call query_gsc_search_analytics for the last 90 days (ending 3 days before today) with dimensions ["page"] and a query filter that contains the keyword's core term.

If a page already gets meaningful impressions for it, say so. Improving that page (the content-refresh or ctr-title-rewrite skill) usually beats publishing a new one, unless it serves a different intent. Ask the user which way to go.

Use the country that sends the site the most impressions (dimensions ["country"]) as location_name. Otherwise default to "United States" and say so.

## 3. Read the SERP (paid)

Before the first paid call, state the total, for example: "The brief uses about 16 CalmSEO credits (1 for the live SERP, 15 for search volume on the keyword and up to 20 variants)." Ask the user to confirm and wait, unless they already approved it earlier in the conversation.

- Call search_serp with depth 10 for the target keyword.
- Call get_keyword_search_volume once with the target keyword and up to 20 close variants. Volume decides which wording leads the title and H1.

If a paid call fails because the account is out of credits, continue with what you have and share the upgrade_url from the error.

## 4. Study what ranks

From the SERP, note the search intent, the kind of page that ranks (guide, list, tool, comparison, product page), and the SERP features shown.

If you have a web fetch tool, read the top three to five organic results. For each, note the format, headings, rough length, and what it covers. Then find:

- Subtopics and questions most of them cover. The page needs these.
- What none of them do well. This is the angle: first-hand experience, original data, a working example, a clearer structure, or a more specific audience.
- Freshness signals: years, versions, prices, and screenshots.

## 5. Write the brief

```markdown
# Brief: <target keyword>

- Target keyword: <keyword> (<volume>/mo)
- Secondary keywords: <keyword (volume)>, ...
- Search intent: <what the searcher wants to get done>
- Page type and format: <for example, step-by-step guide with a comparison table>
- Angle: <what this page offers that the top results do not>
- Location: <country>

## Title options
1. <about 60 characters or fewer>
2. ...
3. ...

## Meta description
<about 155 characters or fewer>

## H1
<text>

## Outline
- H2: <section>
  - What to cover, and the questions to answer
- H2: ...

## Must cover
<subtopics and terms the top results share>

## Proof to include
<screenshots, data, examples, or quotes the writer needs to gather>

## Internal links
- Link to this page from: <existing pages with related Search Console queries, with anchor text>
- Link from this page to: <relevant product or guide pages>

## Call to action
<what the reader should do next>
```

Give a length range only as a rough guide from the top results. Covering the intent matters more than word count.

If you are working in a project folder, offer to save it as `seo/briefs/<slug>.md`, or to draft the page in the codebase from the brief. Make changes only after the user agrees.
