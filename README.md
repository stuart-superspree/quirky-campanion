# Quirky Campanion

An NFC-triggered mobile web companion for Quirky Campers hire vans. Single-file
HTML PWA — handbook, help triage, local gems, share sheet, return checklist, and
an awe-designed experience layer for arrival, systems awareness, and trip
memory.

Built for the Quirky Campers marketplace by **Superspree**.

---

## What's in this repo

| File | Purpose |
|---|---|
| `index.html` | The entire app — a single self-contained HTML file. No external build. |
| `railway.json` | Railway platform config — sets Caddy as the server. |
| `nixpacks.toml` | Build-time instruction to install Caddy. |
| `.gitignore` | Keeps editor / OS junk out of the repo. |

That's it. No bundler, no `package.json`, no backend.

---

## Deploying to Railway via GitHub

### One-time setup

1. **Create a new GitHub repository** (public or private, doesn't matter).
2. **Upload all four files** from this folder into the root of the repo:
   - `index.html`
   - `railway.json`
   - `nixpacks.toml`
   - `.gitignore`

3. **Connect to Railway**:
   - Go to [railway.com](https://railway.com) → New Project → Deploy from GitHub repo
   - Select the repo you just created
   - Railway reads `railway.json` + `nixpacks.toml` automatically
   - First deploy takes 1–2 minutes

4. **Generate a public domain**:
   - In the Railway project → Settings → Networking → Generate Domain
   - You'll get a URL like `quirky-campanion-production.up.railway.app`

5. **Optional: custom domain**
   - Settings → Networking → Custom Domain → add e.g. `companion.quirkycampers.com`
   - Railway will give you a CNAME target to add to your DNS provider

### Ongoing updates

Any push to `main` (or whichever branch you configured) auto-deploys.

```bash
git add index.html
git commit -m "update: handbook copy for heating module"
git push
```

Deploy completes in ~30 seconds.

---

## How the stack works

- **Caddy** serves `/app/index.html` on port 8080.
- Railway injects its own `PORT` in production — but because `railway.json`
  pins `:8080` and that's what Railway expects for Nixpacks static servers,
  routing works.
- The HTML file is fully self-contained: Google Fonts via CDN, no JS
  dependencies, no API calls (the "Anthropic API in artifacts" block is
  dormant in this build).

---

## Writing NFC tags to point at the deployed app

Once live, write NFC tags with:

```
https://YOUR-DOMAIN.up.railway.app/
```

Use NFC Tools (iOS/Android) with the *URL / URI* record type. Tap-to-open
works from the lock screen on most iPhones (13+) and Android (any recent).

For per-van URLs later, you can add hash routing:

```
https://companion.quirkycampers.com/#van=maple&booking=QC-2026-04182
```

The code structure (GLOBAL / VAN_TEMPLATE / VAN_SPECIFIC / BOOKING objects
inside `index.html`) is already set up for this composable CMS pattern.

---

## Local preview

```bash
# simplest
python3 -m http.server 8080

# or with Caddy (matches production)
caddy file-server --listen :8080
```

Open <http://localhost:8080>.

---

## Resetting the arrival moment

The "arrival moment" (Place Awe intro) shows once per trip via localStorage.
To re-trigger it during demos, **long-press the van name "Maple" on the Home
screen** for ~1.2 seconds. A toast confirms the reset.

---

## Version

`v4` · Awe-integrated · April 2026
