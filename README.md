# Vitanet

Vitanet is a Flask organ-donation platform backed by SQLite and
Flask-SQLAlchemy. It supports donor and patient registration, protected
hospital workflows, compatibility matching, Gemini-assisted hospital
verification, DigitalOcean Spaces uploads, and Solana escrow payout handling.

## Technology

- Flask and Flask-SQLAlchemy
- SQLite database at `instance/vitanet.db`
- `python-dotenv` for local environment loading
- Gemini API for hospital verification
- DigitalOcean Spaces through `boto3` for document storage
- Solana RPC proxy integration for escrow operations
- Gunicorn for production deployment

## Run locally

Use Python 3.11 or newer.

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install -r requirements.txt
```

Create a local file named `.env` in the project root. Do not commit it.
At minimum, configure a strong `SECRET_KEY`:

```dotenv
SECRET_KEY=replace-with-a-long-random-value
FLASK_DEBUG=true
```

Optional integrations can be configured when their workflows are needed:

```dotenv
GEMINI_API_KEY=your-gemini-key
GEMINI_MODEL=gemini-2.5-flash

DO_SPACES_ENDPOINT_URL=https://nyc3.digitaloceanspaces.com
DO_SPACES_REGION=nyc3
DO_SPACES_BUCKET=your-space-name
DO_SPACES_KEY=your-spaces-key
DO_SPACES_SECRET=your-spaces-secret

SOLANA_RPC_URL=https://your-rpc-proxy-url
SOLANA_NETWORK=devnet
SOLANA_COMMITMENT=confirmed
SOLANA_ESCROW_PRIVATE_KEY=your-encrypted-or-managed-secret
```

If DigitalOcean Spaces is not configured, uploaded donor and hospital
documents use the local `static/uploads/` fallback. The SQLite database and
required profile columns are created automatically on startup.

Start the development server:

```powershell
python app.py
```

Open <http://127.0.0.1:5000>. The default host and port can be changed with
`FLASK_HOST` and `FLASK_PORT`.

## Main workflows

- Donors register, log in with their generated donor ID, view status, cancel
  an active registration, connect a Solana wallet, edit health details, upload
  an optional medical certificate, and change their password.
- Patients register their required organ, blood group, urgency, wallet, and
  escrow deposit state.
- Hospitals register with a generated four-digit ID, log in, update their
  profile, run verification, search for compatible organs, review matches, and
  verify completed operations for payout processing.
- The matching workflow checks organ type and blood compatibility, considers
  urgency, and stores pending matches in `donations_matching`.

## Important routes

| Route | Purpose |
| --- | --- |
| `/` | Landing page |
| `/donors/register` | Donor registration |
| `/donors/login` | Donor login |
| `/donors/dashboard` | Donor status, wallet, and password controls |
| `/donors/profile` | Donor profile and clinical details |
| `/patients/register` | Patient registration |
| `/hospitals/register` | Hospital registration |
| `/hospitals/login` | Hospital admin login |
| `/hospitals/<hospital_id>/dashboard` | Hospital dashboard and matching actions |
| `/find-organ` | Verified-hospital organ search |
| `/health` | Application and database health check |

JSON endpoints are available under `/api/` and are implemented in `app.py`.

## DigitalOcean App Platform

Connect the repository to DigitalOcean App Platform, configure the required
environment variables in the App Platform settings, and keep private keys and
service credentials encrypted. The root `Procfile` contains:

```text
web: gunicorn app:app
```

For production, set `FLASK_DEBUG=false`, use a strong `SECRET_KEY`, configure
the Solana RPC proxy, and use persistent external storage for uploaded
documents. Never commit `.env`, API keys, wallet private keys, or local upload
files.
