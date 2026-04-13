# Cahootz

Frontend-only prototype for Reflex (retail labor marketplace). Single HTML file, no build step, no framework.

## File Structure

| File | Purpose |
|------|---------|
| `cahootz.html` | The entire prototype (~1474 lines) |

Within the HTML:
- **Lines 10–625** — `<style>` block (design tokens, all CSS)
- **Lines 628–936** — `<body>` markup (nav, form, success screen, sidebar)
- **Lines 937–1472** — `<script>` block (single `init()` function, all JS)

## Assets

- `fonts/CircularSTD/*.ttf` — Circular STD font (8 weights)
- `images/brand/nav-background.*` — Nav background pattern
- `assets/r-logo.svg` — Reflex logo

## Dev Server

```bash
npm run dev    # live-server on port 4173
```

Entry file is `cahootz.html`.
