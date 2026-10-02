# Supabase deployment notes

This project is an academic prototype. Configure the backend before presenting it as a connected system.

## 1. Create the database

Run the SQL in `supabase/migrations/20260224223000_temp_schema.sql` in the Supabase SQL editor, or use the Supabase CLI migration workflow.

> The current migration contains development-oriented RLS policies. Do not use those policies for a production service containing real complainant data. A production version should move sensitive reads/writes behind authenticated Edge Functions and least-privilege policies.

## 2. Configure the frontend

Copy `.env.example` to `.env` and set:

```text
VITE_BACKEND_PROVIDER=supabase
VITE_SUPABASE_URL=https://YOUR_PROJECT_REF.supabase.co
VITE_SUPABASE_ANON_KEY=YOUR_SUPABASE_ANON_KEY
VITE_SUPABASE_FUNCTIONS_URL=https://YOUR_PROJECT_REF.supabase.co/functions/v1
VITE_DEMO_MODE=false
```

Never put a Supabase service-role key in the Vite frontend.

## 3. Deploy Edge Functions

The project contains:

- `send-otp`
- `verify-otp`
- `notify-complaint`
- `send-whatsapp`

Configure provider secrets in Supabase before deploying notification functions. `notify-complaint` requires `RESEND_API_KEY` and `COMPLAINT_NOTIFICATION_EMAIL`. SMS requires the Twilio secrets used by `send-otp`.

## 4. Evidence storage

Durable evidence storage is intentionally not faked by the frontend. Before enabling evidence uploads, create a private Supabase Storage bucket and implement authenticated upload/download policies. The current frontend computes SHA-256 locally but does not claim that a browser object URL is durable evidence storage.

## 5. Demo mode

For a classroom demonstration only:

```text
VITE_DEMO_MODE=true
```

Demo mode uses browser-local sample records. Label any screenshots or presentation as demonstration data; do not describe the sample records as real complaints.
