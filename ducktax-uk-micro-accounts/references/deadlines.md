# Deadlines and the extension window

## The thing most people miss

An extension to the filing deadline is **free**, takes about fifteen minutes,
and must be applied for BEFORE the deadline passes. The option disappears on
the day the penalty starts. So when someone surfaces a deadline that is close,
`check_filing_extension` comes before any work on the figures — the figures will
still be there tomorrow; the extension will not.

It returns a ranked next action. `draft_extension_reason` helps write the
application. **Neither submits anything.** A director makes the application on
the registrar's own service.

## Late filing

`late_filing_penalty` gives the figure for how late a set is. Penalties for
first-year lateness and repeat lateness differ, and the amount doubles if the
previous year was also late — worth stating explicitly, because the jump
surprises people.

## Reminders

`track_company_deadlines` needs only a company number — no figures, no saved
filing. It sends reminders on a ladder of days before the deadline.

Which channels it can actually use depends on how the server is deployed, and
it may be none. The tool's reply reports what will really be sent: relay that
rather than promising an email that may never arrive. Only offer it for a
company that has not been saved, since saved filings are watched anyway.

## Dormant companies

`dormant_figures` produces the minimal set for a company that has not traded.
`check_filing_extension` still applies — a dormant company files on the same
clock as a trading one, and people routinely assume it does not.
