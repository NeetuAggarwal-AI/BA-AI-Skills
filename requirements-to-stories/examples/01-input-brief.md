# Example input — requirements brief (fictional)

> This is a **fictional** requirements brief for a made-up company, "Northwind Utilities", used only to demonstrate the skill. It is not based on any real client or project. It is deliberately a bit rough and incomplete — like a real BRD — so the analysis step has something to work on.

---

## Northwind Utilities — Customer Self-Service Portal (v1)

### Purpose
Northwind Utilities wants a self-service web portal so residential customers can manage their account online instead of calling the contact centre. The goal is to cut inbound "check my balance" and "when is my bill due" calls.

### In scope for v1
- Customers can register and log in.
- Customers can view their current balance and their most recent bill.
- Customers can pay their bill by card.
- Customers can update their contact details (email, phone, mailing address).

### Notes captured from the workshop
- The billing system ("BillMaster") is the source of truth for balances and bills. The portal reads from it.
- Payments go through the existing payment gateway ("PayLink"). Finance said "payments must be reliable" and mentioned they've had duplicate-charge complaints in the call centre before.
- Contact details are held in the CRM, not BillMaster.
- Someone asked what happens if BillMaster is down. No decision was recorded.
- Accessibility was mentioned ("needs to meet our accessibility policy") but the policy wasn't shared.
- There is a mention of "business customers later" — out of scope for v1.
- No decision on how customers who don't have an online account yet get registered (self-register vs invited).

### Success measure (from the sponsor)
"Reduce balance/bill-due calls to the contact centre." No baseline number was given in the workshop.
