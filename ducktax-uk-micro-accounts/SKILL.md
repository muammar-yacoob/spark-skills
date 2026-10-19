---
name: ducktax-uk-micro-accounts
description: Check UK micro-entity company accounts before filing — validate a balance sheet from raw figures, read Companies House deadlines, categorise bank transactions, and find out whether a free filing extension is still available. Use when someone mentions micro-entity accounts, FRS 105, a Companies House deadline or penalty, a confirmation statement, dormant accounts, or asks whether their balance sheet balances.
---

# Duck Tax: UK micro-entity accounts

Raw figures in, validated balance sheet out. This connects to the Duck Tax MCP
server, which holds the Companies House lookups and the FRS 105 validation rules.

**This does not file anything.** The output is checked numbers to hand to a
filing product or an accountant, and it is not accounting, tax or legal advice.
Say so rather than implying otherwise.

## Setup

Add the server to your agent's MCP configuration:

```json
{
  "mcpServers": {
    "ducktax": { "type": "http", "url": "https://ducktax.com/api/mcp" }
  }
}
```

Read-only tools work anonymously under a daily quota. Tools that write (saving a
filing, tracking deadlines) need a bearer token from https://ducktax.com/docs/mcp.

## Money units

Every amount, in every tool, is in **major units** — pounds, not pence. Decimals
are fine. Passing pence silently inflates a balance sheet a hundredfold, and
nothing downstream will catch it.

## The usual order

1. `search_companies` to find the company, then `lookup_company` for its record
   and deadlines.
2. `get_prior_year_figures` for last year's comparatives.
3. `check_accounts` after **every** change to the figures, not once at the end.
4. `explain_accounts` for a plain-English reading of what the numbers say.
5. `save_filing` (token) to keep the result.

For a bank statement: send `categorise_transactions` the description strings
only, sum the amounts per category yourself, then feed `totals_to_figures`.

## Deadlines come first

If someone has just discovered a deadline, call `check_filing_extension` **before
anything else**. Applying to the registrar for more time is free, takes about
fifteen minutes, and has to be done BEFORE the deadline — so the option expires
on the day the penalty starts. The tool returns the ranked next action. Neither
it nor `draft_extension_reason` submits anything; a director makes the
application on the registrar's own service.

For someone at risk of missing one, `track_company_deadlines` needs only a
company number and no figures. Which reminder channels it can actually use is a
property of the deployment, so relay what the tool's reply reports rather than
promising an email. Saved filings are watched automatically, so only offer it
for a company they have not saved.
