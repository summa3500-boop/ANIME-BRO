# AnimeBro - Configuration & Environment Guide

> **Master Guide for Database, Server, and Service Configuration**

---

## 1. Database Configuration (`common/db_config.php`)

The database connection is defined in `common/db_config.php` using standard PHP constants. When installing on a new server, configure this file:

```php
<?php
/**
 * AnimeBro - Database Configuration File
 */
define('DB_HOST', 'localhost');      // Database host (usually localhost or 127.0.0.1)
define('DB_PORT', 3306);             // MySQL/MariaDB port (default: 3306)
define('DB_USER', 'your_db_user');   // Database username
define('DB_PASS', 'your_db_pass');   // Database password
define('DB_NAME', 'your_db_name');   // Database name
```

If `common/db_config.php` is omitted, the application falls back to default settings defined in `common/config.php` or launches the interactive web installer (`install.php`).

---

## 2. Dynamic Site URL Resolution

AnimeBro dynamically resolves the absolute site URL from the active HTTP request in `common/config.php`:

```php
// Protocol detection
$protocol = (!empty($_SERVER['HTTPS']) && $_SERVER['HTTPS'] !== 'off') || 
            ($_SERVER['SERVER_PORT'] ?? 80) == 443 ||
            (isset($_SERVER['HTTP_X_FORWARDED_PROTO']) && $_SERVER['HTTP_X_FORWARDED_PROTO'] === 'https')
            ? 'https://' : 'http://';

// Base URL definition
define('SITE_URL', $protocol . $_SERVER['HTTP_HOST'] . $baseDir);
```

This ensures that whether the platform is hosted on `https://yourdomain.com`, `https://subdomain.yourdomain.com`, or in a subfolder `http://localhost/animebro`, internal links, assets, and API callbacks resolve automatically without manual URL changes.

---

## 3. Database Settings Table (`settings`)

Global system configurations are stored in the MySQL `settings` table as key-value pairs and retrieved efficiently via `get_setting($key, $default)`:

| Setting Key | Description | Default Value | Configurable In |
| :--- | :--- | :--- | :--- |
| `app_name` | Public site name | `AnimeBro` | Admin &rarr; Settings |
| `site_tagline` | Global site tagline | `Watch HD Anime & Cartoons` | Admin &rarr; Settings |
| `site_logo_path` | Global website logo path | `uploads/logos/logo.png` | Admin &rarr; Settings &rarr; Website Logo |
| `maintenance_mode` | Maintenance mode switch (`1` = ON, `0` = OFF) | `0` | Admin &rarr; Settings |
| `tvdb_api_key` | TheTVDB API v4 Key | `YOUR_TVDB_API_KEY` | Admin &rarr; Settings |
| `tvdb_pin` | TheTVDB Subscriber PIN | `YOUR_TVDB_PIN` | Admin &rarr; Settings |
| `provider_streamhg_key` | StreamHG API Key | `YOUR_STREAMHG_KEY` | Admin &rarr; Settings / Video Providers |
| `provider_streamruby_key`| StreamRuby API Key | `YOUR_STREAMRUBY_KEY` | Admin &rarr; Settings / Video Providers |
| `provider_earnvids_key` | EarnVids API Key | `YOUR_EARNVIDS_KEY` | Admin &rarr; Settings / Video Providers |
| `landing_page_enabled` | Public Landing Page switch (`1`/`0`) | `1` | Admin &rarr; Landing Page |
| `landing_hero_title` | Landing Hero main headline | `Watch Anime in Full HD` | Admin &rarr; Landing Page |
| `landing_hero_badge` | Landing Hero glowing badge | `Stream 10,000+ Episodes` | Admin &rarr; Landing Page |
| `landing_sections_order`| Order sequence of landing sections | `hero,about,features...` | Admin &rarr; Landing Page |

---

## 4. Environment Variables (`.env.example`)

For containerized deployments or server environments using environment variables, a reference `.env.example` is provided in `03-Configuration/.env.example`.
