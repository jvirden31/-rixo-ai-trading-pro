# RIXO AI Trading PRO — Deployment & iPhone Installation (Phase 7)

## What Muse can and cannot publish for you — please read this first
Muse does **not** publish this project directly to Netlify, GitHub Pages, or
any public URL on your behalf in this chat, and I have **not** published it
anywhere. There is no verified public HTTPS URL for RIXO yet — do not share
or rely on one until you complete the steps below and open the resulting URL
yourself.

Muse's own artifact publishing (for pages in the Library) is a different
thing: it publishes a static snapshot with a fresh one-tap approval each
time, it does not update automatically, and interactive apps cannot be made
public that way. It also would not move your localStorage account to the
published copy. That is why this phase delivers a Netlify-ready frontend
package plus a safe export/import flow, instead of claiming a hosted URL
that does not exist.

## Step 1 — Deploy the frontend (Netlify Drop, no account setup beyond Netlify itself)
Use the Phase 7 frontend ZIP (the one labelled "frontend" in the delivery).
It contains ONLY frontend files: `index.html`, `manifest.json`,
`service-worker.js`, `icons/`, `netlify.toml`, and this file. It contains
NO backend code, NO `.env` file, and NO secrets — check the ZIP contents
before uploading if you want to be sure.

1. Unzip the frontend ZIP on your computer or phone.
2. Go to Netlify Drop (app.netlify.com/drop) in a desktop browser if possible.
3. Drag the **unzipped frontend folder** onto the Drop page. Netlify will
   publish its contents at a new HTTPS address (something like
   `https://random-name-123456.netlify.app`).
4. Open that HTTPS address yourself and check: the dashboard loads, all six
   tabs work, and the header shows Online/Offline. If it does not load, do
   not install it — re-upload the folder contents (not a ZIP file, and not
   a folder nested inside another folder).
5. Optional: in Netlify, rename the site to something memorable under
   Site settings → Site information. Your address will still be HTTPS.

Alternative: GitHub Pages. Upload the same frontend files to a GitHub
repository root, then Settings → Pages → Deploy from a branch → `main` /
root. The resulting address is `https://YOUR-USERNAME.github.io/repo/`.
Full steps are also in `HOSTING.md` in the full project.

## Step 2 — Move your simulated account to the hosted version (important)
Browser localStorage does **not** transfer between different website
addresses or devices. Opening the new hosted address will start at $300
there, even if your old copy has positions. That is normal browser behaviour,
not data loss — your old copy is untouched until you replace it.

To move it, safely:
1. On your OLD copy (where your account currently lives), go to History →
   "Move My Account to Another Site or Device" → EXPORT SIMULATED ACCOUNT.
   Save the downloaded `.json` file. It contains only simulated account
   data, risk settings, and your journal note — no API keys or real accounts.
2. On the NEW hosted address, go to History → same section → choose that
   `.json` file → IMPORT SIMULATED ACCOUNT. Confirm the replacement prompt.
3. Check the import result message: cash, open positions, closed trades,
   and open orders should match your old copy. If the import is invalid
   (not JSON, missing cash, wrong list types), it is rejected and your
   current account on that site is NOT changed — the error message will say so.
4. Keep the export file as a backup. Do not share it publicly; it is your
   simulated history, not a secret, but there is no reason to publish it.

Do NOT paste an API key into the app, the export file, or GitHub at any point.

## Step 3 — Install on your iPhone (Safari only)
1. On your iPhone, open Safari (not Chrome, and not an in-app browser).
2. Go to your new HTTPS address from Step 1.
3. If you moved your account, do Step 2's import first, in Safari, so the
   installed app starts from the imported account.
4. Tap the Share button (square with an arrow pointing up).
5. Scroll down and tap "Add to Home Screen".
6. Confirm the name "RIXO PRO" (or "RIXO AI Trading PRO") and tap Add.
7. Open the new icon from your Home Screen. It should open full-screen,
   without the Safari address bar, in standalone mode.

If "Add to Home Screen" does not appear:
- Make sure you are in Safari and the address starts with `https://`.
- Make sure `manifest.json`, `service-worker.js`, and both icons loaded
  (load the page once, then try again — the service worker registers on load).
- If you uploaded to a subfolder, the manifest `start_url` and icon paths
  must still resolve relative to that folder (they do in this package, as
  relative `./` paths).

## Step 4 — Backend (optional, only if you want live data or real AI)
The frontend alone is fully usable in simulated mode and needs no backend.
If you later want live data or real AI:
1. Use the separate backend package (not the frontend ZIP).
2. Deploy `server/proxy.js` to a Node.js host (Render, Railway, Fly.io, VPS)
   over HTTPS. Set keys ONLY in that host's environment variables — see
   `BACKEND_SETUP.md` and `AI_SETUP.md` in the backend package.
3. In the app (Markets page), paste ONLY the backend HTTPS URL, tap Save
   Proxy URL, then Test Live Connection. Until that test succeeds, the app
   correctly reports NOT CONNECTED and stays simulated.
4. GitHub Pages and Netlify Drop cannot run this backend or hold its keys —
   that is why it is a separate package and a separate host.

## Step 5 — Offline behaviour
Once installed, the app shell (dashboard, tabs, charts, simulated trading)
works offline from the service-worker cache (cache name `rixo-pro-v7`).
Live data and real AI will correctly report NOT CONNECTED while offline,
and the header shows Offline. Simulated trading still works offline, and
its data stays on that device until you export it.

## External services still requiring setup (nothing here is connected yet)
- Netlify (or GitHub Pages) account + frontend upload — you do this in Step 1.
- Backend host + market-data provider account + API key (server-side only)
  — only if you want live data. See `BACKEND_SETUP.md`.
- AI provider account + API key (server-side only) — only if you want real AI.
  See `AI_SETUP.md`.
- No broker, no real-money trading, and no payment service is required or
  connected at any point in this project.
