# Coupons, trials and the things that fail silently

## A coupon is not a code

`create_coupon` creates the discount. A customer **typing** a code at checkout
needs a promotion code attached to it. Without one, the coupon is redeemable
through the API and invalid at the till — and nothing errors at creation time.

So: create it, attach the promotion code, then actually complete a checkout
with the code typed in. A 200 from the coupon endpoint proves the record
exists, not that anyone can use it.

## A sale nobody can see

`set_coupon_listed` controls whether a discount renders on the pricing page. A
campaign can be perfectly configured, advertised everywhere, and show zero
redemptions because the page never displayed it. If redemptions are flat,
check that the page renders the offer before rewriting the copy.

## Expiry dates that disagree

Check the coupon's `redeem_by` against whatever the site and the emails claim.
A code advertised until December that expires in September produces one support
ticket per attempt, all of them from people who were ready to pay.

## Trials and once-coupons

A `duration: once` coupon is **not** spent by the zero-value invoice that opens
a trial. It applies to the first invoice that actually charges. This trips
people up in both directions — assuming the discount was wasted, or assuming it
will apply later when it already has.

Do not reason about this from first principles. Use a Stripe test clock and
watch the invoices.

## Reading revenue honestly

`revenue_summary` counts what the platform holds, test rows included. Before
reporting a number to anyone, separate real customers from test ones. A
headline MRR that includes your own test subscriptions is worse than no number.
