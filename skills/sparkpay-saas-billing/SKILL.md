---
name: sparkpay-saas-billing
description: Run billing for a SaaS product — inspect plans and pricing, create and manage coupons, look up customers and subscriptions, read revenue and recent payments, and set up a hosted pricing page. Use when someone asks about subscription billing, Stripe plans, discount codes, MRR, churn, failed payments, or why a customer's checkout or subscription looks wrong.
---

# SparkPay: billing for a SaaS product

Connects to the SparkPay MCP server, which sits over Stripe and holds the plan
catalogue, coupons and customer records.

## Setup

```json
{
  "mcpServers": {
    "spark-pay": {
      "type": "http",
      "url": "https://sparkpay.dev/api/mcp/mcp",
      "headers": { "Authorization": "Bearer ${SPARK_PAY_ADMIN_TOKEN}" }
    }
  }
}
```

**This server is for the account owner.** Payment data is admin-only, so every
tool call is gated behind the account's admin bearer; without it the handshake
and the tool list still answer, and the first real call returns
`invalid_token`. There is no per-customer key to request, which is worth
knowing before spending time on the handshake: a tool list that renders is not
evidence of access.

If the goal is to *add* paid plans to an app rather than to operate an account
that already has them, none of this is needed. Use the
`add-payments-to-an-app` skill, which goes through the dashboard and the
integration kit and requires no credential.

## Reading the state

`list_apps` then `list_plans` for the catalogue. `revenue_summary` and
`revenue_by_app` for the headline numbers; `list_recent_payments`,
`list_subscriptions` and `list_recent_cancellations` for what is behind them.
`get_customer` for one account.

## Discounts that actually work

`create_coupon` makes the discount. **A typed code also needs a promotion code**
— a coupon alone is redeemable through the API but a customer typing the code at
checkout will be told it is invalid. Test the redemption path itself; a 200 from
the coupon endpoint is not evidence that anyone can use it.

`set_coupon_listed` controls whether it is shown on the pricing page. A sale
that exists only in the database and never renders is why a campaign can show
zero redemptions while looking correctly configured.

Check `redeem_by` against whatever the marketing says. A code advertised until
December that expires in September is a support ticket per attempt.

## A trap in trials

A `duration: once` coupon is **not** consumed by the zero-value invoice at the
start of a trial: it applies to the first invoice that actually charges. If you
need to settle a question about what a customer will be billed and when, use a
Stripe test clock rather than reasoning about it.
