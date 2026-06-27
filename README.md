# IronLog — install as a home-screen app

## What's in this folder
- `index.html` — the app itself
- `manifest.json` — tells the phone this is installable, sets the icon/name/colors
- `sw.js` — service worker, caches the app shell so it still opens offline
- `icon-192.png`, `icon-512.png` — app icon
- `icon-192-maskable.png`, `icon-512-maskable.png` — same icon with safe padding for Android's circular/squircle icon masks

All five files must stay together in the same folder — the manifest and service worker reference each other and the icons by relative path.

## Why this needs hosting (can't just double-click the file)
Two things only work once these files are served from a real `https://` address:
- **The "Install" / "Add to Home Screen" prompt** — browsers only offer this for pages with a manifest reachable over http(s).
- **Reliable storage** — your saved workouts live in the browser's `localStorage`, which is tied to that exact URL ("origin"). Opening a local file directly often gives it an unstable identity, which is why progress wasn't sticking before. One fixed URL = one fixed storage bucket, permanently.

## Fastest path: GitHub Pages (free, permanent, takes ~5 minutes)
1. Create a new repository on GitHub (public is fine), e.g. `ironlog`.
2. Upload all 5 files in this folder to that repo (drag-and-drop on the GitHub web UI works, or `git add . && git commit -m "ironlog" && git push`).
3. In the repo: **Settings → Pages → Source → Deploy from branch → main → / (root) → Save**.
4. After a minute, your app is live at `https://<your-username>.github.io/ironlog/`.
5. Open that URL on your phone in Chrome (Android) or Safari (iOS).
6. Android Chrome: tap the **⋮ menu → Install app**. iOS Safari: tap the **Share icon → Add to Home Screen**.

## Even faster, just to test (no account, temporary)
Go to **app.netlify.com/drop** on a computer and drag this whole folder onto the page. You'll get a live URL in seconds. Good for testing the install flow; for something you'll rely on long-term, GitHub Pages is the better call since the URL won't expire or change.

## Redeploying after future edits
If you ask Claude for more changes later, re-upload the updated `index.html` (and bump `CACHE_NAME` in `sw.js` to e.g. `'ironlog-v2'` so installed devices pick up the new version instead of serving the old cached one).
