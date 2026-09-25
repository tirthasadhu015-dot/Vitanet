# Vitanet

Vitanet is a Flask-based organ donation platform that combines MongoDB,
DigitalOcean Spaces, Gemini hospital vetting, and Solana escrow settlement.

## Run locally

Prerequisites: Python 3.11+, MongoDB, and credentials for any external
services you want to use.

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install -r requirements.txt
Copy-Item .env.example .env
python app.py
```

Open `http://127.0.0.1:5000`. The local `.env` file is loaded by
`python-dotenv`; all application settings are read from `os.environ`.

For local development, set at least `SECRET_KEY` and `MONGO_URI`. Gemini,
DigitalOcean Spaces, and Solana settings are required only for their
respective workflows. Never commit `.env`, credentials, or private keys.

## DigitalOcean App Platform

Connect the repository to App Platform and set the variables from
`.env.example` in the App Platform Environment Variables panel. Store
`SOLANA_ESCROW_PRIVATE_KEY` as an encrypted secret. App Platform uses the
root `Procfile` command:

```text
web: gunicorn app:app
```

Use the DigitalOcean Managed MongoDB URI, private Spaces credentials, and the
dedicated Solana RPC proxy URL in production. Keep `FLASK_DEBUG=false`.

## Main routes

- `/` — landing page
- `/donors/register` — donor registration and certificate upload
- `/patients/register` — patient registration and escrow state setup
- `/hospitals/register` — hospital registration and Gemini assessment
- `/hospitals/<hospital_id>/dashboard` — hospital match verification dashboard
- `/hospitals/<hospital_id>/reviews` — hospital reviews

The API endpoints are defined in `app.py` under `/api/`.
