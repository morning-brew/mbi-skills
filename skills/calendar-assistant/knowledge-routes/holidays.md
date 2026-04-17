# MBI company holidays

## Where to look

- **BambooHR** → Time Off → Company Calendar. This is the source of truth; HR maintains it.
- **Notion** → People handbook → Company holidays. Usually mirrors BambooHR with links to office-closed details.
- **Google Calendar** → a shared "Morning Brew Holidays" calendar that most employees subscribe to. If the user hasn't, offer to add it via `gws calendar calendarList insert`.

## When to use

- User asks "when is the company closed next?" or "do we work on X day?"
- User is trying to book a meeting — check whether the proposed date falls on a company holiday before confirming.
- User is planning PTO — company holidays are separate from their PTO balance and don't need to be requested.

## How to act

- Read BambooHR or Notion at query time — do NOT cache a holiday list. The schedule shifts yearly (floating holidays, new observances).
- When a meeting request overlaps a company holiday, surface it explicitly: *"That's a company holiday — office is closed. Want to try the next business day?"*
- Federal vs company holidays differ. MBI observes a specific set; don't assume every federal holiday is observed.
