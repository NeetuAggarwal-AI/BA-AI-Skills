# Story-Type Question Sets

Before writing or completing acceptance criteria, work out **what kind of story** each item is and ask the specific questions that type always needs. Types **combine** — a web form that calls an API uses *Data entry* + *Integration*. Ask the specifics; prompt for real values, don't invent them.

When the request is ambiguous, ask which type applies **before** drafting.

---

### Data entry / CRUD
Mandatory vs optional fields; formats & validation rules; duplicate handling; defaults; create vs edit differences; delete vs soft-delete; who can do it; audit trail.

### Workflow / status change
The full list of states; which transitions are allowed; who/what triggers each; what happens on an invalid or out-of-order transition; reversal/rollback; notifications on change.

### Integration / API / messaging
The contract & payload; success vs error responses; timeout/retry; idempotency & duplicates; ordering; authentication; rate limits; versioning; what the caller sees on failure.

### Notification / alert
Which events trigger it; channel(s); delivery guarantee & acceptable latency; de-duplication; behaviour when delivery fails; opt-out/permission.

### Auth / session / access
Identity source; roles & permissions; session lifetime & expiry; revocation; concurrent sessions/devices; lockout; what an unauthorised user sees.

### Permission / consent
Granted vs denied vs later revoked; the recovery path when it's off; privacy — what is collected, why, retention, and when collection starts/stops.

### Background / async / scheduled / long-running
What starts and stops it; behaviour when interrupted or resumed; resource/cost/time limits; how its health is observed; what happens on failure.

### Reporting / analytics / dashboard
Data source & metric definitions; refresh frequency; filters/drill-down; the empty/no-data state; permissions; export.

### Install / provisioning / setup / migration
Prerequisites & supported versions; environments; distribution method; upgrade over an existing version; data mapping & reconciliation (migration); rollback.

### UI / interaction
The loading / empty / error / success states; responsiveness or supported screens; accessibility; localisation; input validation & feedback.

---

**Rule of thumb:** the type tells you which failure modes are mandatory in the AC. A story that only has a happy path is not finished — pull the relevant edge cases from its type(s) above.
