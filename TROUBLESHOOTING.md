# Operational Troubleshooting & Diagnostics Guide

This comprehensive troubleshooting matrix provides diagnostic steps, root causes, and immediate solutions for operational issues across deployment, database, video streaming, API scraping, and admin operations.

---

## 1. Quick Diagnostic Checklist

When encountering any error on your AnimeBro deployment, check these three core items first:

1. **Check PHP Error Log**:
   - Shared Hosting / cPanel: Check `error_log` file in the project root or cPanel **Errors** tool.
   - VPS / Dedicated Server: Check `/var/log/apache2/error.log` or `/var/log/nginx/error.log`.
2. **Verify Database Connectivity**:
   - Ensure credentials in `common/db_config.php` match the active MySQL database user and permissions.
3. **Verify Apache `.htaccess` Support**:
   - Ensure `mod_rewrite` is enabled and `AllowOverride All` is set.

---

## 2. Issue Resolution Matrix

### A. General Server & HTTP Errors

| Error / Symptom | Root Cause | Step-by-Step Resolution |
| :--- | :--- | :--- |
| **HTTP 500 Internal Server Error** | Missing PHP extension (`pdo_mysql`, `curl`, `mbstring`, `gd`), syntax error in modified file, or `.htaccess` syntax conflict. | 1. Check PHP error log.<br>2. Confirm PHP extensions are active in `php.ini` or cPanel "Select PHP Version".<br>3. Verify `.htaccess` has no unsupported directives. |
| **HTTP 404 on Sub-pages (`/home`, `/anime/*`)** | `mod_rewrite` is disabled or `.htaccess` is not being evaluated by Apache. | 1. Ensure `.htaccess` exists in project root.<br>2. In Apache VirtualHost configuration, set `AllowOverride All`.<br>3. Run `sudo a2enmod rewrite && sudo systemctl restart apache2`. |
| **Blank White Screen (WSOD)** | Fatal PHP error with `display_errors` turned off in production. | 1. Inspect server `error_log`.<br>2. Temporarily set `define('DEBUG_MODE', true);` in `common/config.php` to reveal the exact stack trace, then turn it off once resolved. |

---

### B. Database & Installation Errors

| Error / Symptom | Root Cause | Step-by-Step Resolution |
| :--- | :--- | :--- |
| **`Database Connection Error: Access denied for user...`** | Incorrect MySQL username or password in `common/db_config.php`. | 1. Verify `DB_USER` and `DB_PASS`.<br>2. In cPanel MySQL Databases, ensure the user is assigned to the database with `ALL PRIVILEGES`. |
| **`Table 'animebro.settings' doesn't exist`** | Database schema export was not imported prior to running the site. | 1. Open phpMyAdmin or MySQL CLI.<br>2. Import `02-Database/database-export.sql`. |
| **`SQLSTATE[HY000] [2002] Connection refused`** | MySQL server is down or `DB_HOST` is incorrect. | 1. Ensure MySQL daemon is running (`sudo systemctl status mysql`).<br>2. Use `localhost` or `127.0.0.1` as `DB_HOST`. |

---

### C. Admin & Security Issues

| Error / Symptom | Root Cause | Step-by-Step Resolution |
| :--- | :--- | :--- |
| **`CSRF token validation failed`** | Expired session, form submitted across multiple browser tabs, or session directory not writable. | 1. Refresh the admin page and submit again.<br>2. Verify `session.save_path` directory on server is writable (`chmod 773 /tmp` or host session dir). |
| **Cannot log in as Admin (Forgotten Password)** | Forgotten admin credentials. | 1. Use MySQL / phpMyAdmin to open the `admin` table.<br>2. Generate a new bcrypt hash using PHP: `password_hash('NewPassword123!', PASSWORD_DEFAULT)` and update the `password` field for the admin record. |
| **Admin session logs out immediately** | Cookie domain mismatch or mixed HTTP/HTTPS protocol. | 1. Ensure `SITE_URL` uses `https://`.<br>2. Verify browser allows session cookies for your domain. |

---

### D. Video Uploading & Playback Issues

| Error / Symptom | Root Cause | Step-by-Step Resolution |
| :--- | :--- | :--- |
| **Video upload times out or fails at 99%** | PHP file upload limits exceeded (`upload_max_filesize`, `post_max_size`, `max_execution_time`). | 1. In `php.ini`, increase `upload_max_filesize = 2048M`, `post_max_size = 2048M`, `max_execution_time = 600`, `max_input_time = 600`.<br>2. Restart Apache / PHP-FPM. |
| **Video player shows black screen or spinner** | Third-party provider still encoding video or invalid file code. | 1. Check provider dashboard (StreamHG / StreamRuby / EarnVids) to confirm video encoding status is `READY`.<br>2. Switch to an alternative player server tab on the watch page. |
| **Remote URL upload fails** | Remote source link is expired, IP-locked, or provider remote upload queue is full. | 1. Verify direct link works in browser.<br>2. Use Direct Local Upload workflow instead. |

---

### E. TVDB Scraper Issues

| Error / Symptom | Root Cause | Step-by-Step Resolution |
| :--- | :--- | :--- |
| **`TVDB API Key is not configured`** | Settings key is missing in database. | Enter valid v4 API key in **Admin Settings -> API Integrations**. |
| **`TheTVDB returned 401 Unauthorized`** | Key expired, invalid, or requires subscriber PIN. | Generate a fresh API key from thetvdb.com and test connection via Admin Settings. |
| **Missing artwork / Broken poster thumbnails** | Remote TVDB CDN images blocked by browser ad blocker or hotlink restriction. | Ensure image URLs start with `https://artworks.thetvdb.com/`. |
