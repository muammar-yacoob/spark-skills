# Spark Skills

Agent skills for [Claude](https://claude.com/claude-code) and other MCP-capable
agents. Seven of them: one standalone tool, and six that put a usable interface
over an MCP server.

## The skills

| Skill | What it does |
|---|---|
| [`graphify`](graphify/) | Turns a folder of code, documents, papers or media into a navigable knowledge graph, with community detection and query, path and explain tools. Outputs interactive HTML, GraphRAG-ready JSON and a plain-language report. |
| [`ducktax-uk-micro-accounts`](ducktax-uk-micro-accounts/) | UK micro-entity company accounts: validate a balance sheet from raw figures, read Companies House deadlines, categorise bank transactions, and check whether a free filing extension is still available. |
| [`sparkpay-saas-billing`](sparkpay-saas-billing/) | Subscription billing: plans, coupons that actually redeem, customers, MRR and failed payments. |
| [`quickpeek-demo-videos`](quickpeek-demo-videos/) | Turns a live URL into a narrated demo video by driving a real browser through the app. |
| [`bottled-transactional-email`](bottled-transactional-email/) | Transactional email from your own domain: sending domains, aliases, campaigns and replies. |
| [`sellular-directory-visibility`](sellular-directory-visibility/) | SEO audits, visibility scoring and directory submission kits. |
| [`viralcat-social-scheduling`](viralcat-social-scheduling/) | Scheduling social posts, diagnosing failed publishes, and reading back performance. |

## Installing

Each directory is a self-contained skill with `SKILL.md` at its root. Copy the
one you want into your agent's skills directory:

```bash
cp -r ducktax-uk-micro-accounts ~/.claude/skills/
```

The six MCP skills each need their server added to your agent's MCP
configuration. The endpoint and the setup steps are in each skill's `SKILL.md`.
Those are public API surfaces: you bring your own key where one is needed, and
nothing in this repository contains a credential.

## What these are and are not

`graphify` is a tool in its own right.

The other six are interfaces to hosted services, and are most useful if you use
those services. They are published openly because the operational detail in them
is worth having either way: which call order avoids a silent failure, which
settings look configured but do nothing, which "success" response does not mean
the thing actually worked.

## Licence

MIT. See [LICENSE](LICENSE).
