# URL Routing & Rewrite Rules Guide

This document describes the URL architecture, Apache `.htaccess` rewrite rules, clean SEO URL structures, and routing parameters used throughout AnimeBro.

---

## 1. Web Server Requirements & Prerequisites

AnimeBro utilizes standard Apache URL rewriting:
- **Apache Module**: `mod_rewrite` must be enabled.
- **Directory Configuration**: `AllowOverride All` must be configured in your VirtualHost so the root `.htaccess` file can execute rewrite rules.

---

## 2. Complete Route Mapping Table

| Public URL Pattern | Internal Script Target | Parameters & Purpose |
| :--- | :--- | :--- |
| `/` | `index.php` (or `landing_page.php`) | Root entry point (Public Landing Page or Streaming Hub) |
| `/home` or `/home/` | `home.php` | Main Streaming Platform catalog & hero sliders |
| `/anime/123` | `anime_details.php?id=123` | Anime series details, season tabs, episode list |
| `/anime/123/attack-on-titan` | `anime_details.php?id=123&slug=attack-on-titan` | SEO-friendly anime series details |
| `/cartoon/45` | `cartoon_details.php?id=45` | Cartoon series details page |
| `/cartoon/45/sponge-bob` | `cartoon_details.php?id=45&slug=sponge-bob` | SEO-friendly cartoon details |
| `/movie/78` | `movie_details.php?id=78` | Feature movie details and info |
| `/movie/78/demon-slayer-mugen`| `movie_details.php?id=78&slug=demon-slayer-mugen` | SEO-friendly movie details |
| `/watch/999` | `watch.php?id=999` | Video streaming watch page with multi-server player |
| `/watch/999/episode-1` | `watch.php?id=999&slug=episode-1` | SEO-friendly watch page |
| `/robots.txt` | `robots.php` | Dynamic crawler directives with sitemap link |
| `/sitemap.xml` | `sitemap.php` | Dynamic XML sitemap generator |
| `/site.webmanifest` | `manifest.php` | PWA web app manifest with dynamic JSON headers |
| `/sw.js` | `sw.php` | PWA service worker with `Service-Worker-Allowed` header |
| `/admin/*` | `/admin/*` | Admin panel management area (direct script routing) |

---

## 3. Detailed `.htaccess` Code Structure

```apache
# AnimeBro Apache Configuration - Shared Hosting & VPS Compatible
<IfModule mod_rewrite.c>
    RewriteEngine On

    # 1. Dynamic robots.txt, sitemap.xml & PWA routing
    RewriteRule ^robots\.txt$ robots.php [L]
    RewriteRule ^sitemap\.xml$ sitemap.php [L]
    RewriteRule ^favicon/site\.webmanifest$ manifest.php [L]
    RewriteRule ^site\.webmanifest$ manifest.php [L]
    RewriteRule ^manifest\.json$ manifest.php [L]
    RewriteRule ^sw\.js$ sw.php [L]

    # 2. Main Streaming Homepage
    RewriteRule ^home/?$ home.php [QSA,L]

    # 3. Clean SEO URLs for Titles & Players
    RewriteRule ^anime/([0-9]+)(?:/([^/]+))?/?$ anime_details.php?id=$1&slug=$2 [QSA,L]
    RewriteRule ^cartoon/([0-9]+)(?:/([^/]+))?/?$ cartoon_details.php?id=$1&slug=$2 [QSA,L]
    RewriteRule ^movie/([0-9]+)(?:/([^/]+))?/?$ movie_details.php?id=$1&slug=$2 [QSA,L]
    RewriteRule ^watch/([0-9]+)(?:/([^/]+))?/?$ watch.php?id=$1&slug=$2 [QSA,L]

    # 4. Non-Existent File 404 Fallback
    RewriteCond %{REQUEST_FILENAME} !-f
    RewriteCond %{REQUEST_FILENAME} !-d
    RewriteRule ^(.*)$ 404.php [L,QSA]
</IfModule>

# Custom Error Pages
ErrorDocument 404 /404.php
ErrorDocument 403 /403.php
ErrorDocument 500 /500.php

# Security & Direct Config Protection
<Files "db_config.php">
    Require all denied
</Files>
```

---

## 4. Troubleshooting Route & URL Issues

1. **Clicking `/home` or `/anime/1` gives HTTP 404**:
   - Cause: `mod_rewrite` is disabled or `AllowOverride All` is missing in Apache config.
   - Fix: Run `sudo a2enmod rewrite && sudo systemctl restart apache2`.
2. **Infinite Loop or Internal Server Error (500)**:
   - Cause: Conflicting `.htaccess` rules in parent hosting directories or missing PHP modules.
   - Fix: Review error logs (`/var/log/apache2/error.log` or cPanel Error Log).
