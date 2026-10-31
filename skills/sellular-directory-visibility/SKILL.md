---
name: sellular-directory-visibility
description: Get a product listed and found — audit a domain's SEO, score its visibility, generate a submission kit for startup and SaaS directories, track which directories a product is already on, and produce review and comparison content. Use when someone asks about directory submissions, backlinks, launch listings, SEO audits, llms.txt, or why their product is not showing up in search or AI answers.
---

# Sellular: listings and visibility

Connects to the Sellular MCP server.

## Setup

```json
{
  "mcpServers": {
    "sellular": { "type": "http", "url": "https://sellular.online/api/mcp/mcp" }
  }
}
```

## The order that works

1. `track_product` so later scans have something to compare against.
2. `scan_domain` then `seo_audit_summary` for what is actually wrong.
3. `generate_submission_kit` to produce the copy each directory asks for, once,
   instead of rewriting a description per site.
4. `check_submission_readiness` before submitting anything.
5. `record_submission` after each one, so the next session does not resubmit.

## Two things worth knowing before promising results

**An audit fetches the URL it is given.** It will not notice that a *subpage*
declares the homepage as its canonical, which is a defect that quietly
de-indexes every page but the front one. If a site ranks for its homepage and
nothing else, fetch a subpage by hand and compare the `rel="canonical"` it
serves against its own URL.

**Most free directories are not free any more.** Before planning a submission
sweep, check which of the targets actually accept a self-serve submission:
several now charge, several require an account that cannot be created
programmatically, and at least one requires a public repository with stars.
Saying a lane is closed is more useful than manufacturing a submission queue.
