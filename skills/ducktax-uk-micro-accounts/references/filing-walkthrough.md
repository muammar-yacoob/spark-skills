# A filing, start to finish

## 1. Find the company

`search_companies` by name, then `lookup_company` by number. The lookup carries
the accounting reference date and the filing deadlines, which is what everything
else hangs off.

## 2. Pull last year

`get_prior_year_figures`. Micro-entity accounts show comparatives, and a set
filed without them, or with comparatives that do not match what the registrar
already holds, gets rejected.

## 3. Enter the figures

Major units. Pounds, not pence, with decimals where needed. This is the single
most common error and it is silent: pence produce a balance sheet a hundred
times too large and every subsequent check passes against the wrong number.

## 4. Check after every change

`check_accounts` is cheap. Running it once at the end tells you the sheet does
not balance; running it after each change tells you which change broke it.

## 5. Read it back

`explain_accounts` turns the validated set into plain English. Use it before
handing anything to a client or an accountant — if the summary describes a
company you do not recognise, a figure is in the wrong place.

## 6. Keep it

`save_filing` (needs a token). Saved filings are watched for deadlines
automatically.

## What this does not do

It does not file with Companies House or HMRC, and nothing built on it should
suggest otherwise. The output is checked numbers to hand to a filing product or
an accountant. It is not accounting, tax or legal advice, and saying so plainly
is better than hedging.
