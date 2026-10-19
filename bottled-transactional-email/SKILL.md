---
name: bottled-transactional-email
description: Send and manage transactional email from your own domain — send a message, run a campaign, manage contacts and aliases, verify domain DNS, and read replies. Use when someone wants to send email programmatically, set up a sending domain, check SPF/DKIM/DMARC, run an email campaign, or read and reply to a product inbox.
---

# Bottled: transactional email

Connects to the Bottled MCP server for sending, domains, contacts and campaigns.

## Setup

```json
{
  "mcpServers": {
    "bottled": { "type": "http", "url": "https://bottled.email/api/mcp/mcp" }
  }
}
```

An API key is minted in the Bottled dashboard. Keys are stored as a hash, so a
key is shown once at creation and cannot be read back later — if it is lost,
rotate rather than hunt for it.

## Before the first send

`check_domain_setup` then `verify_domain`. A domain that has not verified its
DNS will accept a send call and then fail to deliver, which looks like a bug in
the calling code and is not.

**The From address must be an alias that already exists** on a verified domain.
`list_aliases` shows them; `create_alias` adds one. A send from an address that
was never created is the single most common failure.

## Sending

- `send_email` for one message.
- `create_draft` → `send_draft` when a human should see it before it goes.
- `create_campaign` → `list_campaign_recipients` → `send_campaign` for a list.

Check `get_usage` against the plan before a large campaign rather than
discovering the ceiling halfway through a send.

## A rule worth keeping

A marketing list needs a confirmed opt-in. `list_contacts` returns everyone in
the table, including people who never confirmed, so sending to a raw contact
dump is how a sending domain gets burned. Campaigns go to confirmed contacts;
one-to-one replies are a different thing and need no opt-out line.
