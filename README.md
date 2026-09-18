# Learning Hub

Static, zero-build collection of practice apps. Every push to `main` deploys automatically to Vercel.

## Layout

```
.
├── index.html                  hub landing page (the APPS list lives here)
├── manifest.webmanifest        PWA manifest — one installable app for the whole site
├── sw.js                       service worker (offline support)
├── vercel.json                 cache + security headers
├── icons/                      home-screen and favicon art
└── apps/
    └── english-explorer/
        └── index.html          one self-contained app per folder
```

## Adding a new quiz

1. Create `apps/<folder-name>/index.html` — one self-contained HTML file.
2. Add one entry to the `APPS` array in `index.html`:

   ```js
   { slug: 'folder-name', emoji: '🔢', title: 'Math Explorer',
     desc: 'Times tables and number bonds', ready: true }
   ```

3. Add `'/apps/<folder-name>/'` to the `CORE` array in `sw.js` and bump `CACHE`
   (e.g. `learning-hub-v1` → `learning-hub-v2`) so devices pick up the change.
4. Commit and push. Vercel builds and publishes in under a minute.

### Storage rule for new apps

All apps share one browser origin, so **`localStorage` keys must be namespaced per app**.
English Explorer uses `englishExplorer.v1`. Use `<appName>.v1` for anything new —
never a bare key like `progress`.

## Why the Home Screen matters on iPad / iPhone

Safari's Intelligent Tracking Prevention deletes script-writable storage
(`localStorage` included) after roughly **seven days without visiting the site**.
A week of school holidays is enough to wipe a child's stars.

Web apps launched from the **Home Screen** get their own storage container and are
exempt from that eviction. So the hub shows an "Add to Home Screen" card on iOS until
the site is installed, and English Explorer has a **Backup progress** panel in Settings
that saves a `.json` file as belt-and-braces insurance.

Private Browsing disables persistent storage entirely — progress will not survive there.

## Local preview

```bash
python3 -m http.server 8000
# then open http://localhost:8000
```

Service workers need `localhost` or HTTPS; they will not register from `file://`.
