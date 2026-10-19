# Recipes

## A landing-page demo, 30-60 seconds

The default crawl is fine for a tool whose value is visible on one screen. For
anything with a flow (sign up, configure, see result), script it — a crawler
picks the links it can see, which is rarely the story you want to tell.

Keep one idea per step. A step whose narration runs longer than the action on
screen leaves the viewer watching a static frame while a voice catches up.

## Matching narration to what is on screen

Write the narration first, then size each step to it. The other order — record
first, narrate after — produces either clipped sentences or dead air, and
fixing it means a re-render.

A step needs roughly 0.8s more than its narration to avoid the next line
starting before the current frame settles.

## Sign-in flows

Record against a seeded account, not a real one. Beyond the obvious reason, a
real account has real data in it: other people's names, invoice numbers, email
addresses. Those end up in a file you publish.

If the product must be signed in, authenticate fresh for the run. A reused
session reports itself as signed in long after the server stopped agreeing, and
the recording captures empty skeleton screens at full confidence.

## Aspect ratio

Landscape for a landing page or docs. Vertical only when the destination is a
short-form feed. Re-rendering is cheaper than cropping: a crop moves the
captions out of frame.

## Before you publish

Watch it start to finish with sound on. Specifically check:

- Every captioned step actually speaks. A silent step usually means a bare `&`
  in that caption.
- The outro does not claim a pricing tier the product does not have.
- No real customer data is on screen.
