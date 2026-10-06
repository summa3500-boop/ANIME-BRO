# AnimeBro - Complete Step-by-Step Installation Guide

> **Comprehensive A-to-Z Deployment Manual for Shared Hosting, cPanel, DirectAdmin, and VPS/Dedicated Servers**

---

## 1. Prerequisites Checklist

Before beginning installation, ensure your server environment meets the following specifications:

- **Web Server:** Apache 2.4+ (or Nginx configured with equivalent URL rewrite rules).
- **PHP Version:** PHP 7.4, 8.0, 8.1, 8.2, or 8.3+.
- **Required PHP Extensions:**
  - `pdo_mysql` (Database communication)
  - `curl` (TVDB metadata fetching and Video Provider uploads)
  - `mbstring` (Multi-byte UTF-8 string manipulation)
  - `json` (API payload serialization)
  - `gd` or `imagick` (Image processing and thumbnail handling)
  - `session` (Admin and user authentication sessions)
- **Database Server:** MySQL 5.7+ or MariaDB 10.3+.
- **Apache Modules:** `mod_rewrite`, `mod_headers`, `mod_mime`.

---

## 2. Step-by-Step Installation Procedure

### Step 1: Create Database & User
1. Log in to your hosting control panel (e.g., cPanel, DirectAdmin, Plesk, or phpMyAdmin).
2. Navigate to **MySQL Databases**.
3. Create a new database (e.g., `animebro_db`).
4. Create a new database user (e.g., `animebro_user`) with a strong password.
5. Assign the user to the database and grant **ALL PRIVILEGES**.

---

### Step 2: Upload Files to Hosting
1. Upload the AnimeBro project files to your web root directory:
   - For primary domain: `public_html/` or `htdocs/`.
   - For subdomain / addon domain: `public_html/subdomain/`.
2. Extract the archive if uploaded as a `.zip` file.
3. Ensure the `.htaccess` file is present in the root directory (enable "Show Hidden Files" in your File Manager if necessary).

---

### Step 3: Configure Database Credentials
AnimeBro supports two straightforward methods for database configuration:

#### Method A: Direct File Configuration (Recommended)
Create or edit `common/db_config.php` with your database credentials:

```php
<?php
// Database Connection Settings
define('DB_HOST', 'localhost'); // Usually localhost or 127.0.0.1
define('DB_PORT', 3306);
define('DB_USER', 'your_database_username');
define('DB_PASS', 'your_database_password');
define('DB_NAME', 'your_database_name');
```

#### Method B: Built-in Interactive Web Installer
If `common/db_config.php` does not exist, open your browser and navigate to:
```text
https://yourdomain.com/install.php
```
Follow the on-screen installer to enter database credentials. The installer will test the connection, create `common/db_config.php`, import the database tables, and prompt you to create the initial Super Admin account.

---

### Step 4: Import Database Schema & Seed Data
If importing manually via **phpMyAdmin**:
1. Open **phpMyAdmin**.
2. Select your newly created database.
3. Click the **Import** tab.
4. Choose the file located in `02-Database/database-export.sql`.
5. Click **Go** / **Import** to execute the SQL script.

---

### Step 5: Configure File & Directory Permissions
Set appropriate permissions on server directories:
- Directories: `755` (`rwxr-xr-x`)
- Files: `644` (`rw-r--r--`)
- Uploads & Cache Directories (must be writable by PHP/web server):
  - `uploads/` &rarr; `755` or `775`
  - `uploads/posters/` &rarr; `755`
  - `uploads/banners/` &rarr; `755`
  - `uploads/avatars/` &rarr; `755`
  - `uploads/logos/` &rarr; `755`
  - `uploads/backgrounds/` &rarr; `755`
  - `uploads/cache/` &rarr; `755`
  - `logs/` &rarr; `755`

---

### Step 6: Verify URL Structure & Apache `.htaccess`
Ensure `.htaccess` in the root folder contains the required rewrite rules:

- `/` &rarr; Displays the Public Landing Page.
- `/home` &rarr; Displays the main streaming portal homepage.
- `/anime/:id/:slug` &rarr; Anime details page.
- `/cartoon/:id/:slug` &rarr; Cartoon details page.
- `/movie/:id/:slug` &rarr; Movie details page.
- `/watch/:id/:slug` &rarr; Watch player page.
- `/admin/` &rarr; Admin dashboard.

---

### Step 7: Configure HTTPS / SSL
1. Install an SSL certificate (e.g., Let's Encrypt SSL via cPanel).
2. Verify that visiting `https://yourdomain.com/` opens securely without mixed content warnings.

---

### Step 8: Log In to Admin Panel & Initial Setup
1. Open your browser and navigate to:
   ```text
   https://yourdomain.com/admin/
   ```
2. Log in using your admin credentials (default credentials created during install).
3. **Important:** Immediately navigate to **Admin Panel &rarr; Staff Management** and update your Super Admin password.
4. Navigate to **Admin Panel &rarr; Settings**:
   - Set **Site Name** (e.g., `AnimeBro`).
   - Upload **Website Logo** (this logo automatically populates both the streaming portal and the Landing Page header/footer).
   - Configure **TVDB API v4 Key** and **PIN**.
   - Configure **Video Provider API Keys** (StreamHG, StreamRuby, EarnVids).
5. Navigate to **Admin Panel &rarr; Landing Page**:
   - Customize Hero text, CTAs, Showcases, FAQs, and Social channels.

---

## 3. Post-Installation Verification Checklist

- [ ] `https://yourdomain.com/` loads the Landing Page.
- [ ] `https://yourdomain.com/home` loads the main streaming homepage.
- [ ] `https://yourdomain.com/admin/` loads the Admin Login / Dashboard.
- [ ] TVDB Search test imports anime/movie metadata correctly.
- [ ] Video Provider connection tests return `Connected / Active`.
- [ ] User registration and login work seamlessly.
- [ ] Video playback loads correctly on the watch page.
