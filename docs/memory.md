# Vitanet Project Memory

## 1. Project identity

Vitanet is a secure organ donation platform evolved from the Organise
prototype. The current implementation uses Flask, Jinja templates,
Flask-SQLAlchemy with SQLite, DigitalOcean infrastructure, Gemini AI, and
Solana escrow settlement.

## 2. Current technology decisions

- Frontend: HTML5, CSS3, Jinja templates, and responsive custom CSS.
- Backend: Python Flask served locally or by Gunicorn.
- Database: SQLite through Flask-SQLAlchemy; default path
  `instance/vitanet.db`.
- Object storage: DigitalOcean Spaces through `boto3`, with a local fallback
  for development.
- Hosting: DigitalOcean App Platform.
- Blockchain: Solana through the configured RPC proxy endpoint.
- AI: Gemini API through `google-genai`, default model `gemini-2.5-flash`,
  focused on Google Maps link verification.

## 3. Core workflows

1. Hospitals register and receive a unique four-digit ID after the Gemini
   preliminary assessment is saved.
2. Donors register with hashed credentials and may log in, view status, or
   cancel their active registration.
3. Patients register an organ requirement and escrow deposit state.
4. The matching algorithm pairs active donors and patients using blood group,
   organ type, urgency, wallet availability, and location scoring.
5. A verified hospital reviews a pending match and triggers the Solana payout.
6. Successful matches become completed and eligible donors or patients can
   review the hospital.

## 4. Repository guidance

`docs/prd.md` contains product requirements, `docs/architecture.md` explains
the system design, `docs/rules.md` contains coding and security standards,
`docs/design.md` contains the visual system, and `docs/tasks.md` contains the
implementation roadmap.
