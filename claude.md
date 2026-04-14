# Cahootz

See `[product-considerations.md](./product-considerations.md)` for open product questions and decisions to revisit.

## File Structure


| File                        | Purpose                                                       |
| --------------------------- | ------------------------------------------------------------- |
| `index.html`                | The entire prototype                                          |
| `product-considerations.md` | Running list of open product questions / decisions to revisit |


Inside the file: `<style>` block, then `<body>` markup (app shell + variant `<template>`s + roster modal), then `<script>`:

- `setupShell()` — sidebar collapse + mobile-nav handlers, bound **once** on page load (extracted from `init()` so re-running `init()` per variant doesn't double-bind shell handlers).
- `init()` — form-only handlers (date pickers, role chips, submit, summary). Safe to re-run; expects the active variant's form already cloned into `#variant-host`.
- `loadVariant(id)` — clones `<template id="tpl-{id}">` into `<div id="variant-host">`, then calls `init()`.
- `setupDevMenu()` — wires the floating circle trigger + dropdown.
- Roster helpers (Supabase, `AVATARS` bank, `wireRoster`).
- `bootApp()` runs on DOMContentLoaded: `setupShell` → `setupDevMenu` → `loadVariant('opt01')` → `wireRoster`.

## Variants

The form lives in 4 `<template>` elements (`tpl-original`, `tpl-opt01`, `tpl-opt02`, `tpl-opt03`), all initially identical. The floating dev-menu circle (bottom-right) switches between them. **Opt 01 is the landing default** (`DEFAULT_VARIANT` constant). Edit each template independently as variants diverge — `init()` handles the cloned DOM the same way regardless of which variant is active.

- **Original** — baseline form, untouched reference.
- **Opt 01** — adds "Select Reflexer(s)" field that opens the **Select Reflexer(s) modal** (`#prefs-overlay`). Modal has Favorites + Approved filter chips, search, lazy-loaded list, Cancel + Save footer. State: `prefsState` with `committed` (saved) vs `draft` (in-modal) sets.
- **Opt 02 / Opt 03** — TBD.

## Brand & Prefs Pool

`var BRAND = 'Ralph Lauren Factory Store'` at the top of the script.

**Base pool:** the Select Reflexer(s) modal only shows the 96 Austin workers whose `brands_worked` includes `BRAND` (via `buildBrandPool()`). This is NOT all 720 Austin workers.

**Chip tagging is hardcoded by index position** — the DB `favorited_by_brands` field is not used because real favorites fully overlap with approved in the data. Instead, `buildBrandPool()` sorts the 96 alphabetically and tags them:
- First `FAVORITES_COUNT` (26) → tagged `'favorite'`
- Next `APPROVED_COUNT` (43) → tagged `'approved'`
- Remaining (27) → untagged, still in base pool

**Chip behavior (additive):**
- No chips → all 96
- Favorites → 26
- Approved → 43
- Both → 26 + 43 = 69

Each row shows a "Favorite" (amber) or "Approved" (green) tag next to the name. To change counts, update `FAVORITES_COUNT` / `APPROVED_COUNT` constants.

## Dev Server

```bash
npm run dev    # live-server on port 4173, entry index.html
```

## Deploy

- Frontend-only prototype for Reflex (retail labor marketplace). Single HTML file, no build step, no framework.
- Hosted on Vercel (project name: `cahootz`) at `flex-request.weavrk.com`
- DNS: GoDaddy CNAME `flex-request` → `cname.vercel-dns.com` (matches the matchpoint setup)
- Auto-deploys from git on push
- Static — no build command. Vercel serves `index.html` at `/` directly.

## Supabase (read-only)

- Project: `kxfbismfpmjwvemfznvm` (shared with matchpoint)
- Client: `@supabase/supabase-js@2` via CDN, initialized with the **publishable** key (legacy term: anon). Key is hard-coded in the HTML — safe because RLS is enabled with a SELECT-only `anon` policy.
- Table: `**cahootz_workers`** (NOT the original `workers` table — that one is in use by another app, do not touch)
- Roster modal opens from any `.js-open-roster` element (left nav "Reflexers" + mobile bottom nav). Cached after first fetch.
- Roster is filtered to **Austin only** (`~720 workers`), ordered by name. Do not remove the `.eq('market', 'Austin')` filter without reason.
- Avatars come from the local gendered bank in `images/avatars/{male,female}/`, picked deterministically by hashing `worker.id`. Keep `AVATARS` arrays in sync with files on disk.
- Names display as **First L.** (e.g. "Ryan A.") via `shortName()`. Use it anywhere worker names are surfaced in UI.

