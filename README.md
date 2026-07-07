# Looper

A minimal, single-file focus/countdown timer — installable as a PWA on desktop and mobile.

## Files

```
index.html        the whole app (markup, styles, and logic in one file)
manifest.json      PWA manifest (name, icons, colors, display mode)
sw.js               service worker — always loads the latest index.html
icons/              app icons for install prompts, home screen, favicon
README.md          this file
```

## Hosting on GitHub Pages

1. Create a new GitHub repo (or use an existing one) and push these files to the root of the `main` branch (or a `docs/` folder — your choice).
2. In the repo, go to **Settings → Pages**.
3. Under **Build and deployment → Source**, choose **Deploy from a branch**, pick `main` and the folder (`/root` or `/docs`), then **Save**.
4. GitHub will give you a URL like `https://<username>.github.io/<repo>/`. That's your live app.

> Keep the folder structure exactly as-is (`manifest.json`, `sw.js`, and `icons/` next to `index.html`) — the paths inside those files are relative and assume they all live in the same directory.

## Updating the app

Whenever you push a new version of `index.html` to GitHub Pages, the service worker fetches it fresh on the next load (it never serves a stale cached copy of the HTML — see [How the service worker works](#how-the-service-worker-works)). Just commit and push; there's nothing else to bump for HTML changes.

If you ever change `manifest.json` or the icons and want old cached copies purged immediately, bump `CACHE_VERSION` at the top of `sw.js` — that forces the old cache to be deleted on next activation.

## Installing as an app

### Chrome on desktop (Windows/Mac/Linux)
1. Visit your GitHub Pages URL in Chrome.
2. Click the **install icon** (a small monitor with a down arrow) in the right side of the address bar — or open the **⋮** menu → **Cast, save, and share** → **Install page as app**.
3. Confirm. Looper now opens in its own window and shows up in your OS's app launcher/Start menu.

### Chrome on Android
1. Visit the URL in Chrome.
2. Tap the **⋮** menu → **Add to Home screen** (or you'll see an automatic **Install app** banner).
3. Confirm — it installs like a native app with its own icon.

### Safari on iOS/iPadOS
1. Visit the URL in Safari.
2. Tap the **Share** icon → **Add to Home Screen**.
3. Confirm — this uses the `apple-touch-icon` and `apple-mobile-web-app-*` meta tags already included in `index.html`.

Once installed, the app opens standalone (no browser address bar), using the colors and icons defined in `manifest.json`.

## How the service worker works

- **`index.html` (and any navigation request)**: network-first. The browser always tries to fetch the live file from GitHub Pages first; it only falls back to the last cached copy if you're offline. This means your latest pushed changes show up immediately — no stale-cache surprises.
- **Static assets** (`manifest.json`, icons): cache-first with a background refresh, since these rarely change and don't need to be re-fetched every load.
- **Offline behavior**: if there's no network, the last successfully loaded version of the app is served from cache so it still opens.

## Icons

Icons in `icons/` were generated to match the app's dark/amber theme (`#020101` background, `#ff8a3d` accent, `#f3ead9` cream). Includes standard (`icon-192.png`, `icon-512.png`), maskable (`maskable-192.png`, `maskable-512.png`), an Apple touch icon, and a favicon. Swap any of these out with your own artwork if you'd like — just keep the filenames and sizes the same, or update the paths in `manifest.json` and `index.html` to match.
