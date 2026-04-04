---
inclusion: auto
name: pwa-patterns
description: Use when building a Progressive Web App or a mobile app
---

# PWA Patterns and Best Practices

Use these patterns when building a Progressive Web App for FVST users.

---

## Manifest (manifest.json)

Always use this as a starting point — customise name and colours:

```json
{
  "name": "App name here",
  "short_name": "Short name",
  "description": "What the app does",
  "start_url": "/",
  "display": "standalone",
  "background_color": "#ffffff",
  "theme_color": "#1a56db",
  "icons": [
    {
      "src": "icon.svg",
      "sizes": "any",
      "type": "image/svg+xml",
      "purpose": "any maskable"
    }
  ]
}
```

Icons: Always generate a simple inline SVG icon — no image files to download.

---

## Service Worker (service-worker.js)

Minimal service worker that caches the app for offline use:

```javascript
const CACHE_NAME = 'app-v1';  // Bump version (v2, v3…) on every change so users get the update
const ASSETS = ['/', '/index.html', '/style.css', '/app.js', '/manifest.json'];

self.addEventListener('install', e => {
  e.waitUntil(caches.open(CACHE_NAME).then(c => c.addAll(ASSETS)));
  self.skipWaiting();
});

self.addEventListener('activate', e => {
  e.waitUntil(
    caches.keys().then(keys =>
      Promise.all(keys.filter(k => k !== CACHE_NAME).map(k => caches.delete(k)))
    )
  );
});

self.addEventListener('fetch', e => {
  e.respondWith(
    caches.match(e.request).then(r => r || fetch(e.request))
  );
});
```

**Important:** Every time you change the app's code, bump the `CACHE_NAME` version number. Otherwise users will keep seeing the old cached version.

---

## Data storage with localStorage

For simple apps: store data as JSON in localStorage. No server, no account, works offline.

```javascript
function saveData(key, data) {
  localStorage.setItem(key, JSON.stringify(data));
}

function loadData(key, defaultValue = []) {
  const stored = localStorage.getItem(key);
  return stored ? JSON.parse(stored) : defaultValue;
}
```

**Limitations:**
- localStorage is tied to the browser and device being used. Data is not automatically shared across devices. Always inform the user of this.
- localStorage has a ~5 MB size limit. For most prototypes this is plenty, but if the app stores large amounts of data (thousands of records, images as base64), warn the user early and suggest regular exports.

**Export function:** Always include a button that exports data as CSV or JSON, so the user cannot lose their data.

---

## Mobile-friendly design — always

```css
* { box-sizing: border-box; margin: 0; padding: 0; }

body {
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif;
  font-size: 16px;           /* Never below 16px — prevents zoom on iOS */
  padding: env(safe-area-inset-top) env(safe-area-inset-right)
           env(safe-area-inset-bottom) env(safe-area-inset-left);
}

/* Touch targets must be at least 48px tall */
button, input, select {
  min-height: 48px;
  font-size: 16px;           /* Prevents auto-zoom on iOS */
  border-radius: 8px;
  padding: 12px 16px;
}

button {
  width: 100%;
  background: #1a56db;
  color: white;
  border: none;
  cursor: pointer;
  font-weight: 600;
}
```

---

## Localization

Format numbers, dates, and currency according to the user's locale. Use the browser's built-in `Intl` APIs — never hardcode formatting. Ask the user which locale to use if it isn't obvious.

**Example (Danish locale):**

```javascript
// Date
const date = new Date().toLocaleDateString('da-DK', {
  weekday: 'long', year: 'numeric', month: 'long', day: 'numeric'
});

// Number (comma as decimal separator)
const value = (12.5).toLocaleString('da-DK');  // "12,5"

// Currency
const amount = (1234.5).toLocaleString('da-DK', {
  style: 'currency', currency: 'DKK'
});  // "1.234,50 kr."
```

Replace `'da-DK'` and `'DKK'` with whatever locale and currency the user's app requires.

---

## How to test on a real phone

Always include these instructions in README.md for PWA projects:

```markdown
## Testing on your phone

1. Start the server: `python3 -m http.server 8080`
2. Find your computer's IP address:
   - Mac: System Settings → Network → IP address
   - Windows: Run `ipconfig` in terminal → IPv4 Address
3. Open `http://[your-ip]:8080` on your phone (same WiFi)
4. Install as an app:
   - iPhone: Tap the share button → "Add to Home Screen"
   - Android: Tap the menu button → "Install app" or "Add to Home Screen"
```

---

## GitHub Pages (free hosting, optional)

If the user wants to share the app with others without requiring them to run a server:

1. Create a free account at github.com
2. Create a new repository and upload the files
3. Go to Settings → Pages → select "main branch"
4. The app is available at `https://[username].github.io/[repo-name]`

Always inform the user that the app will be publicly accessible — not suitable for confidential data.
