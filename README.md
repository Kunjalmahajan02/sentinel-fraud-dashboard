# Sentinel — fraud intelligence dashboard

A React + TypeScript dashboard prototype for monitoring UPI payment fraud. The demo workspace is seeded with sample transactions and simulates incoming events every 22 seconds. It includes transaction risk charts, fraud alerts, AI-assisted summaries, filtering/search, review actions, and CSV export.

## Run locally

```bash
npm install
npm run dev
```

Without environment variables, the sign-in screen opens a sample-only demo workspace. Demo events are kept in browser memory and are not persisted.

## Secure authentication and realtime data

1. Create a Supabase project and copy its Project URL and anon/public key.
2. In Supabase SQL Editor, run [`supabase/schema.sql`](./supabase/schema.sql). This creates a per-user transactions table, enables row-level security, and adds table changes to Realtime.
3. In the project root, copy `.env.example` to `.env.local`, then set `VITE_SUPABASE_URL` and `VITE_SUPABASE_ANON_KEY`.
4. Create analyst accounts in Supabase Authentication. The app uses Supabase password authentication and loads only rows allowed by the transaction table's RLS policies. New demo events can be inserted and updates are received over Supabase Realtime.

Do not use demo mode or the sample metrics for real payment decisions. Configure Supabase before storing real transaction records. The Supabase anon key is intended for browser use; access control is enforced by Supabase Auth and RLS.

## AI insights

Set `OPENAI_API_KEY` as a server-only environment variable in Vercel (never `VITE_OPENAI_API_KEY`). Optionally set `OPENAI_MODEL` (defaults to `gpt-4o-mini`). The `/api/insights` Vercel function validates the Supabase access token before sending a minimized transaction summary to the AI provider. The dashboard falls back to local heuristics if AI is not configured or unavailable.

## Deploy to Vercel

- Import the GitHub repository into Vercel and keep the default Vite build settings (`npm run build`, output `dist`).
- Add `VITE_SUPABASE_URL`, `VITE_SUPABASE_ANON_KEY`, `OPENAI_API_KEY`, and optionally `OPENAI_MODEL` to the Vercel project's environment variables. Rebuild after changing Vite-prefixed variables.
- Add your Vercel production URL to the Supabase project's Authentication URL configuration.

The app uses Vercel's `/api` directory for the server-side insights function and `vercel.json` for client-side route fallback.
