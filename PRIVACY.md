# Privacy Policy — Spark Skills

Last updated 4 November 2026.

This policy covers the **Spark Skills plugin** itself. It is issued by Spark
Games Ltd, a company registered in England and Wales (company no. 15379140),
contact muammar.yacoob@gmail.com.

## What the plugin collects

Nothing.

Spark Skills is a bundle of seven Markdown instruction files. It declares no
MCP servers, runs no hooks, ships no executable code and contains no
credential. Installing it, or having it loaded in a session, transmits nothing
to Spark Games Ltd or to anyone else. There is no telemetry, no analytics and
no phone-home of any kind.

## What happens when you follow a skill

Six of the skills tell your agent how to connect to a hosted service, and name
the credential that service needs. You connect that service yourself. Once you
do, your agent sends requests to that host under **your own account with it**,
and that service's own privacy policy governs what it holds and for how long:

| Skill | Host | Its privacy policy |
|---|---|---|
| `add-payments-to-an-app` | `sparkpay.dev` | https://sparkpay.dev/privacy |
| `sparkpay-saas-billing` | `sparkpay.dev` | https://sparkpay.dev/privacy |
| `bottled-transactional-email` | `bottled.email` | https://bottled.email/privacy |
| `sellular-directory-visibility` | `sellular.online` | https://sellular.online/privacy |
| `viralcat-social-scheduling` | `viral-cat.com` | https://viral-cat.com/privacy-policy |
| `ducktax-uk-micro-accounts` | `ducktax.com` | https://ducktax.com/privacy |
| `quickpeek-demo-videos` | runs on your own machine | https://quickpeek.co/privacy |

Data sent to those hosts can include personal data, because that is what the
services are for: contact lists and campaign recipients in Bottled, customer
and subscription records in SparkPay. It is read and written under your
account and **retained there until you delete it**, per each policy above.
`quickpeek-demo-videos` runs a local server over stdio on your own machine; it
reaches `quickpeek.co` only to check your account and plan.

The plugin sends data to no host other than those listed.

## Credentials

Each skill names the credential it needs and expects you to supply it through
your own agent configuration. `sparkpay-saas-billing` references
`SPARK_PAY_ADMIN_TOKEN`, which is SparkPay's own credential for SparkPay's own
API — it is not read, copied or forwarded anywhere else. No credential is
stored in this repository or sent to Spark Games Ltd by the plugin.

## Your rights

For data held by any of the services above, exercise your rights under UK GDPR
with that service, using the contact in its own policy. For the plugin itself
there is no data to access, correct or erase, because none is collected.

## Changes

Material changes are published to this file in the repository, with the date
above updated. The repository's commit history is the record of what changed
and when.
