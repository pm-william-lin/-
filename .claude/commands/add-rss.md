---
description: Add a new RSS source to the PM daily digest
argument-hint: "<source name> [RSS URL]"
allowed-tools:
  - Read
  - Edit
  - Glob
  - Grep
  - Bash
  - WebFetch
  - WebSearch
---

Add a new RSS source to the productmanager-news-summary project.

## Input

The user provides: `$ARGUMENTS`

This can be:
- A source name only (e.g., "Shreyas Doshi") — you need to find the RSS feed URL
- A source name + URL (e.g., "Shreyas Doshi https://shreyas.substack.com/feed")

## Steps

1. **Find RSS feed URL** (if not provided)
   - Search the web for the source's blog/newsletter
   - Try common RSS patterns: `/feed`, `/feed.xml`, `/rss`, `/rss.xml`, `/atom.xml`
   - For Substack: `https://<name>.substack.com/feed`
   - Use WebFetch to verify the feed is valid and returns articles

2. **Validate the RSS feed**
   - WebFetch the URL and confirm it returns valid RSS/Atom XML with recent articles
   - If the feed is invalid or unreachable, report to the user and stop

3. **Add to fetch_articles.py**
   - Read `/Users/william/productmanager-news-summary/fetch_articles.py`
   - Add the new entry to the `RSS_FEEDS` dict
   - Use the source's display name as the key

4. **Update CLAUDE.md**
   - Read `/Users/william/productmanager-news-summary/CLAUDE.md`
   - Add a row to the source table with source name, method (RSS), and a brief note about the author/topic
   - If it's a Substack source, note "Substack，GitHub Actions 上可能被擋"

5. **Run tests**
   - Run `cd /Users/william/productmanager-news-summary && python3 test_fetch.py`
   - All tests must pass. If the new source fails, investigate and fix

6. **Report results**
   - Show the user: source name, RSS URL, number of articles found, test results
   - Ask the user if they want to commit + push

7. **Commit + push** (only if user confirms)
   - `git add fetch_articles.py CLAUDE.md`
   - Commit with message: `Add <source name> RSS source`
   - Push to origin

## Important Notes

- Substack feeds on GitHub Actions cloud IPs get blocked by Cloudflare (403). This is expected — the script handles it gracefully (warns but doesn't fail).
- Do NOT add sources that require JavaScript rendering (e.g., Reforge Blog on Framer).
- Update the memory file at `/Users/william/.claude/projects/-Users-william/memory/pm-news-summary.md` after successfully adding a source.
