# Flight Management System — Starter Project

This is the starting skeleton for your capstone. It matches the architecture
from your feature list:

- **backend/** = FastAPI. This is the ONLY thing allowed to write live
  bookings/admin data. It talks directly to Supabase Postgres.
- **n8n/** = importable workflow(s) for the scheduled/background jobs
  (reminders, waitlist promotion, fraud scans, etc). n8n also talks
  directly to Postgres — it does NOT go through FastAPI.

## Folder structure

```
flight-management-system/
├── backend/
│   ├── app/
│   │   ├── main.py            <- starts the FastAPI app
│   │   ├── database.py        <- connects to Supabase Postgres
│   │   ├── models.py          <- database tables (SQLAlchemy)
│   │   ├── schemas.py         <- request/response shapes (Pydantic)
│   │   └── routers/
│   │       ├── flights.py     <- admin: create/edit/cancel flights
│   │       └── bookings.py    <- live: search + book seats
│   ├── requirements.txt
│   └── .env.example
└── n8n/
    └── checkin_reminder_workflow.json   <- import this into n8n
```

## Step 1 — Get your Supabase Postgres connection string

1. Go to your Supabase project → **Project Settings → Database**
2. Copy the "Connection string" (URI format, choose "Transaction" mode
   pooler if it's offered)
3. It looks like:
   `postgresql://postgres:[YOUR-PASSWORD]@aws-0-xxxx.pooler.supabase.com:6543/postgres`

## Step 2 — Set up the backend

Open a terminal INSIDE the `backend` folder in VS Code, then run:

```bash
python -m venv venv
source venv/bin/activate        # on Windows: venv\Scripts\activate
pip install -r requirements.txt
cp .env.example .env
```

Then open `.env` and paste your real Supabase connection string in.

## Step 3 — Run it

```bash
uvicorn app.main:app --reload
```

Open your browser to **http://127.0.0.1:8000/docs** — you'll see an
interactive page listing every endpoint. This is FastAPI's auto-generated
docs (Swagger UI). This is how you'll test things without building a
frontend yet.

## Step 4 — Import the n8n workflow

1. Open your n8n instance
2. Click **Workflows → Import from File** (or drag the file in)
3. Select `n8n/checkin_reminder_workflow.json`
4. You'll need to add your Postgres credentials inside n8n (Credentials →
   New → Postgres) using the SAME Supabase connection details as above,
   but split into host/port/database/user/password instead of one string.
5. The workflow is currently OFF (inactive) — leave it that way until
   you've built the `flights` and `bookings` tables, or it'll error out
   with "table does not exist."

## What's already built vs. what you build next

**Already working in this starter:**
- Database connection
- Two tables: `flights`, `bookings` (bare minimum columns)
- One admin endpoint: create a flight
- One live endpoint: search flights
- One n8n workflow skeleton: check-in reminder (scheduled, reads Postgres,
  currently just logs — you'll wire in the real Gmail node)

**You build next (in priority order, matching your feature doc):**
1. Booking endpoint with atomic seat decrement (prevents overselling)
2. Cancellation endpoint
3. Waitlist endpoints
4. The rest of your scheduled n8n jobs

Tell me which of those you want to do next and I'll build it the same way.
