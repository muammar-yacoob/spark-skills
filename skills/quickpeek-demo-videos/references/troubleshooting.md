# When a render comes back wrong

## One step is silent, the rest narrate

A bare `&` in that step's caption. Speech synthesis reads it as the start of an
SSML entity and the step produces no audio while everything else works. Write
"and".

To find it in a finished video: count the generated audio clips against the
number of captioned steps. A mismatch names the step.

## The video is shorter than expected

Usually a dead-air pass plus a tempo setting compounding. Read the burned-in
captions and confirm all the content is present before concluding it was
truncated — compressed and truncated look identical in a duration readout and
are not the same problem.

## Black or skeleton screens

A stale authenticated session. The recorder reports `signedIn: true` because the
cookie is still present; the server has already rejected it. Re-authenticate
rather than re-rendering.

## The outro invents pricing

The outro template can assert a free tier by default. On a demo of a product
without one — especially someone else's product — that is a fabricated claim in
the last frame. Set it explicitly for outreach work.

## It recorded the wrong thing

The crawler follows what it can see. If the flow needs a specific route, give
it the step list rather than hoping. This is the usual cause of a demo that
shows a marketing page instead of the product.
