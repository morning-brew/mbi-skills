# MBI time off (PTO)

## **Critical: PTO starts in BambooHR, not Google Calendar**

BambooHR is MBI's HRIS and the system of record for all time-off requests. When an employee submits a PTO request in BambooHR and a manager approves it, BambooHR automatically creates a corresponding out-of-office event in the employee's Google Calendar.

**Never create a PTO event directly in Google Calendar.** Doing so:

- Breaks the approval workflow (manager never sees the request)
- Creates reporting drift (PTO balance in BambooHR is wrong)
- Doesn't trigger the auto-responder and OOO blocks BambooHR sets up

## Where to look

- **BambooHR** at bamboohr.com — login via Okta. Request time off here.
- Notion → People handbook → PTO policy for the policy (accrual, blackouts, rollover).
- Slack #people channel for quick Q&A on borderline cases.

## When to use

- User says "book me off next week" or "schedule vacation" — redirect to BambooHR.
- User asks "do I have PTO available?" — point them at BambooHR (we don't have API access to their balance here).
- User wants to see their own upcoming OOO — check their Google Calendar for events titled `OOO`, `OOTO`, or `PTO` (those are the conventions BambooHR uses when it writes the event).

## How to act

When the user tries to create PTO via Calendar:

> "PTO requests start in BambooHR — it'll auto-sync to your Calendar once approved. That way your manager gets the ping and your balance stays accurate. Want me to open BambooHR for you?"

Offer to open BambooHR via the system browser rather than trying to do it through the calendar.

**Exception**: one-off "working remote" or "focus block" events are fine to create in Calendar directly — they don't involve PTO and don't need BambooHR.
