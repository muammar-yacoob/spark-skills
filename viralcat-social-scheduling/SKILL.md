---
name: viralcat-social-scheduling
description: Schedule and track social posts across platforms from one place — create drafts, upload media, queue posts, retry failures, and read back performance. Use when someone wants to schedule social content, run a posting calendar for a product, check why a post failed to publish, or report on post performance.
---

# Viral Cat: scheduling and tracking

Connects to the Viral Cat MCP server.

## Setup

```json
{
  "mcpServers": {
    "viral-cat": { "type": "http", "url": "https://viral-cat.com/api/mcp/mcp" }
  }
}
```

## Order of operations

1. `list_brands`, then `get_brand_details` for the one you are posting as.
2. `list_connections` **before scheduling anything**. A platform that is
   disconnected, or was never connected, accepts a scheduled post and then fails
   at publish time, which reads as a bug hours after the fact.
3. `create_media_upload` / `upload_media` for anything with an image or video.
4. `create_draft` then `schedule_post`.
5. `preflight_check` catches the per-platform rejections — length, aspect ratio,
   missing alt text — before the queue does.

Afterwards: `list_posts` for state, `diagnose_failures` for why one did not go
out, `retry_failed_posts` once the cause is fixed, and `refresh_post_metrics`
then `post_performance` for numbers.

## Worth knowing

**Do not schedule through a general-purpose cron.** This server is the
scheduler; a second timer posting independently produces duplicates that are
tedious to unpick across platforms.

**Count the platforms you actually have connected** before promising a client or
a landing page that you post to N networks. Connections lapse, and a site that
advertises more platforms than `list_connections` returns is making a claim the
tooling will not honour.
