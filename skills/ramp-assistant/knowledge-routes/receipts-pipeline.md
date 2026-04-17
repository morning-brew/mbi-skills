# Ramp's receipt-forwarding pipeline

## What happens when you forward to receipts@ramp.com

1. Ramp receives the forwarded email at `receipts@ramp.com`.
2. Ramp's pipeline matches the sender address to the employee's Ramp account (must match the Ramp email — MBI employees use their `@morningbrew.com` address).
3. Ramp's OCR + classifier extracts vendor, amount, date, and tax from the receipt.
4. Ramp matches the parsed receipt to an open transaction on the employee's card.
5. If matched, the receipt is attached automatically and the transaction is marked compliant.
6. If not matched within ~24–48 hours, Ramp holds the receipt as "unmatched" for manual linking in-app.

## Troubleshooting forwarded receipts

- **Nothing happened after forwarding**: check the employee's spam/filter rules aren't intercepting the Ramp confirmation email. Ramp pings `receipt received` back within a few minutes.
- **Receipt attached to the wrong transaction**: the employee can detach and re-link in the Ramp app.
- **"Unmatched receipts" piling up**: often means the receipt didn't include the transaction amount clearly. Forward again with the amount in the subject line, or link manually.
- **Forwarding from a different email address than the employee's Ramp account**: Ramp won't match. Always use the employee's `@morningbrew.com` address — Gmail's default from-address for MBI employees.

## Where to look for more

- **Ramp Help Center** → "Forward receipts to receipts@ramp.com" (Ramp's public docs on this feature).
- **Notion** → Finance handbook → "Receipts & reconciliation".
- In-app: open any transaction on Ramp, "Add receipt" menu shows the forwarding address + tips.
