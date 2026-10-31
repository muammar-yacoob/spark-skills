---
name: add-payments-to-an-app
description: Take an app that works but cannot charge anyone, and give it paid plans — a hosted pricing page, Stripe checkout, and server-side feature gating that knows which tier a user is on. Use when someone wants to add payments, subscriptions, a pricing page, a paywall, a free tier, a trial, or Stripe to an app they have built, or asks how to start charging for it or how to gate a feature behind a plan.
---

# Adding paid plans to an app that already works

The hard part of charging for an app is not the checkout. It is everything
around it: a pricing page that stays in step with Stripe, a webhook that does
not silently miss a renewal, and a reliable answer to "is this user allowed to
do this right now" on every request.

This skill wires that up with [SparkPay](https://sparkpay.dev), which hosts the
pricing page and the Stripe plumbing and hands back a typed kit you drop into
the app. The free tier is one app with a live checkout and no card required, so
the whole path below can be finished before anyone decides to pay for it.

## Before touching any code

Ask which of these the app needs, because the answers change what gets
generated and getting them wrong means downloading the kit twice:

1. **The tiers, and what each one unlocks.** Not prices yet, limits: how many
   of the thing a free user gets versus a paid one. These become machine-
   readable `limits` the kit reads at runtime, so "3 projects on free" has to
   be a number, not a sentence on a marketing page.
2. **Subscription, one-time, or both.**
3. **Push or poll.** Push means Stripe events reach a webhook route in the app
   and it needs a public URL plus a webhook secret. Poll means the app asks
   SparkPay for a user's status and needs neither. **Poll is the right default
   for anything deployed on a preview URL or still running locally**, because a
   webhook cannot reach localhost and the resulting silence looks exactly like
   a bug in the gating code.

## The path

1. Sign in at [sparkpay.dev](https://sparkpay.dev) and create the app. The free
   tier allows one.
2. Define the plans. The dashboard imports a `plans.json`, which is the fastest
   route when the tiers are already decided — one object per tier with its
   name, features, and `limits`. Importing creates the tiers and syncs the
   prices to Stripe; re-importing updates the tiers that already exist. Four
   plans is the maximum.
3. Download the integration kit from the dashboard's **General** tab, picking
   the platform and the push/poll mode decided above.
4. Wire it in, following the rules in the next section.
5. Link the hosted pricing page. It is served at
   `sparkpay.dev/<owner>/<app>` and is the page a checkout starts from, so
   nothing about prices needs rendering in the app itself.

## Wiring the kit in

The kit is five files and it is already the abstraction layer:

| File | What it holds |
|---|---|
| `sparkpay-server.ts` | Server-safe. Status, gating, guards, limits, webhook handler. No React, no CSS |
| `SparkPay.tsx` | Client hooks (`useSparkStatus`, `useCheckout`) and UI (`SparkBadge`, `SparkGate`) |
| `plans.json` | Per-tier name, features and limits. Edit it to adjust copy without re-downloading the kit |
| `sparkpay-kit.css` | Badge, gate wall, promo and toast styles |
| `readme.txt` | The generated API reference for this app's own tiers |

**Import from those two entrypoints directly and do not build anything on top
of them.** The wrapper files an agent reaches for by habit are all already
inside the kit, and each one adds a layer that has to be kept in step with the
tiers for no gain:

- `stripe.ts` re-exporting the kit — the kit is the barrel
- `billing.ts` wrapping status — use `getStatusCached` as-is
- `subscription.ts` adding a cache — the kit caches with a 60-second TTL and an LRU already
- `limits.ts` adding limit helpers — `getLimit`, `hasReachedLimit` and `getTierLimitsCached` exist

Read `readme.txt` before writing any billing logic, because it is generated
against this app's actual tiers and will name the real guard functions.

Worth knowing without reading all of it:

- `getTierLimitsCached(email)` returns `{ tier, limits }` with batched caching. Use it on hot paths rather than calling status and limits separately.
- `revalidateStatus(email)` clears the cache for one user. Call it after a webhook event, otherwise an upgrade appears to take up to a minute to land.
- `createWebhookHandler({...})` wires every Stripe event in one call, in push mode.
- `getPlanFeatures(tier)` returns the features array, which is useful for feeding plan context to an LLM prompt.

The only files worth writing by hand are genuine app-specific side effects:
mirroring tier data into the app's own database on a webhook, or formatting plan
information for a prompt.

Set `NEXT_PUBLIC_SPARK_APP_ID` on Next.js, or `SPARK_APP_ID` elsewhere. Push
mode also needs `SPARK_PAY_WEBHOOK_SECRET`, which the dashboard issues.

## Gate on tiers and limits, never on a price

Gate on the tier or the limit, not on an amount or a Stripe price id. Prices
change for marketing reasons several times a year; tiers and limits are the
thing the product actually means, and they are what the kit exposes. A check
written against a price id is a check that breaks the next time someone runs a
sale.

## What goes wrong

**A plan that renders but cannot be bought.** A tier with no Stripe price id is
a "Coming Soon" placeholder, not a purchasable plan. If a plan is meant to sell,
confirm it has a price id after the sync.

**A discount code that is refused at checkout.** A coupon on its own is
redeemable through the API but a customer *typing* the code needs a promotion
code as well, and without one they are told it is invalid. Test the redemption
path itself; a success response from the coupon endpoint is not evidence that
anyone can use it.

**A `once` discount that survives a trial.** It is not consumed by the
zero-value invoice at the start of a trial — it applies to the first invoice
that actually charges. To settle what a customer will be billed and when, run a
Stripe test clock rather than reasoning it out.

**A free tier that is invisible.** If the app has a free plan, say the word
"free" on the landing page and next to the primary call to action. A free tier
that exists in the catalogue and not in the copy converts at zero, and it looks
identical to a traffic problem.

## Once it is live

Billing becomes an operational job: which coupons redeem, which payments
failed, what the MRR is, why one customer's subscription looks wrong. That is
the `sparkpay-saas-billing` skill, which drives the same account over MCP.
