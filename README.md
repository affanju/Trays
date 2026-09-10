# Boxes of Trays Calculator

A small offline-first PWA for a food production floor: look up a product, confirm its trays-per-carton and trays-per-box, enter the cartons made, and get the number of supplier boxes of trays used.

**Core formula:** `cartons made × trays per carton ÷ trays per box = boxes of trays used`

## Features

- **Products** — searchable, grouped by brand (ALDI / Coles / QuiteLike / Unverified), sub-grouped by category (Burgers, Meatballs, Kebabs, etc.) for larger groups. Fully editable: name, code, brand, linked tray size, trays/carton.
- **Tray Sizes** — a shared registry. Batch code and trays-per-box are set **once per tray size**, and every product linked to that size updates automatically. No more re-entering the same figure on five different burger SKUs.
- **Calculator** — enter cartons made, get trays used and boxes of trays used, with the working shown step by step.
- **Import / Export** — JSON backup (round-trips everything, including tray sizes), Excel export (`.xlsx`, one sheet for products, one for tray sizes), and Print (clean, expanded, button-free view).
- **Offline-capable PWA** — installable to a home screen via the included manifest + service worker; works without a live connection once loaded.

## Data & storage

All data lives in the browser's local storage on whatever device the app is opened on. There's no backend and no server-side database — this is a static site.

- If you install it on two different phones, they each keep their **own independent copy**. Use Export → Import to move or merge data between devices.
- Import **merges** into existing data by ID (updates matches, adds anything new, deletes nothing).
- Nothing is sent anywhere; all storage is local to the browser.

If you need one shared, always-in-sync copy across multiple devices, that requires a small real backend (e.g. a lightweight API + database) rather than a static PWA — a different project from this one.

## Running it

No build step — it's plain HTML/CSS/JS.

**Locally:**
```bash
python3 -m http.server 8000
# then open http://localhost:8000
```

**GitHub Pages:**
1. Push this repo to GitHub.
2. Repo Settings → Pages → set source to the `main` branch, root folder.
3. Open the published URL. On a phone, use "Add to Home Screen" (iOS Safari) or the install prompt (Android Chrome) to install it as an app.

## Updating an installed copy

The service worker fetches network-first and only falls back to its cache when offline, so an installed copy should pick up new deploys automatically the next time it's opened with a connection. If it doesn't, force-refresh the page (or uninstall/reinstall) to clear the cache.

## File structure

```
.
├── index.html          # the entire app — markup, styles, and logic
├── manifest.json        # PWA manifest (name, icons, theme colour)
├── service-worker.js    # offline caching, network-first
├── icon-192.png / .svg  # app icon, 192×192
└── icon-512.png / .svg  # app icon, 512×512
```

## Known data caveats

A few tray sizes carry a note flagging unresolved ambiguity from the original handwritten source sheet (e.g. ties between differently-named products that turned out to use the same physical tray). These are visible directly in the app under **Tray Sizes** — check the note text on any entry showing a warning icon before relying on it for a real count.
