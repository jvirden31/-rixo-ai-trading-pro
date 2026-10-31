# RIXO AI Trading PRO — Hosting & iPhone Install Instructions

## What this is
A simulation-only mobile trading dashboard. All prices are sample data
generated in the browser. There is no live market-data provider, no AI
service, no broker connection, and no real trading.

## Files in this package
- `index.html` — the complete app (dashboard, markets, scanner, risk calculator, journal)
- `manifest.json` — PWA manifest
- `service-worker.js` — offline app-shell cache
- `icons/icon-192.png` — 192 × 192 app icon
- `icons/icon-512.png` — 512 × 512 app icon
- `UPGRADE_NOTES.md` — Phase 3 upgrade notes
- `PHASE4_NOTES.md` — Phase 4 testing, fixes, and upgrades notes
- `PHASE5_NOTES.md` — Phase 5 development notes
- `LIVE_DATA_SETUP.md` — secure live market-data setup (not yet connected)
- `BACKEND_SETUP.md` — secure backend + market-data setup (Phase 6, not yet connected)
- `AI_SETUP.md` — real AI backend setup (Phase 6, not yet connected)
- `PHASE6_NOTES.md` — Phase 6 development notes
- `server/proxy.js` — backend proxy code (deploy separately, NOT on GitHub Pages; keys in server environment only)
- `server/.env.example` and `server/README.md` — backend configuration template and instructions

Keep all files together, in the same folder structure, when you upload them.

## Why HTTPS is required
iPhone Home Screen installation (PWA) and service workers require HTTPS,
except when testing on `localhost`. A plain `http://` site will not offer
"Add to Home Screen" as a standalone app, and the service worker will not register.

## Important: frontend vs backend hosting
GitHub Pages and Netlify (below) host the FRONTEND only. They cannot run the
backend proxy or hold API keys. If you later connect live data or real AI,
deploy `server/proxy.js` separately to a backend host (Render, Railway, Fly.io,
or a VPS) over HTTPS, set its keys in that host's environment variables,
and paste only its HTTPS URL into the app (Markets page). See BACKEND_SETUP.md
and AI_SETUP.md. Until then, the app correctly runs on simulated data.

## Option A — GitHub Pages (recommended, free, HTTPS)
1. Create a GitHub repository, for example `rixo-ai-trading-pro`.
2. Upload all files in this package to the repository root
   (not inside a subfolder, unless you adjust the manifest/service-worker paths).
3. In GitHub, go to Settings → Pages.
4. Under "Build and deployment", choose Source: "Deploy from a branch",
   Branch: `main`, Folder: `/ (root)`, then Save.
5. Wait for GitHub Pages to publish. Your HTTPS address will be like:
   `https://YOUR-USERNAME.github.io/rixo-ai-trading-pro/`
6. Open that address in Safari on your iPhone. Do not use the in-app browser
   inside another app — use Safari itself.

## Option B — Netlify Drop (fast alternative, HTTPS)
1. Go to Netlify Drop and drag the whole folder (with all files) onto the page.
2. Netlify will give you an HTTPS address.
3. Open that address in Safari on your iPhone.

## Install on your iPhone Home Screen
1. Open the HTTPS address in Safari.
2. Tap the Share button (the square with an arrow pointing up).
3. Scroll down and tap "Add to Home Screen".
4. Confirm the name "RIXO PRO" (or "RIXO AI Trading PRO").
5. Tap "Add".
6. Open the new icon from your Home Screen. It should open full-screen,
   without the Safari address bar, in standalone mode.

If "Add to Home Screen" does not appear:
- Make sure you are in Safari, not Chrome or an in-app browser.
- Make sure the address starts with `https://`.
- Make sure `manifest.json`, `service-worker.js`, and both icons loaded
  (check the page once, then try again — the service worker registers on load).
- If you uploaded to a subfolder, the manifest `start_url` and icon paths
  must still resolve relative to that folder.

## Testing locally
- `python3 -m http.server 8000` in this folder, then open
  `http://localhost:8000/` — service workers work on localhost for testing.
- Opening `index.html` directly with `file://` will run the dashboard,
  but the service worker and PWA install will not work, because
  service workers require HTTP/HTTPS.

## Resetting the simulation
In the app, go to History → "Reset simulated account". This clears the
simulated cash, positions, and history stored only in that browser
(`localStorage`). It does not affect any real account, because none is connected.
