Vitanet - System Architecture

## 1. High-Level Architecture

Users and hospitals access the Flask application through a browser over HTTPS.
The application is deployed with Gunicorn on DigitalOcean App Platform and
uses the following services:

- SQLite, accessed through Flask-SQLAlchemy, for relational application data.
- DigitalOcean Spaces for optional private medical document storage.
- Gemini API through `google-genai` for lightweight Google Maps verification.
- A dedicated Solana RPC proxy node for escrow payout transactions.

## 2. Component Breakdown

### 2.1 Frontend tier

HTML5, CSS3, and Jinja templates render responsive donor, patient, and
hospital portals, dashboards, forms, and status trackers.

### 2.2 Application tier

Python, Flask, and Gunicorn provide routing, sessions, validation, password
hashing, matching, escrow state transitions, reviews, and service adapters.

### 2.3 Database tier

Flask-SQLAlchemy maps the relational models to SQLite. The database contains
hospital, user, donation matching, review, and restricted-search audit data.
Foreign keys and transactions keep match and payout state changes consistent.
The default local database path is `instance/vitanet.db`.

### 2.4 Object storage tier

DigitalOcean Spaces is an S3-compatible store for optional donor health
certificates. If Spaces is not configured, the application falls back to the
ignored local `static/uploads/` directory.

### 2.5 Blockchain and RPC tier

The Solana SDK sends payout transactions through the configured RPC endpoint.
Hospital verification changes a pending match to verified, invokes the payout
adapter, and marks the match and patient escrow as completed only after a
successful transaction response.

### 2.6 Artificial intelligence tier

The Gemini API receives hospital registration details as untrusted data and
returns a preliminary risk report and verification flag. The report is stored
with the hospital record for later review.

## 3. Core Data Flows

### Workflow A: Hospital registration and AI vetting

1. A hospital submits its name, license, email, and location.
2. Flask validates the request and calls Gemini.
3. SQLAlchemy stores the hospital, generated four-digit ID, AI report, and
   verification state in SQLite.

### Workflow B: Patient deposit and matching

1. A patient submits an organ requirement, urgency, wallet, and deposit.
2. The patient deposit is recorded as `Locked in Escrow`.
3. The matching algorithm checks organ type, blood compatibility, urgency,
   active donor status, wallet availability, and proximity scoring.
4. A compatible pair is stored in `donations_matching` with `Pending` status.

### Workflow C: Verification and payout

1. A verified hospital reviews its pending queue.
2. Hospital verification invokes the configured Solana RPC payout adapter.
3. On success, the match becomes `Completed`, the transaction hash is saved,
   and the patient deposit becomes `Released to Donor`.
