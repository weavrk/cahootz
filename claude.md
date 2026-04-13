# Cahootz

Frontend-only prototype for Reflex (retail labor marketplace). Single HTML file, no build step, no framework.

## File Structure

| File | Purpose |
|------|---------|
| `index.html` | The entire prototype |

Inside the file: `<style>` block, then `<body>` markup, then `<script>` with a single `init()` function plus the roster modal logic appended after `init`.

## Dev Server

```bash
npm run dev    # live-server on port 4173, entry index.html
```

## Deploy

- Hosted on Vercel (project name: `cahootz`) at `flex-request.weavrk.com`
- DNS: GoDaddy CNAME `flex-request` → `cname.vercel-dns.com` (matches the matchpoint setup)
- Auto-deploys from git on push
- Static — no build command. Vercel serves `index.html` at `/` directly.

## Supabase (read-only)

- Project: `kxfbismfpmjwvemfznvm` (shared with matchpoint)
- Client: `@supabase/supabase-js@2` via CDN, initialized with the **publishable** key (legacy term: anon). Key is hard-coded in the HTML — safe because RLS is enabled with a SELECT-only `anon` policy.
- Table: **`cahootz_workers`** (NOT the original `workers` table — that one is in use by another app, do not touch)
- Roster modal opens from any `.js-open-roster` element (left nav "Reflexers" + mobile bottom nav). Cached after first fetch.
- `cahootz_workers.photo` column drives avatars; falls back to initials if missing/broken.
