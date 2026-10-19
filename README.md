# Christmas Planner

A cosy, installable Christmas planner: to-dos, food shopping, gifts, budget and prep tracking, with a live countdown to Christmas Day.

- Static site with no build step and no frameworks (Google Fonts only)
- Works offline as a PWA, and you can add it to the iPhone Home Screen
- Your data stays on your device in `localStorage` (key `xmasPlanner_v1`). Use **Settings → Export** to back it up.

## What's inside

- **Dashboard**: countdown, readiness score, budget left, your next five tasks, and gifts and food still to buy
- **To-Do**: due dates, priorities and categories, with overdue tasks highlighted. You can start from a list of suggested tasks.
- **Food Shopping**: grouped by section, with "needed by" dates so you can split the early stock-up from the last-minute fresh shop. Shows running totals.
- **Gifts**: one card per person, with a per-person budget, gift ideas from Idea → Bought → Wrapped → Given, and over-budget warnings
- **Budget**: a total split into categories. Gift and food spend fill in automatically. Everything else goes in an expense log.
- **Prep Tracker**: a weighted readiness percentage (weights are adjustable in Settings), a breakdown by area, an on-pace check against 1 Nov – 24 Dec, and editable milestones
- **Settings**: light, dark or auto theme, an optional snowfall effect, readiness weights, and export, import and reset

How the numbers work:
- A gift counts as spent once it's **Bought**. It counts towards readiness once it's **Wrapped** or **Given**.
- A person is "sorted" when they have at least one gift and every gift is Bought or further along.
- Areas with nothing in them yet are left out of the readiness percentage, so they don't drag it down.

## Files

| File | Purpose |
| --- | --- |
| `index.html` | The whole app: HTML, CSS and JS inline |
| `manifest.json` | PWA manifest (name, icons, colours) |
| `sw.js` | Service worker that caches the app for offline use |
| `icons/` | App icons and favicon |
| `netlify.toml` | Netlify publish settings and cache headers |

## Run locally

Any static file server works. For example, from this folder:

```bash
python3 -m http.server 8080
```

Or, on a Mac without Python, run `ruby -run -e httpd . -p 8080`. Then open http://localhost:8080. The service worker only runs over `http://localhost` or `https://`. It doesn't run from `file://`.

## Deploy to Netlify from GitHub

1. Create an empty repository on GitHub, for example `christmas-planner`.
2. Push this folder to it:
   ```bash
   git init
   git add .
   git commit -m "Christmas Planner"
   git branch -M main
   git remote add origin https://github.com/<you>/christmas-planner.git
   git push -u origin main
   ```
3. In Netlify, choose **Add new site → Import an existing project → GitHub**, then pick the repository.
4. Leave the build command empty and set the publish directory to `.`. `netlify.toml` already sets both, so you can accept the defaults.
5. Click **Deploy**. Netlify redeploys every time you push to `main`.

## Releasing updates

When you change any file, bump `CACHE_VERSION` in `sw.js` (for example `v1` → `v2`). Installed copies then pick up the new version on their next launch.

## Install on iPhone

Open the site in Safari, tap **Share**, then tap **Add to Home Screen**. The planner opens full-screen and works offline.

## Data and backups

- All data is saved under a single `localStorage` key with a `schemaVersion` field. Future versions migrate older data automatically.
- **Export** downloads a JSON backup. In the installed iPhone app it opens the share sheet instead, so you can save the backup to Files.
- **Import** restores a backup after you confirm. **Reset** wipes everything after you confirm.
