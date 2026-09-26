# Vitanet Product Requirements Document

## 1. Project overview

Vitanet is a Flask organ donation and management platform. It combines a
responsive HTML/CSS frontend, a SQLite database accessed through
Flask-SQLAlchemy, DigitalOcean hosting and Spaces storage, Gemini hospital
vetting, and Solana escrow settlement through a dedicated RPC proxy.

## 2. Functional requirements

### 2.1 Hospital registration and AI vetting

Hospitals submit a name, license number, email, location, and optional
credentials. The backend calls Gemini through `google-genai` for a lightweight
Google Maps link check, generates a unique four-digit hospital ID, and stores the
profile, assessment report, and verification state in the SQLite `hospitals`
table.

### 2.2 Donor registration and authentication

Donors submit their name, blood group, organ type, contact details, medical
history, payout wallet, and a password. Passwords are hashed, a unique donor
login ID is generated, and the donor can review status or cancel an active
registration from the donor dashboard. An optional health certificate can be
stored in DigitalOcean Spaces or the local upload fallback.

### 2.3 Patient registration and escrow state

Patients submit their required organ, blood group, urgency (`Emergency`,
`High`, or `Normal`), wallet, and deposit amount. The application records the
deposit as `Locked in Escrow` until a successful hospital verification and
Solana payout.

### 2.4 Smart organ matching

The backend matches active donors and patients using organ compatibility,
blood-group compatibility, patient urgency, wallet availability, and location
scoring. Matches are stored in the `donations_matching` SQLite table with
`Pending` status.

### 2.5 Hospital verification and payout

Verified hospitals see their pending match queue in the dashboard. After an
operation is confirmed, the dashboard action calls the Solana RPC payout
adapter. A successful transaction records the signature, marks the match
`Completed`, and releases the patient escrow state to the donor.

### 2.6 Reviews

A donor or patient may submit one 1-to-5-star review after a completed match.
The hospital dashboard and review page display the aggregated rating.

## 3. Technical requirements

- Backend: Python, Flask, Flask-SQLAlchemy, and Gunicorn.
- Database: SQLite at `instance/vitanet.db` by default.
- Object storage: DigitalOcean Spaces through `boto3`, with a local fallback.
- AI: Gemini API through `google-genai`, using the configured model and
  defaulting to `gemini-2.5-flash` for Google Maps link verification.
- Blockchain: Solana SDK and a configured RPC endpoint.
- Deployment: DigitalOcean App Platform using the root `Procfile`.

## 4. Security and privacy

Credentials and private keys are loaded from `.env` or deployment environment
variables and are never hardcoded. Passwords are stored as hashes. Medical
uploads use validated filenames and extension checks, and local uploads remain
ignored by version control. Sensitive responses use security headers and
database errors do not expose credentials or connection details.
