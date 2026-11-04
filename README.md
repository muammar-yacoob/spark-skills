# Spark Skills

Agent skills for [Claude Code](https://claude.com/claude-code), claude.ai and
other MCP-capable agents, packaged as one installable plugin.

They cover the part of shipping a product that starts after the code works:
charging for it, emailing the people who bought it, showing it to anyone, and
filing the accounts at the end of the year.

## Install

```bash
claude plugin install spark-skills --marketplace muammar-yacoob/spark-skills
```

Or from inside a session: `/plugin install spark-skills --marketplace muammar-yacoob/spark-skills`

To try it for a single session without installing, clone the repository and
start Claude Code with `claude --plugin-dir ./spark-skills`.

## The skills

| Skill | What it does |
|---|---|
| [`add-payments-to-an-app`](skills/add-payments-to-an-app/) | Turns an app that works but cannot charge anyone into one with paid plans: hosted pricing page, Stripe checkout, and server-side tier and limit gating. Needs no credential to follow. |
| [`sparkpay-saas-billing`](skills/sparkpay-saas-billing/) | Operating billing once it is live: plans, coupons that actually redeem, customers, MRR, failed payments. Account owner only. |
| [`bottled-transactional-email`](skills/bottled-transactional-email/) | Transactional email from your own domain: sending domains, aliases, campaigns and replies. |
| [`quickpeek-demo-videos`](skills/quickpeek-demo-videos/) | Turns a live URL into a narrated demo video by driving a real browser through the app. |
| [`viralcat-social-scheduling`](skills/viralcat-social-scheduling/) | Scheduling social posts, diagnosing failed publishes, and reading back performance. |
| [`sellular-directory-visibility`](skills/sellular-directory-visibility/) | SEO audits, visibility scoring and directory submission kits. |
| [`ducktax-uk-micro-accounts`](skills/ducktax-uk-micro-accounts/) | UK micro-entity company accounts: validate a balance sheet from raw figures, read Companies House deadlines, categorise bank transactions, and check whether a free filing extension is still available. |

## What these are and are not

`add-payments-to-an-app` stands on its own: a walkthrough that needs nothing
but a browser and the app you already have.

The rest are interfaces to hosted services and are most useful if you use those
services. Each one names the credential it needs at the top, and some are
scoped to the account owner rather than to customers — the skill says so where
that is the case. Nothing in this repository contains a credential.

They are published openly because the operational detail in them is worth
having either way: which call order avoids a silent failure, which settings look
configured but do nothing, which "success" response does not mean the thing
actually worked.

## Licence

MIT. See [LICENSE](LICENSE).
