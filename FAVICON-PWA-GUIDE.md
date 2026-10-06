# Favicon & Progressive Web App (PWA) Guide

AnimeBro is configured as a fully installable **Progressive Web App (PWA)**, allowing mobile (iOS/Android) and desktop (Chrome/Edge/macOS) visitors to install AnimeBro directly to their home screen as a native-like app without an app store.

---

## 1. Icon Inventory & Standards

All platform icons reside in the `/favicon/` directory:

| Filename | Dimensions | Purpose |
| :--- | :--- | :--- |
| `favicon.ico` | Multi-size (16x16, 32x32, 48x48) | Legacy browser tabs & bookmarks |
| `favicon-16x16.png` | 16x16 px | Standard desktop browser tab icon |
| `favicon-32x32.png` | 32x32 px | Retina / High-DPI browser tab icon |
| `apple-touch-icon.png` | 180x180 px | iOS Safari "Add to Home Screen" icon |
| `android-chrome-192x192.png` | 192x192 px | Android home screen & splash icon (any + maskable) |
| `android-chrome-512x512.png` | 512x512 px | High-resolution PWA splash screen icon (any + maskable) |
| `site.webmanifest` | JSON metadata | Manifest configuration for standards-compliant browsers |

---

## 2. Dynamic Web App Manifest (`manifest.php`)

To prevent MIME-type issues on shared hosts that do not have `.webmanifest` mime mappings in Apache, AnimeBro delivers the manifest dynamically through `manifest.php`:

- **Route**: `https://yourdomain.com/manifest.php` (aliased to `/site.webmanifest`)
- **Headers**: `Content-Type: application/manifest+json; charset=utf-8`
- **Config**:
  - `start_url`: `/home`
  - `scope`: `/`
  - `display`: `standalone` (removes browser navigation bars for a full-screen app experience)
  - `theme_color` & `background_color`: `#090a0f` (deep modern dark theme)
  - `orientation`: `portrait-primary`

---

## 3. Service Worker Engine (`sw.php` / `sw.js`)

The PWA service worker is served via `sw.php` with the required `Service-Worker-Allowed: /` header:

- **Offline Fallback**: Caches essential UI shells, logo assets, and CSS stylesheets.
- **Fast Navigation**: Pre-caches critical web assets for near-instant page load transitions.
- **Install Prompt**: Emits `beforeinstallprompt` event on supported mobile/desktop browsers, triggering the in-app "Install AnimeBro App" banner.

---

## 4. How to Update Favicons and App Icons

To replace the default AnimeBro branding with your own custom logo:

1. Prepare your master square logo (recommended: 1024x1024 PNG with transparent or solid background).
2. Generate all standard sizes (16x16, 32x32, 180x180, 192x192, 512x512).
3. Overwrite the files inside `/favicon/` in the project root.
4. Clear your browser cache or unregister the service worker in Chrome DevTools (**Application -> Storage -> Clear site data**) to see the new icons immediately.
